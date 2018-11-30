---
title: Referencia de dominio creado previamente - Azure| Microsoft Docs
titleSuffix: Azure
description: La referencia de dominios creados previamente, que son colecciones creadas previamente de intenciones y entidades de Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 287a0986d921798bc7735e5a75d279f010712b16
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335630"
---
# <a name="prebuilt-domain-reference"></a>Referencia de dominio creado previamente
Esta referencia ofrece información sobre los [dominios creados previamente](luis-how-to-use-prebuilt-domains.md), que son colecciones creadas previamente de intenciones y entidades que LUIS ofrece.

Los [dominios personalizados](luis-how-to-start-new-app.md), por el contrario, se inician sin intenciones ni modelos. Puede agregar cualquier intención y entidad de dominios creados previamente a un modelo personalizado.

## <a name="list-of-prebuilt-domains"></a>Lista de dominios creados previamente
LUIS ofrece veinte dominios creados previamente. 

| Dominio creado previamente | DESCRIPCIÓN | Idiomas admitidos |
| ---------------- |-----------------------|:------:|
| Calendario | El dominio del calendario ofrece intenciones y entidades para agregar, eliminar o editar una cita, comprobar la disponibilidad de los participantes y encontrar información sobre un evento de calendario.| en-US<br/> zh-CN |
| Cámara | El dominio de la cámara ofrece intenciones y entidades para captar imágenes, grabar vídeos y difundir vídeos a una aplicación.| en-US |
| Comunicación | Envío de mensajes y realización de llamadas telefónicas.| en-US <br/> zh-CN |
| Entretenimiento  | Control de consultas relacionadas con música, películas y TV.| en-US |
| Eventos | Reservar entradas para conciertos, festivales, eventos deportivos y espectáculos de comedia.| en-US |
| Fitness | Control de solicitudes relacionadas con la supervisión de actividades de fitness.| en-US |
| Juegos | Control de solicitudes relacionadas con una partida de un juego multijugador.| en-US |
| HomeAutomation | Control de dispositivos domésticos inteligentes, como las luces y los electrodomésticos.| en-US<br/> zh-CN |
| MovieTickets | Reserva de entradas para ver películas en el cine.| en-US |
| Música | Reproducción de música en un reproductor.| en-US<br/> zh-CN |
| Nota: | El dominio de notas ofrece intenciones y entidades relacionadas con la creación, edición y búsqueda de notas.| en-US<br/> zh-CN |
| OnDevice | El dominio OnDevice ofrece intenciones y entidades relacionadas con el control del dispositivo.| en-US<br/> zh-CN |
| Lugares  | Control de consultas relacionadas con lugares, como empresas, instituciones, restaurantes, espacios públicos y direcciones.| en-US<br/> zh-CN |
| Reminder | Control de solicitudes relacionadas con la creación, edición y búsqueda de recordatorios.| en-US<br/> zh-CN |
| RestaurantReservation | Control de solicitudes para administrar reservas en restaurantes.| en-US<br/> zh-CN |
| Taxi | Control de reservas de un taxi.| en-US<br/> zh-CN |
| Traducir | Traducción de texto a otro idioma.| en-US<br/> zh-CN |
| TV | Control de TV.| en-US |
| Sectores públicos  | Control de solicitudes que son comunes en muchos dominios, como "ayuda", "repetir" o "volver a empezar".| en-US |
| Tiempo | Obtener informes y previsiones meteorológicos.| en-US<br/> zh-CN |
| Web | Navegar a un sitio web.| en-US<br/> zh-CN |

Para obtener más detalles sobre cada dominio, vea las secciones siguientes.

## <a name="calendar"></a>Calendario 

El dominio de calendario ofrece intenciones y entidades relacionadas con las entradas de calendario. Las intenciones de calendario incluyen la adición, eliminación o edición de una cita, la comprobación de disponibilidad y la búsqueda de información sobre una entrada o cita en el calendario.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Sumar | Agregar un nuevo elemento cada vez al calendario.| Concertar una cita con Lisa a las 14:00 horas el domingo <br/><br/>Quiero programar una reunión<br/><br/>Necesito convocar una reunión|
| CheckAvailability | Buscar disponibilidad para una cita o reunión en el calendario del usuario o en el calendario de otra persona.| ¿Cuándo está Jim disponible para una reunión? <br/><br/>Mostrar cuándo está disponible Carol mañana<br/><br/>¿Chris está disponible el sábado?|
| Eliminar | Solicitar la eliminación de una entrada del calendario.| Cancelar mi cita con Carol. <br/><br/>Eliminar mi reunión de las 9:00 horas<br/>|
| Edit | Solicitar la modificación de una reunión existente o de una entrada del calendario.| Cambiar la reunión de las 9:00 horas a las 10:00.<br/><br/>Quiero actualizar mi programa.<br/><br/>Reprogramar mi reunión con Ryan.|
| Buscar | Mostrar mi calendario semanal.| Buscar la cita de revisión con el dentista. <br/><br/>Mostrar mi calendario<br/>|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Ubicación | Ubicación de elementos, reuniones o citas del calendario. Las direcciones, las ciudades y las regiones son buenos ejemplos de ubicaciones.| 209 Nashville Gym <br/><br/>897 Pancake house<br/><br/>Garaje|
| Asunto | El título de una reunión o de una cita.| Cita con el dentista <br/><br/>Almuerzo con Julia<br/><br/>Cita con el médico|

