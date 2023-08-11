# Utilizar Alpine Linux como SO de uso diario
Esta breve guía es para instalar **Alpine Linux** con el escritorio **Gnome 44** en un computador portátil para que sea mi *Sistema Operativo* de uso diario, esto a pesar de que esta **Distro** es mayormente utilizada en Servidores para contenedores Docker.

Realizare el paso a paso de toda la configuracion del sistema con la finalidad de dejar funcional el equipo tanto para tareas de ofimática, navegación web, edición multimedia, programación y hasta para jugar. Lograrlo involucra dejar optimizado todo el Hardware del equipo. 

Algunas de las configuraciones son **opcionales** porque no las utilice en este equipo pero aparecen ya pueden ser útiles en los casos de conflictos, las mismas serán mostradas con un texto ~~tachado~~.

## HARDWARE
Para esta instalación de Alpine utilice un computador portátil
+ Marca: Lenovo 
+ Modelo: ThinkPad E485 LA
+ CPU: AMD Ryzen 3
+ GPU: AMD Radeon Vega 3
+ RAM: 16GB
+ HDD: SSD M.2 256GB

<img src="/img/lenovo-thinkpad-e485.png" width=40% height=30%>

## SOFTWARE
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

# Primer arranque del sistema
En Alpine Linux el manejador de paquetes es `apk`, debemos actualizar el listado de paquetes e instalar las actualizaciones pendientes:
```
$ apk update
$ apk upgrade
```

## Editor para el Terminal
Instalar el **editor** de nuestra preferencia (ej. vim, nano.)
```
$ apk add <editor>
```

## Doas (no usar Sudo)
`doas` es un programa para ejecutar comandos como otro usuario, generalmente como **root**. Primero debemos agregar el usuario al grupo `wheel` y luego instalar `doas`.
```
$ adduser <usuario> wheel
$ apk add doas
```

~~Permitir que los miembros del grupo `wheel` utilicen privilegios de root con doas. Para hacer esto, abre el archivo de configuración de doas:~~
```
$ <editor> /etc/doas.d/doas.conf
```

~~Agrega la siguiente línea y guarda el archivo:~~
```
permit persist :wheel
```

Desactiva la cuenta de root:
```
$ doas passwd -l root
```

## Repositorios de la Comunidad
Editar los repositorios de `apk` y habilitar los de la comunidad:
```
$ <editor> /etc/apk/repositories
```

Descomentar la línea de los repositorios de la comunidad:
```
#/media/dm-0/apks
http://mirror.leaseweb.com/alpine/v3.18/main
#http://mirror.leaseweb.com/alpine/v3.18/community
```

## Bash Shell
Instalar el intérprete de comandos `bash`
```
apk add bash bash-completion bash-doc
```

Para cambiar el shell de `root` debemos editar:
```
$ <editor> /etc/passwd
```

Buscar el shell predeterminado del usuario, ej. */bin/ash*
```
root:x:0:0:root:/root:/bin/ash
```

Reemplazar con */bin/bash*:
```
root:x:0:0:root:/root:/bin/bash
```
