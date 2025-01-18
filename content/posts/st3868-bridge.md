---
title: Sagemcom st3686
date: 2024-01-14
draft: false
tags:
  - network
  - routing
  - Lowi
---
[HFCWiki]: https://es.wikipedia.org/wiki/H%C3%ADbrido_de_fibra_coaxial
[ST3686DefaultIP]: http://192.168.0.1
[ST3686ModemIP]: http://192.168.100.1
# Configuración modem o bridge router Sagemcom st3686
## Introducción
Recientemente he contratado una línea de fibra de Lowi, en principio [HFC][HFCWiki] (Híbrido Fibra Coaxial), pero la fibra no se encuentra por ningún lado pues me llega directamente coaxial. Lowi me ha enviado el router de Sagemcom ST3686 y no me interesa que haga de router, sino que haga de modem y mi router pfense se encargue del resto. Para ello he investigado como hacerlo y os lo explico aquí mismo.
> Es importante que no tengas ninguna red local que se solape con las redes por defecto del router ST3686, siendo estas 192.168.0.0/24 y 192.168.100.0/24
## Configuración
Una vez el técnico ha dado de alta el equipo y ya funciona como router primero debemos conectar los distintos equipos de la siguiente manera o podemos conectar el modem a un equipo y una vez puesto el modo bridge cambiar la conexión más adelante, pero es más cómodo así: 

ST3686 <-> Router Neutro <-> Red local

Primero deberemos configurar nuestro router neutro para que tenga una IP dentro de la red por defecto 192.168.0.0/24, no usaremos DHCP ya que vamos a apagar múltiples servicios del router antes de hacer que se comporte solo como un modem.

Una vez tengamos configurada la IP estática en el router podremos acceder desde otra red local si las reglas de firewall lo permiten. Debemos acceder a la IP por defecto [192.168.0.1][ST3686DefaultIP]. Una vez aquí desactivaremos el servicio de DHCP, el WiFi y todos servicios de compartición de ficheros que no hacen falta. Para ello debemos activar el modo experto en las opciones que se muestra arriba a la derecha.

Una vez hecho esto podemos activar el modo bridge o modem en la configuración. Una vez hagamos esto la IP del router cambiará a [192.168.100.1][ST3686ModemIP] y asignará la IP mediante DHCP, con lo que debemos cambiar la configuración de la interfaz conectada al modem (porque ahora es efectivamente un modem) para que reciba la IP mediante DHCP.