## <a name="camera"></a>Cámara 
El dominio de cámara ofrece intenciones y entidades relacionadas con el uso de una cámara. Las intenciones abarcan la captura de una foto, selfie, captura de pantalla o vídeo, así como la difusión de vídeo en una aplicación.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| CapturePhoto| Capturar una foto.| Hacer una foto<br/><br/>Captura|
| CaptureScreenshot | Realizar una captura de pantalla.| Hacer una captura de pantalla.<br/><br/>Capturar la pantalla.|
| CaptureSelfie | Capturar un selfie.| Hacer un selfie <br/><br/>Hacerme una foto a mí mismo |
| CaptureVideo | Iniciar grabación de vídeo.| Iniciar grabación <br/><br/>Empezar a grabar|
| StartBroadcasting| Iniciar difusión de vídeo.| Iniciar difusión en Facebook|
| StartBroadcasting| Dejar de difundir vídeo.| Dejar de difundir|
| StopVideoRecording| Detener grabación de un vídeo.| Ya es suficiente<br/><br/>Dejar de grabar|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AppName | El nombre de una aplicación a la que difundir vídeo.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>Comunicación 
El dominio de comunicación ofrece intenciones y entidades relacionadas con el correo electrónico, los mensajes y las llamadas telefónicas.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AddContact| Agregar un contacto nuevo a la lista de contactos del usuario.|Agregar nuevo contacto <br/><br/>Guardar este número y registrarlo con el nombre de Carol|
| AddMore| Agregar más a un correo electrónico o texto, como parte de una composición de texto o de correo electrónico gradual.|Agregar más al texto <br/><br/>Agregar más al cuerpo del correo electrónico|
| Respuesta| Responder a una llamada telefónica entrante.|Responder la llamada <br/><br/>Aceptarla|
| AssignContactNickname| Asignar un alias a un contacto.|Cambiar Isaac por papá <br/>Editar el alias de Jim<br/>Agregar alias para Patti Owens|
| CallVoiceMail| Conectarse al buzón de voz del usuario.|Conectarme a mi buzón de voz <br/>Buzón de voz<br/>Llamar al buzón de voz|
| CheckIMStatus| Comprobar el estado de un contacto en Skype.|¿El estado en línea de Jim indica que está fuera? <br/>¿Carol está disponible para chatear?|
| Confirm| Confirmar una acción.|SÍ<br/>Correcto<br/>De acuerdo<br/>Confirmo que deseo enviar este correo electrónico.<br/>|
| Dial| Hacer una llamada telefónica.|Llamar a Jim<br/>Marcar 311<br/>|
| FindContact| Buscar información del contacto por nombre.|Buscar el número de Carol<br/>Mostrarme el número de Carol<br/>|
| FindSpeedDial| Buscar el número de marcación rápida establecido para un número de teléfono y viceversa.|¿A quién corresponde el número de marcación 5?<br/>¿Tengo establecida la marcación rápida?<br/>¿Cuál es el número de marcación rápida del número 941-5555-333?|
| GetForwardingsStatus| Obtener el estado actual del desvío de llamadas.|¿El desvío de llamadas está activado?<br/>Indicarme si mi estado de llamada está activado o desactivado<br/>|
| Goback| Volver al paso anterior.|Volver a Twitter<br/>Retroceder un paso<br/>Volver|
| Ignorar| Omitir una llamada entrante.|No responder<br/>Omitir llamada|
| IgnoreWithMessage| Omitir una llamada entrante y, en su lugar, responder con texto.|No responder a esa llamada pero enviar un mensaje en su lugar.<br/>Ignorar y enviar texto.|
| PressKey| Presione un botón o un número del teclado.|Marcar asterisco.<br/>Presionar 1 2 3.|
| ReadAloud| Leer un mensaje o un correo electrónico al usuario.|Leer el texto.<br/>¿Qué ha dicho en el mensaje?|
| TurnForwardingOff| Hacer una llamada telefónica.|<br/><br/>|
| Volver a marcar| Volver a marcar un número o rellamar a dicho número.|Volver a marcar.<br/>Volver a marcar mi última llamada.|
| Reject| Rechazar una llamada entrante.|Rechazar llamada<br/>No puedo responder ahora<br/>No está disponible en este momento; le devolverá la llamada más tarde.|
| SendEmail| Enviar un correo electrónico. Esta intención se aplica al correo electrónico, pero no a los mensajes de texto.|Enviar correo electrónico a Mike Waters: Mike, la cena de la semana pasada fue magnífica.<br/>Enviar un correo electrónico a Bob<br/>|
| SendMessage| Enviar un mensaje de texto o un mensaje instantáneo.|Enviar texto a Chris y Carol|
| SetSpeedDial| Establecer un acceso directo de marcación rápida para el número de teléfono de un contacto.|Establecer el número uno como marcación rápida para Carol.<br/>Configurar la marcación rápida para mamá.|
| ShowNext| Ver el siguiente elemento, por ejemplo, en una lista de mensajes de texto o de correos electrónicos.|Mostrar lo siguiente.<br/>Ir a la página siguiente.|
| ShowPrevious| Ver el elemento anterior, por ejemplo, en una lista de mensajes de texto o de correos electrónicos.|Mostrar lo anterior.<br/>Anterior<br/>Ir a anterior.|
| StartOver| Volver a iniciar el sistema o iniciar una nueva sesión.|Volver a empezar<br/>Nueva sesión<br/>restart|
| TurnForwardingOff| Desactivar el reenvío de llamadas.|Dejar de desviar mis llamadas<br/>Desactivar el desvío de llamadas|
| TurnForwardingOn| Desactivar el altavoz del teléfono.|Desviar mis llamadas a 3333<br/>Activar el desvío de llamadas a 3333|
| TurnSpeakerOff| Desactivar el altavoz del teléfono.|Deseo desactivar el altavoz.<br/>Desactivar el altavoz del teléfono.<br/>|
| TurnSpeakerOn| Activar el altavoz del teléfono.|Modo de altavoz.<br/>Encender el altavoz.<br/>|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AudioDeviceType | Tipo de dispositivo de audio (altavoces, auriculares, micrófono, etc.).| Orador<br/>Manos libres<br/>Bluetooth|
| Categoría | La categoría de un mensaje o correo electrónico.| Importante<br/>Prioridad alta|
| ContactAttribute | Un atributo del contacto sobre el que el usuario pregunta.| Cumpleaños<br/>Dirección<br/>Número de teléfono|
| ContactName | El nombre de un contacto o del destinatario de un mensaje.| Carol<br/>Jim<br/>Chris|
| EmailSubject | El texto que se utiliza como la línea de asunto de un correo electrónico.| RE: caso interesante|
| Línea | La línea que el usuario desea usar para realizar una llamada o enviar un texto o correo electrónico.| Línea del trabajo<br/>Teléfono británico<br/>Skype|
| Message | Mensaje que se va a enviar como un texto o correo electrónico.| Ha sido un auténtico placer verlo hoy. Espero verlo pronto.|
| MessageType | El nombre de un contacto o del destinatario de un mensaje.| Texto<br/>Email|
| OrderReference | La posición ordinal o relativa en una lista, que identifica un elemento que se va a recuperar. Por ejemplo, "último" o "reciente" en "¿cuál fue el último mensaje enviado?"| Último<br/>Reciente|
| SenderName | El nombre del remitente.| Patti Owens|

