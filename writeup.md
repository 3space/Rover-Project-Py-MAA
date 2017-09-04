## Project: Search and Sample Return
### Writeup Template: You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

** By: Mark Adan - May 30, 2017 (mark.nada@gmail.com)

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/color_thresh.jpg
[image2]: ./misc/perspective_transform.jpg
[image3]: ./misc/roover_coords.jpg

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
To identify obstacles and rock samples a color threshold was performed on a transformed image recorded during simulation mode.
![alt text][image1]

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 
process_image() modified in the following manner:
    1. source and destination points were taken from a sample calibration image 
    2. a perspective transform was performed using the source, destination and image
![alt text][image2]
    3. a color threshold was used on the transform to identify navigable terrain/obstacles/rock samples
    4. thresholded image pixel values were converted to rover-centric coords
![alt text][image3]
    5. the rover-centric pixel values were converted to world coords
    6. world map was updated using the world coords of obstacles, terrain, and samples
    7. a mosaic image was made using the ground truth map and the warped images from the simulation run


### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.
    The perception_step() function was modified in a similar manner as the process_image() function for simulation mode except telemetry data from the rover was used to populate most of the parameters.

    Another action mode called 'turn_around' was added to decision_step() function.  This mode was effective when the rover was stopped at a wall.  A different called 'reverse'  was then attempted when the rover was stuck on a rock but still had navigable pixels in front of it (i.e. rover would receive of a value of 'throttle = -0.2').  However, this mode did not work well in all situations and needs further testing.  Also, there were times when rover would just drive in circles.  So attempts to stop this behavior made using location, roll, and yaw angles, but this mode was not successful

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  
    On all of my test runs the rover was able to find at least two samples with a fidelity of > 60%.  I would improve on the autonomous rover by improving the color thresholding to better identify obstacles, fine-tuning the reverse mode mentioned above, and prevent the rover from driving in cicrles. 

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  


