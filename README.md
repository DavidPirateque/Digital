<div align="center">
<picture>
    <source srcset="https://imgur.com/5bYAzsb.png" media="(prefers-color-scheme: dark)">
    <source srcset="https://imgur.com/Os03JoE.png" media="(prefers-color-scheme: light)">
    <img src="https://imgur.com/Os03JoE.png" alt="Escudo UNAL" width="350px">
</picture>

<h3>Curso de Robótica 2026-I</h3>
<h1>Laboratorio No. 04</h1>
<h2>Robótica de Desarrollo, Intro a ROS 2 Jazzy Jalisco - Turtlesim</h2>
<h4>Profesores: Pedro Fabián Cárdenas Herrera · Manuel Felipe Carranza Montenegro</h4>
<h4>Estudiantes: David Felipe Cárdenas Cubides · David Santiago Pirateque Suárez </h4>

<p>
  <img alt="Ubuntu 24.04 LTS" src="https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?logo=ubuntu&logoColor=white">
  <img alt="ROS 2 Jazzy" src="https://img.shields.io/badge/ROS%202-Jazzy-22314E?logo=ros&logoColor=white">
  <img alt="TurtleBot3" src="https://img.shields.io/badge/TurtleBot3-RViz2-2ea44f">
  <img alt="Estado" src="https://img.shields.io/badge/Estado-Completo-brightgreen">
</p>

<br>
<img src="Images/ros.png" alt="ros" style="border-radius: 5px; width: 300px;"><br>
<b>Figura 1. Robot Operating System.</b>
</div>


# Laboratorio 04: Control y Arquitectura en ROS 2 Jazzy (Turtlesim)

## 1. Objetivos

### Objetivo General
Implementar un paquete de ROS 2 Jazzy Jalisco que permita controlar de una a dos tortugas en el simulador Turtlesim mediante un nodo programado en Python. Este desarrollo debe incluir funciones para control manual, trayectorias automáticas, dibujo de figuras geométricas y tipográficas, y un sistema de control líder-seguidor. 

### Objetivos Específicos
- Comprender la arquitectura fundamental de ROS 2.
- Implementar nodos personalizados.
- Controlar y gestionar el entorno de Turtlesim.
- Implementar comunicación mediante tópicos (publicadores/suscriptores) y servicios (clientes/servidores).
- Desarrollar y sintonizar un sistema de seguimiento (líder-seguidor).

## 2. Introducción

El presente laboratorio tiene como objetivo principal desarrollar una aplicación en ROS 2 que permita controlar el movimiento de una tortuga dentro del simulador Turtlesim, aplicando los conceptos fundamentales de programación de nodos en Python y la comunicación entre ellos. A lo largo del desarrollo, se implementaron múltiples funciones que permiten interactuar con el simulador de forma manual y automática.

La solución fue desplegada en Ubuntu 24.04 LTS utilizando ROS 2 Jazzy Jalisco, Python y la librería `rclpy`. Entre las principales funcionalidades desarrolladas se destacan: el control fluido y simultáneo mediante el teclado, la generación automática de trayectorias (cuadrados y triángulos), el trazado de las letras correspondientes a las iniciales de los integrantes del grupo (D, F, C, P, S) empleando cinemática de lazo cerrado, la gestión del lápiz de dibujo, el reinicio seguro de las variables, y la instanciación de un sistema líder-seguidor donde una segunda tortuga persigue a la principal analizando los datos compartidos a través de los tópicos de ROS 2.

## 3. Tecnologías Usadas

<br>
<div align="center">

| Tecnología | Versión | Descripción |
| :--- | :--- | :--- |
| **Ubuntu** | 24.04 LTS | Sistema operativo base donde se instaló y ejecutó el entorno de desarrollo ROS 2. |
| **ROS 2**| Jazzy Jalisco | Framework utilizado para el desarrollo de la aplicación robótica basada en nodos, tópicos y servicios. |
| **Python**| 3.9+ | Lenguaje de programación utilizado para implementar la lógica de control. |
| **rclpy**| ROS 2 | Biblioteca oficial de Python para la creación de nodos, publicadores, suscriptores y clientes. |
| **Turtlesim** | ROS 2 | Simulador bidimensional utilizado para visualizar y validar el movimiento cinemático. |
| **Git/GitHub** | Web | Sistema de control de versiones donde se almacena y documenta el repositorio. |
| **VirtualBox** | 7.2.8 | Software de virtualización donde se instaló la máquina virtual con Ubuntu. |

</div>
<br>

## 4. Estructura del Proyecto

