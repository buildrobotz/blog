---
title: "Perspective n Points (PnP) Algorithm"
date: 2020-05-05T21:24:12-04:00
draft: false
banner: "pnp.jpg"
summary: "PnP algorithm estimates the position of the camera based on some 3d points and their projections on the image. Lets learn the math behind this"
categories: []
tags: ['transforms']
math: true
---

## Applications
Given a set of 3d points in an arbitary frame and their projection on the camera frame (pixel coordinates), PnP algorithm estimates the position and rotation (6 degrees of freedom pose) of the camera with respect to the 3d points. PnP algorithm has a lot of applications specifically in domains involving cameras. I have personally worked on two applications.

- **Motion Estimation**  
 PnP algorithm can be used the estimate the frame-to-frame motion in a visual odometry system, typically in a sparse setting. The way it works is we will have feature points tracked from previous frames. We estimate the depth or 3d position of these points using triangulation. The 3d points and the 2d pixel locations can be used to calculate the motion of the camera, from which the motion of the robot can be calculated.

- **Calibration**  
PnP is used during camera calibration to find the location of the camera with respect to the checker board. However PnP assumes we already know the camera intrinsics. If that is not true, then there are other techniques to extract both the intrinsics and the extrinsics. Check out this article on [camera calibration](/posts/camera/camera_calibration_math/) to learn how to do that.

## Math
Let us start with the camera projection equation. 
$$ \begin{bmatrix} u \\\ v \\\ 1 \end{bmatrix} = \alpha \\; K \color{red}{\begin{bmatrix} R |  t \end{bmatrix}} \begin{bmatrix} X \\\ Y \\\ Z \\\ 1 \end{bmatrix} $$

where,  
`$\begin{bmatrix} X & Y & Z & 1 \end{bmatrix}^T$` is the 3d points in the world coordinates (in homogeneous form),  
`$[R|t]$` is the rotation and translation components that transforms the 3d point from the world coordinates to the camera coordinates,  
`$K$` is the camera/intrinsic matrix  
`$ \begin{bmatrix} u & v & 1 \end{bmatrix}$` are the pixel coordinates of the 3d points (in homogeneous form),  
`$\alpha$` is the scale factor.  

### Knowns and Unknowns
In the PnP problem, we know the 3d points in the world coordinates and their projection in the pixel coordinates. We also know the intrinsic matrix. We are trying to find the rotation and translation `$[R | t]$` that moves the 3d point from the world coordinates to the camera pixel coordinates. 

