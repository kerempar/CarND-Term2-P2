# Unscented Kalman Filter Project
Self-Driving Car Engineer Nanodegree Program

## Kerem Par
<kerempar@gmail.com>


---


In this project you will utilize an unscented kalman filter to estimate the state of a moving object of interest with noisy lidar and radar measurements. Passing the project requires obtaining RMSE values that are lower that the tolerance outlined in the project rubric. 


[//]: # (Image References)

[image1]: ./Dataset1.png =400x225 "Dataset1"


### Compiling


Code compiles without errors with cmake and make. 
The following change has been made to the original CMakeLists.txt. The line link_directories(/usr/local/Cellar/libuv/1.11.0/lib) was replaced by link_directories(/usr/local/Cellar/libuv/1.15.0/lib) because of the version of libuv installed.

### Accuracy


Algorithm was run against Dataset 1 in the simulator. The positions that the algorithm outputs compared to ground truth data. It was observed that px, py, vx, and vy RMSE were less than or equal to the values [.09, .10, .40, .30] for the last part of the path.

1.Dataset 1

![alt text][image1]


### Flow

Algorithm uses the first measurements to initialize the state vectors and covariance matrices. Process noise standard deviation for longitudinal acceleration (std_a) was set to 2 and process noise standard deviation for yaw acceleration (std_yawdd) was set to 0.3. Upon receiving a measurement after the first, the algorithm predicts object position to the current timestep and then updates the prediction using the new measurement. Algorithm sets up the appropriate matrices given the type of measurement and calls the measurement function for a given sensor type.

### Remarks

When calculating phi in y = z - h(x) for radar measurements, the resulting angle phi in the y vector was adjusted so that it is between -pi and pi. The Kalman filter is expecting small angle values between the range -pi and pi.