El desarrollo del laboratorio se encapsuló dentro de un paquete denominado **`my_turtle_controller`**, ubicado en el espacio de trabajo (*workspace*) de ROS 2. Este paquete contiene el nodo principal y las configuraciones necesarias para su correcta ejecución. Su estructura es la siguiente:

```text
~/ros2_jazzy/src/
└── my_turtle_controller/
    ├── package.xml
    ├── setup.py
    ├── setup.cfg
    ├── resource/
    │   └── my_turtle_controller
    ├── my_turtle_controller/
    │   ├── __init__.py
    │   └── move_turtle.py
    └── test/
```

El script principal es `move_turtle.py`, el cual concentra la lógica del laboratorio, abarcando:
* Máquina de estados para control manual concurrente.
* Algoritmos de figuras geométricas automáticas.
* Cinemática con curvas para dibujo de letras tipográficas.
* Lógica de control proporcional para el modo líder-seguidor.

Para habilitar la ejecución, se modificó el archivo `setup.py` registrando el nodo ejecutable:
```python
entry_points={
    'console_scripts': [
        'move_turtle = my_turtle_controller.move_turtle:main',
    ],
},
```
Finalmente, las dependencias (`rclpy`, `turtlesim`, `geometry_msgs`, `std_srvs`) fueron declaradas formalmente en el archivo `package.xml`.

## 5. Diagrama de Flujo

```mermaid
flowchart TD
    A([Inicio del programa]) --> B[Inicializar ROS 2]
    B --> C[Crear nodo MoveTurtle]
    C --> D[Crear publicadores, suscriptores, clientes y Timer Líder-Seguidor]
    D --> E[Iniciar hilo dedicado: rclpy.spin]
    D --> F[Hilo Principal: Entrar al bucle de teclado run]

    %% HILO DE PROCESAMIENTO ROS 2 (Sub-proceso de fondo)
    E --> Z[¿Se activa el callback del Timer a 20Hz?]
    Z -->|Sí| AA{¿Existe turtle2 spawned?}
    AA -->|No| Z
    AA -->|Sí| AB[Leer pose de turtle1 y turtle2]
    AB --> AC[Calcular distancia y orientación]
    AC --> AD[Calcular velocidades proporcionalmente]
    AD --> AE[Publicar en /turtle2/cmd_vel]
    AE --> Z

    %% BUCLE PRINCIPAL DE TECLADO
    F --> G[Leer tecla del teclado get_key]
    G --> H{¿Qué tecla fue presionada o evento?}

    H -->|↑ ↓ ← →| I[Control manual fluido]
    I --> J[Publicar velocidad en /turtle1/cmd_vel]
    J --> F

    H -->|I| S[Invertir estado de self.initials_mode]
    S --> F

    H -->|S, T, P, D, F, C| K{¿self.initials_mode es True?}
    K -->|Sí| L[Dibujar Letra según la tecla D, S, P, F o C]
    K -->|No| M{¿Qué tecla estándar?}
    M -->|S| N[Dibujar cuadrado]
    M -->|T| O[Dibujar triángulo]
    M -->|P| P[Activar/Desactivar lápiz toggle_pen]
    L --> J
    N --> J
    O --> J
    P --> F

    H -->|A| Q[Ejecutar trayectoria automática]
    Q --> QA{¿Cerca del borde?}
    QA -->|Sí| QB[Girar a velocidad angular auto]
    QA -->|No| QC[Avanzar en línea recta]
    QB --> J
    QC --> J
    
    H -->|V| U[Ejecutar secuencia automática de video]
    U --> J

    H -->|R| M_R[Reiniciar simulación reset_turtle]
    M_R --> F

    H -->|2| V[Generar turtle2 spawn_turtle2]
    V --> F

    H -->|3| W[Eliminar turtle2 kill_turtle2]
    W --> F

    H -->|Q / Inactividad > 0.15s| X[Detener movimiento total stop]
    X --> J

    H -->|Ctrl+C / Excepción| Y([Restaurar terminal y Finalizar programa])
```

## 6. Arquitectura del Paquete

El paquete implementado orbita en torno al nodo `move_turtle_node`. Este nodo centraliza el procesamiento de entradas del usuario, cálculos cinemáticos y la gestión general del entorno, apoyándose en la infraestructura del middleware de ROS 2 para comunicarse con el simulador.

<br>
<div align="center">
  <img src="Images/graph.png" alt="gra" style="border-radius: 5px; width: 350px;">
  <br>
  <b>Figura 2. Diagrama de arquitectura de programación.</b>
