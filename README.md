# Yolo
```
git clone --recursive https://github.com/kissw/Yolo.git
```

## How to Install
```
$ cd Yolo/catkin_ws
$ catkin_make
```

## How to Run
```
$ source (Yolo repo path)/catkin_ws/devel/setup.bash
$ roslaunch darknet_ros yolo_v4_tiny_intoelv.launch
```

### Subscribe하는 이미지 토픽이름을 변경하려면?
```
gedit (Yolo repo path)/catkin_ws/src/yolov4-for-darknet_ros/darknet_ros/darknet_ros/launch/yolo_v4_tiny_intoelv.launch
```

```xml
<?xml version="1.0" encoding="utf-8"?>

<launch>

  <!-- Use YOLOv4 tiny -->
  <arg name="network_param_file"         default="$(find darknet_ros)/config/yolov4_tiny_intoelv.yaml"/>
  <!--이부분 변경-->
  <arg name="image" default="/image_raw" /> 


  <!-- Include main launch file -->
  <include file="$(find darknet_ros)/launch/darknet_ros.launch">
    <arg name="network_param_file"    value="$(arg network_param_file)"/>
    <arg name="image" value="$(arg image)" />
  </include>

</launch>
```