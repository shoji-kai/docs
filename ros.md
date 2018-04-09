# ROS

## インストール

CODENAME=$(lsb_release -sc)
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt-get update
sudo apt-get install -y ros-$CODENAME-desktop-full
sudo rosdep init
rosdep update
source /opt/ros/$CODENAME/setup.zsh
echo "source /opt/ros/$CODENAME/setup.zsh" >> ~/.zshrc.local
sudo apt-get install -y python-rosinstall python-rosinstall-generator python-wstool build-essentia

## ワークスペースの作成

mkdir -p ~/catkin_wk/src
cd ~/catkin_wk
catkin_make
source devel/setup.zsh
echo "source ~/catkin_ws/devel/setup.zsh" >> ~/.zshrc.local

## パッケージの作成

cd ~/catkin_ws/src
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
cd ~/catkin_ws
catkin_make

## ファイルシステムの操作コマンド

rospack find <package_name>
roscd <location_name[/subdir]>
rosls <location_name[/subdir]>

## パッケージの依存関係の確認

rospack depends1 beginner_tutorials
rospack depends beginner_tutorials

## パッケージをビルドする

cd ~/catkin_ws
catkin_make