## <a name="entertainment"></a>Entretenimiento  
El dominio de entretenimiento ofrece intenciones y entidades relacionadas con la búsqueda de películas, música, juegos y programas de TV.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Search| Búsqueda de películas, música, aplicaciones, juegos y programas de TV.|Buscar Halo en la tienda.<br/>Buscar Avatar.|

### <a name="entities"></a>Entidades
| Nombre de entidades | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| ContentRating | Clasificación de contenido multimedia como G o R para las películas.|Vídeo para niños.<br/>Clasificación PG.|
| Género | Género de una película, juego, aplicación o canción.|Comedias<br/>Dramas<br/>Divertido|
| Palabra clave| Una palabra clave de búsqueda genérica que especifica un atributo que no existe en los espacios multimedia más específicos.|Soundtracks<br/>Moon River<br/>Amelia Earhart|
| Idioma | Idioma hablado en los medios, como el idioma oral de una película o una canción.|Francés<br/>English<br/>Coreano|
| MediaFormat | El tipo técnico especial adicional en que se da formato al elemento multimedia.|Películas de alta definición<br/>Películas tridimensionales<br/>Descargable|
| MediaSource | La tienda o el marketplace donde adquirir el elemento multimedia.|Netflix<br/>Prime|
| MediaSubTypes| Tipos de elementos multimedia más pequeños que las películas y los juegos.|Demostraciones<br/>DLC<br/>Clips finales|
| Nacionalidad| El país en que se creó una película, un programa o una canción.|Francés<br/>Alemán<br/>Coreano|
| Persona| El actor, director, productor, músico o artista asociados con una película, aplicación, juego o programa de televisión.|Madonna<br/>Stanley Kubrick|
| Rol| Rol que desempeña una persona en la creación de un elemento multimedia.|Canciones<br/>Dirigido por<br/>Por|
| Título| El nombre de una película, aplicación, juego, programa de TV o canción.|Friends<br/>Minecraft|
| Escriba| El tipo o formato multimedia de una película, aplicación, juego, programa de TV o canción.|Música<br/>Películas de TV <br/>Programas|
| UserRating| Clasificación del usuario con estrellas o con el dedo pulgar hacia arriba o hacia abajo.|5 estrellas<br/>3 estrellas<br/>4 estrellas|

## <a name="events"></a>Eventos 
El dominio de eventos proporciona intenciones y entidades relacionadas con la reserva de entradas para eventos como conciertos, festivales, eventos deportivos y espectáculos cómicos.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Book| Comprar entradas para un evento.|Me gustaría comprar una entrada para el concierto sinfónico de este fin de semana.|


### <a name="entities"></a>Entidades
| Nombre de entidades | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Dirección | Ubicación o dirección del evento. |Palo Alto<br/>300 112th Ave SE <br/> Seattle |
| NOMBRE | El nombre de un evento.|Shakespeare in the Park|
| PlaceName| El nombre de ubicación del evento.|Louvre<br/>Opera House<br/>Broadway|
| PlaceType | El tipo de ubicación en que se celebrará el evento.|Bar<br/>Teatro<br/>Biblioteca|
| Escriba | El tipo de evento.|Concierto<br/>Encuentro deportivo|

## <a name="fitness"></a>Fitness 
El dominio de fitness ofrece intenciones y entidades relacionadas con el control de actividades de fitness. Las intenciones incluyen guardar notas, el tiempo restante o la distancia, así como guardar los resultados de la actividad.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AddNote| Agregar notas adicionales a una actividad con seguimiento.|La dificultad de esta carrera era de 6/10<br/>El terreno en el que corro es asfalto<br/>Estoy utilizando una bicicleta de 3 velocidades|
|GetRemaining| Obtiene la distancia o el tiempo restante de una actividad.|¿Cuánto tiempo queda para la siguiente vuelta?<br/>¿Cuántos kilómetros de carrera me faltan? ¿Cuánto tiempo para el tramo?|
| LogActivity| Guardar o registrar los resultados de la actividad completada.|Guardar mi última carrera<br/>Registrar mi recorrido del sábado por la mañana<br/>Guardar mi actividad anterior en natación|
| LogWeight| Guardar o registrar el peso actual del usuario.|Guardar mi peso actual<br/>Registrar mi peso ahora<br/>Guardar mi peso corporal actual|

### <a name="entities"></a>Entidades
| Nombre de entidades | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| ActivityType | El tipo de actividad de la que se va a realizar un seguimiento. |Ejecute<br/>Caminata<br/>Natación<br/>Ciclo |
| Alimentos | Un tipo de alimento del que se va a realizar un seguimiento en una aplicación de fitness. |Banana<br/>Salmón<br/>Batido de proteínas|
| MealType| El tipo de comida del que se va a realizar un seguimiento en una aplicación de salud o fitness.|Desayuno<br/>Cena<br/>Comida<br/>Refrigerio|
| Medición| Un tipo de mediciones de tiempo, distancia o peso para utilizarlas en una aplicación de fitness o salud.|Kilómetros<br/>Millas<br/>Minutos<br/>Kilogramos|
| Number | Cantidad numérica que se va a usar en una aplicación de fitness o salud.|19<br/>three<br/>200<br/>one|
| StatType | Un tipo de estadística de datos agregados, para utilizarlo en una aplicación de fitness o salud.|Suma<br/>Media<br/>Máxima<br/>Mínima|

