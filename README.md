# "Fundamentos de Seguridad y Auditoría".
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

## ¿Por qué es una negligencia grave que el archivo /var/log/auth.log tenga permisos de lectura para usuarios no privilegiados?

Porque a menudo incluía la contraseña de los usuarios. No es raro escribir la contraseña en el campo de inicio de sesión, por lo que al leer el archivo auth.log se podría encontrar la contraseña de algunos usuarios (iniciarán sesión después de un intento fallido, por lo que tanto el nombre de usuario como la contraseña estarán disponibles).

¿Qué información específica (como PIDs, nombres de usuario o direcciones IP) diferencia un intento fallido de conexión remota SSH de un simple fallo de contraseña de un usuario local frente a la pantalla?
Cuando se produce un error de `res=failed`, conviene investigar esas direcciones IP y nombres de host para ver qué sistemas intentaban conectarse y con qué nombre de usuario. 

Obviamente, los intentos de SSH exitosos permiten comprender qué ocurre en el sistema; por ejemplo,alguien que se sienta en el mismo escritorio todos los días con el nombre de host `bobscomputer` y la dirección IP `192.168.5.5`. Si ayer a las 2 de la madrugada se registra un intento de SSH exitoso con su nombre de usuario desde la dirección IP `10.10.5.6`, por ejemplo, entonces lo más conveniente sería hablar con Bob para investigar. ¿Posible intento de hackeo por parte de otra persona?

## Reto de Investigación 2. Cumplimiento y Log Management. Busca en Dialnet o Semantic Scholar artículos sobre Log Management (Gestión centralizada de registros). A nivel empresarial y legal (piensa en el RGPD en España), ¿qué ventajas vitales ofrece enviar y custodiar los logs en un servidor externo seguro en lugar de mantenerlos dispersos e indefensos en la propia máquina que podría ser vulnerada?

Inmutabilidad ante Ataques: Si un atacante vulnera una máquina, su primer paso es borrar los logs locales para eliminar pruebas. Si los logs se envían en tiempo real a un servidor externo, la evidencia queda fuera de su alcance.

Garantía Forense y Judicial: Para que un registro sirva como prueba en un juicio en España, debe demostrarse que no ha sido manipulado. Un servidor externo con control de acceso garantiza la cadena de custodia, otorgando validez legal a los registros. ¿Qué es la cadena de custodia? La cadena de custodia es el proceso técnico y administrativo que garantiza la autenticidad, integridad e inalterabilidad de una evidencia desde que se genera hasta que se presenta como prueba.

Cumplimiento Legal: Si sufres una brecha de datos y los logs estaban en la máquina afectada por loq que fueron borrados, no podrás demostrar qué datos se filtraron, lo que conlleva multas severas de la Agencia Española de Protección de Datos.

Visión de Conjunto (Correlación): Los ataques suelen ser distribuidos. Al tener todos los logs en un solo punto, puedes detectar patrones de ataque que serían invisibles si analizaras cada máquina por separada.
