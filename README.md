# Dowload-PX4

#Foi utilizado o Ros Noetic no Ubuntu 20.04, junto com Gazebo 11 (já vem automaticamente quando se instala o ros versão full desktop)#
-----------------------------------------------------------------------------------------------------------------------------------------------
#Git clone PX4-Autopilot

  $ git clone https://github.com/PX4/PX4-Autopilot.git --recursive
  
  $ bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
  
-reiniciar o computador após o processo

------------------------------------------------------------------------------------------------------------------------------------------------
#Instalação MAVROS

    $ mkdir -p ~/catkin_ws/src
    
    $ cd ~/catkin_w
    
    $ catkin init 
    
    $ wstool init src
    
-----------------------------------------------------------------------------------------------------------------------------------------------
#Em catkin_ws/src/  
  
   $ sudo apt-get install python-catkin-tools python-rosinstall-generator -y
  
   $ wstool init ~/catkin_ws/src
  
   $ rosinstall_generator --rosdistro kinetic mavlink | tee /tmp/mavros.rosinstall
  
   $ rosinstall_generator --upstream mavros | tee -a /tmp/mavros.rosinstall
  
   $ wstool merge -t src /tmp/mavros.rosinstall
  
   $ wstool update -t src -j4
  
   $ rosdep install --from-paths src --ignore-src -y
   
-----------------------------------------------------------------------------------------------------------------------------------------------  
 #Em catkin_ws/ 
 
  $ catkin build
  
  $ source devel/setup.bash
  
 -----------------------------------------------------------------------------------------------------------------------------------------------
 #Agora, use o comando cd e logo em seguida dê nano ~/.bashrc e adicione no final:
 
   $ source ~/PX4-Autopilot/Tools/simulation/gazebo-classic/setup_gazebo.bash ~/PX4-Autopilot ~/PX4-Autopilot/build/px4_sitl_default
   
   $ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot
   
   $ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic
   
   $ export GAZEBO_PLUGIN_PATH=$GAZEBO_PLUGIN_PATH:/usr/lib/x86_64-linux-gnu/gazebo-11/plugin
   
 #Salve a modificação dando Ctrl-x e logo em seguida de o comando source .bashrc para atualizar .bashrc
 
 -----------------------------------------------------------------------------------------------------------------------------------------------
 #Logo após de abra 3 terminais diferentes
 
    #No primeiro:
    
         $ roslaunch px4 px4.launch
         
    #No segundo 
    
         $ roslaunch gazebo_ros empty_world.launch world_name:=$(pwd)/Tools/simulation/gazebo-classic/sitl_gazebo-classic/worlds/empty.world
         
    #No terceiro
    
         $ rosrun gazebo_ros spawn_model -sdf -file $(pwd)/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/models/iris_opt_flow/iris_opt_flow.sdf -model iris -x 0 -y 0 -z 0 -R 0 -P 0 -Y 0
         
 #Explicação:No primeiro terminal é iniciado o PX4 junto com o mavlink, no segundo é iniciado a simulação do gazebo em um mundo escolhido contido no repositório do PX4-Autopilot e já no terceiro é setado o modelo a ser utilizado e a sua posição de spawn.
 
-----------------------------------------------------------------------------------------------------------------------------------------------