## <a name="gaming"></a>Juegos 
El dominio de juegos ofrece intenciones y entidades relacionadas con la administración de una partida de un juego multijugador.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| InviteParty| Invitar a un contacto a jugar una partida.|Invitar a este jugador a mi partida<br/>Participar en mi partida<br/>Unirse a mi equipo|
|LeaveParty| Obtiene la distancia o el tiempo restante de una actividad.|Me salgo<br/>Dejo esta partida por otra<br/>Me voy a salir|
| StartParty| Iniciar una partida de un juego multijugador.|Colega, vamos a empezar una partida<br/>Empezar una partida<br/>Podríamos empezar partida esta noche|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Contacto| El nombre de un contacto para usarlo en un juego multijugador.|Carol<br/>Jim|


## <a name="homeautomation"></a>HomeAutomation 
El dominio HomeAutomation ofrece intenciones y entidades relacionadas con el control de dispositivos domésticos inteligentes, como luces y electrodomésticos.

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| TurnOff| Apagar, cerrar o desbloquear un dispositivo.|Apagar las luces<br/>Parar la cafetera<br/>Cerrar la puerta del garaje|
|TurnOn| Encender un dispositivo o definirlo en un ajuste o modo particular.|Encender mi cafetera<br/>¿Puede encender mi cafetera?<br/>Establezca el termostato en 72 grados.|


### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Dispositivo | Un tipo de dispositivo que se puede activar o desactivar.|Cafetera<br/>Termostato<br/>Luces|
| Operación | El estado en el que definir el dispositivo.|lock<br/>abierto<br/>en<br/>apagado|
| Sala | La ubicación o estancia en que se encuentra el dispositivo.|Salón<br/>Habitación<br/>Cocina|

## <a name="movietickets"></a>MovieTickets 
El dominio MovieTickets ofrece intenciones y entidades relacionadas con la reserva de entradas para el cine.

### <a name="examples"></a>Ejemplos
```
Book me two tickets for Captain Omar and the two Musketeers
Cancel tickets
When is Captain Omar showing?
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Book | Comprar entradas para el cine.|Reservar dos entradas para el Capitán Omar y los dos mosqueteros<br/>Quiero comprar una entrada para la película de mañana<br/>Quiero una entrada para Capitán Omar Parte 2 el próximo miércoles|
|GetShowTime| Obtener la hora de inicio de una película.|¿A qué hora empieza el Capitán Omar?|


### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Dirección | La dirección de un cine.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| MovieTitle | El título de una película.|La vida de Pi<br/>Los juegos del hambre<br/>Origen|
| PlaceName | El nombre de un cine o de una sala de cine.|Cinema Amir<br/>Alexandria Theatre<br/>New York Theater|
| PlaceType | El tipo de ubicación en que se proyecta una película.|cine<br/>teatro<br/>Cine IMAX|

## <a name="music"></a>Música 
El dominio de música proporciona intenciones y entidades relacionadas con la reproducción de música en un reproductor.

### <a name="examples"></a>Ejemplos
```
play Beethoven
Increase track volume
Skip to the next song
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| DecreaseVolume | Bajar el volumen del dispositivo.|Reducir el volumen de pista<br/>Bajar el volumen|
| IncreaseVolume | Subir el volumen del dispositivo.|Subir el volumen de pista<br/>Subir el volumen|
| Mute |Silenciar la reproducción de música.|Silenciar canción<br/>Silenciar la pista<br/>Silenciar música |
| Pausar | Pausar la reproducción de música.|Pausar<br/>Pausar música<br/>Pausar pista|
| PlayMusic | Reproducir música en un dispositivo.|Reproducir Kevin Durant<br/>Reproducir Paradise de Coldplay<br/>Reproducir Hello de Adele|
| Repetir |Repetir la reproducción de música.|Repetir canción<br/>Volver a reproducir la pista<br/>Repetir música|
| Reanudación | Reanudar la reproducción de música.|Reanudar canción<br/>Empezar a escuchar música otra vez<br/>Desactivar la pausa|
| SkipBack | Retroceder una pista.|Pasar a la siguiente canción<br/>Reproducir la siguiente canción|
| SkipForward |Adelantar una pista.|Reproducir la canción anterior<br/>Volver a la pista anterior |
| Stop | Detener una acción relacionada con la reproducción de música. |Dejar de reproducir este álbum.|
| Reactivar audio | Reactivar el audio de un dispositivo de reproducción de música.| Reactivar el audio.|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| ArtistName | El actor, director, productor, escritor, músico o artista asociados con un elemento multimedia que se va a reproducir en un dispositivo.|Elvis Presley<br/>Taylor Swift<br/>Adele<br/>Mozart|
| Género | El género de la música solicitada.|Música country<br/>Música clásica Broadway<br/>Reproducir mi música clásica de la época barroca|

## <a name="note"></a>Nota: 
El dominio de notas ofrece intenciones y entidades relacionadas con la creación, edición y búsqueda de notas.

