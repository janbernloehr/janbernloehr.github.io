---
layout: post
title : "Running ROS on Windows 10"
date : 10.06.2017 10:00:00
tags: [wsl, windows10, ros]
---
{% include JB/setup %}

The [Windows Subsystem for Linux (WSL)](https://msdn.microsoft.com/de-de/commandline/wsl/faq) is a compatibility layer which allows to run a whole bunch of linux binaries natively on Windows 10. With the advent of the Windows 10 Creators Update in March 2017, the WSL was heavily updated and now is able to run ROS lunar and melodic.

In this blogpost I will show you how to install WSL and setup ROS to get started.

## Update Windows 10

A recent version of Windows 10 needs to be installed for ROS to work. If you are in doubt, which version is installed go to Settings -> System -> About and check that you have at least `Version 1703`

![](/assets/images/RosOnWsl/WinVersion.png)

but `Version 1809` is recommended for optimal performance.

If you need to update Windows, got to Windows Update and follow the instructions.

![](https://cnet2.cbsistatic.com/img/PKRh3jY_SwHLSHspsOyOLemB5AE=/2017/03/28/1512839d-f203-4e20-96bf-d6345850923e/windows-10-creators-update-opt-in.png)

## Install the WSL and Bash on Windows

To install the Windows Subsystem for Linux and Bash on Windows, follow [this](https://docs.microsoft.com/en-us/windows/wsl/install-win10) guide.

In case you have used the WSL before applying the creators update, you may still have the trusty version (14.04) or the xenial version (16.04). However, to install ros melodic, you need ubuntu bionic 18.04.
To check which version is actually installed, start an instance of bash and run `lsb_release -a`. The output should look like

```
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.1 LTS
Release:        18.04
Codename:       bionic
```

If it shows an older version, you have to uninstall and then reinstall bash on windows from the windows command line as follows **Warning: This will delete all of your existing data in WSL. Make a backup first**

    lxrun /uninstall /full /y
    lxrun /install

## Install ROS

Since WSL is based on ubuntu, you can follow the official [ros installation guide for ubuntu](http://wiki.ros.org/melodic/Installation/Ubuntu) by the word.


    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
    sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
    sudo apt update
    sudo apt install -y ros-melodic-desktop-full
    sudo rosdep init
    rosdep update

If you want to source ros melodic automatically for ever bash session, then

    echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
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
   
   pub = rospy.Publisher('chatter', String, queue_size=10)
   rospy.init_node('talker', anonymous=True)
   rate = rospy.Rate(10) # 10hz
   while not rospy.is_shutdown():
      hello_str = "hello world %s" % rospy.get_time()
      rospy.loginfo(hello_str)
      pub.publish(hello_str)
      rate.sleep()
   ```

4. Run the publisher `python publish.py`

   ![](/assets/images/RosOnWsl/publish.PNG)

5. Start a third bash prompt
6. Create a new file `subscribe.py` with the contents
   ``` python
   #!/usr/bin/env python
   import rospy
   from std_msgs.msg import String

   def callback(data):
       rospy.loginfo(rospy.get_caller_id() + "I heard %s", data.data)

   def listener():
       rospy.init_node('listener', anonymous=True)
       rospy.Subscriber("chatter", String, callback)
       # spin() simply keeps python from exiting until this node is stopped
       rospy.spin()

   if __name__ == '__main__':
       listener()
   ```  
7. Run the subscriber `python subscribe.py`

   ![](/assets/images/RosOnWsl/subscribe.PNG)

## Install VcXsrv

To run applications with graphical output, you need to install an X Server on Windows. To me, [VcXsrv](https://sourceforge.net/projects/vcxsrv/) now works best.

After you have installed VcXsrv, you also need to configure WSL to use it. To do so modify you .bashrc as follows

    echo "export DISPLAY=:0" >> ~/.bashrc
    source ~/.bashrc

Finally, launch VcXsrv from the start menu. You can keep all default settings except `Native opengl` which needs to be  __unchecked__. Otherwise applications such as rviz do not run as expected.

![](/assets/images/RosOnWsl/NoNativeOpenGl.PNG)

## Run turtle_sim

The popular [turtle_sim tutorial](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics) works fine WSL as well.

1. Make sure you have an X Server installed, configured and running as described above.
2. Start a new bash prompt and run

``` bash
    roslaunch turtle_tf turtle_tf_demo.launch
```

  You can control the turtle by using the arrow keys by going back to the second prompt.

   ![](/assets/images/RosOnWsl/turtle1.PNG)

3. Start a second bash prompt and run

``` bash
    rosrun rviz rviz -d `rospack find turtle_tf`/rviz/turtle_rviz.rviz
```

   ![](/assets/images/RosOnWsl/turtle2.PNG)

## OpenGL indirection

Some places recommend to force indirect rendering using `export LIBGL_ALWAYS_INDIRECT=1` for better OpenGL performance on WSL. However, with the versions of VcXsrv and Xming I tested, it is not possible to launch `rviz` when this indirection is active, thus my recommendation is to not use OpenGL indirection for the time being.