</div>
<br>

El simulador (`turtlesim_node`) opera como el actuador y sensor virtual, encargado de:
* Renderizar gráficamente las tortugas.
* Recibir y aplicar comandos de velocidad espacial.
* Actualizar la física y calcular la odometría.
* Publicar la pose actual a alta frecuencia.

Nuestra arquitectura utiliza **dos publicadores** (`/turtle1/cmd_vel` y `/turtle2/cmd_vel`) para el envío de instrucciones de velocidad lineal y angular. A su vez, requiere **dos suscriptores** (`/turtle1/pose` y `/turtle2/pose`) para realimentar el sistema con la posición exacta y lograr el control de lazo cerrado necesario para las figuras y el seguimiento.
## 7. Implementación

La solución del laboratorio se desarrolló principalmente en el archivo `move_turtle.py`, donde se implementó un nodo de ROS 2 encargado de controlar el movimiento de la tortuga, ejecutar trayectorias automáticas, dibujar letras y gestionar el sistema de control líder-seguidor.

### 7.1 Creación de nodo, suscriptores y publicadores.

Para comenzar, se inicia el nodo principal de la aplicación, estableciendo así todos los componentes necesarios para el funcionamiento dentro de la red de ROS 2.

```python
class MoveTurtle(Node):

    def __init__(self):
        super().__init__('move_turtle_node')
```

Después se crean los publicadores y suscriptores. Los publicadores son los encargados de enviar los diferentes comandos de velocidad lineal y angular a las tortugas, mientras que los suscriptores reciben continuamente la posición de estas para realizar cálculos precisos.

```python
self.cmd_vel_pub = self.create_publisher(
    Twist,
    '/turtle1/cmd_vel',
    10
)

self.pose_sub = self.create_subscription(
    Pose,
    '/turtle1/pose',
    self.pose_callback,
    10
)
```

Además de esto, se instanció un segundo par de publicador y suscriptor dedicado exclusivamente al sistema líder-seguidor para monitorear y controlar a la segunda tortuga.

```python
self.cmd_vel2_pub = self.create_publisher(
    Twist,
    '/turtle2/cmd_vel',
    10
)

self.pose2_sub = self.create_subscription(
    Pose,
    '/turtle2/pose',
    self.pose2_callback,
    10
)
```

### 7.2 Creación de clientes de servicios.

Continuando con la arquitectura, se programan los distintos clientes de servicios requeridos para controlar funcionalidades integradas del simulador, permitiendo alterar el entorno mediante peticiones síncronas.

```python
self.teleport_client = self.create_client(
    TeleportAbsolute,
    '/turtle1/teleport_absolute'
)

self.set_pen_client = self.create_client(
    SetPen,
    '/turtle1/set_pen'
)

self.spawn_client = self.create_client(
    Spawn,
    '/spawn'
)

self.kill_client = self.create_client(
    Kill,
    '/kill'
)
```

### 7.3 Control manual.

Usando la función `get_key()`, se lee el teclado de forma directa a una alta frecuencia. A partir de la tecla detectada, se invocan las funciones encargadas de los movimientos espaciales. 

```python
if key == KEY_UP:
    self.move_forward()

elif key == KEY_DOWN:
    self.move_backward()

elif key == KEY_LEFT:
    self.turn_left()

elif key == KEY_RIGHT:
    self.turn_right()
```

Para asegurar un control simultáneo y fluido que permita avanzar y girar de manera concurrente sin bloqueos, se implementó un sistema de memoria de velocidades. La función publica un mensaje Twist sobre el tópico `/turtle1/cmd_vel` combinando ambas velocidades.

```python
def move_forward(self):
    self.current_v = LINEAR_SPEED
    self.publish_velocity(
        linear=self.current_v,
        angular=self.current_w
    )
```

### 7.4 Figuras automáticas.

Para el dibujo de elementos geométricos, se implementaron algoritmos basados en cinemática de lazo cerrado, leyendo constantemente la odometría para evitar los errores acumulativos típicos de las instrucciones por tiempo.

Para el triángulo, se realiza el mismo movimiento 3 veces iterando a través de desplazamientos rectos y giros matemáticos exactos (120 grados) para crear sus tres lados de la siguiente forma:

```python
    def draw_triangle(self, side_length=SHAPE_SIDE_LENGTH):
        self.get_logger().info('Dibujando triangulo...')
        for lado in range(3):
            if not self.move_by_distance(side_length): return
            if not self.turn_by_angle(2 * math.pi / 3): return
        self.get_logger().info('Triangulo completado.')
```

