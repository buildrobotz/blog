---
title: "Brief Survey of Position Estimation Systems"
date: 2019-09-17T00:31:56-04:00
draft: false
banner: "position_estimator_survey.jpg"
summary: "State estimation is critical to autonomy. But should I choose kalman filter or smoothing? Which sensors should I use? "
categories: []
tags: []
---

I am starting a new project and I was wondering what would be the appropriate position estimation system to use. I have used a couple over the years. Each of them have their own strengths and weaknesses. I thought it would be a good exercise to go over them briefly to show how they differ and which one to use.

## Classification based on operation mode
One way to classify these systems is based on the way they operate on the data. There are two broad categories:

### Filtering based
These types of systems use some variant of kalman filter like the vanilla, extended or unscented kalman filter. When a new sensor measurement comes in, they integrate into the current state based on the confidence (covariance) measurements. Filtering based methods are relatively fast as they deal with implementing a few simple equations. However since they don’t store the previous states, it is generally not possible to go back and alter the previous states if we find out that we have drifted (when we identify a loop closure). There are some implementations that store some of the previous states or all the previous states and update them when loop closure happens, but these are generally computationally intensive and not real-time.

### Smoothing based
These types of systems use some kind of graph based methods to integrate their measurements. They store all the states and solve for all of them jointly at every step. One major advantage of graph based methods is that they have a way to update the previous measurements in the case of loop closures without much hassle. They are intrinsically good at that. However as the number of states increases, it becomes computationally intractable. So they were not very popular for estimating positions of robots. The ground breaking work of `Incremental Smoothing and Mapping` (iSAM[1] & iSAM2[2]) by  [Michael Kaess](http://www.cs.cmu.edu/~kaess/) changed this. They figured out a method to keep the computations tractable even for very large problems. It is a clever method to include entries into the graph and maintain sparsity which are much more efficient to operate on compared to dense methods. With more computing at our disposal and efficient ways of operating the graph, more and more systems are opting to use graph based methods for estimating position.

## Classification based on sensors used
Another way to classify these systems is based on the primary sensors they use. There are three major types that are popular these days:
{{< figure src="../position_estimators_overview.png" width="100%" caption="Classification of position estimators based on sensors used" >}}

## LIDAR based
With the advent of self driving cars, the use of LIDAR based systems have skyrocketed. LIDARs are relatively new to the field compared to cameras. Particularly they became famous after the DARPA Grand Challenge in 2006. Velodyne has been the market leader since then, making these LIDAR systems. However there are a lot of new entrants who are giving a tough competition to Velodyne. I heard that Velodyne was one of the competitors in the Grand Challenge. Their autonomy performed poorly but the sensor they developed worked really well and was an envy for many teams. So, after the competition they rolled that out as a product and are very successful with that.

### Iterative Closest Point (ICP)
[ICP](https://en.wikipedia.org/wiki/Iterative_closest_point) is an iterative algorithm which tries to align/stitch point clouds. It was published in the 90's. They became pretty famous with the LIDAR based sensors. The ICP algorithm tries to minimize the distance between the associated points. Multiple variants like point-to-plane and plane-to-plane algorithms exist as well. These methods are generally computationally intensive.  But the accuracy is much higher compared to other methods like using cameras. However the LIDAR sensors are costly as well. With the recent competition the price is dropping substantially. 

### Lidar Odometry and Mapping (LOAM)
LOAM[3] is the state-of-the-art LIDAR matching agorithm by [Ji Zhang](https://frc.ri.cmu.edu/~zhangji/). LOAM beats all the other state estimation algorithms in accuracy and has stayed on top of the pack since its publication. Instead of using ICP on the whole point cloud, they extract corner and edge features from the point cloud and use those for matching. These features help to summarize the information and generalize well to varied environments. 

## Camera based
If LIDAR is not an option, cameras are very good alternatives. Cameras and the associated computing has become so cheap, they could be used in any project. Camera based system can either use single camera (monocular), two cameras (binocular/stereo) or more. Depending on the density of the map, there are three different ways in which the camera system can be setup. 
- **Sparse methods**  
 These are suited for situations where we care about the position estimate and not about the actual map (ie., localization and not mapping in SLAM).
- **Dense methods**  
 For situations where we need a dense map which is consistent then we opt for the dense methods. However these methods are computationally expensive.
- **Semi-dense methods**   
 These are methods which use the dense method principles but maintain some level of sparsity for keeping the computation cost lower. 

 ### Sparse methods
 The general operating procedure is as follows:
- Detect features in the image using feature descriptors like SIFT/SURF or ORB. 
- Associate the features to the same features in a different image (from previous images in the monocular case and other camera in the binocular case)
- Calculate the depth (or 3d position of the feature observed) using some form of triangulation
- Estimate the change in position of the robot based on how these features move in the scene
- Use the estimated position to accumate the points to build a sparse map (generally done using a separate thread)
- Continue tracking these features as long as possible

#### Parallel Tracking and Mapping (PTAM)
PTAM[4] is a landmark work where they seperated the tracking and mapping processes into separate threads. This allowed tracking in real-time. Meanwhile the mapping process which is computationally heavy was not tied to real-time performance and hence could run longer and produce better results. The better results from the mapping process was then fed back to correct tracking. Such a separation is the main insight that came from the PTAM work which was pretty new at that time. 

#### ORB-SLAM and ORB-SLAM2
ORB-SLAM[5] is another landmark paper which showcased a robustified SLAM solution which in addition to tracking and mapping also showed loop-closure using the bag-of-words technique. Though none of these techniques were completely new, ORB-SLAM put them together in a way that made it fast, versatile and robust. They are so good, you can download the code and throw in some camera data and get them working in minimal time. One of the major limitations of PTAM was that it did not scale well with exploration. ORB-SLAM solved this problem in a elegant way using covisibility graphs. The ORB-SLAM2 extended the previous work to stereo camera setup. 

#### Visual Lidar Odometry and Mapping (VLOAM)
VLOAM[6] is an extension of the LOAM work but it employes a monocular camera as well. This work introduced a formulation where visual features, both with and without depth can be used to estimate the motion. This allowed some long distance visual features which are not observed by the lidar to be used which provided stability in bearing. It also gets the added benefit of LOAM. To date (2020) this is the best method on the KITTI dataset.

### Dense methods
Dense methods take a different approach to position estimation. Instead of tracking individual feature points, they employ the whole image and calculate photometric error for every pixel in the image. This makes the process computationally heavy but leads to a very dense map/pointcloud.

#### KinectFusion
KinectFusion[7] is one of the earlier dense methods which worked in real-time using a RGB-D sensor. This work leverages the insights from PTAM by separating the mapping and tracking processes. This helps it to maintain a very high tracking frame-rate which is crucial for the 'small motion' assumption and also helps the mapping process which uses a modified ICP algorithm.

#### Dense tracking and Mapping in Real-time (DTAM)
DTAM[8] is from Richard A. Newcombe who originally developed KinectFusion. It operates on monocular cameras! (instead of RGB-D camera). They estimate depth on keyframes and perform KinectFusion on those keyframes. The key insight is the prediction of depth for each pixel from a range of candidate depths which is precomputed. 

### Semi-dense methods
As opposed to using keypoints, semi-dense methods reduce photometric error similar to dense methods to create semi-dense depth maps which are then fused using dense method techniques.

#### Large Scale Direct Monocular SLAM (LSD-SLAM)
LSD-SLAM[9] is one of the famous semi-dense methods which demonstrated tracking and mapping on a large-scale dataset using photometric formulation. Dense methods typically fall short when it comes to mapping larger areas because they run into memory and computation constraints. LSD-SLAM tackles this issue by constructing semi-dense depth maps. 

## Camera and IMU based
Data from the IMU is generally very noisy. In addition to the noise, the sensor data has biases which are hard to estimate as the biases keep drifting. However recent advancements in the IMU technology has led to sensors which are much cheaper and less noisy. This has led to software systems which can better estimate the biases and form a tightly coupled system commonly referred to as visual-inertial systems. Of course the LIDAR based systems and camera based systems use IMU data. But they are just used as initial estimates in the optimization process. However in visual inertial systems they are tightly integrated.

### VINS Mono & VINS Fusion
VINS-Mon[10] is a monocular odometry system which tightly incorporates the IMU (visual-inertial). VINS-Mono incorporates IMU preintegration which propogates the bias estimates to previous steps and update the IMU measurements without have to recompute everything. This is a smart formulation which leads to efficient computations and better position estimates. VINS-Fusion is an extension of VINS-Mono which supports stereo cameras.

### Visual Inertial Lidar SLAM (VIL SLAM)
VIL-SLAM[11] builds and VINS Mono and VLOAM. It implements a visual-inertial system for odometry which is then fed to lidar mapping. It uses visual-bag-of-words for loop closure and refines it further using ICP. 


## References
[1] Michael Kaess "[Incremental Smoothing and Mapping](http://www.cs.cmu.edu/~kaess/pub/Kaess08thesis.pdf)" Ph.D dissertation, Dec 2008  
[2] Michael Kaess, et al., "[iSAM2: Incremental Smoothing and Mapping Using the Bayes Tree](http://www.cs.cmu.edu/~kaess/pub/Kaess12ijrr.pdf)", Intl. J. of Robotics Research, Feb 2012  
[3] Ji Zhang and Sanjiv Singh "[LOAM: Lidar Odometry and Mapping in Real-time](https://ri.cmu.edu/pub_files/2014/7/Ji_LidarMapping_RSS2014_v8.pdf)", RSS, 2014  
[4] Georg Klein and David Murray "[Parallel Tracking and Mapping for Small AR Workspaces](http://www.robots.ox.ac.uk/~gk/publications/KleinMurray2007ISMAR.pdf)", International Symposium on Mixed and Augmented Reality, 2007  
[5] Raul Mur-Artal, J. M. M. Montiel, Juan D. Tardos "[ORB-SLAM: a Versatile and Accurate Monocular SLAM System](https://arxiv.org/abs/1502.00956)", IEEE transactions on robotics, 2015  
[6] Ji Zhang and Sanjiv Singh "[Visual-lidar Odometry and Mapping: Low-drift, Robust and Fast](https://frc.ri.cmu.edu/~zhangji/publications/ICRA_2015.pdf)" IEEE International Conference on Robotics and Automation (ICRA), 2015  
[7] Richard A. Newcombe et al., "[KinectFusion: Real-Time Dense Surface mapping and Tracking](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ismar2011.pdf)" IEEE International Symposium on Mixed and Augmented Reality, 2011  
[8] Richard A. Newcombe et al., "DTAM: Dense tracking and mapping in real-time" ICCV 2011  
[9] Jakob Engel, et al., "[LSD-SLAM: Large-Scale Direct Monocular SLAM](https://vision.in.tum.de/_media/spezial/bib/engel14eccv.pdf)" European Conference on Computer Vision 2014  
[10] Tong Qin, Peiliang Li and Shaojie Shen "[VINS-Mono: A Robust and Versatile Monocular Visual-Inertial State Estimator](https://arxiv.org/pdf/1708.03852.pdf)" IEEE Transactions on Robotics 2017
[11] Weizhao Shao, et al., "[Stereo Visual Inertial LiDAR Simultaneous Localization and Mapping](https://arxiv.org/abs/1902.10741)" IROS 2019
