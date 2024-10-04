# HA-extras
Una colección de extras sencillos para Home Assistant

## Aviso sobre posibles errores
Todas las automatizaciones, scripts y plantillas han sido realizadas por mi, soy nuevo utilizando jinja y yaml, así que es perfectamente posible que las cosas no funcionen como se espera a veces, contengan errores o no estén hechas de la mejor manera posible

Sentíos libres de sugerirme cambios o correcciones que consideréis necesarias.

Mi objetivo es que sean útiles, que puedan usarse al menos como ejemplo de cómo hacer algo (o cómo no hacerlo) e irlas mejorando con el tiempo

## Automatizaciones disponibles
### Cambio estado puerta
Esta automatización se lanza siempre que una puerta cambia de estado

#### Desencadenantes
- El estado de los sensores, recuerda establecer una lista de sensores de las puertas para que los escuche correctamente. Ejemplo:
```
entity_id:
  - binary_sensor.puerta_abajo
  - binary_sensor.puerta_finca
```
#### Condiciones
- El estado de un ayudante de tipo booleano, recuerda hacer referencia a tu ayudante de forma correcta. Esto permite desactivar o no la automatización

#### Acciones
- Enviar notificación, utiliza el script que se encuentra en la carpeta scripts. Recuerda personalizar tus mensajes a tu gusto

#### Información adicional
Esta automatización lanza alertas cuando cambia de estado el sensor, de forma que si pasa a 'no disponible' y vuelve al estado de disponible, lanzará 2 avisos de golpe, así que puede que genere muchos más avisos de los que te gustaría recibir, tenlo en cuenta

### Detectado movimiento
Esta automatización se lanza siempre que un sensor de movimiento cambia de estado

#### Desencadenantes
- El estado de los sensores, recuerda establecer una lista de sensores de movimiento para que los escuche correctamente. Ejemplo:
```
entity_id:
  - binary_sensor.movimiento_arriba
  - binary_sensor.movimiento_abajo
```
#### Condiciones
- El estado de un ayudante de tipo booleano, recuerda hacer referencia a tu ayudante de forma correcta. Esto permite desactivar o no la automatización

#### Acciones
- Enviar notificación, utiliza el script que se encuentra en la carpeta scripts. Recuerda personalizar tus mensajes a tu gusto

#### Información adicional
Esta automatización lanza alertas cuando cambia de estado el sensor, de forma que si pasa a 'no disponible' y vuelve al estado de disponible, lanzará 2 avisos de golpe, así que puede que genere muchos más avisos de los que te gustaría recibir, tenlo en cuenta

### Puerta abierta de noche
Esta automatización se lanza a una hora determinada (a las 21:00 por defecto) para avisarte que se hace de noche y hay ciertas puertas que siguen abiertas

#### Desencadenantes
- Una hora concreta, por defecto las 21:00 que es una buena hora de "recogida" sin que sea tan tarde que decidas que no merece la pena ir a cerrar las puertas que siguen abiertas

#### Condiciones
- Que haya alguna puerta abierta, se basa en una etiqueta y comprueba todos los sensores que tienen dicha etiqueta, si hay alguno a "on" lanza el mensaje con la lista de puertas

#### Acciones
- Enviar notificación, utiliza el script que se encuentra en la carpeta scripts. Recuerda personalizar tus mensajes a tu gusto

#### Información adicional
Esta automatización utiliza una etiqueta que sólo deberían tener las puertas que quieres controlar (que pueden no ser todas), ya que si no te avisará incluso de automatizaciones, otros sensores, luces...

### Validación horaria puertas
Esta automatización se lanza cada hora y comprueba que puertas siguen abiertas, depende del ayudante booleano. Es útil para controlar si pasa el tiempo y alguna puerta no se ha cerrado todavía

