# Kintinuous installation guide
> Edit: 2016/08/14
> Author: Jacky Liu

Kintinuous GitHub repository
(https://github.com/mp3guy/Kintinuous)

# Testing Environment
## Hardware
* CPU: Core i5-3470 CPU at 3.20GHz
* RAM: 8GB
* GPU: nVidia GeForce GTX560 1GB

## Operating system
* Ubuntu 14.04


# Step by Step tutorial
1. Download CUDA ToolKit
[nVidia's official CUDA repository](https://developer.nvidia.com/cuda-downloads)

2. Install CUDA`
```bash
sudo dpkg -i cuda-repo-ubuntu1404_7.5-18_amd64.deb
sudo apt-get update
```
~~`sudo apt-get install cuda`~~
```bash
sudo apt-get install -y cmake-qt-gui git build-essential libusb-1.0-0-dev libudev-dev openjdk-7-jdk freeglut3-dev python-vtk libvtk-java libglew-dev cuda-7-5 libsuitesparse-dev`
```
3. Install PCL
```bash
sudo add-apt-repository -y ppa:v-launchpad-jochen-sprickerhof-de/pcl
sudo apt-get update
sudo apt-get install -y libpcl-all
```

4. Download [OpenCV](http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.9/opencv-2.4.9.zip)
5. unzip opencv
```bash
sudo apt-get install unzip
unzip opencv
```
6. Install opencv
```bash
cd opencv
mkdir build
cd build
cmake -D BUILD_NEW_PYTHON_SUPPORT=OFF -D WITH_OPENCL=OFF -D WITH_OPENMP=ON -D INSTALL_C_EXAMPLES=OFF -D BUILD_DOCS=OFF -D BUILD_EXAMPLES=OFF -D WITH_QT=OFF -D WITH_OPENGL=OFF -D WITH_VTK=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_TESTS=OFF -D WITH_CUDA=OFF -D BUILD_opencv_gpu=OFF ..
```

7. Install [DLib](https://github.com/dorian3d/DLib/archive/v1.0.zip)
You may encounter `cv::KeyPoint is not a member of 'cv'` issue, then try to use DLib, DBoW2, and DLoopDetector v1.0 (https://github.com/dorian3d/DLib/issues/7)
8. Install [DBoW2](https://github.com/dorian3d/DBoW2/archive/v1.0.zip)
9. Install [DLoopDetector](https://github.com/dorian3d/DLoopDetector/archive/v1.0.zip)

10. Install [iSAM](http://people.csail.mit.edu/kaess/isam/)
```bash
sudo apt-get install subversion
svn co https://svn.csail.mit.edu/isam
cd iSAM
mkdir build
cd build
cmake ..
make
sudo make install
```

11. Install [Pangolin](https://github.com/stevenlovegrove/Pangolin)

12. Build Kintinuous
If GPU memory is not enough(less than recommanded 2GB GPU RAM) for default setting in Kintinuous.  
You could modify the GPU memory value in
```bash
 "{Kintinuous}/src/frontend/cuda/internal.h"
line: 243"  #define VOL 256
```
```
cd Kintinuous
cd src
mkdir build
cd build
cmake ..
make
cd ../..
mkdir build
cd build
bash ../build.sh
```

13. Testing Kintinuous with sample datasets
```bash
cd {Kintinuous}/build
wget http://www.cs.nuim.ie/research/vision/data/loop.klg
./Kintinuous -s 7 -v ../vocab.yml.gz -l loop.klg -ri -fl -od
```
Sample video https://www.youtube.com/watch?v=Y6NedbzQdpY


