# Example of using Blender + fSpy to create augmentations of image data

## Installation
Blender 3.1.2 is required to run the demo. To create augmentations (add potholes) on your own photo, you will also need fSpy (https://fspy.io/) and the Blender addon (https://github.com/stuffmatic/fSpy-Blender) - Both are free and open source.

## Understanding the project file
![image](https://user-images.githubusercontent.com/46105170/174445734-3b6f3e53-95c0-433a-bfd5-00a260a9b7d8.png)
The logic is split between the individual Blender editors - in the Outliner (top right) we can see there are only three objects:
* perspective.fspy - contains the guessed parameters of camera (perspective + FOV) from fSpy, replace this with your own .fspy file you can create like this: https://www.youtube.com/watch?v=rT8-f4cP-eQ
* Pothole_mesh - the actual mesh of the pothole, essentially a polygon grid with vertex displacement done using geometry nodes, which are visible in the bottom middle - Geometry node tree "Pothole". Inside there you can tweak the appearance of the pothole - depth, spread, noise scale etc.
* Pothole_position - an empty object that locally distorts Pothole_mesh - animate it's location to render out a sequence of images with the pothole in different locations. 

## Switching between rendering out ground truth and segmentation
To switch between these two modes of rendering, we need to do two things: 
* switch the "Label" attribute in the modifier tab - 0 is rgb rendering, 1 is segmentation rendering, where the color assigned to the pothole is #FF0000 (can be changed in Materials > Label). 

![image](https://user-images.githubusercontent.com/46105170/174446330-55b5989c-380e-4e77-b891-3f94de442fb7.png)
* change the connection in the compositor tab from alpha over an image, to alpha over pure black.

![blender_fjgivAQXtd](https://user-images.githubusercontent.com/46105170/174446532-b331e8d5-cb9d-40ed-8693-2ed08036aab9.gif)

## Using your own image
To use your own image, run it through fSpy (tutorial - https://www.youtube.com/watch?v=rT8-f4cP-eQ) and import the resulting .fspy file into the project, replacing "perspective.fspy". **You also need** to change the background image in the compositor tab. Same as with rgb/label switching, this is unfortunately splitted into two actions instead of one, it is probably possible to merge these two actions into one for better usability, but sharing information between the different editors proves difficult.

## Ideas for improvement
Speed up generation of the label - it is unnecessarily rendering rgb and then painting over it - can be for sure done multiple orders of magnitude faster. Label could be created in compositor only, eliminating the need for switching between rendering rgb and label, generating both at the same time.
More types of pothole could be added - alligator cracks and long cracks are under development right now, they can be integrated seamlessly into the existing Geometry node setup, creating a much more general pothole generator.
![image](https://user-images.githubusercontent.com/46105170/174447210-763dd959-c6d8-41c9-9497-00cd8bec5a33.png)


