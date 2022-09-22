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
ALTERNATIVELY: Call the service to bag and label the data!
rosservice call /save_bag "topics: ['/tf', '/scan'] filename: 'test' duration_sec: 1" 


Visualize the labeling results
```
roslaunch hri_bringup visualize_scan_bag.launch bag_filename:="test/leg_test_1_extracted"
```