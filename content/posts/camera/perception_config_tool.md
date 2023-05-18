---
title: "Design your Perception System"
date: 2020-06-02T21:54:28-04:00
draft: false
banner: "perception_system_design_tool.jpg"
summary: "What should be the field of view, resolution, baseline of the cameras you choose for you perception system? What other parameters should you consider?"
categories: []
tags: ['camera','perception']
---

Choosing the right parameters for your perception system is a hard problem. There are a plethora of parameters to consider. From the simple parameters like resolution to complex paramters like pixel density of objects, there are a wide range of these. Understanding each of these parameters and choosing appropriate values for those will dictate the success of perception system. The goal of this article is to introduce a web based tool we developed to play with these parameters and understand their effects. We will briefly discuss some of the important parameters and how to explore the design space using our tool. Click the button below to launch the tool in a separate tab on your browser.

{{< launch_button "/tools/perception_design/" "Perception Design Tool" >}}

## Tool
The figure below shows how the tool will look like and briefly goes over each section. This tool helps you to create cameras, change their parameters, create basic shapes. 

{{< figure src="../tool_overview.jpg" widht="100%" >}}

The screen is split vertically into two sections. The left section shows the different `camera views`. The tool starts with a single camera. You can see how the world looks like from that camera. The right section of the screen displays the `world view`. Here you can see the cameras, objects, checkerboards and everything else you created using this tool. You can pan, zoom and navigate around in this view using your mouse controls. The buttons in the top center are called `Action Buttons` and you can use them to create cameras, objects and perform certain actions like saving the image views. Finally the `controls` in the top right corner helps you to change parameters like field-of-view, position, size, rotations, etc. This is how we will manipulate the different entities in the world view. 

## Expanded View
The `world view` can be expanded to cover the full-screen by pressing on the `Expand View` button. In this mode there wil be more space to view and manipulate the objects. The cameras are still there but hidden.  You can get them back by pressing the `Show Camera Views` button. 

## Cameras
To create a camera press the `Add Camera` button. Creating and manipulating cameras is one of the primary goal of this tool. The `Add Camera` action first adds a camera to the world view. It also adds a view of the camera in the `Camera View` section. Finally it adds the camera controls to the `Controls -> Cameras` section. Each new camera gets an index added to it.
- Cameras
  - Camera1 - `Each new camera will have the index incremented by 1`
    - visible - `Toggle if the camera is visible in the world view. The camera view still shows the camera and gets updated when the scene changes`
    - field_of_view - `You can set the horizontal field of view of the camera in degrees`
    - aspect - `Sets the aspect ratio of the image. Eg. 1.3 (for 4/3 aspect). Please note that this also sets the vertical field of view of the camera`
    - width_in_pixels - `Sets the horizontal resolution of the image in pixels. The vertical resolution is calculated from the aspect ratio`
    - Rotation - `in degrees`
      - verge - `Sets the angle to verge the camera left/right. Positive angle turns the camera to the left`
      - tilt - `Sets the angle to tilt the camera up/down. Positive angle turns the camera up`
    - Position - `Sets the position of the camera in the world frame (in meters)`
      - x - `Pointing to the right`
      - y - `Pointing up`
      - z - `Pointing backwards`

## Lasers
This tool also includes a light striping laser. The light striping laser projects a plane of light which when interacting with the world appears as a line. To simulate that effect, the tool casts rays from the laser origin to the world and marks the points of intersection. You can control the number of rays and even visualize those rays using this tool.  

{{< figure src="../laser_rays.png" width="50%" >}}

