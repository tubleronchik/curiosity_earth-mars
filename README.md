# HOWTO: Robonomics demo with Curiosity Rover moving after transaction and storing data in IPFS
Sample of how it works is available on YT: https://youtu.be/pl3eIEC_T2o
### Requirements:
- ROS Melodic + Gazebo + RViz (installation manual [here](http://wiki.ros.org/melodic/Installation))
- extra packages:
```shell
sudo apt-get install ros-melodic-gazebo-ros-control ros-melodic-effort-controllers ros-melodic-joint-state-controller
```
- IPFS 0.4.22 (download from [here](https://dist.ipfs.io/go-ipfs/v0.4.22/go-ipfs_v0.4.22_linux-386.tar.gz) and install)
- ipfshttpclient:
```shell
pip install ipfshttpclient
```
- Robonomics node (binary file) (download latest release [here](https://github.com/airalab/robonomics/releases))
- IPFS browser extension (not necessary)

------------

### 1. Set up a simulation
Download Curiosity rover package:
```shell
mkdir -p robonomics_ws/src
cd robonomics_ws/src
git clone https://bitbucket.org/theconstructcore/curiosity_mars_rover/src/master/
cd ..
catkin build
```
We need to adjust starting conditions to make our rover spawn smoothly:
- Go to

`/robonomics_ws/src/master/curiosity_mars_rover_description/worlds` and change line 14 of the file` mars_curiosity.world` to 
`<pose>0 0 9 0 0 0</pose>`

- Go to

`/robonomics_ws/src/master/curiosity_mars_rover_description/launch` and change line 4 of the file `mars_curiosity_world.launch` to 
`<arg name="paused" default="false"/>`

Dont forget to add source command to `~/.bashrc`
`source /home/$USER/robonomics_ws/devel/setup.bash`


- Reboot console and launch the simulation:

```shell
roslaunch curiosity_mars_rover_description main_real_mars.launch
```
![Curiosity](https://github.com/PaTara43/media/blob/master/Screenshot%20from%202020-08-27%2017-22-28.png?raw=true "Curiosity")

It can be closed for a while

------------

### 2. Download controller package
To download a controller package for Rover type in terminal:
```shell
cd ~/robonomics_ws/src
mkdir robonomics_sample_controller
cd robonomics_sample_controller
git clone https://github.com/tubleronchik/curiosity_earth-mars.git
cd ../..
catkin build
```

------------

### 3. Manage accounts in DAPP


Go to https://parachain.robonomics.network and switch to Earth Test Network

![Earth](https://github.com/tubleronchik/curiosity_earth-mars/blob/main/media/networks.png "Earth")

Go to Accounts and create **EMPLOYER** account. 

Then switch to Mars Test Network and create and **CURIOSITY** account (**NOT_CURIOSITY** is **not** necessary).

**Important**! Copy each account's key and address (to copy address click on account's icon)
Transfer some money (units) to these accounts

Add these addresses and path to robonomics folder to file `config.config` in `robonomics_ws/src/robonomics_sample_controller/src`. The template for the config is in `config-template.config`.

------------


### 4. Start Robonomics
In a separate terminal launch IPFS:
```shell
ifps init #you only need to do this once
ipfs daemon
```

In another separate terminal launch Curiosity simulation:
```shell
roslaunch curiosity_mars_rover_description main_real_mars.launch
```
Wait till it stays still

In another terminal launch the controller:
```shell
rosrun robonomics_sample_controller sample_controller.py
```
![Running controller](https://github.com/PaTara43/media/blob/master/Screenshot%20from%202020-08-27%2018-46-30.png?raw=true "Running controller")

Now you can send a transaction triggering the Rover to start moving and collecting data. To do so, go to the Earth network in https://parachain.robonomics.network and open Developer - Extrinsics. 

Fill in the form same as on the picture below. 


![Form](https://github.com/tubleronchik/curiosity_earth-mars/blob/main/media/form.png "Form")


You should see the following:

![Arming](https://github.com/PaTara43/media/blob/master/Screenshot%20from%202020-08-27%2018-58-36.png?raw=true "Arming")

And the robot should start moving. Later, when the job is done:




