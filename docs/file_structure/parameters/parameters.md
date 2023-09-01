# The parameters group

The initial H5MD proposed a simple and flexible schema for the storage of general "parameter" information within the `parameters` group, with the following structure:

    parameters
     +-- <user_attribute1>
     \-- <user_data1>
     \-- <user_group1>
     |    \-- <user_data2>
     |    \-- ...
     \-- ...

In contrast, the H5MD-NOMAD schema calls for very specific structures to be used when storing parameter information. While the previous groups have attempted to stay away from enforcing NOMAD-specific data structures on the user, instead opting for more intuitive and generally-convenient structures, the `parameters` group utilizes already-existing metadata and structures within NOMAD to efficiently import simulation parameters in a way that is searcheable and comparable to simulations performed by other users.

In this way, the H5MD-NOMAD `parameters` group has the following structure:

    parameters
     \-- <parameter_subgroup_1>
     |    \-- ...
     \-- <parameter_subgroup_2>
     |    \-- ...
     \-- ...

The subgroups `force_calculations` and `workflow` are supported. The following pages describe the detailed data structures for these subgroups, using the NOMAD MetaInfo definitions for each underlying `Quantity`. Please note that:

1. Quantities with `type=MEnum()` are restricted to the provided allowed values.

2. The unit given in the MetaInfo definition does not have to be used within the H5MD-NOMAD file, however, the dimensionality of the unit should match.