### <a name="examples"></a>Ejemplos
```
Add to my groceries note lettuce tomato bread coffee
Check off bananas from my grocery list
Remove all items from my vacation list
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AddToNote | Agregar información a una nota.|Agregar a mi lista de la compra lechuga, tomate, pan y café<br/>Agregar a mi lista de tareas pendientes<br/>Agregar pastelitos a mi lista de la compra|
| CheckOffItem | Seleccionar los elementos de una nota preexistente.|Seleccionar los plátanos de mi lista de la compra<br/>Marcar como comprada la tarta de queso de mi lista de la compra de las vacaciones|
| Clear | Borrar todos los elementos de una nota preexistente.|Quitar todos los elementos de mi lista de vacaciones<br/>Borrar todo de mi lista de lectura|
| Confirm | Confirmar una acción relacionada con una nota.|Me parece bien<br/>Sí<br/>Voy a confirmar que deseo mantener todos los elementos de las listas|
| Crear | Crear una nota. | Crear una lista<br/>Crear una nota para recordarme que Jason estará en la ciudad la primera semana de mayo|
| Eliminar | Eliminar una nota completa. |Eliminar mi lista de vacaciones <br/>Eliminar mi nota de la compra|
| DeleteNoteItem | Eliminar los elementos de una nota preexistente.| Eliminar las patatas fritas de mi lista de la compra<br/>Quitar los bolígrafos de mi lista de la compra para el colegio|
| ReadAloud | Leer en voz alta una lista.|Leerme la primera<br/>Leerme los detalles|
| ShowNext | Ver el elemento siguiente en una lista de notas.|Mostrar el siguiente<br/>Página siguiente<br/>Pasos siguientes|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AppName | El nombre de la aplicación para tomar notas.|Wunderlist<br/>OneNote|
| ContactName | El nombre de un contacto en una nota.|Carol<br/>Jim<br/>Chris|
| DataSource | Ubicación de las notas.|OneDrive<br/>Google Docs<br/>Mi PC|
| DataType | El tipo de archivo o documento, asociado normalmente con programas de software específicos.|Diapositivas<br/>Hoja de cálculo<br/>Hoja de cálculo|
| Texto | El texto de una nota o un aviso.|Estirar antes de andar<br/>Correr mucho mañana|
| Título | Título de una nota.|Lista de la compra<br/>Personas a las que llamar<br/>Tareas pendientes|

## <a name="ondevice"></a>OnDevice 
El dominio OnDevice ofrece intenciones y entidades relacionadas con el control del dispositivo.

### <a name="examples"></a>Ejemplos
```
Close video player
Cancel playback
Can you make the screen brighter?
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AreYouListening | Preguntar si el dispositivo está escuchando.|¿Está activado?<br/>¿Está escuchando?|
|CloseApplication|Cerrar la aplicación del dispositivo.|Cerrar el reproductor de vídeo|
|FileBug|Notificar un error en el dispositivo.|Notificar un error<br/>¿Puede notificar un error por mí?<br/>Permitirme notificar este error|
|Goback|Solicitar retroceder un paso o volver al paso anterior.|Volver atrás<br/>Volver a la pantalla anterior<br/>Volver atrás para dejar de escuchar|
|Ayuda| Solicitar ayuda.|Ayuda<br/>Hola<br/>¿Qué puede hacer?<br/>Necesito ayuda| 
|LocateDevice|Localizar el dispositivo.|¿Puede localizar mi teléfono?<br/>Buscar el iPhone de Tom<br/>Buscar mi teléfono|
|LogIn|Iniciar sesión en un servicio con el dispositivo.|Iniciar sesión<br/>Inicio de sesión en Facebook<br/>Iniciar sesión en LinkedIn|
|LogOut|Cerrar sesión en un servicio con el dispositivo.|Cerrar la sesión de mi teléfono<br/>Conectar con Twitter<br/>Cerrar la sesión|
|MainMenu|Ver el menú principal de un dispositivo.|Ver menú.|
|OpenApplication|Abrir una aplicación en el dispositivo.|Abrir la alarma<br/>Encender la cámara<br/>Iniciar el calendario|
|OpenSetting|Abrir una configuración en el dispositivo.|Abrir la configuración de red.|
|PairDevice|Emparejar el dispositivo.|¿Puede ayudarme a emparejar la señal Bluetooth con el teléfono?<br/>Activar el Bluetooth y emparejarlo con mi portátil<br/>Emparejar la señal Bluetooth con mi portátil|
|PowerOff | Apagar el dispositivo.|¿Puede apagar mi equipo?<br/>Shutdown<br/>Apagar mi móvil|
|QueryBattery|Obtener información sobre la duración de la batería.|Mostrarme la duración de la batería.<br/>¿Cuál es el estado de mi batería?<br/>¿Cuánta batería queda ahora?<br/>Mostrarme la batería|
|QueryWifi|Obtener información sobre WiFi.|Obtener detalles de Wi-Fi.|
|Reinicio|Reiniciar el dispositivo.|Reiniciar.|
|RingDevice| Pedir que el dispositivo suene, para encontrarlo cuando se pierda.|Llamar a mi teléfono.| 
|SetBrightness|Establecer el brillo del dispositivo.|Configurar el brillo en medio<br/>Configurar el brillo en alto<br/>Configurar el brillo en bajo|
|SetupDevice|Iniciar la configuración del dispositivo.|Deseo instalar la configuración del sistema operativo<br/>Configurar<br/>Configurar por mí|
|ShowAppBar|Mostrar una barra de aplicaciones.|Mostrarme la barra de aplicaciones<br/>Barra de aplicaciones<br/>Permitirme ver la barra de aplicaciones|
|ShowContextMenu|Mostrar un menú contextual.|Dejarme ver el menú contextual<br/>Menú contextual<br/>¿Puede mostrarme el menú contextual?|
|Sleep|Poner el dispositivo en reposo.|Entrar en reposo<br/>En reposo<br/>Mi equipo en reposo|
|SwitchApplication|Cambiar la aplicación para usarla en el dispositivo.|Cambiar a mi reproductor multimedia.|
|TurnDownBrightness|Reducir el brillo del dispositivo.|Atenuar la pantalla.|
|TurnOffSetting|Desactivar una configuración del dispositivo.|Desactivar Bluetooth<br/>Deshabilitar datos<br/>Desconectar Bluetooth|
|TurnOnSetting|Activar una configuración del dispositivo.|Por <br/> Off|
|TurnUpBrightness|Aumentar el brillo del dispositivo.|¿Puede aumentar el brillo de la pantalla?|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AppName | Asignar nombre a una aplicación en el dispositivo.|SoundCloud<br/>YouTube|
| BrightnessLevel | Definir el nivel de brillo en el dispositivo.|100 %<br/>Cincuenta<br/>40%|
| ContactName | El nombre de un contacto del dispositivo.|Paul<br/>Marlen Max|
| DeviceType | El tipo de dispositivo. |Teléfono<br/>Kindle<br/>Equipo portátil|
| MediaType | El tipo de elemento multimedia que administra el dispositivo.|Música<br/>Película<br/>Programas de TV|
| SettingType | Un tipo de configuración o panel de configuración que el usuario desea editar.|Wi-Fi<br/>Red inalámbrica<br/>Combinación de colores<br/>Centro de notificaciones|

