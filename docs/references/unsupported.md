# Unsupport features in H5MD-NOMAD

In order to effectively parse and normalize the molecular simulation data, the H5MD-NOMAD schema extends the original H5MD framework while also enforces various restrictions to the schema. This page contains a list of such additions and restrictions. Here we distinguish between "unused" features, i.e., metadata that will be ignored by NOMAD and "unsupported" features, i.e., structures that will likely cause an error if used within an H5MD-NOMAD file for upload to NOMAD.

## New or ammended features

* [additional standardized particles group elements](../file_structure/particles.md#standardized-h5md-nomad-elements-for-particles-group)

* [boundary attribute changed to boolean datatype](../file_structure/particles.md#boundary_anchor)

* [treatment of units](../units.md)

## Unused features

* [modules in h5md metadata](../file_structure/h5md.md#modules-currently-unused-in-h5md-nomad)

* [arbitrary particle groups not parsed, group labeled `all` required](../file_structure/particles.md#the-particles-group)

* [image, species, and id elements of particles group](../file_structure/particles.md#image_anchor)

## Unsupported features

* [fixed step and time storage](quick_H5MD_basics.md#fixed-step-and-time-storage-currently-not-supported-in-h5md-nomad)

* [time-dependent particle lists](quick_H5MD_basics.md#time-dependence-time-dependent-particle-lists-currently-not-supported-in-h5md-nomad)

* [time-dependent model labels for particles](../file_structure/particles.md#species_label_anchor)

