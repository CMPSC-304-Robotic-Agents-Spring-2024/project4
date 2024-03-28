# Project 4: Robot Navigation with Mapping

## Timeline

Activity                                                                                               | Deadline
------------------------------------------------------------------------------------------------------ | -------------------------------------------
Complete Steps 1 and 2 part of the report (At least 1 commit):               | by the end of class on November 9th, 2022
Complete Step 3 part of the report (At least 1 commit):               | by the end of lab on November 9th, 2022
Final Submission and Live or Video Demonstration:                                                                             | April 9th, 2024 by 2:30pm

## Class Community Guidelines

Throughout the completion of this project you must adhere to the [course guidelines](https://github.com/CMPSC-304-Robotic-Agents-Spring-2024/course_information) that we developed as a class. To report any violations of the code of conduct, please submit an [anonymous form](https://forms.gle/tePfnLY12hyN1Xbd6). Students who think that the class should revise some aspect of the guidelines must use the GitHub issue tracker for that repository to suggest, discuss, and implement any required changes.

By working on and completing this assignment you agree to use the hardware given to you in a responsible manner. Each team is responsible for the safety and security of the robot while it is in your possession. You are not allowed to take the robots used in this project outside of ALIC (campus center).

## Summary

This project assignment invites you to work in a team to gain a deeper understanding of ROS 2 while working with a Turtlebot4 robot and implement an autonomous mapping technique. In this project the class members will be split into two categories: 1) those working with Turtlebot 4 Lite, and 2) those working with Turtlebot 4 Standard. In both cases the tasks to be completed are the same but in some instances specific steps to take to complete them may vary (e.g., `teleop` step). Specifically, after completing initial tutorials to get to know Turtlebot 4 robot better, each team will create autonomous mapping for a navigation task of your choice. Finally, teams will reflect on their experiences in writing in the file `writing/report.md` and demonstrate their developed applications in a video. `writing/report.md` is a Markdown file that must adhere to the standards described in the [Markdown Syntax Guide](https://guides.github.com/features/mastering-markdown/).

## Turtlebot 4

You can learn about the hardware specifications of this robot in the [turtlebot4-hardware GitHub repository](https://github.com/turtlebot/turtlebot4-hardware). Please also review the features of this robot on [its website](https://turtlebot.github.io/turtlebot4-user-manual/overview/features.html).

### Getting Started

Get the turtlebot and the dock out of its box. Plug in the dock and place the turtlebot on the dock so that the two metal pieces on the bottom of the robot are connected to the two metal pieces on the charger. The robot should turn on with a flashing white light around the center power button. Once you hear two chimes (the second one will sound a little bit after the first) you are ready for the next steps!

While you are waiting for the robot to start up, get the router out and plug it in. Find a linux laptop for the turtlebot and connect the laptop to the router (name and password are on the router). Turtlebot should automatically connect to the router. Turtlebot should automatically connect to the router. If you think it hasn't done so,  you can press the two buttons on either side of the turtlebot's power button and hold them at the same time. The ring around the power button will change to blue and the robot will beep, which means that it is ready to connect to a network. You will connect to the turtlebot which will have a network name of "CXXX-XXXX"
    
- Once the turtlebot is connected to your laptop, you can go to `http://192.168.10.1/`.
    
- From here, you will connect the turtlebot to the network indicated on the router.
    
- The turtlebot will beep once it is connected.

Before running any commands on a laptop, you need to:

1. Set up the source: `source /opt/ros/humble/setup.bash`
2. Connect to the right robot by running the following command (`ROS_DOMAIN_ID` is noted on the robot): 

`export ROS_DOMAIN_ID={ROS_DOMAIN_ID}` 

Example:
`export ROS_DOMAIN_ID=1`

If you have any trouble communicating with your turtlebot, please consult [Basic Set Up Documentation](https://turtlebot.github.io/turtlebot4-user-manual/setup/basic.html)

## Instructions

The following steps can be completed somewhat simultaneously. Identify a team lead for each step and report the team work distribution that in the `report.md`. 

### Step 1: Getting to know Turtlebot 4

Read [Turtlebot 4 Software Documentation](https://turtlebot.github.io/turtlebot4-user-manual/software/) and complete or verify the necessary installations.

### Step 2: Teleop

Get connected and drive the robot using remote control, `teleop`. Please follow the steps below. 

Now, either follow instructions on [Turtlebot Lite Section](#turtlebot-lite) or [Turtlebot Standard Section](#turtlebot-standard), depending on the robot your team has.

#### Turtlebot Lite

Follow instructions on [Driving your Turtlebot 4](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/driving.html), which are also summarized below.

Please note that Turtlebot 4 Lite does not include Bluetooth Controller. So, in order to move your robot using remote control, you are going to use a keyboard application on your PC! `teleop` package should already be installed on turtlebot laptops, if not, run the following commands:

```
sudo apt update
sudo apt install ros-humble-teleop-twist-keyboard
```

Before doing the next part, you need to connect to the right robot by running the following command (`ROS_DOMAIN_ID` is noted on the robot): 

`export ROS_DOMAIN_ID={ROS_DOMAIN_ID}` 

Example:
`export ROS_DOMAIN_ID=1`

Now you should be able to set up your `teleop` keyboard and drive the turtlebot as follows:

```
source /opt/ros/humble/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

If this does not work, try the following command instead: `ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r __ns:=/tb2`

When you do this, you should see the following messages:

```
This node takes keypresses from the keyboard and publishes them
as Twist messages. It works best with a US keyboard layout.
---------------------------
Moving around:
   u    i    o
   j    k    l
   m    ,    .

For Holonomic mode (strafing), hold down the shift key:
---------------------------
   U    I    O
   J    K    L
   M    <    >

t : up (+z)
b : down (-z)

anything else : stop

q/z : increase/decrease max speeds by 10%
w/x : increase/decrease only linear speed by 10%
e/c : increase/decrease only angular speed by 10%

CTRL-C to quit

currently:	speed 0.5	turn 1.0 
```

As you can see above you use the following commands to move around:

```
U = Forward Left
I = Straight Forward
O = Forward Right
J = Rotate Left
K = Center # This doesn't really do anything
L = Rotate Right
M = Left Backwards
, = Straight Backwards
. = Right Backwards
```

#### Turtlebot Standard

Turtlebot Standard comes with a Bluetooth Controller. To connect your controller to the robot, press the home button on the controller and check that the controller's light turns blue. 

To drive the robot with a controller, press and hold either `L1` or `R1`, and move the left joystick. By default, `L1` will drive the robot at "normal" speeds, and `R1` will drive the robot at "turbo" speeds. The buttons can be changed in the configuration file as described in the [Joystick Teleoperation Section](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/driving.html).

### Step 3: Creating a Node in Turtlebot 4

Complete the [Create First Node in Python Tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/first_node_python.html). Take a video demonstrating the functionality of the lightring that showcases both the lightring on the Turtlebot 4 and also the output produced in the terminal. Upload the video to the [Google Form](https://forms.gle/oxvWEMEW1UFPNpD68)

### Step 4: Building a Map and Navigation

Once the robot is up and running, you need to have it navigate in the environment. You need to come up with a navigation task and have your robot build a map of a relatively complex environment and have it navigate **autonomously** for a specific purpose or application. You can follow [Generating a map tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/generate_map.html) to create your map. Then follow [Navigation tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/navigation.html) or watch [TurtleBot 4 | Mapping & Navigation with ROS 2 Navigation Stack Video](https://www.youtube.com/watch?v=T3if0aPj0Eo) to learn how to have the Turtlebot build a map and navigate in the environment given the map.

#### Map Building

Now that you have your teleop keyboard working, you should be able to run the slam, rviz, and teleop keyboard commands in separate terminals. **SLAM** (Simultaneous localization and mapping) is a method for creating a map of the area around the robot and keeping track of the robot position in that map. The TurtleBot 4 uses `slam_toolbox` by combing the data from Create3 and laser scans from `RPLIDAR`. We use the asynchronous SLAM when generating a map for the robot because it uses less processing power, but the map generated cannot be accurate as the synchronous SLAM method. **Rviz2** is a port of Rviz2 to ROS2. Rviz2 gives us a graphical interface to view the robot, the map and the sensor data while it is running to generate the map.  The needed packages should be installed on the turtlebot laptops. If not,  you can install them from the [turtlebot4 packages](https://turtlebot.github.io/turtlebot4-user-manual/software/turtlebot4_packages.html). You do not need the robot package, however, you do need the Desktop and Simulator packages which will allow you to run the rviz and slam packages.

- To start, run this command in a new terminal: `source /opt/ros/galactic/setup.bash` then run in the same terminal `ros2 launch turtlebot4_navigation slam_sync.launch.py`
    
- Next run this command in a NEW TERMINAL: `source /opt/ros/galactic/setup.bash` then run, in the same terminal, this command: `ros2 launch turtlebot4_viz view_robot.launch.py`
    
- Finally, run a fresh `teleop` keyboard in a NEW TERMINAL. Note: re-run the teleop in a new terminal even if you already have one running, this is because it will sometimes not connect with the new programs you have running if you use something that was already running.

You should now be able to drive your turtlebot and have it generate a map on your computer.

In order to save the map, run this command: `ros2 service call /slam_toolbox/save_map slam_toolbox/srv/SaveMap "name: data: 'map_name'"`

#### Navigation

After you have saved the map of your chosen environment, you can complete the selected navigation task for your application/task by completing the [Navigation tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/navigation.html). Upload the video of your full navigation task to the [Google Form](https://forms.gle/oxvWEMEW1UFPNpD68)

## Other Resources:

- [TurtleBot 4 | Unboxing & Getting Started Video](https://www.youtube.com/watch?v=QN01AXjoLdQ&t=0s).
- [Driving your Turtlebot 4 Tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/driving.html).
- [Turtlebot 4 Software information](https://turtlebot.github.io/turtlebot4-user-manual/software/) to learn about Turtlebot 4 packages, sensors, using `rviz2`, using SLAM technique to create maps, using navigation stack in ROS 2, and setting up Turtlebot 4 simulator.
- Questions and issue logging support can be found at [Turtlebot 4 GitHub Issues](https://github.com/turtlebot/turtlebot4/issues) and [Turtlebot 4 Simulator GitHub Issues](https://github.com/turtlebot/turtlebot4_simulator/issues).

## Assessment

The grade that a student receives on this assignment will have the following components.

- **GitHub Actions CI Build Status [up to 5%]:**: For lab4 repository associated with this assignment students will receive a checkmark grade if their last before-the-deadline build passes.

- **Mastery of Verbal Explanation during the Demonstration [up to 15%]:**: Since the ability to communicate technical details of a project is crucial to building successful software and hardware applications, a portion of students' lab grade will be determined based on the quality of the project demonstration.

- **Mastery of Technical Writing [up to 25%]:**: Students will also receive a part of their grade when the responses to the writing prompts presented in the `report.md` reveal a proficiency of both writing skills and technical knowledge. To receive full points of this component, the submitted writing should have correct answers, required screenshots, and correct spelling, grammar, and punctuation in addition to following the rules of Markdown and providing complete and conceptually and technically accurate answers.

  - Please note that the "Check Spelling" GitHub Actions check may flag proper nouns or other real words if the dictionary it uses does not contain them. If your "Check Spelling" GitHub Actions check is failing due to a correctly spelled word being incorrectly flagged as "unknown" by CSpell, you will need to add the word to the list of words in `.github/cspell.json`.

- **Mastery of Technical Knowledge and Skills [up to 55%]**: Students will receive a portion of their assignment grade when their project design and implementation reveals that they have mastered all of the technical knowledge and skills developed during the completion of this project. Any written programs must be inside `src/` directory. As a part of this grade, the instructor will assess aspects of the project including, but not limited to, the appropriate design of the robot task, the completeness and correctness of the implemented software, effectiveness of experiments, and the use of effective source code comments and Git commit messages.

- **Continuous Progress [up to 40% deducted points]**: To ensure equal team effort and timely troubleshooting, students may lose up to 40% of points from their final deliverable for not demonstrating continuous team effort on this project. Each activity not submitted by the stated deadline in the [Timeline](#timeline) section by ALL team members will result in -10% unless the effected team member or the whole team (if the entire team was effected) can demonstrate circumstances beyond their control (e.g., illness, hardware challenges unsolvable in time, etc.).

All grades for this project will be reported through a student's gradebook GitHub repository.

### GatorGrade

You can check the baseline writing and commit requirements of this project by running department's assignment checking `gatorgrade` tool To use `gatorgrade`, you first need to make sure you have Python installed. If not, please see:

- [Setting Up Python on Windows](https://realpython.com/lessons/python-windows-setup/)
- [Python 3 Installation and Setup Guide](https://realpython.com/installing-python/)
- [How to Install Python 3 and Set Up a Local Programming Environment on Windows 10](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-windows-10)

Then, you need to install `gatorgrade`:

- First, [install `pipx`](https://pypa.github.io/pipx/installation/)
- Then, install `gatorgrade` with `pipx install gatorgrade`

Finally, you can run `gatorgrade`:

`gatorgrade --config config/gatorgrade.yml`

## Assistance

If you are having trouble completing any part of this project, then please talk with the course instructor or a technical leader during the class or laboratory session. Alternatively, you may ask questions in the Discord channel for this course. Finally, you can schedule a meeting during the course instructor's office hours.