## <a name="places"></a>Lugares  
El dominio de lugares ofrece intenciones para controlar consultas relacionadas con lugares, como empresas, instituciones, restaurantes, espacios públicos y direcciones.

### <a name="examples"></a>Ejemplos
```
Save this location to my favorites
How far away is Holiday Inn?
At what time does Safeway close?
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Agregar una ubicación a la lista de favoritos del usuario.|Guardar esta ubicación en Mis favoritos<br/>Agregar esta dirección a Mis favoritos|
|CheckAccident|Preguntar si hay un accidente en una carretera concreta.|¿Hay un accidente en 880?<br/>Mostrarme información sobre accidentes|
|CheckAreaTraffic|Comprobar el tráfico de una carretera o un área general, no de una ruta específica.|Tráfico de Seattle<br/>¿Cómo está el tráfico en Seattle?|
|CheckIntoPlace|Registrarse en un lugar mediante una red social.|Registrarme en Foursquare<br/>Registrarme aquí|
|CheckRouteTraffic| Comprobar el tráfico de una ruta específica definida por el usuario.|¿Cómo está el tráfico en Mashiko?<br/>Mostrarme el tráfico en Kirkland<br/>¿Cómo está el tráfico en Seattle?| 
|Confirm|Confirmar una acción relacionada con un lugar.|Confirmar mi reserva en el restaurante.|
|Salir|Acción para cerrar una tarea relacionada con un lugar.|Salir<br/>Dejar de proporcionarme direcciones|
|FindPlace|Buscar un lugar (empresa, institución, restaurante, espacio público o dirección).|¿Dónde está la biblioteca más próxima?<br/>Encontrarme un buen restaurante italiano en Mountain View|
|GetAddress| Preguntar la dirección de un lugar.|Mostrarme la dirección de la calle Guu on Robson<br/>¿Cuál es la dirección de la cafetería Starbucks más cercana?| 
|GetDistance|Preguntar la distancia hasta un lugar específico.|¿A qué distancia está Holiday Inn?<br/>¿A qué distancia está Bellevue square de aquí?<br/>¿A qué distancia está Tahoe?|
|GetHours|Preguntar el horario comercial de un lugar.|¿A qué hora cierra Safeway?<br/>¿Cuál es el horario comercial de Home Depot?<br/>¿Starbucks todavía está abierto?|
|GetMenu|Preguntar los elementos de menú de un restaurante.|¿Zucca sirve comida vegana?<br/>¿Qué hay en el menú de Sizzler?<br/>Mostrarme el menú de Applebee|
|GetPhoneNumber| Solicitar el número de teléfono de un lugar.|¿Cuál es el número de teléfono de la cafetería Starbucks más cercana?<br/>Dar el número de Home Depot| 
|GetPriceRange| Preguntar las tarifas de un lugar.|¿Zucca es barato?<br/>¿Cineplex tiene las tarifas a mitad de precio los miércoles?<br/>¿Cuánto cuesta una langosta entera en Sizzler?|
|GetReviews|Solicitar opiniones de un lugar.|Mostrarme las opiniones sobre Cheesecase Factory<br/>Leer las opiniones sobre Cineplex en Yelp|
|GetRoute|Preguntar por la dirección de un lugar.|¿Cómo puedo llegar caminando a Bellevue square?<br/>Mostrarme la ruta más corta hasta 8th y 59th desde aquí<br/>Mostrarme las direcciones hasta Mountain View CA|
|GetStarRating|Solicitar la clasificación por estrellas de un lugar.|¿Cuál es la clasificación de Zucca según Yelp?<br/>¿Cuántas estrellas tiene The French Laundry?<br/>¿El acuario de Monterrey es bueno?|
|GetTransportationSchedule|Consultar el horario de autobuses de un lugar.|¿A qué hora pasa el próximo autobús para ir al centro?<br/>Mostrarme los autobuses que van a King County|
|GetTravelTime|Preguntar la duración del viaje hasta un destino específico.|¿Cuánto tiempo se tarda en llegar a San Francisco desde aquí?<br/>¿Cuánto tiempo se tarda en coche hasta Denver desde SF?|
|MakeCall|Realizar una llamada de teléfono a un lugar.|Llamar a mamá<br/>Me gustaría realizar una llamada de Skype a Ana<br/>Llamar a Jim|
|MakeReservation|Solicitar una reserva en un restaurante o en otro lugar.|Reservar en Zucca para dos esta noche<br/>Reservar una mesa para mañana<br/>Mesa para tres en Palo Alto para las 8:00|
|MapQuestions|Solicitar información sobre direcciones o si una carretera específica llega a un destino.|¿El 13 atraviesa el centro?<br/>¿Puedo ir por la 880 hasta Oakland?|
|Rating|Obtener la descripción de la clasificación de un restaurante o lugar.|¿Cuántas estrellas tiene Contoso Inn?|
|ReadAloud|Leer una lista de lugares en voz alta.|Leerme la primera<br/>Leerme los detalles|
|SelectItem|Elegir un elemento de una lista de opciones relacionadas con uno o varios lugares.|Seleccionar el segundo<br/>Seleccionar el primero|
|ShowMap|Mostrar el mapa de un área.|Mostrar un mapa para la segunda opción<br/>Mostrar mapa<br/>Buscar San Francisco en el mapa|
|ShowNext|Mostrar el elemento siguiente de una serie.|Mostrar el siguiente<br/>Ir a la página siguiente|
|ShowPrevious|Mostrar el elemento anterior de una serie.|Mostrar anterior<br/>previous<br/>Ir a anterior|
|StartOver|Reiniciar la aplicación o iniciar una nueva sesión.|Volver a empezar<br/>Nueva sesión<br/>
restart|
|TakesReservations|Preguntar si un lugar acepta reservas.|¿La galería de arte acepta reservas?<br/>Es posible realizar una reserva en Olive Garden

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| AbsoluteLocation | La ubicación o dirección de un lugar.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Amenities | Características y ventajas objetivas de un lugar.|Los niños comen gratis<br/>Frente al mar<br/>Aparcamiento gratuito|
| Atmosphere | El ambiente de un lugar.|Atractivo para los niños<br/>Restaurante convencional<br/>Deportivo|
| Cuisine | La cocina de un lugar. |Mediterránea<br/>Italiano<br/>India|
| DestinationAddress| La ubicación o dirección de un destino.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName| El nombre de un destino que es un negocio, restaurante, atracción pública o institución.|Central Park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | El tipo de un destino que es un negocio local, restaurante, atracción pública o institución. |Restaurante<br/>Opera<br/>Cine|
| Distancia | La distancia hasta un lugar.|15 millas<br/>5 millas<br/>10 millas|
| MealType | Tipo de comida como desayuno o almuerzo. |Desayuno<br/>Cena<br/>Almuerzo<br/>Refrigerio|
| OpenStatus | Indica si un lugar está abierto o cerrado.|Abrir<br/>closed<br/>Abriendo|
| PlaceName | El nombre de un lugar.|Cheesecake Factory|
| PlaceType | El tipo de lugar.|Bar<br/>Teatro<br/>Biblioteca|
| PreferredRoute | La ruta preferida especificada por el usuario. | 101 <br/>202 <br/>Route 401|
| Producto | El producto que ofrece un lugar. | Ropa<br/>Cámaras digitales ASR<br/>Pescado fresco | 
| PublicTransportationRoute | El nombre de la ruta de transporte público que el usuario está buscando. | Línea ferroviaria Corredor Noreste<br/>Ruta de autobús 3 X |
| Rating | La clasificación de un lugar. | 5 estrellas<br/>3 estrellas<br/>4 estrellas|
| RouteAvoidanceCriteria | Criterios para evitar rutas específicas, como accidentes, obras o peajes | Peajes <br/>Obras<br/>Carretera 11|
| ServiceProvided | Se trata del servicio que presta un negocio o lugar, como corte de pelo, quitanieves o paisajismo. | Corte de pelo<br/>Mecánico<br/>Fontanero|
| TransportationCompany | El nombre de un proveedor de transporte.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | El tipo de transporte.|Autobús<br/>Train<br/>Conducción|

## <a name="reminder"></a>Reminder 
El dominio de recordatorio ofrece intenciones y entidades relacionadas con la creación, edición y búsqueda de recordatorios.

### <a name="examples"></a>Ejemplos
```
Change my interview to 9 am tomorrow
Remind me to buy milk on my way back home
Can you check if I have a reminder about Christine's birthday?
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Change| Cambiar un recordatorio.|Cambiar mi entrevista a las 9:00 horas de mañana<br/>Mover el recordatorio de mi tarea a mañana|
| Crear| Crear un recordatorio.|Crear un aviso<br/>Recordarme que tengo que comprar leche<br/>Quiero acordarme de llamar a Rebecca cuando llegue a casa|
| Eliminar | Eliminar un recordatorio.|Eliminar el recordatorio de mi imagen<br/>Eliminar este recordatorio|
| Buscar | Buscar un recordatorio.|¿Tengo algún recordatorio de mi aniversario?<br/>¿Puede comprobar si tengo algún recordatorio del cumpleaños de Christine?|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Texto | Descripción de texto de un recordatorio.|Recoger la colada cuando esté seca<br/>Llevar mi coche al taller|

