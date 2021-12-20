# iam-interface

## Installation Instructions
1. Install Ubuntu 18.04.6 following instructions here:[https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)
2. Install ROS Melodic following instructions here: [http://wiki.ros.org/melodic/Installation/Ubuntu](http://wiki.ros.org/melodic/Installation/Ubuntu)
3. Make sure that you have added the following line at the end of your `~/.bashrc` file:
   ```bash
   source /opt/ros/melodic/setup.bash
   ```
4. Install Node.js using the following commands:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```
5. Install rosbridge server:
   ```bash
   sudo apt install ros-melodic-rosbridge-server
   ```
6. Install web_video_server:
   ```bash
   sudo apt install ros-melodic-web-video-server
   ```
7. Install Frankapy following instructions here:[https://iamlab-cmu.github.io/frankapy/](https://iamlab-cmu.github.io/frankapy/)
8. Clone the repo using the following command:
   ```bash
   git clone --recurse-submodules git@github.com:iamlab-cmu/iam-interface.git
   ```
9. Make sure to enter into the same `virtualenv` you created during the Frankapy installation before running the following commands.
10. Install pillar-state:
   ```bash
   cd pillar-state
   sudo apt install libyaml-cpp-dev
   ./make_scripts/make_proto_cpp.sh --clean
   ./make_scripts/make_view_cpp.sh --clean
   
   pip install -e python

   ./cpp/build/pillar_state_test
   pytest --capture=no test/pillar_state_py_test.py
   cd ..
   ```
11. Install pillar-skills:
   ```bash
   cd pillar-skills
   pip install -e .
   cd ..
   ```
12. Install iam-skills:
   ```bash
   cd iam-skills
   pip install -e .
   cd ..
   ```
13. Install Open3d:
   ```bash
   pip install open3d
   ```
14. Install iam-domain-hander:
   ```bash
   cd iam-domain-handler
   pip install -e .
   cd ..
   ```
15. Install Bokeh:
   ```bash
   pip install bokeh
   ```
16. Install iam-bt:
   ```bash
   cd iam-bt
   pip install -e .
   cd .. 
   ```
17. Build the web-msgs:
   ```bash
   cd catkin_ws
   catkin build
   cd ..
   ```
18. Add the following line to your `~/.bashrc` file:
   ```bash
   source /path/to/iam-interface/catkin_ws/devel/setup.bash
   ``` 
19. Install npm packages:
   ```bash
   cd web-interface/javascript
   npm install
   ```
20. Modify Line 1 in `javascript/example/scripts.js` and set it to the ip address of the computer, which you can find using the `ifconfig` command.
   ```bash
   let host_name = "localhost"
   ```
21. Install DEXTR following the commands below.
   ```bash
   cd ../..
   git clone https://github.com/iamlab-cmu/DEXTR-KerasTensorflow.git
   cd DEXTR-KerasTensorflow
   pip install matplotlib opencv pillow scikit-learn scikit-image h5py tensorflow keras

   cd models/
   chmod +x download_dextr_model.sh
   ./download_dextr_model.sh
   cd ../..
   ```
22. Install the Azure Kinect Driver and Azure Kinect ROS Driver (Change 18.04 to either 16.04 or 20.04 if you are not using Ubuntu 18.04):
   ```bash
   curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
   sudo apt-get update
   sudo apt install libk4a1.4 libk4a1.4-dev k4a-tools

   cd catkin_ws/src
   git clone https://github.com/microsoft/Azure_Kinect_ROS_Driver.git
   cd ..
   catkin build
   cd ..

   source /path/to/iam-interface/catkin_ws/devel/setup.bash
   ```

## Running Instructions
1. Open a new terminal and run the following commands: (Terminal 1)
   ```bash
   cd /path/to/iam-interface/web-interface/javascript
   source /path/to/virtualenv/bin/activate
   npm start
   ```

2. Navigate on a web browser to `http://<your_ip_address>:9080/javascript/example/index.html`

3. Open a new terminal and run the following commands: (Terminal 2)
   ```bash
   roscore
   ```

4. Open a new terminal and run the following commands: (Terminal 3)
   ```bash
   roslaunch rosbridge_server rosbridge_websocket.launch
   ```

5. Open a new terminal and run the following commands: (Terminal 4)
   ```bash
   rosrun web_video_server web_video_server
   ```

6. Open a new terminal and run the following commands: (Terminal 5)
   ```bash
   roslaunch azure_kinect_ros_driver driver.launch
   ```

7. Open a new terminal and run the following commands: (Terminal 6)
   ```
   cd /path/to/iam-interface/DEXTR-KerasTensorflow
   source /path/to/virtualenv/bin/activate
   python dextr_ros_service.py
   ```

8. Open a new terminal and run the following commands: (Terminal 7)
   ```bash
   cd /path/to/iam-interface/iam-bokeh-server
   source /path/to/virtualenv/bin/activate
   bokeh serve --allow-websocket-origin='*' bokeh_server.py
   ```

9. Open a new terminal and run the following commands: (Terminal 8)
   ```bash
   cd /path/to/frankapy
   ./bash_scripts/start_control_pc.sh -i <control_pc_ip_address>
   ```

10. Open a new terminal and run the following commands: (Terminal 9)
   ```bash
   cd /path/to/iam-interface/iam-vision
   source /path/to/virtualenv/bin/activate
   python iam_vision_server.py
   ```

11. (Optional) Open a new terminal and run the following commands: (Terminal 10)
   ```bash
   cd /path/to/iam-interface/voxel_publisher
   source /path/to/virtualenv/bin/activate
   python scripts/point_cloud_to_voxel.py
   ```

12. Open a new terminal and run the following commands: (Terminal 11)
   ```bash
   cd /path/to/iam-interface/iam-domain-handler
   source /path/to/virtualenv/bin/activate
   ./examples/RunAll.sh
   ```

13. Open a new terminal and run the following commands: (Terminal 12)
   ```bash
   cd /path/to/iam-interface/iam-bt
   source /path/to/virtualenv/bin/activate 
   python examples/main_bt.py
   ```