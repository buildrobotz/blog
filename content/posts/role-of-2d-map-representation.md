---
title: "2D Maps in Navigation"
date: 2024-12-21
draft: false
banner: "2d-map-representations.webp"
summary: "How do autonomous robots find their way through complex outdoor environments? From avoiding obstacles to scaling uneven terrain, field robots rely on smart mapping strategies to navigate efficiently. Dive into this post to explore how occupancy grids, elevation maps, and topological maps enable robots to make sense of their surroundings and tackle real-world challenges."
math: true
categories: []
tags: ['FieldRobotics', 'Mapping', '2DMaps', 'OccupancyGrid', 'TopologicalMaps', 'RoboticsNavigation', 'AutonomousSystems', 'RobotMapping', 'TerrainNavigation', 'RoboticsResearch', 'RobotPathPlanning', 'SmartRobots' ]
---

## The Role of 2D Map Representations in Navigation for Field Robotics

In the realm of field robotics, effective navigation depends on the robot’s ability to accurately perceive and interpret its environment. Unlike controlled indoor spaces, outdoor environments present numerous challenges such as unpredictable terrain, obstacles, and dynamic elements. To navigate efficiently, robots employ several types of **2D map representations** that balance simplicity and functionality. Among the most widely used approaches are **occupancy grids**, **elevation maps**, and **topological maps**. These different mapping techniques serve unique purposes in field robotics, often complementing each other to ensure safe and efficient navigation.

---

### 1. **Occupancy Grid Maps: Mapping the Known and Unknown**

Occupancy grids are one of the most popular 2D representations for robot navigation. First introduced by Moravec and Elfes in the 1980s, this method divides the environment into a discrete grid, where each cell holds a probability value representing whether the cell is occupied or free.

- **Binary Occupancy Grid**: Each grid cell is either marked as occupied (1) or free (0), simplifying path planning.
- **Probabilistic Grid** :[1,2] In more complex environments, the grid can store probabilities (ranging from 0 to 1) to account for uncertainty in sensor measurements.

Occupancy grids are particularly suited for robots that need to operate in environments with obstacles that can block the robot’s path. They rely heavily on sensors like **LiDAR** and **ultrasonic sensors** to detect free space and objects. These maps also facilitate **path planning algorithms** like A* and Dijkstra, which work well in known environments.

{{< figure src="/images/2D map representations-20241014153719853.webp" caption="A simple binary occupancy grid with cells marked as free (white), occupied (black), and unknown (gray)[3]" >}}


---

### 2. **Elevation Maps: Navigating Uneven Terrain**

While occupancy grids are excellent for flat environments, field robots often need to handle uneven or hilly terrains, especially in agricultural, mining, or disaster response missions. **Elevation maps** extend 2D grid concepts by associating a height value with each cell. This enables robots to account for vertical variations in the environment.

- **Use Case**: A robot deployed in a forest or on a hill must understand the slope of the terrain to avoid tipping over. Elevation maps help it predict the slope and ensure stable navigation.
- **Data Sources**: Elevation data is typically collected using **stereo cameras**, **LiDAR**, or **GPS-based altimeters**.

Robots use elevation maps to plan energy-efficient paths and avoid regions that are too steep or impassable. Furthermore, terrain analysis algorithms process the elevation data to identify **safe zones** and **regions with high risk of slippage**.

{{< figure src="/images/2D map representations-20241014154119633.webp" width="80%" caption="An elevation map with height values color-coded (e.g., blue for low elevation, red for high elevation) [4]" >}}

---

### 3. **Topological Maps: Abstracting the Environment**

While grid-based maps provide detailed metric information, they can become computationally expensive when scaled to large environments. This is where **topological maps** shine by offering an abstraction of the environment. Instead of storing information about every point in space, topological maps represent the environment as a **graph of nodes and edges**.

- **Nodes**: Represent important waypoints (e.g., intersections, narrow passages, or significant landmarks).
- **Edges**: Define possible paths or connections between nodes.

Topological maps are particularly useful for **long-range navigation**, especially in **outdoor environments like campuses or parks**, where robots need to traverse large areas without processing unnecessary details. A typical example is the navigation of autonomous delivery robots that follow pedestrian walkways and intersections.

- **Hybrid Approaches**: Many field robots combine **occupancy grids for local navigation** and **topological maps for global path planning**, leveraging the strengths of both approaches.

![Topological map](https://transportist.org/wp-content/uploads/2016/09/topology-primal.png)
A topological street map with nodes representing intersections and edges representing links

---

### 4. **Choosing the Right Map for the Job**

The choice of a suitable 2D map representation depends on several factors, including the nature of the environment, the robot’s task, and computational limitations.

|**Map Type**|**Best Use Case**|**Limitations**|
|---|---|---|
|Occupancy Grid|Indoor or flat environments with obstacles|Inefficient for large or dynamic areas|
|Elevation Map|Uneven outdoor terrains (hills, forests)|Requires more complex sensors and processing|
|Topological Map|Large environments with abstract paths|Limited spatial precision|

In some advanced field robotics applications, these map types are integrated to handle both local and global navigation challenges. For instance, **agricultural robots** might use **occupancy grids to avoid obstacles in real-time** while following **topological paths defined by crop rows**. Similarly, **autonomous ground vehicles** in rough terrains may utilize **elevation maps to assess slope safety** and switch to **occupancy grids for obstacle avoidance**.

---

### 5. **Conclusion: The Future of 2D Mapping in Field Robotics**

The versatility of 2D maps in field robotics allows robots to adapt to diverse environments, from structured indoor spaces to unpredictable outdoor terrains. **Occupancy grids, elevation maps, and topological maps** each offer distinct advantages and limitations. As robotics technology advances, we are seeing increasing efforts to **integrate multiple map types** within the same system to enable more robust and flexible navigation.

Research into **multi-modal mapping**—where 2D and 3D representations coexist—continues to expand, offering new possibilities for **autonomous exploration and long-range missions**. With improved sensor technologies and more efficient computational algorithms, the use of these 2D maps will remain essential for building the next generation of field robots.

---

### References
1. Thrun, S., Burgard, W., & Fox, D. (2005). _Probabilistic Robotics_. MIT Press.
2. Moravec, H., & Elfes, A. (1985). "High Resolution Maps from Wide Angle Sonar," _IEEE International Conference on Robotics and Automation_.
3. Sundram, Jugesh & Nguyen, Harry Duong & Soh, Gim & Bouffanais, Roland & Wood, Kristin. (2018). Development of a Miniature Robot for Multi-robot Occupancy Grid Mapping. 10.1109/ICARM.2018.8610745. 
4. Piburn, Michael & Reynolds, Stephen & Leedy, Debra & McAuliffe, Carla & Birk, James & Johnson, Julia. (2002). The hidden earth: Visualization of geologic features and their subsurface geometry. 

