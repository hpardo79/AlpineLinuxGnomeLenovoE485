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

## Herramientas Básicas
Instala una selección de paquetes comúnmente utilizados:
```
$ apk add sed attr dialog bash bash-completion grep util-linux pciutils usbutils binutils findutils readline lsof less nano curl
```

### Configurar "Bash Shell"

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

## Habilitar repositorios "Community"
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

## Agregar soporte para idiomas con "Locale"
Instala paquete para idiomas
```
$ apk add musl-locales
```

Define el idioma en */etc/profile.d/locale.sh*
```
LANG=es_PA.UTF-8
```

# Entorno de Escritorio (Gnome 44)
```
$ setup-desktop gnome
```

## Habilitar instalación por la Tienda Gnome
Para que la **Tienda de Gnome** instale los paquetes `apk` es necesario habilitar el servicio `apk-polkit-server`.
```
$ rc-update add apk-polkit-server default
$ rc-service apk-polkit-server start
```

## Instalar "Network Manager"
```
$ apk add networkmanager
$ apk add networkmanager-wifi
$ apk add network-manager-applet
```

Iniciar el servicio y agregarlo al arranque:
```
$ rc-service networkmanager start
$ rc-update add networkmanager default
```

~~Agregar el usuario al grupo plugdev:~~
```
$ adduser plugdev
```

Detener servicios en conflicto
Edita la interfaces en */etc/network/interfaces* comenta el contenido:
```
#auto eth0
#iface eth0 inet dhcp
```

## Sonido (Pipewire)
Pipewire permisos para acceder a los dispositivos:
```
$ addgroup  audio
$ addgroup  video
```

Instalar los paquetes para `Pipewire` y el manejador de sesión `Wireplumber`:
```
$ apk add pipewire gst-plugin-pipewire wireplumber
```

Paquetes para compatibilidad `PulseAudio` `JACK` `ALSA`:
```
$ apk add pipewire-pulse pipewire-jack pipewire-alsa
```

### Configuración de Pipewire
Los archivos de las configuraciones de PipeWire y WirePlumber estan por defecto en */usr/share/pipewire* y */usr/share/wireplumber*. Para realizar los cambios en la configuración se requieren copiar a */etc*:
```
$ cp -a /usr/share/pipewire /etc
$ cp -a /usr/share/wireplumber /etc
```
### Deshabilitar el Soporte D-Bus
Editar los siguientes parametros:

El contenido de */etc/pipewire/pipewire.conf*:
```
context.properties = {    
...
support.dbus = false
}
```

El contenido de */etc/wireplumber/wireplumber.conf*
```
context.properties = {
...
support.dbus = false
}
```

El contenido de */etc/wireplumber/bluetooth.lua.d/50-bluez-config.lua*
```
bluez_monitor.properties = {
...
["with-logind"] = false,
}
```

El contenido de */etc/wireplumber/main.lua.d/50-alsa-config.lua*
```
alsa_monitor.properties = {
...
["alsa.reserve"] = false,
}
```

El contenido de */etc/wireplumber/main.lua.d/50-default-access-config.lua*
```
default_access.properties = {
...
["enable-flatpak-portal"] = false,
}
```

### "Realtime Scheduling"

Asegurar que el usuario tenga los permisos ilimitados. Agregar al grupo `pipewire`.

$ addgroup pipewire


### Video Webcams
El video debe funcionar para dispositivos video4linux *v4l2* (por ejemplo *webcams*) y aplicaciones GStreamer. 
```
$ apk add gstreamer
```

### Audio Bluetooth
Para habilitar el soporte con PulseAudio; instalar los paquetes de servicio bluetooth:
```
$ apk add bluez bluez-openrc pipewire-spa-bluez
```

Opcional la GUI del manejador para bluetooth:
```
$ apk add blueman
```

Habilitar e iniciar el servicio bluetooth:
```
$ rc-update add bluetooth
$ rc-service bluetooth start
```

### Compartir pantalla
Se necesita el **backend** del entorno de escritorio instalado *xdg-desktop-portal*:
Para **GNOME**:
```
$ apk add xdg-desktop-portal-gtk
```

## Video Radeon

## Paquetes Adicionales
 Completar el soporte de idioma: [*](https://github.com/hpardo79/AlpineLinuxGnomeLenovoE485/edit/main/README.md#hardware)

