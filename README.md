# Cannula Kinematics Library

**Active cannula (aka concentric tube) robot kinematics library**

## Build Instructions (for Linux)
**Follow these steps to build as a shared library (e.g. for use with [medlab_common](https://github.com/vanderbiltmedlab/medlab_common) ROS package)**

1. Download and extract repository to your desired location
   - Using GUI:
      - Click the green 'Clone or Download' button above
      - Click 'Download ZIP'
      - Open with Archive Manager
      - Click 'Extract' and choose your desired location
   - Using Terminal/Command Line:
      - Download ZIP
      ```
      wget -O ./CannulaKinematics.zip "https://github.com/vanderbiltmedlab/CannulaKinematics/zipball/master"
      ```
      - Extract to your desired location
      ```
      unzip CannulaKinematics.zip -d /path/to/desired/location
      ```
 2. Compile shared library
   - Open a terminal and navigate to the /build directory
   - Run CMake
     ```
     cmake ..
     ```
   - Run make
     ```
     make
     ```
   - You should now have a shared library called libcannula_kinematics.so
   
   