Obteniendo como resultado:

<br>
<div align="center">
  <img src="Images/triangulo.png" alt="triag" style="border-radius: 5px; width: 350px;">
  <br>
  <b>Figura 3. Figura de triángulo.</b>
</div>
<br>

Para el rectángulo se realiza el mismo procedimiento, iterando 4 veces para crear sus lados ortogonales de la siguiente forma:

```python
    def draw_square(self, side_length=SHAPE_SIDE_LENGTH):
        self.get_logger().info('Dibujando cuadrado...')
        for lado in range(4):
            if not self.move_by_distance(side_length): return
            if not self.turn_by_angle(math.pi / 2): return
        self.get_logger().info('Cuadrado completado.')
```

Obteniendo como resultado:

<br>
<div align="center">
  <img src="Images/rectangulo.png" alt="rec" style="border-radius: 5px; width: 350px;">
  <br>
  <b>Figura 4. Figura de rectángulo.</b>
</div>
<br>

### 7.5 Dibujo de letras.

En el caso del dibujo de letras, se programó una rutina independiente para cada una de ellas, con el fin de facilitar su construcción a partir de líneas rectas y semicircunferencias. Para garantizar que los arcos de las letras sean perfectos, se implementó un sistema de frenado dinámico que reduce la velocidad al acercarse al ángulo final, evitando que las figuras se tuerzan por la inercia. El código se estructura como se puede ver a continuación:

```python 
    def draw_D(self):
        self.get_logger().info('Dibujando letra D...')
        self._set_pen(False) 
        if not self.turn_to_angle(math.pi / 2): return       
        if not self.move_by_distance(2.0): return            
        if not self.turn_to_angle(0.0): return               
        if not self.draw_arc(-math.pi, radius=1.0): return   
        self._set_pen(True)  
        self.turn_to_angle(0.0)

    def draw_S(self):
        self.get_logger().info('Dibujando letra S...')
        self._set_pen(False) 
        if not self.turn_to_angle(math.pi / 2): return               
        if not self.draw_arc(1.5 * math.pi, radius=0.5): return    
        if not self.turn_to_angle(0.0): return           
        if not self.draw_arc(-1.5 * math.pi, radius=0.5): return   
        self._set_pen(True)  
        self.turn_to_angle(0.0) 

    def draw_P(self):
        self.get_logger().info('Dibujando letra P...')
        self._set_pen(False) 
        if not self.turn_to_angle(math.pi / 2): return       
        if not self.move_by_distance(2.0): return            
        if not self.turn_to_angle(0.0): return               
        if not self.draw_arc(-math.pi, radius=0.65): return   
        self._set_pen(True)  
        self.turn_to_angle(0.0)

    def draw_F(self):
        self.get_logger().info('Dibujando letra F...')
        self._set_pen(False) 
        if not self.turn_to_angle(math.pi / 2): return       
        if not self.move_by_distance(2.0): return            
        if not self.turn_to_angle(0.0): return               
        if not self.move_by_distance(1.0): return            
        
        self._set_pen(True)  
        if not self.turn_to_angle(math.pi): return           
        if not self.move_by_distance(1.0): return            
        if not self.turn_to_angle(-math.pi / 2): return      
        if not self.move_by_distance(1.0): return            
        if not self.turn_to_angle(0.0): return               
        
        self._set_pen(False) 
        if not self.move_by_distance(0.8): return            
        self._set_pen(True)  
        self.turn_to_angle(0.0)

    def draw_C(self):
        self.get_logger().info('Dibujando letra C...')
        self._set_pen(True)  
        if not self.turn_to_angle(math.pi / 2): return       
        if not self.move_by_distance(2.0): return
        if not self.turn_to_angle(0.0): return               
        if not self.move_by_distance(1.0): return
        if not self.turn_to_angle(math.pi): return           
        
        self._set_pen(False) 
        if not self.draw_arc(math.pi, radius=1.0): return    
        self._set_pen(True)  
        self.turn_to_angle(0.0)
```

Con esto, al presionar la tecla `V`, se ejecuta la rutina automatizada definida a partir de estos movimientos que ubica cada una de las letras de forma ordenada mediante teletransporte absoluto, generando un arreglo de tipografías sin colisiones y dando como resultado:

<br>
<div align="center">
  <img src="Images/letras.png" alt="letr" style="border-radius: 5px; width: 350px;">
  <br>
  <b>Figura 5. Dibujo de letras.</b>
</div>
<br>

