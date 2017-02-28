# Kintinuous installation guide for Ubuntu 16.04
> Edit: 2017/02/28  
> Author: Kangel Zenn

## Testing Environment

### Hardware
+ CPU: Intel® Core™ i5-4590 CPU @ 3.30GHz × 4 
+ RAM: 8GB
+ GPU: GeForce GTX 760/PCIe/SSE2

### Operating System
+ ubuntu 16.04 LTS 64 bits

## Pre-installation

1. Install nvidia driver on your system 
2. Follow [this guide][CUDA installation guide] to make sure that CUDA can be installed on your system.


## Installation

### Part I : Install dependencies

#### 1. Install CUDA

1. Download the [NVIDIA CUDA Toolkit]
2. Install repository meta-data  
```
   $ sudo dpkg -i cuda-repo-<distro>_<version>_<architecture>.deb
```
3. Update the Apt repository cache  
```
$ sudo apt-get update
```
4. Install CUDA
```
$ sudo apt-get install cuda
```

#### 2. Run the following command to pull in most dependencies from the official repos:
```
sudo apt-get install -y cmake-qt-gui git build-essential libusb-1.0-0-dev libudev-dev freeglut3-dev python-vtk libvtk-java libglew-dev  libsuitesparse-dev openexr
```

#### 3. Install openjdk-7-jdk
Since openjdk-7-jdk is no longer included in the official repos, you have to install it from a ppa: 

```
sudo add-apt-repository ppa:openjdk-r/ppa  
sudo apt-get update   
sudo apt-get install openjdk-7-jdk  
```

#### 4. Intall PCL
Just follow the author @mp3guy's guide:  
To avoid getting stuck while building PCL, it's better not to run the following commands in a GUI-Environment. Use TTY instead.
```
sudo apt-get install g++ cmake cmake-gui doxygen mpi-default-dev openmpi-bin openmpi-common libflann-dev libeigen3-dev libboost-all-dev libvtk5-qt4-dev libvtk6.2 libvtk5-dev libqhull* libusb-dev libgtest-dev git-core freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev graphviz mono-complete qt-sdk openjdk-7-jdk openjdk-7-jre
git clone https://github.com/PointCloudLibrary/pcl.git
cd pcl
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_GPU=OFF -DBUILD_apps=OFF -DBUILD_examples=OFF ..
make -j8
sudo make install
sudo ldconfig
```
#### 5. Install Openni2
install it from the Ubuntu repos: 

```
sudo apt install libopenni2-dev
```

#### 6. Install ffmpeg: 
```
git clone git://source.ffmpeg.org/ffmpeg.git
cd ffmpeg/
git reset --hard cee7acfcfc1bc806044ff35ff7ec7b64528f99b1
./configure --enable-shared
make -j8
sudo make install
sudo ldconfig
```

#### 7. Install OpenCV: 
1. Download [OpenCV]
2. Unzip it
3. Install :
```
cd opencv
mkdir build
cd build
cmake -D BUILD_NEW_PYTHON_SUPPORT=OFF -D WITH_OPENCL=OFF -D WITH_OPENMP=ON -D INSTALL_C_EXAMPLES=OFF -D BUILD_DOCS=OFF -D BUILD_EXAMPLES=OFF -D WITH_QT=OFF -D WITH_OPENGL=OFF -D WITH_VTK=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_TESTS=OFF -D WITH_CUDA=OFF -D BUILD_opencv_gpu=OFF ..
```
#### 8. Install [DLib], [DBoW2] and [DLoopDetector]:
1. Install DLib
```
git clone https://github.com/dorian3d/DLib.git
cd DLib
mkdir build
cd build
cmake ..
make
sudo make install
```
2. Install [DBoW2] and [DLoopDetector] in the same way

#### 9. Install ISAM
```
svn co https://svn.csail.mit.edu/isam
cd iSAM
mkdir build
cd build
cmake ..
make
sudo make install
```
#### 10. Install [Pangolin]
Just follow the building guide provided by the author of the Pangolin:   
```
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build
cd build
cmake ..
make -j
```

### Part II: Build Kintinuous
```
git clone https://github.com/mp3guy/Kintinuous.git
cd Kintinuous
cd src
mkdir build
cd build
cmake ..
make
```

Now you can find **Kintinuous** under the build directory.
### Part III: Test Datasets:
1. Download sample datasets [here][Kintinuous datasets]
2. Run the following commands to launch it:
```
cd {Kintinuous}/src/build
./Kintinuous -s 7 -v ../../vocab.yml.gz -l loop.klg -ri -fl -od
```

[Kintinuous datasets]:[http://www.cs.nuim.ie/research/vision/data/loop.klg]
[Pangolin]:[https://github.com/stevenlovegrove/Pangolin]
[DLib]:[https://github.com/dorian3d/DLib]
[DBoW2]:[https://github.com/dorian3d/DBoW2]
[DLoopDetector]:[https://github.com/dorian3d/DLoopDetector]
[OpenCV]:[http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.9/opencv-2.4.9.zip]
[CUDA installation guide]:[http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#axzz4ZIwZGrX6]
[NVIDIA CUDA Toolkit]:[https://developer.nvidia.com/cuda-downloads]