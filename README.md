# hri_bringup

# Use-Cases

## Launching ONLY the leg detector with intent to train
Launch the sensor node & leg tracker
```
roslaunch hri_bringup hokuyo_leg_tracker.launch
```

Record a bag file containing positive leg patterns
```
rosbag record -O leg_data --duration=1m /scan /tf
```

Define the geometry of the positive leg area for the trainer script
```
roslaunch leg_tracker extract_positive_training_clusters_args.launch x_min:="0.0" x_max:="3.5" y_min:="0.0" y_max:="1.25" filename:="test/leg_test_1"

# Alternatively, define the test/leg_test_1.yaml file
roslaunch leg_tracker extract_positive_training_clusters_yaml.launch filename:="test/leg_test_1"

```
ALTERNATIVELY: Call the service to bag and/or label the data!
```
rosrun leg_tracker leg_tracker_service
rosservice call /save_bag "topics: ['/tf', '/scan'] filename: 'rosbags/test/testbag' duration_sec: 1" 
rosservice call /label_cartesian "{scan_topic: '/scan', laser_frame: '/laser', label_frame: '/map', class_label: 1, filename: 'rosbags/test/testbag', x_center: 1.75, y_center: 0.65, x_length: 3.5, y_width: 1.3, yaw: 0.0}" 
rosservice call /label_arc "{scan_topic: '/scan', laser_frame: '/laser', label_frame: '/map', class_label: 1, filename: 'rosbags/test/testbag', min_angle: 0.0, max_angle: 45.0, max_radius: 3.7}"

rosservice call /bag_and_label_cartesian "{scan_topic: '/scan', laser_frame: 'laser', label_frame: '/map', class_label: 0, filename: 'rosbags/test/cartesian_test_clutter', x_center: 2.0, y_center: 1.0, x_length: 4.0, y_width: 2.0, yaw: 0.0, duration_sec: 30}"

rosservice call /bag_and_label_arc "{scan_topic: '/scan', laser_frame: 'laser', label_frame: '/map', class_label: 1, filename: 'rosbags/test/arc_test_leg', min_angle: -45.0, max_angle: 0.0, max_radius: 2.0, duration_sec: 30}"

rosservice call /bag_and_label_circ "{scan_topic: '/scan', laser_frame: 'laser', label_frame: '/map', class_label: 1, filename: 'rosbags/test/circ_test_leg', x_center: 1.0, y_center: -1.0, radius: 1.0, duration_sec: 30}"

```

Label the data/extract clusters
```
roslaunch leg_tracker extract_positive_training_clusters_yaml.launch filename:="train/not_extracted/arctestbag"
```


Visualize the labeling results
```
roslaunch hri_bringup visualize_scan_bag.launch bag_filename:="test/leg_test_1_extracted"
```