# Bitacora3
Antes de realizar cambios a lo loco en un entorno de producción, un buen administrador debe investigar y fundamentar sus decisiones. La improvisación técnica suele derivar en fallos de seguridad críticos o en servidores inoperativos. Utilizad vuestro acceso a las bases de datos académicas [1] para dotar de base científica y metodológica a vuestro trabajo.

## Reto de Investigación 1:
## Reto de Investigación 1. Anatomía de Syslog. Profundiza en el estándar Syslog, el sistema de mensajería vital de Linux. Investiga cómo el sistema clasifica los eventos cruzando dos variables: "Facilidad" (Facility, es decir, qué parte del sistema genera el log, como auth, cron o daemon) y "Prioridad" (Severity, desde debug hasta emerg). 
## ¿Qué es facility?
La instalación representa el proceso de la máquina que generó el evento Syslog. Por ejemplo, ¿el evento fue generado por el kernel, el sistema de correo, los procesos de seguridad/autorización, etc.? 
En este campo, la instalación actúa como un filtro, indicando a SMS que reenvíe al servidor Syslog remoto solo aquellos eventos cuya instalación coincida con la definida en este campo. Por lo tanto, al cambiar el número de instalación o el nivel de gravedad, se modifica la cantidad de alertas (mensajes) que se envían al servidor Syslog remoto.

El valor de la instalación permite determinar qué proceso de la máquina generó el mensaje. Dado que el protocolo Syslog se desarrolló originalmente en BSD Unix, las instalaciones reflejan los nombres de los procesos y demonios de UNIX.

## ¿Qué es Severity/prioridad?
Se recomienda utilizar el nivel de Aviso o Informativo para los mensajes normales.



La configuración predeterminada de SMS para la opción Seguridad de Syslog. Esta configuración enviará todos los eventos al sistema Syslog remoto.
0 EMERGENCIA Situación de emergencia: ¿notificar a todo el personal técnico de guardia? (¿Terremoto? ¿Tornado?) - afecta a varias aplicaciones, servidores o sitios.
1 ALERTA Debe corregirse de inmediato: notificar al personal que pueda solucionar el problema. Un ejemplo es la pérdida de la conexión de respaldo del ISP.
2 CRÍTICO Debe corregirse de inmediato, pero indica un fallo en el sistema principal: solucionar los problemas CRÍTICOS antes que los de ALERTA. Un ejemplo es la pérdida de la conexión principal del ISP.
3 ERROR Fallos no urgentes: deben comunicarse a los desarrolladores o administradores; cada problema debe resolverse en un plazo determinado.
4. ADVERTENCIA: Mensajes de advertencia que no son errores, pero indican que ocurrirá un error si no se toman medidas, por ejemplo, sistema de archivos al 85 % de su capacidad. Cada elemento debe resolverse en un plazo determinado.
5. AVISO: Eventos inusuales que no constituyen errores. Pueden resumirse en un correo electrónico para desarrolladores o administradores con el fin de detectar posibles problemas. No se requiere ninguna acción inmediata.
6. INFORMACIÓN: Mensajes operativos normales. Pueden recopilarse para generar informes, medir el rendimiento, etc. No se requiere ninguna acción.
7. DEPURACIÓN: La información es útil para que los desarrolladores depuren la aplicación, pero no durante su funcionamiento.
