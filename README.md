# Utilizar Alpine Linux como SO de uso diario
El principal objetivo de esta breve guía es de instalar la **Distro Alpine Linux** con el **Escritorio Gnome 44** en un computador portátil para que sea mi sistema de uso diario, esto a pesar que de la Distro Alpine es mayormente utilizada en la instalación de Servidores mediante el uso de Docker.

Mostrare toda la configuracion del sistema para dejar funcional el equipo tanto para tareas de ofimática, navegación web, edición multimedia, programación y hasta para jugar. Para lograrlo se debe dejar optimizado todo el Hardware del equipo. Algunas de las configuraciones opcionales que no utilice en este equipo pero que aparecen con recomendaciones para los casos de conflictos serán mostradas con un texto ~~tachado~~.

## Equipo (HARDWARE)
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
+ APPS: Flatpak

# Instalar Alpine
Iniciamos el equipo y arrancamos desde la imagen de la Distro, inmediatamente hay que loguearse como usuario `root`.

Luego ejecutamos el script de instalación que viene incluido en Alpine Linux:
```
$ setup-alpine
```

Continuar con las opciones de configuración:
+ **Keyboard Layout** (Seleccionar el idioma y la distribución del teclado, ej. `es` variante `latam`.)
+ **Hostname** (Nombre para el computador.)
+ **Network** (Por ejemplo, utilizar descubrir la dirección IP automáticamente mediante el Protocolo `DHCP`.)
+ **Udhcpc** (Cliente DHCP por defecto para Busybox para las interfaces, ej. eth0, wlan0.)
+ ~~**DNS Servers** (El sistema de nombres de dominio para consultar. Por razones de privacidad no es recomendable redirigir todas las solicitudes locales a servidores como el 8.8.8.8 de Google.)~~
+ **Change password root** (Nueva contraseña para el usuario `root`.)
+ **Timezone** (Configurar la Zona Horaria, ej. America/Bogota.)
+ ~~**Proxy** (Servidor proxy para acceder a Internet. Use `none` para conexiones directas hacia Internet.)~~
+ **Mirror** (Desde donde descargar los paquetes de aplicaciones. Elegir un servidor de confianza.)
+ **Add new user** (Agregar una nueva cuenta de usuario `username`.)
+ ~~**SSH** (Secure Shell es un servidor de acceso remoto. **Openssh** es parte de la instalación predeterminada. Use `none` para deshabilitar el logueo remoto, ej. en portátiles.)~~
+ **NTP** (Cliente de protocolo de tiempo de red usado para mantener el reloj del sistema sincronizado con servidor de tiempo. El cliente `chrony` es predeterminado en la instalación.)
+ **Disk Mode** (Seleccionar entre sin disco `disk=none`, `data`, `sys` o según la opción requerida.

## Primer arranque del sistema
En Alpine Linux el manejador de paquetes es `apk`, debemos actualizar el listado de paquetes e instalar las actualizaciones pendientes:
```
$ apk update
$ apk upgrade
```
