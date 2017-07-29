---
layout: post
title : "Running ROS on Windows 10"
date : 10.06.2017 10:00:00
tags: [wsl, windows10, ros]
---
{% include JB/setup %}

The [Windows Subsystem for Linux (WSL)](https://msdn.microsoft.com/de-de/commandline/wsl/faq) is a compatibility layer which allows to run a whole bunch of linux binaries natively on Windows 10. With the advent of the Windows 10 Creators Update in March 2017, the WSL was heavily updated and now is able to run ROS lunar. There is just one caveat: In the currently released version (1.13) of [ros_comm](https://github.com/ros/ros_comm) there is a [bug](https://github.com/ros/ros_comm/pull/1050) which needs manual patching until the fix is included in an official release.

In this blogpost I will show you how to install WSL, setup ROS, and how to apply the patch to ros_comm in an overlay workspace to get started.

## Update to Windows 10 Creators update

The Creators update of Windows 10 needs to be installed for ROS to work. If you are in doubt, which version is installed go to Settings -> System -> About and check that you have at least `Version 1703`

![](/assets/images/RosOnWsl/WinVersion.png)

If you need to update Windows, got to Windows Update and follow the instructions.

![](https://cnet2.cbsistatic.com/img/PKRh3jY_SwHLSHspsOyOLemB5AE=/2017/03/28/1512839d-f203-4e20-96bf-d6345850923e/windows-10-creators-update-opt-in.png)

## Install the WSL and Bash on Windows

To install the Windows Subsystem for Linux and Bash on Windows, follow [this](https://msdn.microsoft.com/de-de/commandline/wsl/install_guide) guide.

In case you have used the WSL before applying the creators update, you may still have the trusty version (14.04) of Ubuntu for Windows installed. However, you need to upgrade to xenial (16.04).
To check which version is actually installed, start an instance of bash and run `lsb_release -a`. The output should look like

```
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.2 LTS
Release:        16.04
Codename:       xenial
```

If it shows an older version, you have to uninstall and then reinstall bash on windows from the windows command line as follows **Warning: This will delete all of your existing data in WSL. Make a backup first**

    lxrun /uninstall /full /y
    lxrun /install

## Install ROS

Since WSL is based on ubuntu, you can follow the official [ros installation guide for ubuntu](http://wiki.ros.org/lunar/Installation/Ubuntu) by the word.


    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
    sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
    sudo apt-get update
    sudo apt-get install ros-lunar-desktop-full
    sudo rosdep init
    rosdep update

If you want to source ros lunar automatically for ever bash session, then

    echo "source /opt/ros/lunar/setup.bash" >> ~/.bashrc
    source ~/.bashrc

## Install wstool

To create an overlay workspace, you also need to install the [wstool](http://wiki.ros.org/wstool).

    sudo apt-get install python-wstool

## Create overlay worksape with ros_comm

Now we create an overlay workspace which just includes the patched version of the ros_comm package.

First, create the workspace in some place

    mkdir -p ~/overlay_ws/src
    cd ~/overlay_ws/src

Next initialize the workspace and include ros_comm

    wstool init
    wstool set ros_comm --git git://github.com/ros/ros_comm.git
    wstool update

Build the workspace

    cd ~/overlay_ws
    catkin_make

Finally, update the `.bashrc` file in your home folder to source the overlay workspace

    echo "source ~/overlay_ws/devel/setup.bash" >> ~/.bashrc
    source ~/.bashrc

## Run a simple test

1. Start a new bash prompt and run `roscore`

   ![](/assets/images/RosOnWsl/roscore.PNG)
2. Start a second bash prompt
3. Create a new file `publish.py` with the contents
   ``` python
   #!/usr/bin/env python
   import rospy
   from std_msgs.msg import String
   
   pub = rospy.Publisher('chatter', String, queue_size=None)
   rospy.init_node('demo_pub_node')
   r = rospy.Rate(1) # 10hz
   while not rospy.is_shutdown():
      pub.publish("hello world")
      print('sending data...')
      r.sleep()
   ```

4. Run the publisher `python publish.py`

   ![](/assets/images/RosOnWsl/publish.PNG)

5. Start a third bash prompt
6. Crate a new file `subscribe.py` with the contents
   ``` python
   #!/usr/bin/env python
   import rospy
   from std_msgs.msg import String
   
   def callback(data):
       rospy.loginfo("I heard %s",data.data)
   
   def listener():
       rospy.init_node('demo_sub_node')
       rospy.Subscriber("chatter", String, callback)
       # spin() simply keeps python from exiting until this node is stopped
       rospy.spin()
   
   listener()
   ```  
7. Run the subscriber `python subscribe.py`

   ![](/assets/images/RosOnWsl/subscribe.PNG)

## Install Xming

To run applications with graphical output, you need to install an X Server on Windows. For me, [Xming](http://www.straightrunning.com/XmingNotes/) did a great job.

After you have installed Xming, you also need to configure WSL to use it. To do so modify you .bashrc as follows

    echo "export DISPLAY=:0" >> ~/.bashrc
    source ~/.bashrc

Finally launch the Xming application from the start menu.

## Run turtle_sim

The popular [turtle_sim tutorial](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics) works fine WSL as well.

1. Make sure you have an X Server installed, configured and running as described above.
2. Start a new bash prompt and run `rosrun turtlesim turtlesim_node`.
3. Start a second bash promt and run `rosrun turtlesim turtle_teleop_key`.
   You can control the turtle using the arrow keys

   ![](/assets/images/RosOnWsl/turtle1.PNG)

## OpenGL indirection

Some places recommend to force indirect rendering using `export LIBGL_ALWAYS_INDIRECT=1` for better OpenGL performance on WSL. However, with the version of Xming I tested, it is not possible to launch `rviz` when this indirection is active, thus my recommendation is to not use OpenGL indirection for the time being.#

## D-Bus machine-id missing

On some installations I have seen the error

```
D-Bus library appears to be incorrectly set up; failed to read machine uuid: UUID file '/etc/machine-id' should contain a hex string of length 32, not length 0, with no other text
```

To fix it, run 
```
sudo sh -c 'dbus-uuidgen > /etc/machine-id'
```