# thermal-and-visible-images-fusion
This is a student project for a module on face detection and temperature calculation.

Objective: To capture thermal and visual images of participants, fuse them, and figure out the facial temperature for further implementations (thermal data published onto a server, then subscribed into the docker of a robot for a diagnosis of a symptom of COVID-19) 

In this file will you find prerequisites on hardware, middleware, and software installation along with their purposes; wiring instructions; explanations on how the module works; some limitations, and corresponding approaches.

Let's begin!
<img src="./figures/intro.png" width="960" height="540" />

## 1. Requirements:

### a/ Hardware
- MLX90640 module (IR Array Thermal Imaging Camera): To capture thermal image of participants
<img src="./figures/hardwares/mlx90640.png" width="316" height="194" />

- Raspberry Pi Camera Module: To capture visible images of participants
<img src="./figures/hardwares/picam.png" width="335" height="284" />

- Raspberry Pi 3 Model B: To collect sensor data
<img src="./figures/hardwares/pi3.png" width="399" height="265" />

### b/ Middleware
Install Raspberry Pi OS or Ubuntu 20.04 

### c/ Software
Lists of lib used and their main effect on the project:

* OpenCV: https://opencv.org/
* Mediapiipe: https://google.github.io/mediapipe/
* Numpy: https://numpy.org/
* Scipy: https://scipy.org/
* Adafruit_MLX90640: https://github.com/adafruit/Adafruit_MLX90640.git


## 2. Instructions:

- Wiring diagram table (for MLX90640)
<img src="./figures/wiring/mlx90640%20diagram.png" width="575" height="370" />

- Hardware mounting
<img src="./figures/wiring/hardware%20mounting.png" width="452" height="437" />

## 3. Operations:

[>>> Code <<<](./code/thermal_visible_fusion.py)

- The face detection module is developed based on the "mediapipe" package, and it provides a highly precise result for detected faces with
the rate of detection can be set over 90%.

- The thermal sensor MLX90640 is connected to the Raspberry Pi via I2C. Then, its thermal data array is collected in real-time and fused
with the RGB image captured from the Picam.

- Finally, the algorithm is to consider all thermal data in the bounding box of the detected face, which is in this case I have narrowed down this region to only the forehead of the detected face (~1/3.3 top height of the bounding box), then taking the maximum value of this data
array in this forehead region.

- The result shows a relatively accurate facial temperature with an error of approximately only ± 0.1 to 0.2 °C (provided that the optimal nominal distance for participants: 40-50cm away from the cameras).


## 4. Limitations:

   a) Low fps rate when processing face detection function on RGB image and collecting thermal data from MLX90640 sensor simultaneously due to low performance of the processor Raspberry Pi 3B.

   b) MLX90640 sensor cannot capture accurate temperature when the face is covered by objectives, i.e. glasses, face mask.


## 5. Solution:

   a) Select a specific frame to display instead of real-time displaying (Capture the 11th frame after 10 captured frames). Then, the lagging between the displayed image and real-time motion reduces down to only ~4-5 seconds.

   b) Extend the bounding box contour and capture only the max value of the forehead region [~1/3.3 top height of the box] as stated above.


## 6. Proof of concept:
[>>> Click here <<<](./figures/proof_of_concepts/README.md)

