---
title: RDP Wrapper
date: 2023-09-14
draft: false
tags:
  - windows
  - remote
  - opensource
---
[RDP-Wrapper]: https://github.com/stascorp/rdpwrap/releases/
[RDP-Wrapper-Autoupdater]: https://github.com/asmtron/rdpwrap/raw/master/autoupdate.zip
# Conexión remota multisesión en Windows | RDP Wrapper
¿Alguna vez has pensado en compartir la potencia de tu ordenador con otras personas pero no has encontrado forma fácil de hacerlo?
RDP Wrapper es una es una librería de proyectos para permitir el uso concurrente de Escritorio Remoto en un solo dispositivo, pudiéndose conectar hasta 8 usuarios, aunque dependerá de tu ordenador.

## Funcionamiento normal
El funcionamiento normal de escritorio remoto impide el uso concurrente incluso con distintos usuarios, quitando la sesión al usuario conectado. Es por esto que en situaciones muy concretas nos podría interesar compartir los recursos de nuestro ordenador a múltiples usuarios o incluso múltiples sesiones de un mismo usuario😏.

## RDP Wrapper Library a.k.a La Salsa
El programa a instalar y todo el código del que depende es abierto y lo puedes inspeccionar tu mismo en la página de [github][RDP-Wrapper] del proyecto. Allí también encontrarás información relativa a versiones cambios, problemas y más.

### Versiones de Windows Soportadas
- Windows Vista
- Windows 7
- Windows 8
- Windows 8.1
- Windows 10
- Windows 11

### Contenido del paquete de instalación
El paquete `.zip` contiene los siguientes ficheros:

| Archivo | Descripción |
| --------- | ----------- |
| `RDPWInst.exe`  | Instalador |
| `RDPCheck.exe`  | Comprobador de RDP local |
| `RDPConf.exe`   | Panel de configuración de RDP Wrapper |
| `install.bat`   | Archivo .batch de instalación |
| `uninstall.bat` | Archivo .batch de desinstalación |
| `update.bat`    | Archivo .batch de actualización |
_Tabla extraída del github oficial_

## Instalación
Para la instalación de RDP Wrapper lo que debemos hacer es lo siguiente:
1. Descargar el archivo .zip de la página de [github][RDP-Wrapper]. Selecciona la última versión disponible (aunque lleva sin actualizarse desde 2017)
2. Crea una carpeta llamada RDP Wrapper en la carpeta `C:\Program Files` y descomprime el fichero `.zip` dentro. 
3. Para más adelante debemos crear una excepción en Windows Defender para evitar que marque el nuevo programa y los pasos siguientes como una amenaza
	1. Selecciona el icono de Windows Defender de la barra de Tareas
	2. Selecciona Protección antivirus y contra amenazas > Configuración de antivirus y protección contra amenazas > Exclusiones
	3. Agregamos una exclusión con el siguiente directorio: `C:\Program Files\RDP Wrapper`
4. Descargamos el [actualizador][RDP-Wrapper-Autoupdater] y lo descomprimimos en la misma carpeta.
5. Ejecutamos el archivo `autoupdate.bat` como administrador
	1. Si queremos que la actualización se realice automáticamente en iniciar el ordenador entrar en la carpeta `helper` y ejecutar como administrador el archivo `.bat` correspondiente. Para deshacerlo solo deberemos ejecutar el otro.
6. Ahora podemos ejecutar el archivo `install.bat` como administrador.
7. Una vez instalado podemos comprobar el funcionamiento con `RDPCheck.exe` y cambiar las opciones con `RDPConf.exe`

## Configuración
El programa `RDPConf.exe` nos indicará la configuración de escritorio remoto y parámetros a modificar. 

### *Diagnostics*
Nos indica el estado:
- ***Wrapper state***: si esta instalado correctamente el programa o no
- ***Service state***: si el servicio de escritorio remoto se esta ejecutando
- ***Listener state***: si se están esperando conexiones

### *General Settings*
Nos permite:
- **Habilitar el servicio**: muy típico que se nos haya olvidado habilitarlo
- **Cambiar puerto donde conectarse**: típicamente es el 3389
- **Permitir una o más sesiones por usuario**: si queremos realizar pruebas de conexión con el mismo usuario con el que estamos seria recomendable habilitar múltiples inicios de sesión
- **Ocultar los usuarios en la pantalla de inicio de sesión**: esto requerirá indicar usuario y contraseña, y afectará al comportamiento normal de la pantalla de inicio de sesión
- **Si se permite iniciar programas personalizados en inicio**: esto hará que se acepte en realizar la conexión el arranque de algún programa

