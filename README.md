# Odometry-Visual
Proyecto final de el curso de imágenes de la Universidad Católica San Pablo - Arequipa ( paper "Real-Time Visual Odometry from Dense RGB-D Images, F. Steinbucker, J. Strum, D. Cremers, ICCV, 2011") 
## Instruccions:

First install Kinect v2 with https://github.com/OpenKinect/libfreenect2, you must to do the steps and show it.
![Alt text](protonect.png?raw=true "Title")

Clone the repository:
```
https://github.com/jeffersonquispe/Odometry-Visual.git
``` 
### Requirement
[OpenCV >= 3.0](http://tzutalin.blogspot.tw/2016/01/installing-opencv-310-and-contrib-lib.html)

### Setup
This  step is only test proves.
Download RGB-D dataset from [TUM](http://vision.in.tum.de/data/datasets) 

 [main.cpp](main.cpp#L160) will read the path of color and depth images from ./assoc.txt, then run OpenCV RgbdOdometry to compute visual odometry

The format of assoc.txt looks like:
```
timestamp1 rgb/[color_image_filename1] timestamp1 depth/[depth_image_filename1]
...
timestampN rgb/[color_image_filenameN] timestampN depth/[depth_image_filenameN]
```

You should change camera paramerts at the top of [main.cpp](main.cpp#L24)
```
#define FOCUS_LENGTH 525.0
#define CX 319.5
#define CY 239.5
```

### Build & Run
For the first time, you should download the dataset with TUM O generate iamges on the folder RGB and Depth. You can use the below command


Start building

`$ mkdir -p build; cd build`

`$ cmake ..; make`

also put in [protonect.cpp](main.cpp#L374)
```
  //CAPTURAR RGB
      cv::Mat rgbMat = cv::Mat(rgb->height, rgb->width,CV_8UC4, rgb->data);
      cv::resize(rgbMat,rgbMat,cv::Size(640,480),CV_INTER_LINEAR);
      std::stringstream ss1;
      now = clock();
      ss1 << "../image/rgb/"<<1000+(int)now/ CLOCKS_PER_SEC<<(double)now/ CLOCKS_PER_SEC-(int)now/ CLOCKS_PER_SEC << ".png";
      std::string filename1 = ss1.str();
      cv::imwrite(filename1, rgbMat);
      //CAPTURAR DEPTH  
      cv::Mat depthMat0 = cv::Mat(depth->height, depth->width,CV_8UC4, depth->data); //CV_8UC4
      //cv::Mat depthMat1;
      cv::Mat depthMat;
      cv::cvtColor(depthMat0, depthMat, CV_BGR2GRAY);
      depthMat.convertTo(depthMat,CV_16UC1,50);
      cv::resize(depthMat,depthMat,cv::Size(640,480),CV_INTER_LINEAR);
      std::stringstream ss2;
      now = clock();
      ss2 << "../image/depth/"<<1000+(int)now/ CLOCKS_PER_SEC<<(double)now/ CLOCKS_PER_SEC-(int)now/ CLOCKS_PER_SEC << ".png"; 
```

Start running

`$ cd [Opencv-RgbdOdometry]`

Create assoc.txt having synchronized rgb and depth images

`$ cd rgbd_dataset_freiburg2_pioneer_slam3`

`$ python associate.py rgb.txt depth.txt > assoc.txt`

`$ ../build/rgbd-odometry`
![Alt text](odometry.gif?raw=true "Title")

### paper
https://www.overleaf.com/read/yzqrmfgnvfff
### video
https://www.youtube.com/watch?v=5u0dKOktUoQ&index=3&list=UUN8zMoUDtRrEI9ri--yu9eQ
