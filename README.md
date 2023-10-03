# Instalar ROS en Ubuntu 20.04

## Preconfiguración para instalar ROS
Configurar el sources.list
Configurar el ordenador para permitir instalar paquetes desde packages.ros.org. 

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

Configurar las claves.

    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
      
## Instalación de ROS
Primero, asegurarse que el índice de paquetes de Ubuntu esta actualizado.

    sudo apt update

Ahora instalar ROS de escritorio más los paquetes de simulación 2D/3D y los paquetes de perceción 2D/3D    

    sudo apt install ros-noetic-desktop-full

## Configuración del entorno

    echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
    source ~/.bashrc

## Instalar dependencia de paquetes para la compilación


Hasta ahora has instalado lo que necesitas para ejecutar los paquetes centrales de ROS. Para crear y gestionar tus propios espacios de trabajo de ROS, existen varias herramientas y requisitos que se distribuyen por separado. Por ejemplo, "rosinstall" es una herramienta de línea de comandos frecuentemente utilizada que te permite descargar fácilmente muchos árboles de código fuente para paquetes de ROS con un solo comando.
Para instalar esta herramienta y otras dependencias para construir paquetes de ROS, ejecuta:

    sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
    sudo apt-get install -y --no-install-recommends build-essential git cmake libasio-dev

## Iniciar rosdep

Antes de poder utilizar muchas de las herramientas de ROS, deberás inicializar rosdep. rosdep te permite instalar fácilmente las dependencias del sistema necesarias para compilar el código fuente que desees y es necesario para ejecutar algunos componentes principales de ROS. Si aún no has instalado rosdep, hazlo de la siguiente manera:

    sudo apt install python3-rosdep

Con lo siguiente, puedes inicializar rosdep.

    sudo rosdep init
    rosdep update

# Clonado repositorio SocialTech_ws

Este repositorio configura un area de trabajo que contiene todos los paquetes imprescindible para poder manipular y simular la plataforma móvil empleada en SocialTech-Challenge.

    git clone –recurse-submodules https://github.com/SocialTech-Challenge/SocialTech_ws

Compilación del area de trabajo.

    cd SocialTech_ws
    catkin_make

# Configuración del interface de comunicaciones y probar la plataforma real

Estos pasos solo hay que realizarlos si de desea utilizar el host que se esta configurando para controlar directamente la plataforma móvil Tracer.

## Setup CAN-To-USB adapter

Habilita el módulo del kernel "gs_usb" (Si ya has añadido este módulo, no necesitas añadirlo de nuevo).

    sudo modprobe gs_usb

Primer uso del paquete tracer-ros.

    rosrun tracer_bringup setup_can2usb.bash

Si no es la primera vez que utilizas el paquete tracer-ros (Ejecuta este comando cada vez que enciendas el sistema).
    
    rosrun tracer_bringup bringup_can2usb.bash

## Lanzar los nodos de control del robot Real

Ejecutar los siguiente nodos de ROS, cada uno en un terminal diferente:

1. Iniciar el nodo base para el robot real con CAN.
    
        roslaunch tracer_bringup tracer_robot_base.launch

2. Iniciar el nodo de teleoperación por teclado.

        roslaunch tracer_bringup tracer_teleop_keyboard.launch

## Lanzar los nodos de control del robot en entorno Simulado

Ejecutar los siguiente nodos de ROS, cada uno en un terminal diferente:

1. Iniciar el nodo base para el robot simulado.
    
        roslaunch SocialTech-Gazebo gazebo.launch

2. Iniciar el nodo de teleoperación por teclado.

        rosrun teleop_twist_keyboard teleop_twist_keyboard.py