## <a name="restaurantreservation"></a>RestaurantReservation 
El dominio RestaurantReservation ofrece intenciones y entidades relacionadas con la administración de reservas en restaurantes.

### <a name="examples"></a>Ejemplos
```
Reserve at Zucca for two for tonight
Book a table at BJ's for tomorrow
Table for 3 in Palo Alto at 7
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Reservar | Solicitar una reserva en un restaurante. |Reservar en Zucca para dos esta noche<br/>Reservar una mesa para mañana<br/>Mesa para tres en Palo Alto para las 7:00|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Dirección| La ubicación o dirección de un evento para una reserva.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Amenities | Un atributo que describe los servicios de un lugar.|Vistas al mar<br/>Prohibido fumar|
| AppName | El nombre de una aplicación para realizar reservas.|OpenTable<br/>Yelp<br/>TripAdvisor|
| Atmosphere | Una descripción del ambiente de un restaurante o de otro lugar.|Romántico<br/>Casual<br/>Ideal para grupos|
| Cuisine | Un tipo de comida, plato o gastronomía típica. |Chino<br/>Italiano<br/>Mejicano|
| MealType | El tipo de comida asociado a una reserva.|Desayuno<br/>Cena<br/>Almuerzo<br/>Refrigerio|
| PlaceName | El nombre de un negocio local, restaurante, atracción pública o institución.|IHOP<br/>Cheesecake Factory<br/>Louvre|
| PlaceType | El tipo de negocio local, restaurante, atracción pública o institución.|restaurante<br/>ópera<br/>cine|
| Rating | La clasificación de un lugar o restaurante.|5 estrellas<br/>3 estrellas<br/>4 estrellas|

## <a name="taxi"></a>Taxi 
 
El dominio Taxi ofrece intenciones y entidades para crear y administrar las reservas de taxis.

### <a name="examples"></a>Ejemplos
```
Get me a cab at 3 pm
How much longer do I have to wait for my taxi?
Cancel my Uber
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Book | Llamar a un taxi. |Llamarme a un taxi<br/>Buscar un taxi<br/>Reservarme un uberX|
| Cancelar | Cancelar una acción relacionada con la reserva de un taxi.|Cancelar mi taxi<br/>Cancelar mi Uber|
| Track | Realizar un seguimiento de una ruta de taxi.|¿Cuánto tiempo tengo que esperar el taxi?<br/>¿Dónde está mi Uber?|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Dirección| La dirección asociada a la reserva de un taxi. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationAddress| La ubicación o dirección de un destino. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName | El nombre de un destino que es un negocio local, restaurante, atracción pública o institución. |Central Park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | El tipo de un destino que es un negocio local, restaurante, atracción pública o institución. |Restaurante<br/>Opera<br/>Cine|
| PlaceName | Nombre de un negocio local, restaurante, atracción pública o institución. |Central Park<br/>Safeway<br/>Walmart|
| PlaceType| Tipo de lugar en el que se realiza la solicitud de reserva de un taxi.|Restaurante<br/>Opera<br/>Cine|
| TransportationCompany | El nombre de un proveedor de transporte.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | El tipo de transporte.|Autobús<br/>Train<br/>Conducción|

