# The particles group

Particle attributes, i.e., information about each particle in the system, are stored within the `particles` group.
According to the original H5MD schema, the `particles` group is a container for subgroups that
represent different subsets of the system under consideration.
For simplicity of parsing, H5MD-NOMAD currently requires one such group, labeled `all`, to contain all the particles and corresponding attributes to be stored in the NOMAD archive.
Additional particle groups will be ignored.

For each dataset, the ordering of indices (whenever relevant) are as follows: frame index, particle index, dimension index.
Thus, the contents of the `particles` group for a trajectory with `N_frames` frames and `N_part` particles in a `D`-dimensional space can be represented:

    particles
     \-- all
     |    \-- box
     |    \-- (time-dependent_vector_attribute_name)
     |    |    \-- step: Integer[N_frames]
     |    |    \-- time: Float[N_frames]
     |    |    \-- value: <type>[N_frames][N_part][D]
     |    \-- (time-dependent_scalar_attribute_name)
     |    |    \-- step: Integer[N_frames]
     |    |    \-- time: Float[N_frames]
     |    |    \-- value: <type>[N_frames][N_part]
     |    \-- (time-independent_vector_attribute_name): <type>[N_part][D]
     |    \-- (time-independent_scalar_attribute_name): <type>[N_part]
     |    \-- ...
     \-- <group2>
          \-- ...

Standard H5MD elements for the particles group:

`position`
:   An element that describes the particle positions as coordinate vectors of
    `Float` or `Integer` type.

