#HOW TO INSTALL PCL 1.8 WITH QT SUPPORT
guide source: https://robotract.com/2016/05/19/installing-pcl-qt5-and-vtk-on-ubuntu/  
additional source: http://www.pcl-users.org/Endless-troubles-installing-PCL-on-Ubuntu-16-04-td4043733.html    
PCL Compilation and Dependencies: http://pointclouds.org/documentation/tutorials/compiling_pcl_posix.php   

##README
The guide details how to set up the development environment for the PCL-GUI.    
This setup was tested on Ubuntu 16.04 using VMware, and thus could only use    
OpenGL 1 & 2 without harware acceleration.  See the VTK 7 setup guide if your   
graphics driver supports OpenGL 3.0+.    

##----SOFTWARE
cmake & cmake-gui   
git   
USB 1.0   
OpenNI2   
Boost 1.60   
Eigen 3.3.2   
FLANN 1.8   
QHULL  
Qt 5.6   
VTK 6.3       
pcl 1.8   
  

##----INSTALL SOME NECESSITIES    
```
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install git
sudo apt-get install cmake  
sudo apt-get install cmake-curses-gui 
```

##----INSTALL SOME ADDITIONAL LIBRARIES    
```
sudo apt-get install libusb-1.0-0-dev libusb-dev libudev-dev   
sudo apt-get install freeglut3-dev pkg-config 
sudo apt-get install mpi-default-dev openmpi-bin openmpi-common
sudo apt-get install libxmu-dev libxi-dev 
sudo apt-get install mono-complete
sudo apt-get install openjdk-8-jdk openjdk-8-jre
```

##----INSTALL eigen and flann libraries     
```
sudo apt-get install libeigen3-doc libeigen3-dev libflann-dev
```  

##----INSTALL qhull   
```
sudo apt-get install libqhull-dev libqhull-doc
```   

##----INSTALL OpenNI   
```
sudo apt-get install libopenni2-dev libopenni2-0 openni2-doc
```   

##----INSTALL gtest
This is needed to prevent an error when building PCL with VTK  
```
sudo apt-get install libgtest-dev
cd /usr/src/gtest
sudo mkdir build && cd build
sudo cmake ..
sudo make

# copy or symlink libgtest.a and libgtest_main.a to your /usr/lib folder
sudo ln -s *.a /usr/lib
```   

##----INSTALL boost 1.60
download Boost from http://www.boost.org/ and extract the folders to ~/PCL_Dependencies/   
In the terminal do the following for boost_iostream library   
```
sudo apt-get install zlib1g-dev libbz2-dev
```
then navigate to where you extracted boost and type the following:   
```
cd boost_1_60_0
./bootstrap.sh
sudo ./b2 install
```   

##----INSTALL opengl   
```
sudo apt-get install build-essential libgl1-mesa-dev mesa-utils
sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev
```   

###Check that everything was setup correctly 
```
glxinfo | grep opengl
```   

if you get Error: couldn't find RGB GLX visual or fbconfig ubuntu 12.04 error   
Do the following:   
```
sudo apt-get remove --purge xserver-xorg
sudo apt-get install xserver-xorg
sudo dpkg-reconfigure xserver-xorg
sudo reboot
```   

##----INSTALL Qt 5.6   
Use the online installer at https://www.qt.io/download/  

To run the installer you must first change the file permissions      
```   
chmod 755 <path to Qt Downloader>   
```   

##----INSTALL vtk 6.3   

Install dependency:     
```
sudo apt -y install libxt-dev
```
Then we need to install qt5webkitwidget, which is required by vtk qt support.   
```
sudo apt-get install libqt5webkit5 libqt5webkit5-dev
```
Clone the git repo for VTK
```
cd ~/PCL_Dependencies
git clone https://github.com/Kitware/VTK.git
```
Next we need to switch to vtk 6.3
```
git checkout 21df122f4186aec9baae298bfc35b5a380869748
```

After that run cmake by specifying the path to this installation.   
```
cd ~/PCL_Dependencies/VTK-6.3.0   
mkdir vtk-build && cd vtk-build
cmake -DVTK_QT_VERSION:STRING=5 \
-DQT_QMAKE_EXECUTABLE:PATH=/home/hsean/Qt/5.6/gcc_64/bin/qmake \
-DVTK_Group_Qt:BOOL=ON \
-DCMAKE_PREFIX_PATH:PATH=/home/hsean/Qt/5.6/gcc_64/lib/cmake \
-DBUILD_SHARED_LIBS:BOOL=ON \
-DQt5WebKitWidgets_DIR:STRING=/usr/lib/x86_64-linux-gnu/cmake/Qt5WebKitWidgets \
.. 
```
Make sure you replace /home/hsean/Qt/5.6 with where you installed Qt.    
Also, Qt/5.x must be the version you are using.   

NOTE: for Qt5.8    
-DCMAKE_PREFIX_PATH:PATH=/home/hsean/Qt/5.8/gcc_64/lib/cmake is now   
-DCMAKE_PREFIX_PATH:PATH=/home/hsean/Qt/5.8/gcc_64/lib/cmake/Qt5    
                
then build and install VTK:
```
make -j2
sudo make -j2 install
```
##----INSTALL pcl 1.8   
download pcl from https://github.com/PointCloudLibrary/pcl/releases/tag/pcl-1.8.0   
extract pcl to you home directory and rename to pcl-1.x.x   
NOTE: installing curses cmake will make options easier to edit.   
      If VTK was not found remember to set -DVTK_DIR to your VTK build directory.    
navigate to where you saved pcl and type the following to install:  
```
cd pcl
mkdir build && cd build
cmake .. (or ccmake .. if using curses)
make -j2
sudo make -j2 install
```
If you come accross "internal compiler error: killed (program cc1plus)" follow the   
link to solve the problem: https://bitcointalk.org/index.php?topic=304389.0  

That’s it, now you can enjoy PCL.   
