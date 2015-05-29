---
layout: post
title:  "Installing KnowRob on ROS (Debian)"
date:   2015-05-29 13:36:25
categories: KnowRob
---

Install ROS from [here][ROSinstall], i.e.:

{% highlight bash %}
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install ros-indigo-desktop-full
sudo rosdep init
rosdep update
{% endhighlight %}
but do not set it to your `.bashrc` simply `source` it for now
{% highlight bash %}
source /opt/ros/indigo/setup.bash
{% endhighlight %}
and finally install
{% highlight bash %}
sudo apt-get install python-rosinstall
{% endhighlight %}

The next step is to install `rosjava` in separate *workspace* according to [wiki][rosjava]:
{% highlight bash %}
sudo apt-get install ros-indigo-catkin ros-indigo-rospack python-wstool
mkdir -p ~/workspace
wstool init -j4 ~/workspace/src https://raw.githubusercontent.com/rosjava/rosjava/indigo/rosjava.rosinstall
cd ~/workspace
rosdep update
rosdep install --from-paths src -i -y
catkin_make
{% endhighlight %}

Now install the `catkin` with *my custom packages* (inspired by [this][catkin]):
{% highlight bash %}
source ~/workspace/devel/setup.bash
mkdir -p ~/workspace_devel/src
rosdep update
cd ~/workspace_devel/src
catkin_init_workspace
cd ~/workspace_devel/
catkin_make

cd ~/workspace_devel/src
wstool init
wstool merge https://raw.githubusercontent.com/So-Cool/ReportCardGenerator/gh-pages/docs/custom.rosinstall
wstool update

rosdep install --ignore-src --from-paths ./knowrob

cd ~/workspace_devel
source ~/workspace_devel/devel/setup.bash
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
catkin_make
{% endhighlight %}

Finally, append these to the `.bashrc` (remember to verify the paths including `<userName>`):
{% highlight bash %}
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
source /home/<userName>/workspace_devel/devel/setup.bash
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":/usr/lib/jvm/java-7-openjdk-amd64/jre/lib/amd64/
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":/usr/lib/jvm/java-7-openjdk-amd64/jre/lib/amd64/server/

alias rswipl='rosrun rosprolog rosprolog knowrob_cram'

alias rcswipl='rosrun rosprolog rosprolog report_card'
alias mim1='mongoimport --db roslog --collection tf tf.json'
alias mim2='mongoimport --db roslog --collection logged_designators logged_designators.json'
alias makeMe='catkin_make -DCATKIN_WHITELIST_PACKAGES="report_card"'
{% endhighlight %}

Now you're ready to go.
{% highlight bash %}
source ~/.bashrc

cd ~/workspace_devel
mkdir data
cd data
wget http://www.robots-doing-things.com/incoming/log-packaged-2014-12-30-16-10-37.tar.gz
mkdir log1
tar -zxvf log-packaged-2014-12-30-16-10-37.tar.gz -C ./log1
{% endhighlight %}

and
{% highlight bash %}
cd ~/workspace_devel/data/log1
mim1
mim2
rcswipl
{% endhighlight %}

and load data in `Prolog`

{% highlight Prolog %}
?- load_experiment('/home/<userName>/workspace_devel/data/log1/cram_log.owl').
{% endhighlight %}

done.

[ROSinstall]: http://wiki.ros.org/indigo/Installation/Ubuntu
[rosjava]: http://wiki.ros.org/rosjava/Tutorials/indigo/Source%20Installation
[catkin]: http://knowrob.org/installation/catkin
