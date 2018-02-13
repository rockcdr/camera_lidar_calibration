1. Capture ros bag
Path: github/Software_Interation/catkin_ws
Commands:
1) roslaunch saic sensor_white.launch
2) rosrun rviz rviz (visualize)

UI ops:
Add
1) press 'add topic'
2)
/camera/image_raw
image: select raw
3)
/velodyne_points/pointCloud2

Record:
rosbag record /camera/image_raw /points_raw
rosbag play -l xxx.bag


Some other commands:
1) source devel/steup.sh
2) check if the sensor is responding:
rostopic hz /camera/image_raw
rostopic hz /velodyne_points
rostopic llist
rosbag info



Issues: 
1) fxied frame error 
Solution: input Fixed Frame with "velodyne"

2)change the ignore file (missing drivers)
catkin-make to re-compile

3) white_suv
lidar: 192.168.1.201
port:2368
can check it in sensor_white.launch

4) camera name: tls camera
-----------------------------------------------------------------------


2. Video record
1) Use the large chess board
2) 2 (up, down) * 3 (left, right, middle) * 2 (near, far) * 5 (front + 30 degree tilt, left, right, forward, backward) 
3) Confirm the leftmost and rightmost boundary
4) The near is about 3m, the far is about 6m
5) Record the video quickly, the rosbag size can be large

-----------------------------------------------------------------------

3. Label data and get camera matrix

Issue 1: can't load the lidar points in Autoware 
Reason: the lidar topic name has to be /points_raw
change topic name when play:
rosrun rosbag topic_renamer.py /velodyne_points ~/rosbag/image_lidar_calib_white_suv.bag /points_raw 

Issue 2: can only see topic name after play

1) Immediately stop after start, use 'space'
2) Play the video by step, use 's'
3) For lidar image, 
   1> rotate the cloud to center by 'e' or 'q', so that the blank area after the chess board is orthogonal
   2> use 'up arrow' to zoom in
   3> use 'w' to lift it to center
   4> press 'up arrow' twice to zoom in for the chess board
   5> 'page down' to lift up the chessboard
   6> use mouse wheel to control the sensitivity
   7> use 'w' and 'page up' to tune

4) press 'calibrate' to get the calibration matrix, need to compute for a while
5) press 'project' to see the lidar points on the camera image

-----------------------------------------------------------------------

4. Check correctness
1) In autoware, press 'project' to see the lidar points on the camera image
2) For lidar to camera calibration, you can check how the lidar point projection aligns well to camera images.
Especially, you can check regions with discontinuities such as adjoining walls.

  
-----------------------------------------------------------------

other resources:
1) https://www.youtube.com/watch?v=pfBmfgHf6zg
2) https://github.com/CPFL/Autoware/wiki/Calibration


