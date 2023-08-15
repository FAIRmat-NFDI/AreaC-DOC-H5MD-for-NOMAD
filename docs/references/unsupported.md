# Unsupport features in H5MD-NOMAD

In order to effectively parse and normalize the molecular simulation data, the H5MD-NOMAD schema enforces various restrictions to the original H5MD framework. This page contains a list of such restrictions. Here we distinguish between "unused" features, i.e., metadata that will be ignored by NOMAD and "unsupported" features, i.e., structures that will likely cause an error if used within an H5MD-NOMAD file for upload to NOMAD.

## Unused features

* [modules in h5md metadata](../file_structure/h5md.md#modules-not-currently-used-in-h5md-nomad)

* [arbitrary particle groups not parsed, group labeled `all` required](../file_structure/particles.md#the-particles-group)

## Unsupported features

* [fixed step and time storage](quick_H5MD_basics.md#fixed-step-and-time-storage-not-currently-supported-in-h5md-nomad)

* [time-dependent particle lists](quick_H5MD_basics.md#time-dependence-time-dependent-particle-lists-currently-not-supported-in-h5md-nomad)

