# Exercise: Object detection

**Objective**: To grab images from the robot's camera and detect the distance to well-known shapes

The robot is equipped with an Asus Xtion PRO camera, which contains an RGB and Depth sensor, similar to the Microsoft Kinect sensor. In this exercise we will work only on images from the RGB sensor.

## 1. Connect to the robot and camera

Connect your computer to the robot -- either the physical one or a simulation in Gazebo.

## 2. Connecting Matlab to Turtlebot
If you have access to a physical turtlebot launch the camera node. This will automatically start-up a ROS master
```
roslaunch turtlebot_bringup 3dsensor.launch
```

If you are using a simulated turtlebot in Gazebo, launch it using a command like this:

```
roslaunch turtlebot_gazebo turtlebot_world.launch
```

In Matlab initialize ROS (use ip-address corresponding to your computer and VM):
```
setenv('ROS_MASTER_URI','http://192.168.1.200:11311')
setenv('ROS_IP','192.168.1.100')
rosinit('http://192.168.1.200:11311','NodeHost','192.168.1.100');
```

Try running `rostopic list` and `rostopic info /topic_name`to inspect available ros topics related to the camera.

Images are published to a topic named `/camera/rgb/image_raw`

## 2b. Alternatively connnect to webcam
If you don't have access to the turtlebot or prefer using your own webcam, you can do write the following in Matlab:
```
cam = webcam
img = snapshot(cam);
```

## 3. Write a script that reads images and determine the distance to known objects

Your script should grab an images from the turtlebot, detect known objects and calculate the distance to those objects. 

1. Place the robot approximately 2 m and perpendicular to a wall holding a number of test objects.

2. Try to estimate the constant ![k](https://latex.codecogs.com/svg.latex?k) from the perspective transform. I.e. ![\Delta r=\frac{f}{s_x}\frac{\Delta X}{Z}=k\frac{\Delta X](https://latex.codecogs.com/svg.latex?\Large&space;\Delta%20r=\frac{f}{s_x}\frac{\Delta%20X}{Z}=k\frac{\Delta%20X}{Z}), where ![f](https://latex.codecogs.com/svg.latex?f) is the focal length, ![s](https://latex.codecogs.com/svg.latex?s_x) is the pixel size on the sensor, ![Z](https://latex.codecogs.com/svg.latex?Z) is the real distance, and ![\Delta X](https://latex.codecogs.com/svg.latex?\Delta%20X) is the real height of the object. From manual measurements you can determine ![\Delta r](https://latex.codecogs.com/svg.latex?\Delta%20r), ![\Delta X](https://latex.codecogs.com/svg.latex?\Delta%20X), and ![Z](https://latex.codecogs.com/svg.latex?Z). For measuring pixel-coordinates in Matlab, you can display images using the `imtool(im)` function, or you can use the "data courser"-tool for `imshow` in Matlab, which can be found in "Tools>Data courser" in an image window.
3. Using the k-value from 2., try plotting the relation between the distance to the object ![Z](https://latex.codecogs.com/svg.latex?Z) and its hight in pixels ![r](https://latex.codecogs.com/svg.latex?\Delta%20r) when changing ![Z](https://latex.codecogs.com/svg.latex?Z) – relate this plot to the perspective transform, i.e. an “1/z” shape.

4. Experiment with automatic detection of various test objects and calculate the distance to those objects automatically. [This pdf-file](shapes.pdf) holds a number of shapes in different colours, which you can use for testing your script. 
	* For this part you have to detect squares / circles
	* Detect colours of the objects. Hint: Use HSV format and detect a certain Hue interval according to color range.
5. Test the robustness of your algorithm -- especially when light changes


You might want to take a look at [this demo](https://github.com/au-mobile-robots/Tutorials/blob/master/read%20image%20from%20camera/demo_grabImageFromRobot.m), which demonstrates how to read an image from the turtle-bot.

How to detect connected components in BW image: [bwconncomp](https://www.mathworks.com/help/images/ref/bwconncomp.html)

More on: [https://se.mathworks.com/help/images/morphological-filtering.html](https://se.mathworks.com/help/images/morphological-filtering.html) and [https://www.mathworks.com/help/images/ref/regionprops.html](https://www.mathworks.com/help/images/ref/regionprops.html)

Bonus: If you can't get enough of detecting objects, try taking a look at the following project, which demonstrates a clever way to detect black/white markers and their orientation: [https://github.com/henrikmidtiby/MarkerLocator](https://github.com/henrikmidtiby/MarkerLocator)
