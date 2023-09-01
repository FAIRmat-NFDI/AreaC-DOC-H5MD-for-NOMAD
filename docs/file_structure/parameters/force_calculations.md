# Force calculations

The `force_calculations` group contains the parameters for force calculations according to the a force field during a molecular dynamics run.

The following json template illustrates the structure of the `force_calculations` group, with example values for clarity:

```json
{
    "vdw_cutoff": {"value": 1.2, "unit": "nm"},
    "coulomb_type": "particle_mesh_ewald",
    "coulomb_cutoff": {"value": 1.2, "unit": "nm"},
    "neighbor_searching": {
        "neighbor_update_frequency": 1,
        "neighbor_update_cutoff": {"value": 1.2, "unit": "nm"}
        }
    }
```

In the following, we provide the NOMAD definitions for each of these quantities:

* `vdw_cutoff`
:

        Quantity(
                type=np.float64,
                shape=[],
                unit='m',
                description='''
                Cutoff for calculating VDW forces.
                ''')

* `coulomb_type`
:

        Quantity(
            type=MEnum('cutoff', 'ewald', 'multilevel_summation', 'particle_mesh_ewald',
                    'particle_particle_particle_mesh', 'reaction_field'),
            shape=[],
            description='''
            Method used for calculating long-ranged Coulomb forces.

            Allowed values are:

            | Barostat Name          | Description                               |

            | ---------------------- | ----------------------------------------- |

            | `""`                   | No thermostat               |

            | `"Cutoff"`          | Simple cutoff scheme. |

            | `"Ewald"` | Standard Ewald summation as described in any solid-state physics text. |

            | `"Multi-Level Summation"` |  D. Hardy, J.E. Stone, and K. Schulten,
            [Parallel. Comput. **35**, 164](https://doi.org/10.1016/j.parco.2008.12.005)|

            | `"Particle-Mesh-Ewald"`        | T. Darden, D. York, and L. Pedersen,
            [J. Chem. Phys. **98**, 10089 (1993)](https://doi.org/10.1063/1.464397) |

            | `"Particle-Particle Particle-Mesh"` | See e.g. Hockney and Eastwood, Computer Simulation Using Particles,
            Adam Hilger, NY (1989). |

            | `"Reaction-Field"` | J.A. Barker and R.O. Watts,
            [Mol. Phys. **26**, 789 (1973)](https://doi.org/10.1080/00268977300102101)|
            ''')


* `coulomb_cutoff`
:

        Quantity(
            type=np.float64,
            shape=[],
            unit='m',
            description='''
            Cutoff for calculating short-ranged Coulomb forces.
            ''')

* `neighbor_searching`
:
Section containing the parameters for neighbor searching/lists during a molecular dynamics run.

* `neighbor_update_frequency`
:

        Quantity(
            type=int,
            shape=[],
            description='''
            Number of timesteps between updating the neighbor list.
            ''')

* `neighbor_update_cutoff`
:

        Quantity(
            type=np.float64,
            shape=[],
            unit='m',
            description='''
            The distance cutoff for determining the neighbor list.
            ''')
