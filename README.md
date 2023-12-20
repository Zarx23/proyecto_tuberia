# Detector de fugas
Este proyecto fue realizado en el contexto del curso Duckietown del año 2023 en la Universidad de Chile
# Requerimientos
Duckiebot Mark 3
RPLIDAR A1M8
Joystick para controlar el bot

  Requerimientos de software (información compartida por colegas): 
imu_tools: https://github.com/CCNYRoboticsLab/imu_tools.git

mpu6050_driver: https://github.com/Brazilian-Institute-of-Robotics/mpu6050_driver.git

rplidar_ros: https://github.com/Slamtec/rplidar_ros.git

robot_localization: https://github.com/ros-perception/slam_gmapping.git

gmapping: https://github.com/cra-ros-pkg/robot_localization.git

map_server y amcl: https://github.com/ros-planning/navigation.git
# Iniciar los programas
Conectado el robot como se hace siempre, se va hasta /catkin, de aqui se va a /proyecto_tuberia/duckiebot_odometry/src con cd, como se ve ahora:
```
cd /proyecto_tuberia/duckiebot_odometry/src
```
Aqui, se ejecutan los siguientes comandos para abrir los archivos, los cuales de deben hacer cada comando en diferentes pestañas de la misma terminal, las cuales se pueden abrir con la tecla f2:
```
python joy_challenge.py
python odometria.py
```
Luego, desde otra terminal mas se lanzan una transformada y el launch de el Rplidar:

# Ver Odometría
Desde una terminal del computador se ejecutan los siguientes comandos mientras se esta ejecutando el RPlidar, odometria, joy_challenge, etc.
```
export ROS_MASTER_URI=http://duckiebot.local:11311
rviz
```
Luego, en la pestaña de rviz se selecciona la opcion de "odom" en la seccion de "Fixed Frame". Luego, apretamos add, ordenamos "By topic" y agregamos LaserScan para ver los escaneos. Tambien se puede agregar Odometry para ver como evoluciona esta misma. Una parte importante es cambiar el Decay Time en la seccion de LaserScan, para mayor valor, mas tiempo se mantiene en pantalla los escaneos del sensor.
# Grabar rosbag
Para grabar un rosbag que se usa posteriormente, hay que estar ejecutando todo lo que queremos grabar, es decir, todo lo mencionado anteriormente y ejecutar el siguiente comando en una terminal de robot:
```
rosbag record -a
```
# lanzar el detector de circulos junto al rosbag
Primero hay que dar permisos de ros con los comandos:
```
roscore
export ROS_MASTER_URI=http://duckiebot.local:11311
```
Para luego ejecutar el detector que toma un archivo de rosbag, entonces solo hay que usar el comando:
```
python3 circulo_con_rosbags.py 
```
Todo esto se hace en una terminal del computador, no a una conectada con el robot.
# Motivaciones
El objetivo final de este proyecto era detectar fallas en tuberías ocupando un Lidar. La idea era recrear la tubería en Rviz utilizando las mediciones del Lidar y un sistema de odometría donde el Lidar al trabajar en 2D correspondería con las dimensiones del círculo en los ejex XY y la odometría ayudaría a indicar cuanto se había movido el robot en el eje Z.

Para implementar la odometría, primero calibramos la velocidad de las ruedas, pues en un inicio una avanzaba más que la otra. Luego de esto, corrimos el bot varias veces, midiendo la distancia recorrida y el tiempo empleado para así encontrar mediante regresión lineal la velocidad a la que avanzaban las ruedas. Una vez hecho esto, pudimos implementar el módulo de odometría, donde agregamos también un botón para reiniciar la cuenta de la odometría por si se querían tomar las muestras nuevamente.

Para implementar el Lidar, tuvimos que descargar los paquetes necesarios y aprender a utilizar Rviz para ver lo que detectaba el sensor. Con respecto a la detección de círculos, decidimos que íbamos a determinar la ubicación de las fallas con un programa que leía un rosbag ya grabado previamente para no consumir toda la memoria del bot. Para determinar cuales puntos estaban dentro y fuera de la circunferencia utilizamos una regresión.

Juntando estas dos partes es que pudimos completar satisfactoriamente el proyecto, la única cosa que por temas de tiempo no alcanzamos a hacer pero simplifica mucho el uso de este programa es el roslaunch que active todos los sistemas en conjunto, pues como pueden apreciar en el resto del texto, estos se deben correr por separado y en distintas terminales, lo que no es muy cómodo ni productivo.

# Agradecimientos

Estamos muy agradecidos de haber tenido la oportunidad de desarrollar este proyecto, queremos por ello, dar las gracias a toda la gente de Duckietown Engineering y especialmente a la de Maquintel por hacer de esta una increíble experiencia.
