# Proyecto-ASIR-monitorizaci-n-en-Nagios
A continuación explico lo que es cada archivo y su función:
##############################
#######   cgi.cfg   #######
##############################

Es el archivo donde defines accesos a la interfaz web tales cómo:
Autenticación y permisos (use_authentication, authorized_for_*): Controla qué usuarios pueden acceder a la interfaz y qué acciones pueden realizar.
Este archivo no lo he modicado para mi proyecto pero lo considero importante para comprender bien el potencial de la herramienta.

Rutas de archivos HTML (physical_html_path, url_html_path): Define dónde se encuentran los archivos web de Nagios.

Frecuencia de actualización (refresh_rate): Ajusta cada cuántos segundos se actualizan las páginas de estado.

Opciones de visualización (default_statusmap_layout, default_statuswrl_layout): Configura cómo se muestran los mapas de estado y la información de la red.

Sonido en alertas (host_down_sound, service_critical_sound): Permite agregar sonidos cuando hay problemas en la red.

Seguridad en comentarios y acciones (lock_author_names, escape_html_tags): Evita cambios de autor en comentarios y controla la visualización de etiquetas HTML.

Incluye también una integración con Esplunk, un software para macrodatos generados por servicios, hosts, etc...


##############################
#######  commands.cfg  #######
##############################

Con este archivo defino los comandos que pueden ejecutarse para supervisar hosts y servicios.
Es un archivo muy importante porque establece cómo Nagios interactúa con los plugins y cómo se ejecutan los chequeos, notificaciones y otras acciones automatizadas.
Comandos de monitoreo: Define cómo Nagios verifica el estado de los servicios y hosts.
En este archivo se puede definir estos parámetros:

Notificaciones: Especifica los comandos que se utilizan para alertar sobre problemas.

Acciones remotas: Permite ejecutar comandos para reiniciar servicios, verificar conectividad y más.

Uso de macros: Nagios usa variables como $HOSTADDRESS$ o $SERVICESTATE$ para personalizar los comandos:
$HOSTADDRESS$ → Dirección IP o nombre de host del dispositivo que se está monitoreando.
$SERVICESTATE$ → Estado actual del servicio (OK, WARNING, CRITICAL, UNKNOWN).

##############################
#######  contacts.cfg  #######
##############################

En este archivo defino los contactos los cuáles reciben notificaciones cuando hay problemas con los hosts o servicios monitoreados. Es fundamental para gestionar alertas y garantizar que las personas adecuadas sean notificadas.
En este caso no he modificado el archivo pero también lo considero importante, para conocer el potencial del servicio y para darle funcionalidad habría que configurar un servidor Postfix o SMTP.

Estos son algunas configuraciones que se pueden integrar:
Contactos individuales (define contact): Especifica cada usuario que recibirá notificaciones.

Grupos de contactos (define contactgroup): Agrupa varios contactos para facilitar la gestión de alertas.

Horario de notificación (host_notification_period, service_notification_period): Define en qué horarios el contacto puede recibir alertas (por ejemplo, 24x7).

Opciones de notificación (host_notification_options, service_notification_options): Controla qué tipos de alertas recibe el contacto (d para host caído, u para inaccesible, c para crítico, etc.).

Métodos de notificación (host_notification_commands, service_notification_commands): Indica los comandos que se ejecutan para enviar alertas (correo, SMS, etc.).

Alias y correo electrónico (alias, email): Personaliza el nombre y la dirección de contacto.

##############################
####  htpasswd.users  ####
##############################
Es el archivo de credenciales para los usuarios y contiene:
Nombres de usuario: Cada línea del archivo representa un usuario autorizado.

Contraseñas encriptadas: Las contraseñas no están en texto plano, sino cifradas mediante htpasswd.

Un ejemplo para crear un usuario:
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin2

En mi caso no he creado ningún usuario poruqe he utilizado el usuario administrador por defecto pero es importante si en alguno momento queremos crear un usuario.
##############################
####### localhost.cfg #######
##############################
Este archivo tampoco lo he tenido que modificar pero lo considero importante porque contiene la configuración de monitoreo para el propio servidor donde Nagios está instalado.

Tiene las siguientes configuraciones:
Definición del host local: Especifica el nombre, dirección y parámetros básicos del servidor.

Monitoreo de servicios esenciales: Configura chequeos de CPU, uso de disco, memoria, carga del sistema, procesos en ejecución, y conectividad de red.

Alertas y notificaciones: Define cuándo y cómo se envían alertas si el sistema local presenta problemas.

Comandos de chequeo: Usa plugins como check_ping, check_http, check_cpu_load, entre otros, para monitorear el estado del servidor.

##############################
#######  nagios.cfg  #######
##############################
Es el archivo principal de configuración del servicio y es donde se establecen las rutas de los archivos de configuración. 
Tiene otras funciones como:
Intervalos de monitoreo: Establece cada cuánto tiempo Nagios revisa el estado de hosts y servicios (check_interval, max_check_attempts).

Gestión de notificaciones: Configura cuándo, cómo y con qué frecuencia se envían alertas (notification_interval, notification_options).

Control de procesos: Ajusta la forma en que Nagios ejecuta sus chequeos y maneja el rendimiento (use_aggressive_host_checking, process_performance_data).

Configuración de seguridad: Define opciones para el acceso y uso de la interfaz web (use_authentication, lock_author_names).

Archivos de configuración adicionales: Permite incluir otros archivos (cfg_file y cfg_dir) para organizar la configuración de hosts, servicios y comandos.

##############################
#######  services.cfg  #######
##############################
Es uno de los archivo más importante puesto que aquí se definen los servicios que se van a monitorear en los hosts. Es clave porque permite a Nagios verificar el estado de distintos componentes dentro de un sistema, como la conectividad de red, el uso de CPU, el estado de bases de datos, servidores web y mucho más:
Ejemplos de definiciones:
Servicios a monitorear: Se especifica qué servicios de cada host deben ser supervisados.

Comandos de chequeo: Define los plugins que Nagios ejecutará para comprobar el estado de un servicio.

Frecuencia de monitoreo: Establece cada cuánto tiempo se verifica un servicio (check_interval).

Notificaciones: Configura cuándo se envían alertas y a quién si un servicio presenta problemas.

Umbrales de alerta: Permite definir cuándo se considera un estado WARNING, CRITICAL, o OK.

##############################
####### timeperiods.cfg ######
##############################
En este archivo se definen los períodos de tiempo en los que se pueden realizar chequeos, enviar notificaciones y ejecutar comandos. 
Es fundamental para establecer horarios específicos y evitar alertas fuera de los tiempos deseados.

Ejemplo de definición: define timeperiod {
    timeperiod_name horario_laboral
    alias Horario de oficina
    monday 08:00-17:00
    tuesday 08:00-17:00
    wednesday 08:00-17:00
    thursday 08:00-17:00
    friday 08:00-17:00
}


