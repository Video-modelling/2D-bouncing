# 2D-bouncing [![](https://github.com/Visual-modelling/2D-bouncing/workflows/2D-bouncing/badge.svg)](https://github.com/Visual-modelling/2D-bouncing/actions)
Simple 2D dataset of bouncing balls

![](example.gif)

## Requirements

Install the python dependancies (`pip install -r requirements.txt`) and [imagemagick](https://www.archlinux.org/packages/?name=imagemagick).

Additionally, for the 3d version install blender.

## Usage
### 2D
To generate a dataset of N videos, run:

`python src/generate_dataset.py N my_dataset_name`

This will create a directory `my_dataset_name` with a directory per simulation:
```
.
└── my_dataset_name
    ├── config.yaml
    ├── 0000
    │   ├── config.yaml
    │   ├── positions.csv
    │   ├── frame_000.png
    │   ...
    │   ├── frame_099.png
    │   └── simulation.gif
    ....
    └── N
        └── ...
```

Each sequence contains a `config.yaml` file recording the simulation parameters.


# 3D

The 3D dataset takes much longer than the 2D one to render so the parameter space can be divided between multiple machines. First generate lists of parameters for each node to render:
```
python src/generate_parameter_space.py DATASET_NAME NUM_MACHINES SIZE_OF_DATASET
```
This will create a directory with the files:
```
.
└── DATASET_NAME
    ├── config.yaml
    ├── parameter_space0.yaml
    ├── ...
    └── parameter_spaceN.yaml
```
Then on each machine run the following to render the dataset:
```
python src/generate_dataset.py DATASET_NAME/parameter_spaceN.yaml [--segmentation_map]
```
This will add the simulations to the `DATASET_NAME` directory. The contents of each machine's `DATASET_NAME` dir can be safely combined to form the full dataset.


## Components
Made of 3 components:

1. Script which generates a dataset of unique simulation parameters (`generate_dataset.py`). Edit `parameter_space` to define which parameters the dataset is created over.

2. Script which takes in the simulation parameters (radius,speed,initial position, etc..), and calculates the position of the ball(s) at each timestep. (`simulate.py`)

4. Script which takes the position and draws the ball(s) as an image

    E.g. `./drawCircle.sh 10 50 frame_0.png` will draw a circle at (10,50)

    (images are 64x64)

There are also optional utility scripts to apply random obscuring objects (`random_masking.sh`), and random blurs (`random_blurring.sh`) to a generated dataset.

## Initial proof-of-concept

- [x] White circle on black background
- [x] Ball bounces off image edges
- [ ] Vary physical properties:
    - [x] Starting position
    - [x] Ball radius
    - [x] Initial direction
    - [x] Initial speed
    - [x] Gravity
    - [ ] Ball mass (only relevent for ball-ball collisions)
    - [ ] Random ball intensity jitter
    - [x] Random ball colour/background color
    - [ ] Add random ball jitter
    - [x] Add gausian blur
    - [x] Add multiple balls
    - [x] Add segmentation mask
