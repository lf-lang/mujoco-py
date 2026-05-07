# Video and YOLO Library

This library provides video capture, display, and DNN-based object recognition using YOLOv8 for use in the `PandaDemoCamCtrl.lf` programs.

## Setup

### 1. Install PyTorch

Go to the [PyTorch website](https://pytorch.org/get-started/locally/) and follow the instructions for your platform.

**If you have an NVIDIA GPU**, select the correct CUDA version on the installation page and follow the generated command, for example:

```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
```

**If you have no GPU (CPU only)**, install the CPU builds explicitly. Using the generic PyPI packages will cause a version mismatch and a segfault at runtime:

```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

### 2. Install remaining dependencies

```bash
pip install -r requirements.txt
```

> **Note:** `requirements.txt` intentionally omits `torch` and `torchvision` so that the correct build variant (CPU or CUDA) can be chosen in the step above.

### 3. Font warning fix (OpenCV / Qt)

If you see repeated `QFontDatabase: Cannot find font directory` warnings, Qt's bundled font directory is missing. Fix it by symlinking system DejaVu fonts into the expected location:

```bash
mkdir -p "$(python3 -c 'import cv2, os; print(os.path.dirname(cv2.__file__))')/qt/fonts"
ln -s /usr/share/fonts/truetype/dejavu/*.ttf \
      "$(python3 -c 'import cv2, os; print(os.path.dirname(cv2.__file__))')/qt/fonts/"
```

### 4. Compile and run

Compile with `lfc` and run the generated Python program:

```bash
lfc src/PandaDemoCamCtrl.lf
bin/PandaDemoCamCtrl
```
