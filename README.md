# REEVRP-Instances
This repository contains test instances for the Range-Extended Electric Vehicle Routing Problem (REEVRP) studied in the following paper:
```
@article{subramanyam2021joint,
  title={Joint Routing of Conventional and Range-Extended Electric Vehicles in a Large Metropolitan Network},
  author={Subramanyam, Anirudh and Cokyasar, Taner and Larson, Jeffrey and Stinson, Monique},
  journal={arXiv preprint arXiv:2112.12769},
  year={2021}
}
```

# Explanation of data files
## Overview
There are 450 test instances in the `data` subfolder. Each test instance is described using 4 files which are named according to the format `I*_N*_M*_Q*_D*_T*_$Type.csv`, where
* `I` prefixes the unique instance number
* `N` prefixes the number of delivery locations
* `M` prefixes the number of electric vehicles (EVs)
* `Q` prefixes the capacity of each vehicle
* `D` prefixes the EV mode distance range of each EV
* `T` prefixes the duration limit (i.e., total time limit) of each vehicle
* `$Type` is one of `Demands`, `Distances`, `Fleet` or `TravelTimes`

## Description of `*_Distances.csv` and `*_TravelTimes.csv` files
These files are distance and travel time matrices of size `(N+1) x (N+1)`, respectively. The first row and first column in both files denote the unique indices of each of the `N+1` locations: the first entry is the index of the depot while the remaining `N` entries are the indices of the delivery locations.

## Description of `*_Fleet.csv` files
This file contains information about the package capacity (`Q`) of each vehicle, duration limit (`T`), cost coefficients etc. along with their units. Entries in these files are self-explanatory. 

## Description of `*_Demands.csv` files
Each row (except the first header row) contains the following information in order:
1. Unique index of delivery location
2. Number of packages to be delivered to this delivery location
3. Service time incurred at this delivery location (including `total_package_dropping_time` which is explained below)
4. Service distance incurred at this delivery location

The service time corresponds to the intra-location travel and service time. The `total_package_dropping_time` at any location is equal to the number of packages delivered at that location (2nd entry) times the `Package drop time` whose value can be found in the `*_Fleet.csv` file. Therefore, the intra-location travel time (excluding `total_package_dropping_time`) can be obtained by subtracting the `total_package_dropping_time` from the value reported in the 3rd entry of the row. Note, however, that the duration limit (i.e., total time limit) of each vehicle must take into account both the intra-location travel time as well as the `total_package_dropping_time`.

Similarly, the service distance (4th entry) represents the total intra-location distance to be traveled at that location. The intra-location service distance for location `j` can be added to the distance matrix entries `(i, j)` for all `i != j`. This distance must be accounted for when calculating the total distance covered by a vehicle (e.g., for purposes of calculating costs or EV mode distances).

