# Author: TaeYoung Kim
# email: tyoung96@yonsei.ac.kr

FROM osrf/ros:humble-desktop-full

# Install PCL & Eigen & essential libraries
RUN apt-get update && apt-get install -y cmake libatlas-base-dev libeigen3-dev libpcl-dev libgoogle-glog-dev libsuitesparse-dev libglew-dev wget unzip git python3-pip
RUN apt-get install -y ros-humble-tf2 ros-humble-cv-bridge ros-humble-pcl-conversions ros-humble-xacro ros-humble-robot-state-publisher \
    ros-humble-rviz2 ros-humble-image-transport ros-humble-image-transport-plugins ros-humble-pcl-ros

# Install livox SDK 2
WORKDIR /root/
RUN git clone https://github.com/Livox-SDK/Livox-SDK2.git
WORKDIR /root/Livox-SDK2
RUN mkdir build
WORKDIR /root/Livox-SDK2/build
RUN cmake .. && make -j5 && make install

# Install livox ros driver2
WORKDIR /root/ros2_ws/src
RUN git clone https://github.com/Livox-SDK/livox_ros_driver2.git 
RUN cd /root/ros2_ws/src/livox_ros_driver2 && mv package_ROS2.xml package.xml
# Source and build the workspace
RUN /bin/bash -c 'source /opt/ros/humble/setup.bash && cd /root/ros2_ws && colcon build --cmake-args -DROS_EDITION=ROS2 -DHUMBLE_ROS=humble'

# Load ROS environment at each run
COPY ./ros_entrypoint.sh /
RUN chmod 755 /ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]

CMD ["bash"]
