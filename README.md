# Object-Detection-based-on-color

## 💫 About Developer:

### Banish J

🤖 AI & ML Developer | Data Science & Analytics Expert<br>🌐 Web Apps & IoT Skilled | Awesome UI Creator<br>💻 AI is my main focus! 👾

## 📞 Contact
##### **☎️**   [9444333914](tel:9444333914)
##### **📧**  mail@banish.in
##### **🌐**  [Banish](https://www.banish.in)

## Core Concept

***This code captures video from the webcam, detects a specified color range in the HSV color space, tracks the largest object of that color, and draws a circle around it. Based on the object's position and size, it prints directional commands. The video feed is displayed in a window, and the program can be terminated by pressing the 'q' key.***



Importing Necessary Libraries
python

import imutils
import cv2
imutils: A convenience library for basic image processing functions, making it easier to work with OpenCV.
cv2: The OpenCV library for computer vision tasks.
Defining Color Ranges
The code snippet defines several sets of color ranges for different objects using HSV color space values. The active color range is set for detecting a light source:

python

#for Light source
redLower = (0,0,255)       
redUpper = (0,0,255)
redLower and redUpper: Define the lower and upper bounds of the HSV color range for the target object.
Initializing the Webcam
python

camera = cv2.VideoCapture(0)
cv2.VideoCapture(0): Initializes the webcam for video capture. The argument 0 typically refers to the default webcam on the computer.
Real-Time Object Detection and Tracking Loop
python

while True:
    (grabbed, frame) = camera.read()
camera.read(): Captures a frame from the webcam. grabbed is a boolean indicating if the frame was successfully read, and frame stores the captured image.
Resizing and Preprocessing the Frame
python

    frame = imutils.resize(frame, width = 1000)
    blurred = cv2.GaussianBlur(frame, (11, 11), 0)
    hsv = cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV)
imutils.resize(frame, width = 1000): Resizes the frame to a width of 1000 pixels while maintaining the aspect ratio.
cv2.GaussianBlur(frame, (11, 11), 0): Applies a Gaussian blur to the frame to reduce noise and smooth the image.
cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV): Converts the blurred frame from BGR to HSV color space.
Creating a Mask Based on Color Range
python

    mask = cv2.inRange(hsv, redLower, redUpper)
    mask = cv2.erode(mask, None, iterations = 2)
    mask = cv2.dilate(mask, None, iterations = 2)
cv2.inRange(hsv, redLower, redUpper): Creates a binary mask where the pixels within the specified HSV color range are white, and others are black.
cv2.erode(mask, None, iterations = 2): Performs erosion to remove small white noise from the mask.
cv2.dilate(mask, None, iterations = 2): Performs dilation to restore the eroded parts of the mask.
Finding Contours and Detecting the Largest Object
python

    cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)[-2]
    center = None

    if len(cnts) > 0:
        c = max(cnts, key = cv2.contourArea)
        ((x, y), radius) = cv2.minEnclosingCircle(c)
        M = cv2.moments(c)
        center = (int(M["m10"] / M["m00"]), int(M["m10"] / M["m00"]))
cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE): Finds contours in the binary mask.
max(cnts, key = cv2.contourArea): Finds the largest contour by area.
cv2.minEnclosingCircle(c): Finds the minimum enclosing circle around the largest contour.
cv2.moments(c): Computes moments of the largest contour to find its centroid.
center = (int(M["m10"] / M["m00"]), int(M["m10"] / M["m00"])): Calculates the centroid coordinates of the contour.
Drawing and Tracking the Object
python

        if radius > 10:
            cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), -1)
            print(center, radius)
            if radius > 250:
                print("stop")
            else:
                if center[0] < 150:
                    print("Right")
                elif center[0] > 450:
                    print("Left")
                elif radius < 250:
                    print("Front")
                else:
                    print("Stop")
.

	cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), -1): 
Draws a filled circle around the detected object.
Prints the center coordinates and radius of the detected object.
Depending on the radius and position of the detected object, prints different commands ("Right", "Left", "Front", "Stop").
Displaying the Frame and Handling Key Press
python

    cv2.imshow("Frame", frame)
    key = cv2.waitKey(1)
    if key == ord("q"):
        break
.  
	
	cv2.imshow("Frame", frame): 
Displays the processed frame in a window titled 	"Frame".

	cv2.waitKey(1): 
Waits for 1 millisecond for a key press.

	if key == ord("q"): 
Checks if the 'q' key is pressed to break the loop and stop the program.
Releasing Resources
python

	camera.release()
	cv2.destroyAllWindows()
	camera.release(): Releases the webcam resource.
	cv2.destroyAllWindows(): 
Closes all OpenCV windows.