The `Add Laser` action button adds a light striper to the world view. You can change the parameters of the laser in a way similar to the camera. 
- Lasers
  - Laser1 - `Each laser gets a name starting with 'Laser' and an index at the end`
    - num_rays - `Sets the number of rays used for ray casting step`
    - show_rays - `Toggles the visibility of the ray casting vectors`
    - point_size - `Sets the size of the point of intersection of the ray and the ground/object`
    - Position - `Sets the position in the world frame (in meters)`
      - x - `Pointing to the right`
      - y - `Pointing up`
      - z - `Pointing backwards`
    - Rotation - `Sets the rotation angle of the laser plane (in degrees)`
      - roll - `Rolls the laser plane left/right. Positive angle rotates the plane to the left`
      - pitch - `Pitches the laser plane up/down. Positive angle pitches the plane down`
      - yaw - `Turns the plane left/right. Positive yaw turns to the left`

## Checkerboard
We realized that a checkerboard will be very useful addition to this tool. The checkerboard can help you run some calibration or other vision based algorithms as though you are getting images for a real camera. However this is much better because you can control all the parameters without a lot of effort. This will be very helpful when you are testing your new vision based algorithm, but not sure if the accuracy hit is because of the distortions or calibrations issues or a deficiency in the algorithm itself. The `Add Checkerboard` button adds a single 7x9 checkerboard to the world. The checkerboard will have a white background to help the opencv calibration procedures.

- Checkerboards
  - Checkerboard1 - `The checkerboards are numbered sequentially as the cameras and lasers starting with the index 1`
    - white - `The exact shade of white you want the white squares to be at`
    - black - `The exact shade of black you want the black squares to be at`
    - scale - `Determines the size of each square (in meters). This parameter is necessary when you use this for calibration`
    - height - `Number of squares you want in the vertical dimension. The default is set to 7`
    - width - `Number of squares you want in the horizontal dimension. The default is set to 9`
    - visible - `Toggles the visibility of the checkerboard. You cannot delete a checkerboard once it is created.`
    - Position - `Similar to the laser. Sets the position in the world frame (in meters)`
        - x - `Pointing to the right`
        - y - `Pointing up`
        - z - `Pointing backwards`
      - Rotation - `Sets the rotation angle of the checkerboard (in degrees)`
        - roll - `Spins the checkerboard left/right. Positive angle spins it to the right`
        - pitch - `Pitches the checkerboard up/down. Positive angle pitches it away`
        - yaw - `Rotates the checkerboard left/right. Positive yaw rotates it to the anti-clockwise`

## Objects
One of the primary goals of this tool is the study how objects appear in a camera. To that end you can create cuboids/ellipsoids and place them at different locations, orientations and sizes. Each object shows up with a random color. 

- Objects
  - Object1 - `Objects are numbered sequentially like the other entities`
    - type - `You can choose a cube or a sphere`
    - visible - `Sets the visibility of the object`
    - Position - `Sets the position in the world frame (in meters)`
      - x - `Pointing to the right`
      - y - `Pointing up`
      - z - `Pointing backwards`
    - Size - `Sets the size of the object in each dimension (in meters). For a sphere, these parameters specify the axis in each dimension`
      - x - `Sets the width`
      - y - `Sets the height`
      - z - `Sets the length`

## Robot
Once a perception system is created with multiple cameras and lasers, it will be nice to visualize them moving around an environment. This section is created with that goal in mind. This feature is currently in development and hence might not work fully. All the cameras are attached to the robot frame. So moving the robot changes the position of all the cameras simultaneously. Currently the lasers are not attached to the robot yet. Also notice that there is only one robot and there are no buttons to add multiple robots. This is an intentional design decision. We want this tool to focus on the perception problem and do not want to turn this into a multi-robot simulator. Also, the robot is visualized using a cuboid. This is also intentional as it keeps things simpler.

- Robot - `Notices that there is only one robot`
  - visible - `Sets the visibility of the cuboid that gets drawn to represent the robot`
  - Position - `Sets the position of the robot and the cameras on it`
    - x - `Changes position along x (left or right)`
    - y - `Changes position along y (up or down)`
    - z - `Changes position along z (closer or far)`
  - Size - `Sets the size of the cuboid. This doesn't affect the cameras in any way`
    - x - `Sets the width`
    - y - `Sets the height`
    - z - `Sets the length`

Please feel free to send any feedback/suggestions you may have after using the tool.

