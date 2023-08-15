Observables group
-----------------

Macroscopic observables, or more generally, averages of some property over many
particles, are stored in the root group `observables`. Observables representing
only a subset of the particles may be stored in appropriate subgroups similarly
to the `particles` tree. Each observable is stored as an H5MD element. The
shape of the corresponding dataset (the element itself for time-independent data
and `value` for time-dependent data) is the tensor shape of the observable,
prepended by a `[variable]` dimension for time-dependent data.

The contents of the observables group has the following structure:

    observables
     \-- <observable1>
     |    \-- step: Integer[variable]
     |    \-- time: Float[variable]
     |    \-- value: <type>[variable]
     \-- <observable2>
     |    \-- step: Integer[variable]
     |    \-- time: Float[variable]
     |    \-- value: <type>[variable][D]
     \-- <group1>
     |    \-- <observable3>
     |         \-- step: Integer[variable]
     |         \-- time: Float[variable]
     |         \-- value: <type>[variable][D][D]
     \-- <observable4>: <type>[]
     \-- ...