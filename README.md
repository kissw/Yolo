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

## Error issue

```
/usr/include/opencv4/opencv2/core/cvdef.h:690:4: error: #error "OpenCV 4.x+ requires enabled C++11 support"
 #  error "OpenCV 4.x+ requires enabled C++11 support"
```
catkin_make 명령 실행시 만약 아래와같은 에러가 뜬다면 opencv4 경로가 잡혀서임.
catkin_ws/src/yolov4-for-darknet_ros/darknet_ros/darknet/Makefile 을 아래와 같이 수정해주면 됨
```Makefile
...
#LDFLAGS+= `pkg-config --libs opencv4 2> /dev/null || pkg-config --libs opencv`
#COMMON+= `pkg-config --cflags opencv4 2> /dev/null || pkg-config --cflags opencv`
LDFLAGS+= `pkg-config --libs opencv` # 이부분 수정
COMMON+= `pkg-config --cflags opencv` # 이부분 수정
...
```
