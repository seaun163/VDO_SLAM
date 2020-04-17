# VDO-SLAM
**Authors:** [Jun Zhang](https://halajun.github.io/), [Mina Henein](https://minahenein.github.io/), [Robert Mahony](https://users.cecs.anu.edu.au/~Robert.Mahony/) and [Viorela Ila](http://viorelaila.net/) 

VDO-SLAM is a Visual Object-aware Dynamic SLAM library for **RGB-D** cameras that is able to track dynamic objects, estimate the camera poses along with the static and dynamic structure, the full SE(3) pose change of every rigid object in the scene, extract velocity information, and be demonstrable in real-world outdoor scenarios. We provide examples to run the SLAM system in the [KITTI Tracking Dataset](http://cvlibs.net/datasets/kitti/eval_tracking.php), and in the [Oxford Multi-motion Dataset](https://robotic-esp.com/datasets/omd/). 


### Related Publications:


# 1. License

VDO is released under a [GPLv3 license](https://github.com/halajun/DynamicObjectSLAM/blob/master/LICENSE-GPL.txt). For a list of all code/library dependencies (and associated licenses), please see [Dependencies.md](https://github.com/halajun/DynamicObjectSLAM/blob/master/Dependencies.md).

If you use VDO-SLAM in an academic work, please cite:


# 2. Prerequisites
We have tested the library in **Mac OS X 10.14** and **Ubuntu 16.04**, but it should be easy to compile in other platforms. 

## OpenCV
We use [OpenCV](http://opencv.org) to manipulate images and features. Download and install instructions can be found at: http://opencv.org. **Required at least 3.0. Tested with OpenCV OpenCV 3.2**.

## Eigen3
Required by g2o (see below). Download and install instructions can be found at: http://eigen.tuxfamily.org. **Required at least 3.1.0**.

## g2o (Included in dependencies folder)
We use modified versions of [g2o](https://github.com/RainerKuemmerle/g2o) library to perform non-linear optimizations. The modified libraries (which are BSD) are included in the *dependencies* folder.


# 3. Building VDO-SLAM library and example

Clone the repository:
```
git clone https://github.com/halajun/DynamicObjectSLAM.git VDO-SLAM
```

We provide a script `build.sh` to build the *dependencies* libraries and *VDO-SLAM*. Please make sure you have installed all required dependencies (see section 2). Execute:
```
cd VDO-SLAM
chmod +x build.sh
./build.sh
```

This will create 

1. **libVDO_SLAM.dylib (Mac)** or **libVDO_SLAM.so (Ubuntu)** at *lib* folder,

2. **libg2o.dylib (Mac)** or **libg2o.so (Ubuntu)** at */dependencies/g2o/lib* folder (Note that when compiling in different platforms, the library file suffix, i.e., '.dylib' or '.so', needs to be changed accordingly in the main CMakeLists.txt.),

3. and the executable **vdo_slam** in *example* folder.

## For Ubuntu Platform

1. some detailed differences.

# 4. Examples

## KITTI Tracking Dataset  

1. Download the a demo sequence (0003) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php 

2. Execute the following command. Make sure that in the yaml file, the 'ChooseData' item is set to 2 (which refers to KITTI), as different dataset comes with different configures, such as input format, ground truth, etc.
```
./example/vdo_slam example/kitti-0000-0013.yaml PATH_TO_KITTI_SEQUENCE_DATA_FOLDER
```

## Oxford Multi-motion Dataset  

1. Download the a demo sequence (swinging_4_unconstrained) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php 

2. Execute the following command. Make sure that in the yaml file, the 'ChooseData' item is set to 1 (which refers to OMD), as different dataset comes with different configures, such as input format, ground truth, etc.
```
./example/vdo_slam example/omd.yaml PATH_TO_OMD_SEQUENCE_DATA_FOLDER
```

# 5. Processing your own sequences
You will need to create a settings (yaml) file with the calibration of your camera. See the settings files provided in the *example/* folder. RGB-D input must be synchronized and depth registered. The system also requires image pre-processing as input, which includes instance-level semantic segmentation and optical flow estimation. For evaluation purpose, ground truth data of camera pose and object pose are also needed as input. Details of input format are shown as follows,

## Input Data Pre-processing

1. The input of segmentation mask is saved as matrix, same size as image, in .txt file. Each element of the matrix is integer, with 0 stands for background, and 1,2,...,n stands for different instance label.

2. The input of optical flow is the standard .flo file that can be read and processed directly using OpenCV.

## Ground Truth Input for Evaluation

1. The input of ground truth camera pose is saved as .txt file. Each row is organized as follows,
```
FrameID R11 R12 R13 t1 R21 R22 R23 t2 R31 R32 R33 t3 0 0 0 1
```

Here Rij are the coefficients of the camera rotation matrix **R** and ti are the coefficients of the camera translation vector **t**.

2. The input of ground truth object pose is also saved as .txt file. One example of such file (**KITTI Tracking Dataset**), which each row is organized as follows,
```
FrameID ObjectID B1 B2 B3 B4 t1 t2 t3 r1
```

Where ti are the coefficients of 3D object location **t** in camera coordinates, and r1 is the Rotation around Y-axis in camera coordinates. B1-4 is 2D bounding box of object in the image, used for visualization. Please refer to the details in **KITTI Tracking Dataset** if necessary.

The provided object pose of **OMD** dataset is slightly different from **KITTI**. So in a word, a user can input a custom data format, but need to write a new function to input the data.







