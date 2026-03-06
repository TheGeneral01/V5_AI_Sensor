The (Unofficial) Documentation of the Vex V5 AI Vision Sensor for C++:
Specifications:
Native Camera Resolution:
640 x 480
Downscaled to 320 x 240 when detecting objects, likely for backwards compatibility with the previous vision sensor.
Size:
2.495" x 2.125" x 0.89"
Performance:
The AI Vision Sensor is much more susceptible to not detecting targets under abnormal lighting conditions. Performance will vary depending on the color of the object being detected and the color temperature of the environment.
The color temperature of the room has a cubic relationship with the maximum distance an object can be detected.
Functions:
colorDetection(bool bEnable, bool bMerge = false):
Accepts an argument of type bool.
Determines whether the sensor will detect colors.
bMerge functionality is unknown, call with only one boolean argument (ie, colorDetection(true);)
The default state is true.
index():
Gets the current index of the port that the AI Vision sensor is located on.
Returns an integer.
Zero-Based: starts at 0 and goes up.
installed():
Returns a Boolean value of whether the AI Vision sensor is detected or not.
largestObject:
Represents a vex::object of the largest object detected in the last snapshot taken.
modelDetection():
Accepts a Boolean value of true or false, which determines whether the sensor uses AI detection.
The default state is False.
objectCount:
Represents an integer determining the total number of objects, colors, and recognized objects in the last snapshot.
objects:
An array containing the objects found in the last snapshot.
The array has a size of 24, using the vex::safearray function.
reset:
Resets all of the settings, such as modelDetection and colorDetection, back to their default states.
startAwb():
Accepts no input
Starts the automatic white balance process, which takes roughly two seconds.
Helps with detection in different lighting environments to a limited extent.
tagDetection():
Determines whether or not to detect AprilTags.
Default value is False.
takeSnapshot():
Accepts an id, type, and count.
Returns the number of objects detected that match the ID and type.
The ID must be a viable color signature, or 
Setup:
Color Signature Creation:
As of right now, the only way to get the values for the color signatures is to go to https://codev5.vex.com/.
Create a new project by selecting “File”, and select “New Text Project”, then click “C++”.

Now, connect the AI Vision sensor to the computer and select the  button to connect a new device. Choose “AI Vision Sensor”, and select any port.
Then, select  and select , and allow https://codev5.vex.com/ to access serial connections (if needed). Then select the AI Vision Sensor from the page, and allow access to cameras if needed.
Now, place your object in front of the vision sensor, and using the vision utility, select “Freeze Video”, draw a rectangle around the object, and minimize getting another color into it.
Now select “Set Color”, and adjust the Hue and Saturation values as needed. Once done, click “Close”.
Now, expand the header section of the document:

Navigate to roughly lines 29-33, which should look like this:

Line 31, in this example, contains a color signature. You can now copy and paste it and use it in external code.
Sensor Creation:
To set up the sensor, you must first have all of your color signatures defined. Define each of them like so:
vex::aivision::colordesc COLOR1 = aivision::colordesc(1, 193, 48, 71, 15, 0.23);
This is an example of how to define a color signature. “COLOR1” can be changed to any name, and the numbers will vary depending on your object.
Now, you can create your sensor. The color signatures need to be pushed to the sensor upon creation. This can be done like so:
aivision AI_Sensor = aivision(PORT14, COLOR1);
PORT#:
The port that the sensor uses on the Vex V5 Brain.
Colors:
1-7 Color signatures that the AI sensor should detect.
Must be of type aivision::colordesc
The name “AI_Sensor” can be changed to anything needed.
Getting Data:
To get data, we need to get a sensor snapshot. This can be done like so:
sensors.AIVisionSensor.takeSnapshot(COLOR1);
It takes a valid color signature, which you should have set up before.
Now, we can get the object:
targetObject = sensors.AIVisionSensor.objects[0];
And get data from the object using:
targetObject.centerX
Where centerX could be any of the other valid information types.