## <a name="translate"></a>Traducir 
El dominio de traducción ofrece intenciones y entidades relacionadas con la traducción de texto a un idioma de destino.

### <a name="examples"></a>Ejemplos
```
Translate to French
Translate hello to German
Translate this sentence to English
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Traducir| Traducir texto a otro idioma.|Traducir al francés<br/>Traducir hola en alemán|


### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| TargetLanguage | El idioma de destino de una traducción.|Francés<br/>Alemán<br/>Coreano|
| Texto | Texto que se traducirá.|Hello World<br/>Good morning<br/>Good evening|

## <a name="tv"></a>TV 
 
El dominio de TV ofrece intenciones y entidades para controlar la televisión.

### <a name="examples"></a>Ejemplos
```
Switch channel to BBC
Show TV guide
Watch National Geographic
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| ChangeChannel| Cambiar de canal en una televisión.|Cambiar canal a la CNN<br/>Cambiar canal a la BBC<br/>Ir al canal 4|
| ShowGuide| Mostrar la guía de televisión.|Mostrar guía de TV<br/>¿Qué hay ahora en Movie Channel?<br/>Mostrar mi lista de programas|
| WatchTV| Solicitud para ver un canal de TV.|Quiero ver Disney Channel<br/>Ir a TV<br/>Ver National Geographic|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| ChannelName | El nombre de un canal de TV.|CNN<br/>BBC<br/>Movie Channel|

## <a name="utilities"></a>Sectores públicos  
El dominio de sectores públicos ofrece intenciones para tareas comunes de muchas tareas, como saludos, cancelaciones, confirmaciones, ayuda, repetición, navegación, inicio y detención.

### <a name="examples"></a>Ejemplos
```
Go back to Twitter
Please help
Repeat last question please
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Cancelar | Cancelar una acción.|Cancelar el mensaje<br/>Ya no deseo enviar el correo electrónico|
| Confirm | Confirmar una acción.|Sí, claro que confirmo<br/>Bien, voy a confirmar<br/>Perfecto, voy a confirmar|
| FinishTask | Finalizar una tarea que el usuario inició.|Ya he terminado<br/>He acabado<br/>Listo|
| Goback | Retroceder un paso o volver al paso anterior.|Volver a Twitter<br/>Retroceder un paso<br/>Volver|
| Ayuda | Pedir ayuda.|Ayuda, por favor<br/>Abrir la ayuda<br/>help|
| Repetir | Repetir una acción.|Repetir la última pregunta<br/>Repetir la última canción|
| ShowNext | Mostrar el elemento siguiente de una serie. |Mostrar el siguiente<br/>Ir a la página siguiente|
| ShowPrevious | Mostrar el elemento anterior de una serie.|Mostrar anterior|
| StartOver | Reiniciar la aplicación o iniciar una nueva sesión.|Volver a empezar<br/>Nueva sesión<br/>restart|
| Stop | Detener una acción.| Dejar de decir esto<br/>Callarse<br/>Pararse|

## <a name="weather"></a>Tiempo 
El dominio de tiempo ofrece intenciones y entidades para obtener informes y previsiones meteorológicos.

### <a name="examples"></a>Ejemplos
```
weather in London in september
What?s the 10 day forecast?
What's the average temperature in India in september?
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| GetCondition | Obtener datos históricos relacionados con el tiempo. |Tiempo en Londres en septiembre<br/>¿Cuál es la temperatura media en la India en septiembre?|
| GetForecast | Obtener el tiempo actual y la previsión para los próximos días. |¿Cómo está el tiempo hoy?<br/>¿Cuál es la previsión para los 10 próximos días?<br/>¿Cómo estará el tiempo este fin de semana?|

### <a name="entities"></a>Entidades
| Nombre de entidad | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Ubicación| La ubicación absoluta de una solicitud de tiempo.|Seattle<br/>París<br/>Palo Alto|

## <a name="web"></a>Web 
El dominio Web ofrece una intención para navegar a un sitio web.

### <a name="examples"></a>Ejemplos
```
Navigate to facebook.com
Go to www.twitter.com
Navigate to www.bing.com
```

### <a name="intents"></a>Intenciones
| Nombre de intención | DESCRIPCIÓN | Ejemplos |
| ---------------- |-----------------------|----|
| Navegar | Una solicitud para navegar a un sitio web específico. |Navegar a facebook.com<br/>Ir a www.twitter.com|

