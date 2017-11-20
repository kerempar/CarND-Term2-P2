# Unscented Kalman Filter Project
Self-Driving Car Engineer Nanodegree Program

## Kerem Par
<kerempar@gmail.com>


---


In this project you will utilize an unscented kalman filter to estimate the state of a moving object of interest with noisy lidar and radar measurements. Passing the project requires obtaining RMSE values that are lower that the tolerance outlined in the project rubric. 


[//]: # (Image References)

[image1]: ./output_images/dataset1.png =500x250 "Dataset1"
[image2]: ./output_images/plot1.png =650x300 "Plot1"
[image3]: ./output_images/plot2.png =650x250 "Plot2"
[image4]: ./output_images/table.png =650x300 "Table"


### Compiling


Code compiles without errors with cmake and make. 
The following change has been made to the original CMakeLists.txt. The line link_directories(/usr/local/Cellar/libuv/1.11.0/lib) was replaced by link_directories(/usr/local/Cellar/libuv/1.15.0/lib) because of the version of libuv installed.

### Accuracy


Algorithm was run against Dataset 1 in the simulator. The positions that the algorithm outputs compared to ground truth data. It was observed that px, py, vx, and vy RMSE were less than or equal to the values [.09, .10, .40, .30] for the final part of the path.

The following are the output of the simulator:

1.Simulation with Dataset 1

![alt text][image1]

The estimations and RMSE values were also output to a file and plotted using the visualization components in IPython notebook. The notebook is also included in the repository (ukf-visualization.ipynb). The following are the output of the visualisations:

2.Visualizations

![alt text][image2]
![alt text][image3]

The algorithm was run only using Lidar measurements, only using Radar measurements and, using both Lidar and Radar measurements. The following shows the RMSE values for each run. It is clearly observed that better RMSE values can be obtained with the fused version. 

3.RMSE Values

| State | UKF-Lidar | UKF-Radar | UKF-Lidar+Radar |
| ----- | --------- | --------- | --------------- |
|  px   |  0.1667   |  0.2105   |  0.0731         |
|  py   |  0.1468   |  0.2566   |  0.0822         |
|  vx   |  0.2312   |  0.2524   |  0.1770         |
|  vy   |  0.2494   |  0.2966   |  0.1983         |



### Flow

Process noise standard deviation for longitudinal acceleration (std_a) was set to 1.22 and process noise standard deviation for yaw acceleration (std_yawdd) was set to 0.275.
For tuning 'std_a', accelerations were computed and analyzed by using the ground truth velocity values in the input data. accelerations in y dimension, varies between -2.44 and +2.44 with an average of zero. That's why 'std_a' was approximated as 1.22. Similarly, yawrate varies between -0.55 and 0.55. 'std_yawdd' was set approximately to 0.275. The following shows the contents of the output table.

![alt text][image4]
    
Algorithm uses the first measurements to initialize the state vectors and covariance matrices.
Different initalizations were made for Lidar and Radar. Since there is no velocity information in Lidar measurement, the velocity was set to approximately to 5. For state covariance matrix of Lidar, since standard deviation of measurement noise for position x and position y are given, the variances of these values were used for diagonal values of covariance matrix.
For radar, an approximate velocity was calculated by using rho_dot and phi. And for the covariance matrix of radar, variance of measurement noise of radius was used for px and py,  the other values were left as 1.     

Upon receiving a measurement after the first, the algorithm predicts object position to the current timestep and then updates the prediction using the new measurement. Algorithm sets up the appropriate matrices given the type of measurement and calls the update function for a given sensor type.


### Code Efficiency

The algorithm performs angle normalization several times. Code for angle normalization could be defined as a separate method for a better code structure.

The final update UKF part is mostly common for UpdateRadar and UpdateLidar. This code can be defined as a separate method and called both from UpdateRadar and UpdateLidar for a better code structure.  