### Cross Product
We will use [cross product](https://en.wikipedia.org/wiki/Cross_product) and one of its properties to solve this. The cross product is defined between two 3d vectors given by,
$$\vec{a} \times \vec{b} = [a]_\times \vec{b}  = \begin{bmatrix} 0 & -a_3 & a_2 \\\ a_3 & 0 & -a_1 \\\ -a_2 & a_1 & 0 \end{bmatrix}  \begin{bmatrix} b_1 \\\ b2 \\\ b3 \end{bmatrix}$$

The propery we will use is the cross product of a vector with itself is zero, ie.,
$$\vec{a} \times \vec{a} = 0$$

### Derivation
Now lets start from the projection equation.
$$ \begin{bmatrix} u \\\ v \\\ 1 \end{bmatrix} = \alpha \\; P \begin{bmatrix} X \\\ Y \\\ Z \\\ 1 \end{bmatrix} $$

where $P = K [R | t]$ is the projection matrix of dimension $3 \times 4$.  
$P \begin{bmatrix} X & Y & Z & 1\end{bmatrix}^T$ yeilds a $3 \times 1$ vector, let us call it $M$

$$ \begin{bmatrix} u \\\ v \\\ 1 \end{bmatrix} = \alpha \\; M $$

Let us take a cross product of M on both sides,

$$ \begin{bmatrix} u \\\ v \\\ 1 \end{bmatrix} \times M = \alpha \\; M  \times M = 0 $$

Using the property of cross product $M \times M = 0$

{{< special_quote by="Interesting Note">}}
$P$ is not a square matrix. So it does not have an inverse. It is a clever idea to use the cross product to deal with $P$.
{{< /special_quote >}}

Replacing $M$,
$$ \begin{bmatrix} u \\\ v \\\ 1 \end{bmatrix} \times M = \begin{bmatrix} u \\\ v \\\ 1 \end{bmatrix} \times P \begin{bmatrix} X \\\ Y \\\ Z \\\ 1 \end{bmatrix} = 0$$

$$ \begin{bmatrix} 0 & -1 & v \\\ 1 & 0  & -u \\\ -v & u & 0 \end{bmatrix} P \begin{bmatrix} X \\\ Y \\\ Z \\\ 1 \end{bmatrix}  = 0 $$

Expanding $P$,
$$ \begin{bmatrix} 0 & -1 & v \\\ 1 & 0  & -u \\\ -v & u & 0 \end{bmatrix} \begin{bmatrix} P_{11} & P_{12} & P_{13} & P_{14}  \\\ P_{21} & P_{22} & P_{23} & P_{24} \\\ P_{31} & P_{32} & P_{33} & P_{34} \end{bmatrix} \begin{bmatrix} X \\\ Y \\\ Z \\\ 1 \end{bmatrix}  = 0 $$

$$ \begin{bmatrix} 0 & -1 & v \\\ 1 & 0  & -u \\\ -v & u & 0 \end{bmatrix} \begin{bmatrix} P_{11}X + P_{12}Y + P_{13}Z + P_{14}  \\\ P_{21}X + P_{22}Y + P_{23}Z + P_{24} \\\ P_{31}X + P_{32}Y + P_{33}Z + P_{34} \end{bmatrix}   = 0 $$

$$ \begin{bmatrix} -\left(P_{21}X + P_{22}Y + P_{23}Z + P_{24}\right)  + v\left(P_{31}X + P_{32}Y + P_{33}Z + P_{34}\right) \\\ P_{11}X + P_{12}Y + P_{13}Z + P_{14} -u \left(P_{31}X + P_{32}Y + P_{33}Z + P_{34}\right) \\\ -v\left(P_{11}X + P_{12}Y + P_{13}Z + P_{14}\right) +u\left(P_{21}X + P_{22}Y + P_{23}Z + P_{24}\right) \end{bmatrix}   = 0 $$

Rearranging and pulling out the $P_{ij}$ entries to a vector, we get,

$$\begin{aligned} & \left[\begin{matrix}X - vX & Y-vY & Z - vZ & 1-v & -X+uX & -Y+uY \end{matrix}\right.\\\ &\qquad\qquad
 \left.\begin{matrix} -Z+uZ & -1+u & vX -uX & vY - uY & vZ - uZ & v-u \\\ \end{matrix}\right]\end{aligned}
  \begin{bmatrix} P_{11} \\\ P_{12} \\\ P_{13} \\\ P_{14} \\\ P_{21} \\\ P_{22} \\\ P_{23} \\\ P_{24} \\\ P_{31} \\\ P_{32} \\\ P_{33} \\\ P_{34}  \end{bmatrix} = 0$$

There are 12 unknown, however each point given two values $u$ and $v$. So we will need atleast 6 points to solve for $P$. However in practice we will use a lot more points to account for the inaccuracies in the pixel locations and solve using the least squares approach.

$$\begin{aligned} & \left[\begin{matrix}X_1 - v_1X_1 & Y_1-v_1Y_1 & Z_1 - v_1Z_1 & 1-v_1 & -X_1+u_1X_1 & -Y_1+u_1Y_1 \\\ ... \\\ X_n - v_nX_n & Y_n-v_nY_n & Z_n - v_nZ_n & 1-v_n & -X_n+u_nX_n & -Y_n+u_nY_n\end{matrix}\right.\\\ &\qquad\qquad
\left.\begin{matrix}  -Z_1+u_1Z_1 & -1+u_1 & v_1X_1 -u_1X_1 & v_1Y_1 - u_1Y_1 & v_1Z_1 - u_1Z_1 & v_1-u_1 \\\ ... \\\ -Z_n+u_nZ_n & -1+u_n & v_nX_n -u_nX_n & v_nY_n - u_nY_n & v_nZ_n - u_nZ_n & v_n-u_n  \end{matrix}\right]\end{aligned}  \begin{bmatrix} P_{11} \\\ P_{12} \\\ P_{13} \\\ P_{14} \\\ P_{21} \\\ P_{22} \\\ P_{23} \\\ P_{24} \\\ P_{31} \\\ P_{32} \\\ P_{33} \\\ P_{34}  \end{bmatrix} = 0$$

This is of the form $Ax = 0$ and can be solved by taking the SVD of A.  
$ svd(A) = UDV^T$ and $x = last\ column\ of\ V$

We know $P = K [R | t]$, since we already know $K$ and we just calculated $P$ from SVD, we can get $[R|t]$

$$ [R | t] = K^{-1} P = P^n_{3x4}$$

$P^n$ is $3 \times 4$. The first three columns of $P_n$ corresponds to $R$ and the last column corresponds to $t$ but we cannot assign it directly as $R$ is a rotation matrix and the columns need to be orthogonal. Hence we use SVD again to extract the orthogonal components.

$$svd(P_{c1:c3}) = UDV^T \implies R = UV^T$$

The diagonal matrix $D$ has the eigen values of the form $D = diag(\alpha_1,\alpha_2, \alpha_3)$. The translation component $t$ is given by dividing the last column of $P^n$ by the first eigen value.

$$t = \frac{P^n_{c4}}{\alpha_1}$$

This methods finds the transform between the world coordinate and the camera from scratch. It is generally used to initialize the system. However if we want to estimate this transform for every time instance it will get pretty heavy. There are iterative methods available which can calculate these transforms pretty quickly. They feed in the previous estimate as initial guess and refine the transform based on the updated pixel locations.

