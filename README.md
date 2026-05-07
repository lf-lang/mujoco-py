# mujoco-py

[MuJoCo](https://mujoco.org) (Multi-Joint dynamics with Contact) is a physics-based simulation engine with graphics and animation for the Python target.
This repo defines a base reactor and some example derived reactors.  The [MuJoCoBase](src/lib/MuJoCoBase.lf) reactor provides a single simulator with graphical animation.
The derived reactors customize this base class for particular MuJoCo model files.

## Prerequisites

Install Python MuJoCo and LF tooling:

```sh
python3 -m pip install mujoco
```

## Library Reactors

* [MuJoCoBase](src/lib/MuJoCoBase.lf): Base class providing navigation of the view and methods to update the scene and advance the simulator. This is not meant to be directly instantiated.
* [MuJoCoAdvance](src/lib/MuJoCoAdvance.lf) extends [MuJoCoBase](src/lib/MuJoCoBase.lf): Base class providing an `advance` input to advance the simulation to the logical time and update the scene. This refers to the [hello](src/models/hello.xml) basic demo model, which has a box and a floor.
* [MuJoCoAuto](src/lib/MuJoCoAuto.lf) extends [MuJoCoBase](src/lib/MuJoCoBase.lf): Base class that automatically advances the simulation and outputs a tick for each step. This separates the updating of the scene, which is driven by a periodic timer. This refers to the [hello](src/models/hello.xml) basic demo model, which has a box and a floor.
* [MuJoCoCar](src/lib/MuJoCoCar.lf) extends [MuJoCoAdvance](src/lib/MuJoCoAdvance.lf): Simulator for the [car](src/models/car.xml) basic demo model, providing a two-wheel vehicle and keyboard controlled driving. This version actively controls the simulator advance. 
* [MuJoCoCarAuto](src/lib/MuJoCoCarAuto.lf) extends [MuJoCoAuto](src/lib/MuJoCoAuto.lf): Simulator for the [car](src/models/car.xml) basic demo model, providing a two-wheel vehicle and keyboard controlled driving. This version lets the simulator advance automatically.
* [MuJoCoPanda.lf](src/lib/MuJoCoPanda.lf) extends [MuJoCoAuto](src/lib/MuJoCoAuto.lf): Simulator for a Franka Emika Panda robot.

## Demos

Build the demos using `lfc` or `make`:

* [MuJoCoBasicDemo](src/MuJoCoBasicDemo.lf): Rectangular object that falls to the floor.
* [MuJoCoCarDemo](src/MuJoCoCarDemo.lf): Simple drivable car.
* [MuJoCoCarAutoDemo](src/MuJoCoCarAutoDemo.lf): Simple drivable car.
* [PandaDemo](src/PandaDemo.lf): Franka Emika Panda robot doing gyrations.
* [PandaDemoCamCtrl](src/PandaDemoCamCtrl.lf): Franka Emika Panda robot with camera-based person detection. The robot pauses automatically when two or more people are detected by a YOLOv8 DNN monitoring the webcam feed, and resumes when the area is clear.

### PandaDemoCamCtrl Prerequisites

This demo uses [YOLOv8](https://github.com/ultralytics/ultralytics) and OpenCV for real-time object detection. It requires a Python virtual environment with the dependencies listed in [src/lib/video/requirements.txt](src/lib/video/requirements.txt), which covers all reactors under [src/lib/video/](src/lib/video/).

Set up the virtual environment:

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu  # or cu121 for NVIDIA GPU
pip install -r src/lib/video/requirements.txt
```

The YOLOv8 model weights (`yolov8n.pt`) are downloaded automatically on first run.

Then compile and run:

```sh
lfc src/PandaDemoCamCtrl.lf
bin/PandaDemoCamCtrl.py
```

Press `Backspace` to reset the robot arm, `q` to quit.
