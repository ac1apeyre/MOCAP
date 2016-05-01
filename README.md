# Motion Capture with Kinect
_________________________________________________________________
Clemence Lapeyre (acl26), Sanmi Oyenuga (ojo), Gray Williams (gmw9)

Duke University, Spring 2016

CS290-04: 3D Geometry

Final Project: Animating MOCAP Data
_________________________________________________________________

Downloaded Microsoft's Visual Studio 2015

Downloaded SDK browser v2.0
_________________________________________________________________


#MOCAP
What is MOCAP?
	MOCAP is short for ‘motion capture’
It is the process of sampling and recording the motion of humans, animals, and inanimate objects with the ability to play it back
Why MOCAP?
Motion data is an invaluable part of many fields of study including biomechanics, computer vision, and graphics 
It is most popularly known to be used in filmmaking and video game development, where information from MOCAP data of human actors is used to animate digital character models in 2D or 3D 
{Titanic—used extensively for filler characters in between real actors and for shots that would have been impractical or too dangerous for real actors}
Motion capture only digitizes movement so the appearance of the actor(s) is irrelevant. Theoretically, this means that an entire movie can be created using only two or three people!
MOCAP Methodologies
Marker-based: various types of markers or sensors can be fitted on the actor, which then allows movement to be recorded 
Marker-less: no special equipment is necessary; instead, the actors’ movement is recorded in multiple video streams and computer vision algorithms analyze these streams to identify human forms and track isolated parts. Microsoft’s Kinect v2—the device used in our research—employs this technology and can track as many as six (6) complete skeletons with twenty-five (25) joints per person.

#Body Tracking with the Kinect
Previous tracking algorithms required the user to stand in a calibration pose so that body parts could be located and tracked using a simple matching algorithm, based on closest positions between frames. 
Body tracking with the Kinect is better described as body recognition because instead of tracking, it locates body parts based on a local analysis of each pixel, per-frame. This involves two main stages: depth mapping and body position inference.
The Hardware for Depth Mapping
The Kinect is based on PrimeSense technology, which generates depth images at 30 FPS by combining data from multiple sensors:
-	1080p RGB camera
-	Infrared (IR) projector
-	Infrared camera
-	Microphone array
Depth images are generated using a structured light approach. A single beam of infrared light is split into multiple beams by a diffraction grating, creating a constant pattern of speckles. This fixed dot pattern is projected and distorted by the objects present in the scene. An image capture of the distorted pattern is compared to the reference pattern, which is known to the Kinect. The detected disparities are used to compute the distance from each pixel to the sensor. Comparisons between pixel depths are used to build the decision trees that infer body position.
The Software for Body Position Inference
A random subset of 2000 pixels from a depth image is used to calculate features that will help disambiguate between body parts. The Kinect uses the normalized difference in depth from a target pixel to two other pixels. The target pixel is assigned a probability of belonging to a particular body part according to trained classifiers. The Kinect contains a decision forest, which is a collection of decision trees. Each tree was trained on depth images such that they gave the correct classification for a particular body part using 1 million test images.
When probabilities have been assigned to all pixels in the random subset, the algorithm picks out areas of maximum probability for each body part. Joint positions are inferred based on these areas using density estimates. A skeleton can then be drawn based on the joint positions. For example, to render the right arm, a line segment is drawn from the right shoulder coordinates to that of the right elbow; then from the right elbow to the right writs; and finally from the right wrist to the right hand.
Using the BodyBasics app developed for use with the Kinect, we have been successful generating accurate skeletal data.

#Skeletal Animations & Skinning
Skeletal animation is a technique in which a character is represented in two parts: a surface representation (mesh) and a hierarchy of interconnected bones. The skeletal data is used to animate the mesh by associating each bone with a portion of mesh in a process called ‘skinning’. 
The Kinect v2 tracks the following joints (constant name and index in ¬joints array):
    JointType_SpineBase = 0,
    JointType_SpineMid = 1,
    JointType_Neck = 2,
    JointType_Head = 3,
    JointType_ShoulderLeft = 4,
    JointType_ElbowLeft = 5,
    JointType_WristLeft = 6,
    JointType_HandLeft = 7,
    JointType_ShoulderRight = 8,
    JointType_ElbowRight = 9,
    JointType_WristRight = 10,
    JointType_HandRight = 11,
    JointType_HipLeft = 12,
    JointType_KneeLeft = 13,
    JointType_AnkleLeft = 14,
    JointType_FootLeft = 15,
    JointType_HipRight = 16,
    JointType_KneeRight = 17,
    JointType_AnkleRight = 18,
    JointType_FootRight = 19,
    JointType_SpineShoulder = 20,
    JointType_HandTipLeft = 21,
    JointType_ThumbLeft = 22,
    JointType_HandTipRight = 23,
    JointType_ThumbRight = 24,


The position of each joint is determined at every frame and can be retrieved with the following code:

foreach (Joint joint in skeleton.Joints){
Joints[joint].Position.X;
Joints[joint].Position.Y;
Joints[joint].Position.Z;
}

Using this data, we intend to animate human meshes in Blender, an open source 3D animation suite.

#Lemur Tracking
The Kinect is configured to recognize and track the human skeleton. We would like to see if the device can be used to track non-human subjects—in our case, ring-tailed lemurs.
This requires changing not only the reference skeleton but also the learned behavior of the Kinect. Since training for the human skeleton took 1 million test images over 24 hours using a 1000 core cluster, a complete implementation within the time frame of our research is unlikely. However, we have made headway in the form of data collection and skeletal design, which may be integrated in future research. A compilation of clips with depth and infrared data will be made available. 
We believe this would be a good public resource to develop, since the popular understanding of lemurs is predominantly based on the Madagascar movie series, which was animated using clay models and CGI techniques, not MOCAP data: 
http://generator.acmi.net.au/gallery/media/dreamworks-animation-exhibition-madagascar-shape-shifters

#MOCAP for the masses
While there is a lot of interesting motion capture data on the web, viewing this data requires more technical prowess than the average computer user has. We hope to implement an interactive database so that users with limited technical skill can browse and visualize MOCAP data. Ideally, we would also be able to create an interface that captures and displays new data in real-time. For example, a user would upload a stream from their Kinect, and our interface would extract the joint data, feed it to Blender, and output an animated clip.

______________________________________________________________________________

KINECT HOW-TO

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
