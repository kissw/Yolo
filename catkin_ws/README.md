```
$ cd src/
$ catkin_make
```
위 명령 실행시 만약 아래와같은 에러가 뜬다면 opencv4 경로가 잡혀서임.
```
/usr/include/opencv4/opencv2/core/cvdef.h:690:4: error: #error "OpenCV 4.x+ requires enabled C++11 support"
 #  error "OpenCV 4.x+ requires enabled C++11 support"
```
yolov4-for-darknet_ros/darknet_ros/darknet/Makefile 을 아래와 같이 수정해주면 됨
```Makefile
GPU=1
CUDNN=1
CUDNN_HALF=1
OPENCV=1
AVX=0
OPENMP=0
LIBSO=0
ZED_CAMERA=0
ZED_CAMERA_v2_8=0

# set GPU=1 and CUDNN=1 to speedup on GPU
# set CUDNN_HALF=1 to further speedup 3 x times (Mixed-precision on Tensor Cores) GPU: Volta, Xavier, Turing and higher
# set AVX=1 and OPENMP=1 to speedup on CPU (if error occurs then set AVX=0)
# set ZED_CAMERA=1 to enable ZED SDK 3.0 and above
# set ZED_CAMERA_v2_8=1 to enable ZED SDK 2.X

USE_CPP=0
DEBUG=0

ARCH= -gencode arch=compute_35,code=sm_35 \
      -gencode arch=compute_50,code=[sm_50,compute_50] \
      -gencode arch=compute_52,code=[sm_52,compute_52] \
	    -gencode arch=compute_61,code=[sm_61,compute_61] \
	-gencode arch=compute_72,code=[sm_72,compute_72] # 이부분 추가

...

#LDFLAGS+= `pkg-config --libs opencv4 2> /dev/null || pkg-config --libs opencv`
#COMMON+= `pkg-config --cflags opencv4 2> /dev/null || pkg-config --cflags opencv`
LDFLAGS+= `pkg-config --libs opencv` # 이부분 수정
COMMON+= `pkg-config --cflags opencv` # 이부분 수정
```
