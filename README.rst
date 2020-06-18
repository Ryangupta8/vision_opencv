vision_opencv
=============

.. image:: https://travis-ci.org/ros-perception/vision_opencv.svg?branch=indigo
    :target: https://travis-ci.org/ros-perception/vision_opencv

Packages for interfacing ROS with OpenCV, a library of programming functions for real time computer vision.


INSTRUCTIONS FOR MAKING THIS WORK----------------------------------------
1) install Ubuntu 18.04
2) install ROS melodic: http://wiki.ros.org/melodic/Installation/Ubuntu
	a) make sure to do rosdep (at the end of the page)
3) check to make sure you have python (version 2.7.x) python3 (version 3.6.x) and python3.7 (version3.7.x)
	a) to check, simply use python --version or python3 --version or python3.7 --version
4) install anaconda using python 3.7.x: https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart
	a) run: conda create -n py36 python=3.6
	b) run: gedit ~/.bashrc
		i) scroll to the bottom and ensure the following code block either does not exist or is commented out (commented out means that there are #'s in front of each line)
			-) the code block:
				# # >>> conda initialize >>>
				# # !! Contents within this block are managed by 'conda init' !!
				# __conda_setup="$('/home/ryan/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
				# if [ $? -eq 0 ]; then
				#     eval "$__conda_setup"
				# else
				#     if [ -f "/home/ryan/anaconda3/etc/profile.d/conda.sh" ]; then
				#         . "/home/ryan/anaconda3/etc/profile.d/conda.sh"
				#     else
				#         export PATH="/home/ryan/anaconda3/bin:$PATH"
				#     fi
				# fi
				# unset __conda_setup
				# # <<< conda initialize <<<
		ii) now, above or below that, add the two lines to the .bashrc:
				alias condaenv="export PATH=/home/YOURUSERNAME/anaconda3/bin:$PATH"
					for ex: mine looks like: alias condaenv="export PATH=/home/ryan/anaconda3/bin:$PATH"
				alias py36="source activate py36"
		iii) save your .bashrc
		iv) conda deactivate
	c) in terminal, run: source ~/.bashrc
	d) in terminal, run: condaenv
	e) in terminal, run: py36      you should now be in a conda environment, it should be obvious
5) Follow the steps here https://medium.com/@zuxinl/ubuntu-18-04-ros-python3-anaconda-cuda-environment-configuration-cb8c8e42c68d
	a) Ensuring you are in the anaconda py36 environment, run:
		i) sudo apt-get install python3-pip python3-yaml
		ii) pip install rospkg catkin_pkg
		iii) sudo apt-get install python-catkin-tools python3-dev python3-numpy
		iv) conda install -c conda-forge opencv
	b) Now test to make sure it works by entering into the terminal:
		i) python
		ii) >>> import rospy
		iii) if it goes without error, type: exit()
	c) Now we need to make sure versions are correct
		i) in the terminal (still in py36 env), type: cmake --version
			-) this should be 3.18.0-rc1 or 3.18.0-rc2
			-) if it reads something else, it needs to be updated
			-) instructions to do so are found here: https://askubuntu.com/questions/829310/how-to-upgrade-cmake-in-ubuntu
				-) DO NOT DO STEP 2
				-) note when downloading, you want to download the binary file for the platform: Linux x86_64
		ii) in the terminal(still in py36 env), type: python
			-) at the prompt, type: 
				import cv2
				cv2.__version__
			-) if the output is not 4.2.0, we will need to upgrade it following the instructions at: https://github.com/conda-forge/opencv-feedstock/issues/93
	d) now, you are going to create a ROS workspace and build cv_bridge:
		i) mkdir ~/name_of_your_ws
		ii) cd ~/name_of_your_ws
		iii) catkin config -DPYTHON_EXECUTABLE=/home/YOURUSERNAME/anaconda3/envs/py36/bin/python3 -DPYTHON_INCLUDE_DIR=/home/YOURUSERNAME/anaconda3/envs/py36/include/python3.6m -DPYTHON_LIBRARY=/home/YOURUSERNAME/anaconda3/envs/py36/lib/libpython3.6m.so -DSETUPTOOLS_DEB_LAYOUT=OFF
		iv) mkdir src
		v) cd src
		vi) git clone -b melodic https://github.com/ros-perception/vision_opencv.git
		vii) cd ..
		viii) catkin build cv_bridge
	e) now we need to setup a test to see if this is working
		i) go to the following link and download a dataset. I chose 2011-08-08-14-18-37.bag
		ii) Wait for the download to finish, it takes a while. Then save the download as ~/name_of_your_ws/src/vision_opencv/opencv_tests/rosbag/2011-08-08-14-18-37.bag
	f) now we need to run the test
		i) in one terminal, make sure your workspace is sourced.
			-) cd ~/name_of_your_ws
			-) source /opt/ros/melodic/setup.bash 
			-) roscore 
		ii) in another terminal,
			-) cd ~/name_of_your_ws
			-) source /opt/ros/melodic/setup.bash
			-) cd ~/name_of_your_ws/src/vision_opencv/opencv_tests/rosbag/
			-) rosbag play 2011-08-08-14-18-37.bag
		iii) finally in another terminal: 
			-) condaenv
			-) py36
			-) cd ~/name_of_your_ws
			-) source devel/setup.bash
			-) cd src/src/vision_opencv/opencv_tests/nodes
			-) python test.py
