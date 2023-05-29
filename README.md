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

