# Ros2_Humble_Docker-compose
Use devcontainer.json file in container_2 folder.

Params file at the root is an example change the address of the camera to make it working.

camera.launch.py file at the root replace at with the file inside the launch folder of usb_cam.

First run the following commands accoridngly.
```
git clone this-repo
git submodule init
git submodule update
source /opt/ros/humble/setup.bash
sudo apt update
rosdep install -i -y --from-paths src --rosdistro humble
colcon build
```

Should run this in the docker container.

P.S.
```
sudo apt-get install net-tools && apt-get install iputils-ping
```