### 7.6 Sistema líder-seguidor.

El seguimiento de trayectoria funciona primero con la implementación de un temporizador de alta frecuencia (20Hz) que calcula en tiempo real la distancia euclidiana y el ángulo relativo entre las coordenadas de ambas tortugas.

```python
dx = self.current_pose.x - self.pose2.x
dy = self.current_pose.y - self.pose2.y

distance = math.hypot(dx, dy)

target_theta = math.atan2(dy, dx)
```

Ya con estos datos de error frente a la posición deseada, se calcula la velocidad proporcional aplicando ganancias predefinidas para la corrección lineal y angular.

```python
msg.linear.x = 1.5 * distance
msg.angular.z = 6.0 * error_theta
```

Finalmente, la velocidad calculada es enviada al tópico de comando correspondiente para movilizar al seguidor.

```python
self.cmd_vel2_pub.publish(msg)
```

Una vez implementadas todas estas funcionalidades, al presionar la tecla de activación del seguidor (`2`), se puede observar mediante la instanciación de un nuevo nodo la aparición de una segunda tortuga, la cual sigue activamente la trayectoria de la tortuga principal. Su recorrido se representa gráficamente con un color ligeramente más gris.

<br>
<div align="center">
  <img src="Images/seguidor.png" alt="seg" style="border-radius: 5px; width: 350px;">
  <br>
  <b>Figura 6. Sistema Líder-seguidor.</b>
</div>
<br>

## 8. Verificación de la Arquitectura ROS 2

Durante la ejecución, se procedió a inspeccionar el sistema utilizando las herramientas de terminal que provee ROS 2 para confirmar la correcta comunicación entre componentes. A continuación, se detalla la evidencia:

*(Añade debajo de cada numeral la captura de pantalla de la terminal correspondiente)*

### 8.1 `ros2 node list`
**[AÑADE TU CAPTURA DE PANTALLA AQUÍ]**
> **Análisis:** Este comando lista los nodos activos. La captura demuestra que tanto `/turtlesim` como nuestro `/move_turtle_node` están operando simultáneamente en el middleware.

### 8.2 `ros2 topic list`
**[AÑADE TU CAPTURA DE PANTALLA AQUÍ]**
> **Análisis:** Despliega todos los canales de comunicación activos. Permite visualizar que la red cuenta con los tópicos de control cinemático y odometría para ambas tortugas (`/turtle1/cmd_vel`, `/turtle2/pose`, etc.).

### 8.3 `ros2 topic echo /turtle1/pose`
**[AÑADE TU CAPTURA DE PANTALLA AQUÍ]**
> **Análisis:** Permite monitorear el flujo de datos en tiempo real. En la captura se observa el mensaje `Pose` actualizándose con las coordenadas espaciales ($x, y, \theta$) que nuestro código procesa en el lazo cerrado.

### 8.4 `ros2 topic info /turtle1/cmd_vel`
**[AÑADE TU CAPTURA DE PANTALLA AQUÍ]**
> **Análisis:** Entrega metadatos vitales del tópico. Confirma el tipo de mensaje esperado (`geometry_msgs/msg/Twist`) y valida que existe 1 *Publisher* (nuestro script) y 1 *Subscriber* (el simulador).

### 8.5 `ros2 service list`
**[AÑADE TU CAPTURA DE PANTALLA AQUÍ]**
> **Análisis:** Muestra todos los servicios síncronos disponibles. Aquí se evidencian los servicios nativos que invocamos por código, como `/spawn`, `/kill`, `/clear` y `/turtle1/set_pen`.

### 8.6 `rqt_graph`
**[AÑADE TU CAPTURA DE PANTALLA DE RQT_GRAPH AQUÍ]**
> **Análisis:** Esta herramienta gráfica renderiza la topología del sistema. Los elipses validan nuestros nodos y las flechas confirman el correcto enrutamiento de los tópicos entre el líder, el seguidor y el controlador.

## 9. Instrucciones de Ejecución

Para iniciar la simulación y el control, asumiendo que el espacio de trabajo ya ha sido compilado, abre dos terminales independientes:

**Terminal A (Simulador)**
```bash
source /opt/ros/jazzy/setup.bash
ros2 run turtlesim turtlesim_node
```

**Terminal B (Controlador)**
```bash
cd ~/ros2_jazzy
colcon build --packages-select my_turtle_controller --symlink-install
source install/setup.bash
ros2 run my_turtle_controller move_turtle
```

## 10. Video Explicativo
[Inserta aquí el enlace de YouTube/Vimeo de tu video]
