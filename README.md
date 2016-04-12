# Motion Capture with Kinect
_________________________________________________________________
Clemence Lapeyre (acl26), Sanmi Oyenuga (ojo), Gray Williams (gmw9)
Duke University, Fall Spring 2016
CS290-04: 3D Geometry
Final Project: Motion Capture with Kinect, 3D Lemur Tracking
_________________________________________________________________

Downloaded Microsoft's Visual Studio 2015
Downloaded SDK browser v2.0

Purpose:
- Enable people with limited technical skills to browse & interactively visualize motion capture databases
- Create an interface to capture and display new data

Features:
- "Skinning" animations (animation of surfaces based on motion capture data)
- Lemur Motion Tracking 

_________________________________________________________________

Paper and Slide Document Summary:

The two main stages that involved in image processing and data collection are
1) Depth Mapping
2) Body Position Inference.

Depth Mapping is performed by using the 3D Depth sensors on the kinect controller
with the depth computation performed by a PrimeSense hardware built in the Kinect.
Details of the hardware are not publicly known, but speculative analysis had
deduced the depth computation is performed using structured light. The kinect
projects infrared light with a speckle pattern and computes the relative
distances from and between objects using the depth focus principle (farther
objects appear to be blurrier). This distinction is performed using astigmatic
lenses with varying focal lengths e.g. circles become ellipses when projected
and the orientation of these ellipses depend on depth.

2) Body Position Inference:
