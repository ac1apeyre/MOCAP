# Motion Capture with Kinect
_________________________________________________________________
Clemence Lapeyre (acl26), Sanmi Oyenuga (ojo), Gray Williams (gmw9)

Duke University, Spring 2016

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

Paper Summary and Slide Document Summary:

The two main stages that involved in image processing and data collection are
1) Depth Mapping
2) Body Position Inference.

1) Depth Mapping is performed by using the 3D Depth sensors on the kinect controller
with the depth computation performed by a PrimeSense hardware built in the Kinect.
Details of the hardware are not publicly known, but speculative analysis had
deduced the depth computation is performed using structured light. The kinect
projects infrared light with a speckle pattern and computes the relative
distances from and between objects using the depth focus principle (farther
objects appear to be blurrier). This distinction is performed using astigmatic
lenses with varying focal lengths e.g. circles become ellipses when projected
and the orientation of these ellipses depend on depth.

2) Body Position Inference:
Using known 100,000 depth images with known skeletons from motion capture data,
we can obtain millions of training examples by rendering dozens more for each
real image. These examples are then used as templates in machine learning.
A randomized decision tree is generated using the knowledge from the known
skeletons to help infer the position of a body. Due the millions of training
examples the decision tree will have too many possibilities of questions so a
random subset of about 2,000 questions are utilized in each run to generate
the decision trees. A randomized decision forest is generated from conglomerated
trees with results as probability distributions as opposed to single decisions in
the case of trees. Using this randomized decisions tree as a training data set,
the mean shift algorithm can then be used to compute modes of probability
distributions.

______________________________________________________________________________

Recording and Playback:

1) Types of Data

Color, audio, depth, and infrared data can be recorded and played back. BodyIndex and
BodyFrame data can only be recorded. If you want to playback skeleton, you need to 
record infrared and depth; the body tracking / skeleton data will be generated from
that data.

2) How to Record

Go to the recording tab in the Kinect Studio. Select the desired streams (for body-
tracking and gesture building, these are Body Fram, Depth, IR, and Sensor Telemetry).
Click record, then perform the gestures that you want to use for training- both 
positive and negative. When done, click stop recording. The PLAY tab will open with a
timeline and swim lanes. The first swim lane can be used to select range, place pause
points (for debugging), and putting markers for adding metadata. The recordings are 
saved in Documents->KinectStudio->Repository by default. 

3) Playback

The original data can be played back in the Kinect Studio and seen in 2D or 3D. This 
can be done offline and does not require being connected to the Kinect. To see the 
skeletal data, connect to the Kinect and open the SDK browser. Select a BodyBasics
app to run. Then, in the Kinect Studio, click the play button. Skeletal Data will 
be displayed in the BodyBasics App.


Recordings are saved as .xef files. Because of size, we will be uploading them to
Google Drive. Our test data file can be found here:
https://drive.google.com/folderview?id=0B8IeT0G1k4MaSEQ3c195NkJmS0U&usp=sharing



_________________________________________________________________________

Joint Position Data


foreach (Joint joint in body.Joints)
{
    // 3D coordinates in meters
    CameraSpacePoint cameraPoint = joint.Position;
    float x = cameraPoint.X;
    float y = cameraPoint.Y;
    float z = cameraPoint.Z;
}

#TODO: get x, y, and z coordinates of each joint for each frame
