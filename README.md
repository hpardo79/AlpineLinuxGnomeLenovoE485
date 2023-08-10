# Utilizar Alpine Linux como SO de uso diario
El principal objetivo de esta breve guía es de instalar la **Distro Alpine Linux** con el **Escritorio Gnome 44** en un computador portátil para que sea mi sistema de uso diario, esto a pesar que de la Distro Alpine es mayormente utilizada en la instalación de Servidores mediante el uso de Docker.

Mostrare toda la configuracion del sistema para dejar funcional el equipo tanto para tareas de ofimática, navegación web, edición multimedia, programación y hasta para jugar. Para lograrlo se debe dejar optimizado todo el Hardware del equipo. Algunas de las configuraciones opcionales que no utilice en este equipo pero que aparecen con recomendaciones para los casos de conflictos serán mostradas con un texto ~~tachado~~.

## El Equipo (HARDWARE)
Para esta instalación de Alpine utilice un computador portátil
+ Marca: Lenovo 
+ Modelo: ThinkPad E485 LA
+ CPU: AMD Ryzen 3
+ GPU: AMD Radeon Vega 3
+ RAM: 16GB
+ HDD: SSD M.2 256GB

## Sistema (SOFTWARE)
La versión de Alpine y el entorno de Escritorio
+ OS: Alpine Linux v3.18
+ DE: Gnome 44.3
+ APPS: Variadas

# Instalar Alpine
Iniciamos con el encendido del equipo e iniciar desde la imagen de la Distro, inmediatamente hay que loguearse como usuario `root`.

Luego ejecutamos el script de instalación que viene incliudo en Alpine
```
$ setup-alpine
```

Continuar con las opciones de configuración:
+ **Keyboard Layout** (Seleccionar el idioma y la distribución del teclado, ej. `es` variante `latam`.)
