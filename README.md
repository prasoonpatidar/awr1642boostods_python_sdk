# Read Data from awr1642boostods (Python 3.9)

Python program to read the data in real time from the **AWR1642 BOOST- ODS** mmWave radar board (Texas Instruments, MMWAVE SDK 3). The program has been tested with Linux and is based on the Matlab demo from Texas Instruments.

Credit for starter code: https://github.com/ibaiGorordo/AWR1843-Read-Data-Python-MMWAVE-SDK-3-

# Pre-setup steps

Before using this codebase, You need to flash radar sensor with demo visualizer binary file provided in Ti MMWave SDK. 
Currently, Ti only supports flashing from windows OS. I've used windows 7(Should work with later versions as well).

You need to download following software to setup mmWave sensor:

1- [MMWave SDK](https://www.ti.com/tool/MMWAVE-SDK#tech-docs) (Tested with v3.6.0)

2- [Uniflash](https://www.ti.com/tool/UNIFLASH) 

Then follow step by step process from mmWave SDK userguide (mmwave_sdk_user_guide.pdf) 
from sections 1, 2, 3.1, 3.2.1, 3.4, 4.1 and 4.2.

At this stage, you should be able to get demo running in windows visualizer. 

# Setting up custom cfg files

The config files used in this module are created using [visualizer demo](https://dev.ti.com/gallery/view/mmwave/mmWave_Demo_Visualizer/ver/3.6.0/).
Save relevant config(.cfg) from running demo(some example configs are present in repository) 

First, the program configures the Serial ports and sends the CLI commands defined in the configuration file to the radar. 
Next, the data coming from the radar is parsed to extract relevant information based on set config.


## Required Python packages
* **numpy**: For the array calculations.
* **serial**: To read the serial data from the radar.
* **time**: To wait until more data is generated.
* **matplotlib**: For plotting rangeDoppler Heatmaps(if enabled in configs).

## Program Functions
* **serialConfig()**: Configures the serial ports and sends the CLI commands to the radar. It outputs the serial objects for the data and CLI ports.
* **parseConfigFile()**: Parses the configuration file to extract the configuration parameters. It returns the **configParameters** dictionary with the extracted parameters.
* **readAndParseData1642Boost()**: It reads the data from the data serial port and parses the recived buffer to extract the data from the **Detected objects** package only. Othe package types (range profile, range-azimuth heat map...) are done similarly. 
This functions returns a boolean variable (**dataOK**) that stores if the data has been correctly, the frame number and the **detObj** dictionary with the all kinds of information requested by config(.cfg) file which (might) include number of detected objects, 3D position (m) and doppler velocity (m/s).
* **update()**: Runs the **readAndParseData1642Boost()** function to read the current data and if the data has been read correctly the scatter with the 2D position is updated.

More details on how incoming packet is organised can be found in attached HTML document()


## HOW TO USE
* Download the required packages.
* Change the name of the configuration file (.cfg).
* Change the serial ports.
* If **not all the antennas** are being used, then change the value of **numRxAnt** and **numTxAnt**.
* Run the program.
* The data of each frame with the position and velocities of the reflected points is stored in the **detObj** dictionary. Each frame data is stored in the **frameData** dictionary array.

