# RoboSub CV Samples
Example CV implementations for the RoboSub competition, using classical CV methods. Why classical CV methods instead of Deep Learning approaches? Because the aquatic environment has a *ton* of noise! Also, it is much easier/faster to tune parameters with classical CV methods than Deep Learning training. Classical CV works by running a series of transformations on the image.

## Gate Task

![](gate.png?raw=true)

The Gate task features a PVC portal with bright orange sides. There are two images hanging from the top, the G-Man (police) and the Gangster (robber). You need to go through the side of the gate corresponding to the team you want to play on (police or robbers).

Since the water gives everything a blue/green tinge, trying to recognize the images will be difficult and unreliable. However, the orange sides will stand out. Try to filter out everything except the orange colors (or the orange-ish color that Leviathan sees in the water), and use contour detection to get the two sides.

I used the process:
1. Convert from BGR (or RGB) to HSV (cv2.cvtColor)
2. Filter out everything except orange/yellow (cv2.inRange)
3. Erode to remove noise (cv2.erode)
4. Find contours (cv2.findContours)

Resulting in the following:
![](gate_demo.png?raw=true)


## Buoy Task

![](buoy.png?raw=true)

The Buoy task features two images hanging in the water, a badge (police) and a tommy gun (robber). You need to hit the buoy corresponding to the team you are playing on (police or robbers).

Trying to do Deep Learning (feature recognition) will be difficult since neither the tommy gun nor the badge will be well defined at a distance. Feature recognition works best when the different image classes have distinct shapes, so for this task it will be unreliable. The most notable feature of the buoys is the dark circle in the center of the badge. I would approach this by choosing police and trying to pick up that circle. To do this, filter out everything except the dark colors, and use circle detection to get the badge circle.

I used the process:
1. Convert from BGR (or RGB) to HSV (cv2.cvtColor)
2. Filter out everything except dark colors (cv2.inRange)
3. Dilate to fill in the circle gaps (cv2.dilate)
4. Erode to remove noise (cv2.erode)
5. Blur to smooth out the edges, to help circle detection (cv2.blur)
6. Find circles (cv2.HoughCircles)

Resulting in the following:
![](buoy_demo.png?raw=true)

Naturally the real-world images will be look a bit different than these screenshots: dark water background, less image contrast, etc. I would still use the simple approach for the gate, however for the buoy I would pre-process the image: filter in only the light contours (since the images have a white background, this would require some dilation/erosion/contours), turn everything outside those regions white (so the buoys are the raw images but the water space around them is all white), and then locate the badge's circle.