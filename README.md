# P5-MundoGazebo

En esta práctica se pretendía integrar un robot en el simulador Gazebo, sin embargo, dicha práctica estaba planificada en ROS, una versión de ROS que no actualmente ya no tiene soporte en Ubuntu 22.04. Para intentar solventar este problema,se intentó realizar la práctica empleando un docker con ROS,sin embargo, no se podía disponer del entorno gráfico de Gazebo, por lo que no se podía realizar ninguna prueba al respecto, siendo imposible realizar la práctica.

## Modelos en SDF.

Se pretende transformar de **URDF** a **SDF** el diseño de un robot, un cubo y las rampas de la anterior práctica.
Para transformar los ficheros **URDF** a **SDF** se puede emplear el siguiente comando:
```
gz sdf -p <fichero.urdf>
```
La salida de este comando proporciona el modelo transformado en **SDF** si el modelo en **URDF** que se proporciona es compatible.

Por ejemplo, el diseño del cubo:
```
<?xml version="1.0" ?>
<robot name="cubo">

    <material name="green">
        <color rgba="0.0 0.8 0.0 1.0" />
    </material>

  <link name="baseLink">
    <inertial>
      <origin rpy="0 0 0" xyz="0 0 0"/>
       <mass value="0.1"/>
       <inertia ixx="1" ixy="0" ixz="0" iyy="1" iyz="0" izz="1"/>
    </inertial>
    
    <visual>
      <origin rpy="0 0 0" xyz="0 0 0"/>
       <geometry>
	 	<box size="3.0 1.0 0.01"/>
       </geometry>
       <material name="green" />
    </visual>
    
    <collision>
      <origin rpy="0 0 0" xyz="0 0 0"/>
      <geometry>
	 	<box size="0.5 0.5 0.5"/>
      </geometry>
    </collision>
  </link>
</robot>
```
transformado en SDF es:
```
$ gz sdf -p cubo.urdf 
<sdf version='1.7'>
  <model name='cubo'>
    <link name='baseLink'>
      <inertial>
        <pose>0 0 0 0 -0 0</pose>
        <mass>0.1</mass>
        <inertia>
          <ixx>1</ixx>
          <ixy>0</ixy>
          <ixz>0</ixz>
          <iyy>1</iyy>
          <iyz>0</iyz>
          <izz>1</izz>
        </inertia>
      </inertial>
      <collision name='baseLink_collision'>
        <pose>0 0 0 0 -0 0</pose>
        <geometry>
          <box>
            <size>0.5 0.5 0.5</size>
          </box>
        </geometry>
      </collision>
      <visual name='baseLink_visual'>
        <pose>0 0 0 0 -0 0</pose>
        <geometry>
          <box>
            <size>3 1 0.01</size>
          </box>
        </geometry>
      </visual>
    </link>
  </model>
</sdf>
```
De la misma manera se obtienen los **SDF** de las rampas y el robot.

## Creación del mundo
Para poder simular el robot en *Gazebo* es necesario crear un mundo y un launcher que lance el mundo.
Para la creación del mundo se crea un fichero *.world* en el cual se incluyen los modelos de las rampas y el cubo 
```
<!-- Incluir modelos -->
		<include>
			<uri>/home/danikg/Escritorio/Modelado_Sim_Repos/p5-mundogazebo-daquinga2020/cubo</uri>
			<pose>20 0 0 0 0 0</pose>
		</include>

		<include>
			<uri>/home/danikg/Escritorio/Modelado_Sim_Repos/p5-mundogazebo-daquinga2020/plataformas_rampas</uri>
			<pose>10 0 0 0 0 0</pose>
		</include>
```
y se establecen las físicas del mundo.
```
<!-- Configuración del mundo -->
		<physics:ode>
			<stepTime>0.001</stepTime>
			<gravity>0 0 -9.8</gravity>
			<cfm>0.0000000001</cfm>
			<erp>0.2</erp>
			<quickStep>true</quickStep>
			<quickStepIters>10</quickStepIters>
			<quickStepW>1.3</quickStepW>
			<contactMaxCorrectingVel>100.0</contactMaxCorrectingVel>
			<contactSurfaceLayer>0.001</contactSurfaceLayer>
		</physics:ode>

  		<model:physical name="gplane">
			<xyz>0 0 0</xyz>
			<rpy>0 0 0</rpy>
			<static>true</static>
			<body:plane name="plane">
				<geom:plane name="plane">
					<laserRetro>2000.0</laserRetro>
					<mu1>50.0</mu1>
					<mu2>50.0</mu2>
					<kp>1000000000.0</kp>
					<kd>1.0</kd>
					<normal>0 0 1</normal>
					<size>51.3 51.3</size>
					<segments>10 10</segments>
					<uvTile>100 100</uvTile>
					<material>Gazebo/GrayGrid</material>
				</geom:plane>
			</body:plane>
		</model:physical>
```
Para lanzar el mundo se crean dos ficheros *.launch*, un fichero *launch_1.launch* el cual lanza únicamente el mundo sin el robot, y un *launch_2.launch*, que además de lanzar el mundo también spawnea el robot en la posición (0,0,2).

## Modificaciones del robot URDF
Para añadir cámaras al robot es necesario modificar su **URDF**, añadiendo los links para las cámaras del chasis y del end-effector.
Los topics en los que se publican cada respectiva imagen son *image_raw_chasis* y *image_raw_end_effector*.

Por último, mencionar de nuevo, que todo lo hecho en esta práctica **no está probado**, sin poder continuar ni terminar la práctica debido a la falta de un entorno gráfico con el que realizar pruebas y testeos de lo realizado anteriormente.