#### Desencadenantes
- La hora, se lanza cada hora en punto
#### Condiciones
- Que haya alguna puerta abierta, se basa en una etiqueta y comprueba todos los sensores que tienen dicha etiqueta, si hay alguno a "on" lanza el mensaje con la lista de puertas
- Que la hora se encuentre entre unas horas máxima y mínima (para evitar que avise durante las horas de descanso). Recuerda personalizarlas (por defecto antes de las 23:05 y después de la 07:50, no se pone la hora en punto para que a esas horas sí que salte)
- El estado de un ayudante de tipo booleano, recuerda hacer referencia a tu ayudante de forma correcta. Esto permite desactivar o no la automatización

#### Acciones
- Enviar notificación, utiliza el script que se encuentra en la carpeta scripts. Recuerda personalizar tus mensajes a tu gusto

#### Información adicional
Esta automatización se va a lanzar cada hora y se tendrá en cuenta una etiqueta concreta, recuerda establecer la etiqueta sólo a las puertas que quieras tener controladas y deberían estar cerradas. También ten en cuenta el horario de descanso si no quieres recibir notificaciones cuando duermes

## Scripts
### Enviar notificación
Este script simplifica enviar notificaciones a varios dispositivos de forma que sólo hace falta llamar al script y se enviará a los dispositivos que tengas definidos de forma paralela

#### Datos de entrada
- Titulo: el título del mensaje, según la aplicación variará como se muestra, por ejemplo para telegram se le han añadido asteriscos para formatearlo como negrita ya que no se diferenciaba
- Mensaje: el cuerpo del mensaje que se va a enviar, admite varias líneas y puede ser largo, aunque en las notificaciones de app móvil no se ve si es largo

#### Acciones
- Envia mensaje a las entidades definidas, recuerda renombrarlas en base a lo que tienes de verdad, ya que es posible que la aplicación de móvil incluya el modelo de móvil y la de telegram tenga un nombre diferente según hayas definido cuando la configuraste

## Scripts
### Enviar notificación con etiqueta
Este script simplifica enviar notificaciones a varios dispositivos de forma que sólo hace falta llamar al script y se enviará a los dispositivos que tengas definidos de forma paralela. Incluye etiquetas que luego se usarán para definir cómo actúa la aplicación que muestra la notificación

#### Datos de entrada
- Titulo: el título del mensaje, según la aplicación variará como se muestra, por ejemplo para telegram se le han añadido asteriscos para formatearlo como negrita ya que no se diferenciaba
- Mensaje: el cuerpo del mensaje que se va a enviar, admite varias líneas y puede ser largo, aunque en las notificaciones de app móvil no se ve si es largo
- Etiqueta: admite sólo una palabra, puede ser personalizada, aunque ten en cuenta lo descrito en información adicional

#### Acciones
- Envia mensaje a las entidades definidas, recuerda renombrarlas en base a lo que tienes de verdad, ya que es posible que la aplicación de móvil incluya el modelo de móvil y la de telegram tenga un nombre diferente según hayas definido cuando la configuraste

#### Información adicional
Las etiquetas que recoge el script son:
- Alarmas
- Info
- Verificacion
- cualquier otra cosa

Para telegram, las etiquetas no aportarán ningún cambio visual, sólo se verán en los registros, podría ser útil por ejemplo para ver cuántos mensajes de tipo "Alarmas" se enviaron, pero no va a formatearlas ni actuar diferente telegram

Para android sí que hay cambios, por un lado, se establece un canal de notificaciones. Ten en cuenta que cada etiqueta diferente creará un canal, así que igual si usas demasiadas, luego te sea complicado "gestionarlas" desde las opciones de configuración de tu móvil (en la guía de la integración explica cómo borrar etiquetas).

Estos canales te permitirán hacer que tu móvil suene o muestre la notificación de forma diferente, por ejemplo podrías hacer que "Alarmas" suene con un tono específico mientras que "Info" no suene, sólo muestre la notificación o no aparezca siquiera en la pantalla de bloqueo

También se utiliza la etiqueta en android para definir un color de notificación, si vas a usar otras etiquetas o quieres cambiar los colores, recuerda modificarlo en la sección del script correspondiente