<!-- If the component $k$ of `box/boundary` (see [below](#simulation-box)) is set
to `none`, the data indicate for each particle the component $k$ of its
absolute position in space. If the component $k$ of `box/boundary` is set to
`periodic`, the data indicate for each particle the component $k$ of the
absolute position in space of an *arbitrary* periodic image of that particle. -->

`image`
:   An element that represents periodic images of the box as coordinate vectors
    of `Float` or `Integer` type and allows one to compute for each particle its
    absolute position in space. If `image` is present, `position` must be
    present as well. For time-dependent data, the `step` and `time` datasets of
    `image` must equal those of `position`, which must be accomplished by
    hard-linking the respective datasets.

<!-- If the component $k$ of `box/boundary` (see [below](#simulation-box)) is set
to `none`, the values of the corresponding component $k$ of `image` serve as
placeholders. If the component $k$ of `box/boundary` is set to `periodic`,
for a cuboid box, the component $k$ of the absolute position of particle $i$
is computed as $R_{ik} = r_{ik} + L_k a_{ik}$, where $\vec r_i$ is taken
from `position`, $\vec a_i$ is taken from `image`, and $\vec L$ from
`box/edges`. -->

`velocity`
:   An element that contains the velocities for each particle as a vector of
    `Float` or `Integer` type.

`force`
:   An element that contains the total forces (i.e., the accelerations
    multiplied by the particle mass) for each particle as a vector of `Float`
    or `Integer` type.

`mass`
:   An element that holds the mass for each particle as a scalar of `Float`
    type.

`species`
:   An element that describes the species for each particle, i.e., its
    atomic or chemical identity, as a scalar of `Enumeration` or `Integer`
    data type. Particles of the same species are assumed to be identical with
    respect to their properties and unbonded interactions.

`id`
:   An element that holds a scalar identifier for each particle of `Integer`
    type, which is unique within the given particle subgroup. The `id` serves
    to identify particles over the course of the simulation in the case when
    the order of the particles changes, or when new particles are inserted and
    removed. If `id` is absent, the identity of the particles is given by their
    index in the `value` datasets of the elements within the same subgroup.

<!-- A *fill value* (see
[§ 6.6](http://www.hdfgroup.org/HDF5/doc/UG/11_Datatypes.html#Fvalues) in
[@HDF5_users_guide]) may be defined for `id/value` upon dataset creation.
When the identifier of a particle is equal to this user-defined value,
the particle is considered non-existing, the entry serves as a
placeholder. This permits the storage of subsystems whose number of
particles varies in time. For the case of varying particle number, the
dimension denoted by `[N]` above may be variable. -->

`charge`
:   An element that contains the charge associated to each particle as a
    scalar, of `Integer` or `Float` type.

<!-- `charge` has the optional attribute `type` of fixed-length string datatype
and of scalar dataspace, possible values are `effective` and `formal`. In
the case `effective`, the charge is part of an effective description of the
interactions with the precise meaning depending on the underlying empirical
force fields or coarse-grained models.

In the case `formal`, the charge is the so-called "formal charge" assigned
to an atom (see <http://en.wikipedia.org/wiki/Formal_charge>) and must be
of `Integer` type. This case corresponds to the entries in PDB files (see
definition in the PDBx/mmCIF dictionary
<http://mmcif.wwpdb.org/dictionaries/mmcif_pdbx_v40.dic/Items/_atom_site.pdbx_formal_charge.html>).

If none of `effective` or `formal` describes the data properly, the
attribute `type` may be omitted. -->

## Simulation box

The specification of the simulation box is stored in the group `box`, which
must be contained within each of the subgroups of the `particles` group.
Storing the box information at several places reflects the fact that different
subgroups may be sampled at different time grids. This way, the box information
remains associated to a group of particles.  A specific requirement for `box`
groups inside `particles` is that the `step` and `time` datasets exactly match
those of the corresponding `position` groups, which must be accomplished by
hard-linking the respective datasets.

The spatial dimension and the boundary conditions of the box are stored as
attributes to the `box` group, e.g., :

    particles
     \-- <group1>
          \-- box
               +-- dimension: Integer[]
               +-- boundary: String[D]
               \-- (edges)

`dimension`
:   An attribute that stores the spatial dimension `D` of the simulation box
    and is of `Integer` datatype and scalar dataspace.

`boundary`
:   An attribute, of fixed-length string datatype and of simple dataspace of
    rank 1 and size `D`, that specifies the boundary condition of the box along
    each dimension. The values in `boundary` are either `periodic` or `none`:

    `periodic` The simulation box is periodically continued along the given
    dimension and serves as the unit cell for an infinite tiling of space.

    `none` No boundary condition is imposed. This summarizes the situations of
    open systems (i.e., an infinitely large box) and closed systems (e.g., due
    to an impenetrable wall). For those components where `boundary` is set to
    `none`, the corresponding values of `edges` serve as placeholders.

Information on the geometry of the box edges is stored as an H5MD element,
allowing for the box to be fixed in time or not.  Supported box shapes are the
cuboid and triclinic unit cell, for other shapes a transformation to the
triclinic shape may be considered [@Bekker:1997]. If all values in `boundary`
are `none`, `edges` may be omitted.

`edges`
:   A `D`-dimensional vector or a `D` × `D` matrix, depending on the geometry
    of the box, of `Float` or `Integer` type. If `edges` is a vector, it
    specifies the space diagonal of a cuboid-shaped box. If `edges` is a
    matrix, the box is of triclinic shape with the edge vectors given by the
    rows of the matrix.

    For a time-dependent box, a cuboid geometry is encoded by a dataset `value`
    (within the H5MD element) of rank 2 (1 dimension for the time and 1 for the
    vector) and a triclinic geometry by a dataset `value` of rank 3 (1
    dimension for the time and 2 for the matrix).

    For a time-independent box, a cuboid geometry is encoded by a dataset
    `edges` of rank 1 and a triclinic geometry by a dataset of rank 2.

For instance, a cuboid box that changes in time would appear as:

    particles
     \-- <group1>
          \-- box
               +-- dimension: Integer[]
               +-- boundary: String[D]
               \-- edges
                    \-- step: Integer[variable]
                    \-- time: Float[variable]
                    \-- value: <type>[variable][D]

where `dimension` is equal to `D`. A triclinic box that is fixed in time would
appear as:

    particles
     \-- <group1>
          \-- box
               +-- dimension: Integer[]
               +-- boundary: String[D]
               \-- edges: <type>[D][D]

where `dimension` is equal to `D`.
