---
title: 'Referencia del dominio precompilado: LUIS'
titleSuffix: Azure Cognitive Services
description: La referencia de dominios creados previamente, que son colecciones creadas previamente de intenciones y entidades de Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: b840f1ce42c9d7e4af8854a2c6bd7fd26f5b88e9
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70307446"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Referencia de dominio creado previamente para la aplicación de LUIS
Esta referencia ofrece información sobre los [dominios creados previamente](luis-how-to-use-prebuilt-domains.md), que son colecciones creadas previamente de intenciones y entidades que LUIS ofrece.

Los [dominios personalizados](luis-how-to-start-new-app.md), por el contrario, se inician sin intenciones ni modelos. Puede agregar cualquier intención y entidad de dominios creados previamente a un modelo personalizado.

# <a name="supported-domains-across-cultures"></a>Dominios admitidos en distintas referencias culturales

En la tabla siguiente se resumen los dominios admitidos actualmente. La compatibilidad con el inglés suele ser más completa que otras. 

| Tipo de entidad       | EN-US      | ZH-CN   | DE    | VF     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| [Calendar](#calendar)    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Communication](#communication)   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Correo electrónico](#email)           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [HomeAutomation](#homeautomation)           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Notas](#notes)      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Places](#places)    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [RestaurantReservation](#restaurantreservation)   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [ToDo](#todo)     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Utilities](#utilities)          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Weather](#weather)        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Web](#web)    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Los dominios pregenerados **no se admiten** en:

* Francés (canadiense)
* Hindi
* Español (México)

# <a name="description-for-luis-prebuilt-domains"></a>Descripción de los dominios pregenerados de LUIS
## <a name="calendar"></a>**Calendar**
Calendar no tiene que ver con reuniones personales y citas, ni sobre eventos públicos (como horarios de la copa del mundo, calendarios de eventos de Seattle) o calendarios genéricos (por ejemplo, qué día es hoy, cuándo comienza el otoño o cuándo es el Día del trabajo).
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------------
 AcceptEventEntry | Aceptar una cita, una reunión o un evento en el calendario. | Accept an appointment. (Aceptar una cita) <br> Accept the event (Aceptar el evento) <br> accept today's meeting. (aceptar la reunión de hoy)
 Cancelar | Cancelar la acción en curso mediante el asistente virtual, como cancelar el proceso de creación de una reunión. <br> ***Aviso**: Esta intención incluye principalmente la acción "Cancel" en el escenario Calendar. Si necesita una expresión general en "Cancel", aproveche la intención "Cancel" en el dominio de **Utilities**.* | It's ok, just cancel the event. (De acuerdo, solo cancelar el evento) <br> No, I just cancel the appointment. (No, acabo de cancelar la cita)
 ChangeCalendarEntry | Cambiar o volver a programar la entrada del calendario. | Reschedule my 6 a.m. (Volver a programar las 6 a. m.) appointment tomorrow to 2 p.m. (cita para mañana a las 2 p. m.) <br> Reschedule doctor's appointment for 5 PM (Volver a programar la cita del médico para las 5 p. m.) <br> Reschedule lunch with jenny olson to Friday. (Volver a programar el almuerzo con jenny olson al viernes) <br> Change event time. (Cambiar la hora del evento)
 CheckAvailability | Buscar disponibilidad para una cita o reunión en el calendario del usuario o en el calendario de otra persona. | ¿Cuándo está Jim disponible para una reunión? <br> Show when Carol is available tomorrow. (Mostrar cuándo está disponible Carol mañana) <br> ¿Chris está disponible el sábado?
 Confirm | Confirmar si desea realizar una operación o acción en función de la intención anterior. <br> ***Aviso**: Esta intención incluye principalmente la acción "Confirm" para el escenario de calendario. Si necesita expresiones más generales en "Confirm", aproveche la intención "Confirm" en el dominio **Utilities**.*| That's correct, please create the meeting (Eso es correcto, crea la reunión) <br> Yes, thanks, connect to the meeting. (Sí, gracias, conectarse a la reunión)
 ConnectToMeeting | Conectarse con una reunión. | Connect me to 11:00 conference call with Andy. (Conectarme a la llamada de conferencia de las 11:00 con Andy) <br> Accept the budget meeting call. (Aceptar la llamada de reunión de presupuesto)
 ContactMeetingAttendees | Ponerse en contacto con los asistentes de la reunión. | Tell the meeting I am running late to 3:00 meeting. (Decir a la reunión que llego tarde a la reunión de las 3:00) <br> Notify colleagues for 8 am meeting that it needs to start at 8:30. (Notificar a los compañeros de 8 a. m. que deben comenzar a las 8:30)
 CreateCalendarEntry | Agregar un nuevo elemento cada vez al calendario. | Create a meeting about discussing issues. (Crear una reunión para hablar de los problemas) <br> create a meeting with abc@microsoft.com (reservar una reunión de 2 horas con XX)
 DeleteCalendarEntry | Solicitar la eliminación de una entrada del calendario.| Cancelar mi cita con Carol. <br> Delete my 9 a.m. meeting. (Eliminar mi reunión de las 9 a. m.) <br> Delete my event. (Eliminar mi evento)
  FindCalendarEntry | Abrir la aplicación de calendario o buscar la entrada de calendario. | Buscar la cita de revisión con el dentista. <br> show my calendar (mostrar mi calendario) <br> What's in my calendar on Thursday? (¿Qué hay en mi calendario el jueves?)
 FindCalendarWhen | Comprobar la hora en que tiene lugar la programación. | When do I meet with Amber and Susan? (¿Cuándo me reúno con Ámbar y Susan?) <br> When do I have a brunch scheduled? (¿Cuándo tengo un brunch? programado) 
 FindCalendarWhere | Comprobar cuándo tiene lugar la programación. | Where are my appointments tomorrow? (¿Dónde son mis citas mañana?) <br>Where am i meeting with Stacy and Michael tomorrow for dinner? (¿Dónde puedo reunirme con Stacy y Michael mañana para cenar?)
  FindCalendarWho | Comprobar los asistentes que van a asistir a la programación de destino. | I want the confirmed attendants on tomorrow's meeting at 2. (Deseo los asistentes confirmados en la reunión de mañana a las 2) <br> Will jim be at the next nurses' meeting? (¿Estará Jim en la próxima reunión de enfermeras?)
 FindCalendarDetail | Comprobar y mostrar los detalles de la programación. | I need you to provide me the details of the meeting I have scheduled with my colleague Paul. (Necesito que nos proporcione los detalles de la reunión que he programado con mi colega Paul)
 FindDuration | Comprobar la duración. | How much time will I have to pick up groceries? (¿Cuánto tiempo tengo para recoger la compra?) <br> How long do I have for lunch? (¿Cuánto tiempo tengo para comer?)
 FindMeetingRoom | Buscar las salas de reuniones disponibles. | What meet rooms do I have? (¿Que salas de reuniones tengo?) <br> A new meeting location, find one. (Una nueva ubicación de reunión, buscar una)
 Goback | Volver al último paso o elemento.  <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre las expresiones generales de GoBack.* | Previous one (Mostrar anterior) <br> Back to last email. (Volver al último correo electrónico)
 Reject | El usuario rechaza el asistente virtual propuesto. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre la expresiones generales de Reject.* | Not need to set the event. (No es necesario establecer el evento) <br> I have other things to do at that time. (En ese momento tengo otras cosas que hacer)
ShowNext | Comprobar el evento siguiente. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre las expresiones generales de ShowNext.* | Give me my next event. (Enviarme mi evento siguiente) <br> What is next in calendar? (¿Qué es lo siguiente en el calendario?)
 ShowPrevious | Comprobar el evento anterior. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre las expresiones generales de ShowPrevious.* | What is the schedule before that? (¿Cuál es el programa anterior?)
 TimeRemaining | Comprobar el tiempo restante hasta el siguiente evento. | Display how much time i have before my meetings. (Mostrar cuánto tiempo tengo antes de mis reuniones) <br> Display the amount of time I have before my next meeting begins. (Mostrar la cantidad de tiempo que tengo antes de comenzar la próxima reunión)
 
### <a name="entities"></a>**Entidades**
Nombre de entidad | Tipo de entidad | DESCRIPCIÓN | Ejemplos | Slots
-------|-----------------------|-------------|---------|--------
ContactName | personName | El nombre de un contacto o del destinatario de un mensaje. | Meet with **Betsy**. (Reunión con Betsy). <br>  Meet with **Aubrey** on July 3rd at 7 p.m. (Reunión con Aubrey el 3 de julio a las 7 p. m.) | Betsy <br> Aubrey <br> Amy 
DestinationCalendar | simple | Nombre del calendario de destino. | lunch with mom Tuesday 12 **personal** (comida con mamá el martes 12 personal) <br> Use my **Google** calendar as my default calendar. (Usar mi calendario de Google como calendario predeterminado) <br> Update yoga class to mon wed at 3 p.m. (Actualizar la clase de yoga al Lun-Mié a las 3 p. m.) list in **personal** calendar. (lista en calendario personal) | Google <br> Personal <br> Patrón de unidad de negocio <br> main (principal)
Duration | datetime | Duración de una reunión, cita o tiempo restante. | Add to work calender meeting with Gary to discuss scholarship details tomorrow at 11 am for **20 minutes**. (Agregar a la reunión de calendario de trabajo con Gary para hablar de los detalles de la beca mañana a las 11 a. m. durante 20 minutos) <br> Add to the calendar an event at subway on Friday I'll be eating with Thomas for **an hour** at 9 p.m. (Agregar al calendario un evento en Subway el viernes estaré comiendo con Thomas durante una hora a las 9 p. m.) | an hour (una hora) <br> 2 días <br> 20 minutos 
EndDate | datetime | Fecha de finalización de una reunión o una cita. | Calendar add concert at bass hall Mary 3rd to **Mary 5th** (Agregar al calendario un concierto en Bass Hall con Mary del 3 al 5) | Mary 5th (Mary el 5)  
EndTime | datetime | Hora de finalización de una reunión o de una cita. | can you make it two thirty to **three** (puede pasar de 2:30 a tres) | three 
Location | simple | Ubicación de elementos, reuniones o citas del calendario.  Las direcciones, las ciudades y las regiones son buenos ejemplos de ubicaciones. | put a meeting in **fremont** to put the tablet in Burma (concertar una reunión en Fremont para poner la tableta en Birmania) <br> pro bono meeting in **Edina** (reunión pro bono en Edina) | 209 Nashville Gym <br> 897 Pancake house <br> Garaje 
MeetingRoom | simple | Sala para una reunión o cita. | Add to work calendar meeting with jake at 2 p.m. (Agregar a la reunión de calendario de trabajo con jake a las 2 p. m.) in his **office** this Friday (en su oficina este viernes) | his office (su oficina) <br> conference room (sala de conferencia) <br> Room 2 (Sala 2)
MoveEarlierTimeSpan | datetime | Hora en la que el usuario desea adelantar una reunión o cita. | Move my lunch date ahead by **30 minutes**. (Adelantar la fecha de la comida 30 minutos) | 30 minutos <br> two hours 
MoveLaterTimeSpan |  datetime | La hora a la que el usuario desea retrasar una reunión o cita. | push my meeting with orchid box back **4 hours**. (Retrase mi reunión con Orchid box 4 horas) | 4 horas <br> 15 minutos 
OrderReference | simple | La posición ordinal o relativa en una lista, que identifica un elemento que se va a recuperar. | What's my next appointment for tomorrow? (Cuál es mi siguiente cita de mañana) | Siguiente
OriginalEndDate | datetime | Fecha de finalización original de una reunión o cita. | Change my vacation from ending on **Friday** to Monday (Cambiar mis vacaciones del final del viernes al lunes) | Viernes 
OriginalEndTime | datetime | Hora de finalización original de una reunión o cita. | Make the one ending at **3** go until 4 (Pasar la que acaba a las 3 hasta las 4) | 3
OriginalStartDate | datetime | Fecha de inicio original de una reunión o cita. | Change **tomorrow**'s appointment from 10 a.m. to Wednesday at 9 a.m. (Cambiar la cita de mañana desde las 10 a. m. al miércoles a las 9:00 a. m.)  | tomorrow (mañana) 
OriginalStartTime | datetime | La hora de inicio original de una reunión o cita. | Change tomorrow's appointment from **10 a. m.** to Wednesday at 9 a.m. (Cambiar la cita de mañana desde las 10 a. m. al miércoles a las 9:00 a. m.) | 10 a.m.
PositionReference | ordinal | La posición absoluta en una lista, que identifica un elemento que se va a recuperar. | The **second** one (El segundo) | segundo <br> No. 3 <br> number 5 (número 5)
RelationshipName | list | Nombre de la relación de un contacto. | add lunch at 1:00 P.M. (Agregar almuerzo a las 1:00 p. m.) with my **wife** (con mi esposa) | wife (esposa) <br> husband (marido) <br> sister (hermana) 
SlotAttribute | simple | El atributo que el usuario desea consultar o editar. | change event **location** (cambiar la ubicación del evento) <br> change it the **time** to seven o'clock (cambiar la hora a las 7 en punto) | location <br> time 
StartDate | datetime | Fecha de inicio de una reunión o cita. | Create a meeting on **Wednesday** at 4 p.m. (Crear una reunión el miércoles a las 4 p. m.) | Miércoles 
StartTime | datetime | Hora de inicio de una reunión o cita. | Create a meeting on Wednesday at **4 p.m.** (Crear una reunión el miércoles a las 4 p. m.) | 4 p.m.
Asunto | simple, pattern.Any | Asunto, como el título de una reunión o cita. | What time is the **party preparation** meeting? (¿A qué hora es la reunión de la preparación de la fiesta?) | Dentist's (En el dentista) <br> Almuerzo con Julia 
Message | simple, pattern.Any | Mensaje que se va a enviar a los asistentes. | Alert attendees of dinner meeting that **I will be late**. (Avisar a los asistentes de la cena que llegaré tarde) | I will be late (Iré tarde)

## <a name="communication"></a>**Communication**
Solicitudes para realizar llamadas, enviar mensajes de texto o mensajes instantáneos, buscar y agregar contactos y otras diversas solicitudes relacionadas con la comunicación (generalmente saliente). Las expresiones de _solo nombres de contacto_ no pertenecen al dominio de comunicación.

### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
AddContact | Agregar un contacto nuevo a la lista de contactos del usuario. | Add new contact. (Agregar nuevo contacto)  <br>   Save this number and put the name as jose (Guardar este número y llamarlo José)
AddFlag | Agregar una marca a un correo electrónico | Flag this email (Marcar este correo electrónico) <br> Add a flag to this email. (Agregar una marca a este correo electrónico)
AddMore | Agregar más a un correo electrónico o texto, como parte de una composición de texto o de correo electrónico gradual. | Add more to text. (Agregar más al texto)  <br>   Add more to email body. (Agregar más al cuerpo del correo electrónico)
Respuesta | Responder a una llamada telefónica entrante. | Answer the call (Responder la llamada)  <br>   Pick it up. (Aceptarla)
AssignContactNickname | Asignar un alias a un contacto. | Change Isaac to Dad. (Cambiar Isaac por papá)  <br>   Edit Jim's nickname. (Editar el alias de Jim) <br>   Add nickname to Patti Owens. (Agregar alias para Patti Owens)
CallBack | Devolver una llamada telefónica. | Call back (Devolución de llamada)
CallVoiceMail | Conectarse al buzón de voz del usuario. | Connect me to my voicemail box. (Conectarme a mi buzón de voz) <br>Voice mail (Buzón de voz) <br>   Call voicemail (Llamar al buzón de voz)
Cancelar | Cancelar una operación. | Cancel (Cancelar) <br>   End it. (Finalizarla)
CheckIMStatus | Comprobar el estado de un contacto en la mensajería instantánea. | ¿El estado en línea de Jim indica que está fuera? 
CheckMessages | Comprobar si hay nuevos mensajes o correos electrónicos. | Check my inbox (Comprobar mi bandeja de entrada) <br>  Do I have any new mail? (¿Tengo correo nuevo?)
Confirm | Confirmar una acción. | Yes, send it. (Sí, enviarlo) <br> Right, I confirm that I want to send this email. (Cierto, confirmo que deseo enviar este correo electrónico)
EndCall | Finalizar una llamada telefónica. | Hang up the call (Colgar la llamada) <br> End up. (Terminarla)
FindContact | Buscar información del contacto por nombre. | Find mum's number. (Buscar el número de mamá) <br>   Show me Carol's number. (Mostrarme el número de Carol)
FinishTask | Estado actual de la tarea. | I'm done (Ya he terminado) <br> That is all. (Eso es todo)
TurnForwardingOff | Desactivar el reenvío de llamadas. | Stop forwarding my calls. (Dejar de desviar mis llamadas) <br> Switch off call forwarding. (Desactivar el desvío de llamadas)
TurnForwardingOn | Activar el reenvío de llamadas. | Desviar mis llamadas a 3333 <br>  Switch on call forwarding to 3333. (Activar el desvío de llamadas a 3333)
GetForwardingsStatus | Obtener el estado actual del desvío de llamadas. | ¿El desvío de llamadas está activado? <br>   Tell me if my call status is on or off. (Indicarme si mi estado de llamada está activado o desactivado)
GetNotifications | Obtener las notificaciones. | open my notifications (abrir mis notificaciones) <br>   can you check my phone facebook notifications (¿puede comprobar las notificaciones de Facebook en mi teléfono?)
Goback | Volver al paso anterior. | Volver a Twitter <br>   Retroceder un paso <br>   Volver
Ignorar | Omitir una llamada entrante. | No responder <br>   Omitir llamada
IgnoreWithMessage | Omitir una llamada entrante y, en su lugar, responder con texto. | No responder a esa llamada pero enviar un mensaje en su lugar. <br>   Ignorar y enviar texto.
Dial | Hacer una llamada telefónica. | Llamar a Jim <br>   Please dial 311. (Marcar el 311)
FindSpeedDial | Buscar el número de marcación rápida establecido para un número de teléfono y viceversa. | ¿A quién corresponde el número de marcación 5? <br>   ¿Tengo establecida la marcación rápida? <br>   ¿Cuál es el número de marcación rápida del número 941-5555-333?
Adelante | Reenviar un correo electrónico | Forward this email to Greg. (Reenviar este correo electrónico a Greg)
ReadAloud | Leer un mensaje o un correo electrónico al usuario. | Leer el texto. <br>   ¿Qué ha dicho en el mensaje?
PressKey | Presione un botón o un número del teclado. | Marcar asterisco. <br>   Presionar 1 2 3.
QueryLastText | Consultar el último texto o mensaje. | Who texted me? (¿Quién me ha enviado el SMS?) <br>   Who recently messaged me? (¿Quién me ha enviado un mensaje recientemente?)
Volver a marcar | Volver a marcar un número o rellamar a dicho número. | Volver a marcar. <br>   Volver a marcar mi última llamada.
Reject | Rechazar una llamada entrante. | Rechazar llamada <br>   No puedo responder ahora <br>   No está disponible en este momento; le devolverá la llamada más tarde.
Reply | Responder a un mensaje. | respond to lore hound (responder a Lore Hound) <br>   reply by typing i am on my way (responder escribiendo que estoy en camino)
SearchMessages | Buscar los mensajes según algunas condiciones. | Can you search my emails sent by Jane? (¿Puede buscar los correos electrónicos enviados por Jane?)
SendEmail | Enviar un correo electrónico. Esta intención se aplica al correo electrónico, pero no a los mensajes de texto. | Correo electrónico para Mike Waters: Mike, la cena de la semana pasada fue espléndida. <br>   Send an email to Bob. (Enviar un correo electrónico a Bob)
SendMessage | Enviar un mensaje de texto o un mensaje instantáneo. | Enviar texto a Chris y Carol <br>   Facebook message greg (Enviar un mensaje de Facebook a Greg) <br>   
SetSpeedDial | Establecer un acceso directo de marcación rápida para el número de teléfono de un contacto. | Establecer el número uno como marcación rápida para Carol. <br>   Configurar la marcación rápida para mamá.
ShowCurrentNotification | Mostrar notificaciones actuales. | Are there any new notifications? (¿Hay nuevas notificaciones?) <br> Show me a notification. (Mostrarme una notificación)
ShowNext | Ver el siguiente elemento, por ejemplo, en una lista de mensajes de texto o de correos electrónicos. | Mostrar lo siguiente. <br>   Ir a la página siguiente.
ShowPrevious | Ver el elemento anterior, por ejemplo, en una lista de mensajes de texto o de correos electrónicos. | Mostrar lo anterior. <br>   Previous (Anterior) <br>   Ir a anterior.
TurnSpeakerOff | Desactivar el altavoz del teléfono. | Deseo desactivar el altavoz. <br>   Desactivar el altavoz del teléfono.
TurnSpeakerOn | Activar el altavoz del teléfono. | Modo de altavoz. <br>   Encender el altavoz.

### <a name="entities"></a>**Entidades**
Nombre de entidad | Tipo de entidad | DESCRIPCIÓN | Ejemplos | Slots
------|-------|----------|--------------|---------------
Datos adjuntos | simple | Los datos adjuntos que el usuario desea enviar por SMS o correo electrónico. | Email a **file** from onenote. (Enviar un archivo desde OneNote) <br> Send my housekeeping **doc** to Katie. (Enviar mi documento de mantenimiento a Katie) | file <br> doc
AudioDeviceType | simple | Tipo de dispositivo de audio (altavoces, auriculares, micrófono, etc.). | Answer using **hands-free**. (Responder usando manos libres) <br> Redial on **speaker phone**. (Volver a marcar en el teléfono de altavoz) | speaker (altavoz) <br> hands-free (manos libres) <br> Bluetooth
Categoría | simple | La categoría de un mensaje o correo electrónico, la categoría debe tener una definición clara en el sistema de correo electrónico, como "no leído", "marca". Las descripción sin una definición clara, por ejemplo, "nuevo" y "reciente", no son categorías. | Mark all email as **read** (Marcar todo el correo electrónico como leído)  <br> New **high priority** email for Paul (Nuevo correo electrónico de alta prioridad para Paul) | important (importante) <br> high priority (prioridad alta) <br> read
ContactAttribute | simple | Un atributo del contacto sobre el que el usuario pregunta.| Any **birthdays** next month I should know about? (¿Qué cumpleaños del próximo mes debo conocer?) | birthdays (cumpleaños) <br> address <br> phone number (número de teléfono)
ContactName | personName  | El nombre de un contacto o del destinatario de un mensaje. | Send the mail to **Stevens** (Enviar el correo electrónico a Stevens) | Stevens
Fecha y hora | datetime | Fecha y hora de un correo electrónico recibido. | Read **today**'s mail (Leer el correo de hoy) <br> Who emailed me **today**? (¿Quién me ha enviado un correo electrónico hoy?) <br> who phoned at **7 p.m.** ? (¿Quién me ha telefoneado a las 7 p. m.?) | today (hoy) <br> tomorrow (mañana)
DestinationPhone | simple | El destino al que el usuario quiere llamar o enviar un SMS. | make a call to **house** (hacer una llamada a casa) <br> send a text message to **home** (enviar un mensaje de texto a casa) | house (casa) <br> home
EmailAddress | email | La dirección de correo electrónico que el usuario desea enviar o consultar. | send email to Megan.Flynn@MKF.com (enviar correo electrónico a xx)<br> abc@outlook.com 
EmailSubject | simple, pattern.Any | El texto que se utiliza como la línea de asunto de un correo electrónico. | Compose email to David with subject **hey** (Redactar correo electrónico para David con el asunto Hola)  | RE: caso interesante
Clave | simple | La tecla que el usuario quiere presionar. | Press the **space** key. (Pulsar la barra espaciadora) <br> press **9** (presionar 9) | pound (almohadilla) <br> star (asterisco) <br> 8
Línea | simple | La línea que el usuario quiere usar para enviar un correo electrónico o un SMS. | Read my last **hotmail** email. (Leer mi último correo electrónico de hotmail) <br> Call Peter by **mobile**. (Llamar a Peter por el móvil) <br> Call Dad using my **work** line. (Llamar a papá con mi línea de trabajo)| hotmail <br> Skype <br> Teléfono británico
SenderName | personName | El nombre del remitente. | Read the email from **David** (Leer el correo electrónico de David) <br> Emails from Chanda (Correos electrónicos de Chanda) | David <br> Chanda
FromRelationshipName | simple | Nombre de la relación del remitente. | Read message from **Dad**. (Leer el mensaje de papá) <br> Can you read me all text messages from **mom**? (Me puede leer todos los mensajes SMS de mamá?) | Dad (papá) <br> Mom (mamá) 
Message | simple, pattern.Any |  Mensaje que se va a enviar como un texto o correo electrónico.  | Send email saying "**I am busy**". (Enviar un mensaje electrónico que dice "Estoy ocupado") | I am busy (Estoy ocupado)
OrderReference | simple | La posición ordinal o relativa en una lista, que identifica un elemento que se va a recuperar. | What was the **last** message I sent? (¿Cuál fue el último mensaje que envié?) <br> Read **latest** nokia email. (Leer el correo electrónico más reciente del Nokia) <br> Read **new** text messages. (Leer nuevos mensajes de texto) | last <br> más reciente <br> recent (reciente) <br> newest (más reciente)
PositionReference | simple, ordinal | La posición ordinal o relativa en una lista, que identifica un elemento que se va a recuperar.| What was the **first** message I sent? (Cuál fue el primer mensaje que envié) <br> The **3rd** one. (El tercero)| Primero <br> 3rd (tercero)
phoneNumber | phoneNumber | El número de teléfono al que desea llamar o enviar un SMS. | send a text to **four one five six eight four five two eight four** (enviar un mensaje SMS al cuatro uno cinco seis ocho cuatro cinco dos ocho cuatro) | 3525214446
RelationshipName | simple | El nombre de relación de un contacto o del destinatario de un mensaje. | Email to my **wife** (Enviar un correo electrónico a mi esposa) | wife (esposa)
SearchTexts | simple, pattern.any | Los textos que se usan para filtrar los mensajes de correo electrónico o mensajes de texto. | Search all emails that contain "**Surface Pro**" (Buscar todos los correos electrónicos que contengan "Surface Pro") | Surface Pro
SpeedDial | simple | Marcación rápida. | Call **three four five**. (Llamar al tres cuatro cinco) <br> Set speed dial **one**. (Establecer la marcación rápida en uno) | 345 <br> 5

## <a name="email"></a>**Correo electrónico**
El correo electrónico es un subdominio del dominio *Communication*. Contiene principalmente solicitudes para enviar y recibir mensajes por correo electrónico.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
AddMore | Agregar más a un correo electrónico o texto, como parte de una composición de texto o de correo electrónico gradual. | Add more to email body. (Agregar más al cuerpo del correo electrónico)
Cancelar | Cancelar una operación. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre la expresiones generales de Cancel.* | Cancel (Cancelar) Don't send it. (No enviarlo) <br> End it. (Finalizar)
CheckMessages | Comprobar los nuevos mensajes o correos electrónicos sin la consulta condicional. Si hay alguna condición, las expresiones pertenecen a la intención *SearchMessage*. | Check my inbox. (Comprobar mi bandeja de entrada) <br> Do i have any new mails? (¿Tengo correos electrónicos nuevos?) 
Confirm | Confirmar una acción en curso relacionada con el procesamiento de correos electrónicos. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre la expresiones generales de Confirm.* | Yes, send it. (Sí, enviarlo) <br> Confirmo que deseo enviar este correo electrónico.
Eliminar | Eliminar un correo electrónico o los mensajes filtrados por algunas condiciones. | Put the email into recycle bin (Poner el correo electrónico en la papelera de reciclaje) <br> Empty my inbox. (Vaciar mi bandeja de entrada) <br> Remove Mary's email. (Borrar el correo electrónico de Mary)
ReadAloud | Leer un mensaje o un correo electrónico al usuario. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre la expresiones generales de ReadAloud.*  | Read the email sent by Mary. (Leer el correo electrónico enviado por Mary)
Reply | Responder con un mensaje al correo electrónico actual. | Create a response to the email. (Crear una respuesta para el correo electrónico) <br> Reply by typing "thank you very much, best regards, John". (Responder escribiendo "muchas gracias y atentamente, John")
SearchMessages | Buscar mensajes según algunas condiciones, incluidos el nombre del remitente, la hora y el asunto. | Show me the messages from Nisheen. (Mostrar los mensajes de Nisheen) <br> Can you search my emails titled "ABC"? (¿Puede buscar mis correos electrónicos con el nombre "ABC"?)
SendEmail | Enviar un correo electrónico. | Email to Mike: (Enviar un correo electrónico a Mike) Mike, la cena de la semana pasada fue espléndida. <br> Send an email to Bob. (Enviar un correo electrónico a Bob)
ShowNext | Ver el siguiente elemento o elementos en una lista de mensajes de texto o de correos electrónicos. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre las expresiones generales de ShowNext.* | Mostrar lo siguiente. <br> Ir a la página siguiente.
ShowPrevious | Mostrar el elemento o elementos anteriores en una lista de mensajes de texto o de correos electrónicos. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre las expresiones generales de ShowPrevious.* | Mostrar lo anterior. <br> Previous (Anterior) <br> Ir a anterior.
Adelante | Reenviar un correo electrónico. | Forward this email to Greg. (Reenviar este correo electrónico a Greg)
AddFlag | Agregar marca a un correo electrónico. | Flag this email (Marcar este correo electrónico) <br> Add a flag to this email. (Agregar una marca a este correo electrónico)
QueryLastText | Consultar el último correo electrónico. | Who sent email to me? (¿Quién me ha enviado correo electrónico?) <br> Who recently emailed me? (¿Quién me ha enviado correo electrónico recientemente?)


### <a name="entities"></a>**Entidades**
Nombre de entidad | Tipo de entidad | DESCRIPCIÓN | Ejemplos | Slots
------|-------|----------|--------------|---------------
Datos adjuntos | simple | Los datos adjuntos que el usuario desea enviar por SMS o correo electrónico. | Email a **file** from onenote. (Enviar un archivo desde OneNote) <br> Send my housekeeping **doc** to Katie. (Enviar mi documento de mantenimiento a Katie) | file <br> doc
ContactName | personName  | El nombre de un contacto o del destinatario de un mensaje. | Send the mail to **Stevens** (Enviar el correo electrónico a Stevens) | Stevens
Date | datetime | Fecha de un correo electrónico recibido. | Read **today**'s mail (Leer el correo de hoy) <br> Who emailed me **today**? (¿Quién me ha enviado un correo electrónico hoy?) | today (hoy)
EmailAddress | email | La dirección de correo electrónico que el usuario desea enviar o consultar. | send email to Megan.Flynn@MKF.com (Enviar correo electrónico a xx)<br> abc@outlook.com 
EmailSubject | simple, pattern.Any | El texto que se utiliza como la línea de asunto de un correo electrónico. | Compose email to David with subject **hey** (Redactar correo electrónico para David con el asunto Hola)  | RE: caso interesante
SenderName | personName | El nombre del remitente. | Read the email from **David** (Leer el correo electrónico de David) <br> Emails from Chanda (Correos electrónicos de Chanda) | David <br> Chanda
FromRelationshipName | simple | Nombre de la relación del remitente. | Read message from **Dad**. (Leer el mensaje de papá) <br> Can you read me all text messages from **mom**? (Me puede leer todos los mensajes SMS de mamá?) | Dad (papá) <br> Mom (mamá) 
Message | simple, pattern.Any |  Mensaje que se va a enviar como un texto o correo electrónico.  | Send email saying "**I am busy**". (Enviar un mensaje electrónico que dice "Estoy ocupado") | I am busy (Estoy ocupado)
Categoría | simple | La categoría de un mensaje o correo electrónico, la categoría debe tener una definición clara en el sistema de correo electrónico, como "no leído", "marca". Las descripción sin una definición clara, por ejemplo, "nuevo" y "reciente", no son categorías. | Mark all email as **read** (Marcar todo el correo electrónico como leído)  <br> New **high priority** email for Paul (Nuevo correo electrónico de alta prioridad para Paul) | important (importante) <br> high priority (prioridad alta) <br> read
OrderReference | simple | La posición ordinal o relativa en una lista, que identifica un elemento que se va a recuperar. | What was the **last** message I sent? (¿Cuál fue el último mensaje que envié?) <br> Read **latest** nokia email. (Leer el correo electrónico más reciente del Nokia) <br> Read **new** text messages. (Leer nuevos mensajes de texto) | last <br> más reciente <br> recent (reciente) <br> newest (más reciente)
PositionReference | simple, ordinal | La posición ordinal o relativa en una lista, que identifica un elemento que se va a recuperar.| What was the **first** message I sent? (Cuál fue el primer mensaje que envié) <br> The **3rd** one. (El tercero)| Primero <br> 3rd (tercero)
RelationshipName | simple | El nombre de relación de un contacto o del destinatario de un mensaje. | Email to my **wife** (Enviar un correo electrónico a mi esposa) | wife (esposa)
Hora | datetime | Hora | send email **tonight**. (Enviar correo electrónico esta noche) | tonight (esta noche)
SearchTexts | simple, pattern.any | Los textos que se usan para filtrar los mensajes de correo electrónico o mensajes de texto. | Search all emails that contain "**Surface Pro**" (Buscar todos los correos electrónicos que contengan "Surface Pro") | Surface Pro 
Línea | simple | La línea que el usuario quiere usar para enviar un correo electrónico. | Read my last **hotmail** email. (Leer mi último correo electrónico de hotmail) <br> Send an email from my **work account**. (Enviar un correo electrónico desde mi cuenta de trabajo)| hotmail <br> work account (cuenta profesional) 

## <a name="homeautomation"></a>**HomeAutomation**
El dominio HomeAutomation ofrece intenciones y entidades relacionadas con el control de dispositivos domésticos inteligentes. Principalmente, se admite el comando de control relacionado con las luces y el aire acondicionado. Pero tiene algunas capacidades de generalización en otras aplicaciones eléctricas.
### <a name="supported-devices-and-properties"></a>**Dispositivos y propiedades compatibles**
Dispositivo | properties (Propiedades)
-------|---------
Sensor de temperatura | Temperatura
Luz, bombilla | Encendido y apagado, brillo, color
TV, radio | Encendido y apagado, volumen
Aire acondicionado (A/C), termostato | Encendido y apagado, temperatura, potencia
Ventilador | Encendido y apagado
Enchufes, reproductor de DVD, máquina de hielo, etc. | Encendido y apagado

### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
 TurnOff | El usuario quiere desactivar el dispositivo o la configuración.  | Turn off the lights. (Apagar las luces) <br> Switch off something. (Desactivar algo) <br> Something off. (Algo desactivado)
 TurnOn | Activar un dispositivo o activar y establecer el dispositivo en un ajuste o modo particular. | Turn on the light to 50%. (Encender la luz al 50 %) <br> Turn on my coffee maker (Encender mi cafetera) <br> Can you turn on my coffee maker? (¿Puede encender mi cafetera?)
 SetDevice | El usuario quiere establecer el dispositivo en una configuración o tipo particular.  | Set the air conditioner to 26 degrees. (Establecer el aire acondicionado a 26 grados) <br> Turn the lights blue. (Cambiar las luces a azules) <br> Call this light as nightlight. (Llamar esta luz como luz nocturna)
 QueryState | El usuario solicita el estado de un dispositivo o configuración.  | What is the thermostat set to? (¿Cuál es la configuración del termostato?) <br> Is the A/C on? (¿Está encendido el aire acondicionado?) <br> What's the bedroom temperature? (¿Cuál es la temperatura del dormitorio?)
 TurnUp | Subir la configuración o el brillo de los dispositivos. | Brighten the lights by 75 percent. (Iluminar las luces un 75 por ciento) <br> Up the brightness here by 10 percent. (Subir el brillo aquí un 10 por ciento)  <br> Make it warmer in the living room. (Aumentar la temperatura en el comedor)
 TurnDown | Bajar pero no apagar un dispositivo, ya sea mediante la atenuación, la temperatura o el brillo de las luces. | Turn down the library by 44%. (Bajar la biblioteca un 44%) <br> Dim the lights. (Atenuar las luces) <br> Make the room cooler. (Enfriar la habitación)
### <a name="entities"></a>**Entidades**
Nombre de entidad | Tipo de entidad | DESCRIPCIÓN | Ejemplos
-------|----------|--------------|---------------
DeviceName | List | Texto definido por el usuario para sus dispositivos. | Azul<br> Christmas (Navidad) <br> Desk (Escritorio)
DeviceType | List | Dispositivos compatibles | Lights (Luces) <br> Air conditioner (Aire acondicionado) <br> nightlight (luz nocturna)
Location | simple | La ubicación o habitación en donde se encuentra el dispositivo. | Kitchen (Cocina)<br> Downstairs (Piso de abajo) <br> Bedroom (Dormitorio)
NumericalChange | simple | Cantidad por la que se aumenta o disminuye una configuración. <br> <br> _La ranura solo aparece con las intenciones turn_up y turn_down._ | 3<br> 50 %<br>
OrderReference | ordinal | El propósito de esta ranura es capturar la selección de los elementos. Indica la posición del elemento en una lista. | Primero<br> Segundo
Quantifier | List | Quantifier indica el número de instancias de una entidad determinada a las que se hace referencia. Por ejemplo, "all", "every", etc. | Todo<br> Every (Cada)<br> Everything (Todo)
Configuración | Simple | La configuración que el usuario desea establecer en el dispositivo, que incluye la escena, el nivel, la intensidad, el color, el modo, la temperatura y el estado del dispositivo. | Azul<br> 72 <br> Heat (Calor) 
SettingType | List | La configuración del dispositivo en la que está interesado el usuario. | Temperatura<br> 
Time/Date (Fecha y hora) |  datetime | Tiempo y duración para el funcionamiento de un dispositivo| 5 minutos <br> 3 p. m.
Unidad | List | Para etiquetar palabras como "degrees", "Percent", "Fahrenheit", "Celsius", cada término de unidad debe etiquetarse por separado. | Degrees (Grados)<br> Percent


## <a name="notes"></a>**Notas**
El dominio Note facilita la creación de notas y la escritura de elementos para los usuarios.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
AddToNote | Agregar información a una nota abierta. | Add to my planning note to contact my boss about the project. (Agregar a mi nota de planeamiento para ponerme en contacto con mi jefe sobre el proyecto)
Clear | Borrar todos los elementos de una nota preexistente. | Remove all items from my vacation note. (Quitar todos los elementos de mi nota de vacaciones) <br>Clear all from my reading note. (Borrar todo de mi nota de lectura)
Confirm | Confirmar una acción relacionada con una nota. <br> ***Aviso**: Esta intención incluye principalmente la acción "Confirm" para el escenario Note. Si necesita expresiones más generales en "Confirm", aproveche la intención "Confirm" en el dominio **Utilities**.* | It's okay by me for this note. (Por mí está bien para esta nota) <br>I am confirming keeping all items on lists. (Estoy confirmando que mantengo todos los elementos en las listas)
Crear | Crear una nota. | Create a note. (Crear una nota) <br>Note to remind me that Jason is in town first week of May. (Crear una nota para recordarme que Jason estará en la ciudad la primera semana de mayo) 
Eliminar | Eliminar una nota completa. | Delete my vacation note. (Eliminar mi nota de vacaciones) <br>Delete my groceries note. (Eliminar mi nota de la compra)
ReadAloud | Leer una nota en voz alta. | Read me the first note. (Leerme la primera nota) <br>Read me the details. (Leerme los detalles)
cierre | Cerrar la nota actual | Close the note. (Cerrar la nota) <br>Cerrar la nota actual
Abrir | Abrir una nota existente previamente. | Open my vacation note. (Abrir mi nota de vacaciones) <br>Open the note "planning". (Abrir la nota "planeamiento")
RemoveSentence | Quitar una frase de una nota. | Remove the last sentence. (Quitar la última frase) <br>Delete chips from my grocery note. (Eliminar las patatas fritas de mi nota de la compra) <br>Remove pens from my school shopping note (Quitar los bolígrafos de mi lista de la compra para el colegio)
ChangeTitle | Cambiar el título de la nota. | Named this note as "planning". (Llamar esta nota "planeamiento")

### <a name="entities"></a>**Entidades**
Nombre de entidad | Tipo de entidad | DESCRIPCIÓN | Ejemplos 
------- | ------- | ------- | -------
Texto | simple, pattern.Any | El texto de una nota o un aviso. | Estirar antes de andar <br> Correr mucho mañana
Título | simple, pattern.Any | Título de una nota. | Lista de la compra <br> Personas a las que llamar <br> Tareas pendientes
CreationDate | datetimeV2 | Esta ranura es para cuando el usuario solicita notas creadas dentro de una determinada ventana de fecha y hora. | 
Quantifier | List | Cuando un usuario solicita realizar una acción en todos los elementos "all", "every" o "any" o todo el texto de una nota. | todas <br> cualquiera <br> every (cada)
OrderReference | ordinal | El usuario desea realizar acciones con los valores "first", "last" o "next", entre otros, elementos. | first <br> last


## <a name="places"></a>**Places**
Places incluye empresas, instituciones, restaurantes, espacios públicos y direcciones. El dominio admite la búsqueda de lugares y la solicitud de información sobre un lugar público, como la ubicación, las horas de servicio y la distancia. 
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
MakeCall | Realizar una llamada de teléfono a un lugar. | applebees on 1000/200 Rojas St and call. (applebees en 1000/200 Rojas St y llamar).
FindPlace | Buscar un lugar (empresa, institución, restaurante, espacio público o dirección). Pero no: <li> Solo el nombre de la empresa: Starbucks <li> Solo nombre de la ubicación: Seattle/Noruega <li> Solo cocina, alimentos o productos: Pizza, guacamole o italiano | ¿Dónde está la biblioteca más próxima? <br> Starbucks in Seattle. (Starbucks en Seattle) <br> ¿Dónde está la biblioteca más próxima? <br> 
GetAddress | Preguntar la dirección de un lugar. | Show me the address of Guu on Robson street (Muéstrame la dirección de Guu en Robson street) <br> ¿Cuál es la dirección de la cafetería Starbucks más cercana?
GetDistance | Preguntar sobre la distancia desde la ubicación actual hasta un lugar específico. | ¿A qué distancia está Holiday Inn?<br> How far is it to Bellevue square from here? (¿A qué distancia está Bellevue square de aquí?) <br> What's the distance to tahoe (¿A qué distancia está Tahoe?)
GetPhoneNumber | Solicitar el número de teléfono de un lugar. | ¿Cuál es el número de teléfono de la cafetería Starbucks más cercana?<br> Give the number for home depot (Dame el número de home depot) <br> A phone number for Disneyland. (Un número de teléfono para Disneyland)
GetPriceRange | Ask for the range of per capita consumption for a place. (Consulte el rango de consumo por habitante para un lugar) | Average price of J.Sushi in Seattle. (Precio medio de J. Sushi en Seattle) <br> ¿Cineplex tiene las tarifas a mitad de precio los miércoles? <br> ¿Cuánto cuesta una langosta entera en Sizzler?
GetStarRating | Solicitar la clasificación por estrellas de un lugar. | How is Zucca rated? (¿Cómo se clasifica Zucca?)<br> ¿Cuántas estrellas tiene The French Laundry?<br> ¿El acuario de Monterrey es bueno?
GetHours | Preguntar el horario comercial de un lugar. | ¿A qué hora cierra Safeway?<br> ¿Cuál es el horario comercial de Home Depot?<br> ¿Starbucks todavía está abierto?
GetReviews | Solicitar opiniones de un lugar. | Show me reviews for Cheesecake Factory. (Mostrarme las opiniones sobre Cheesecase Factory) <br> Read Cineplex reviews. (Leer las reseñas sobre Cineplex) <br> Are there any recent reviews for Humperdinks? (¿Hay reseñas pendientes para Humperdinks?)
GetMenu | Preguntar los elementos de menú de un restaurante. | ¿Zucca sirve comida vegana? <br> What's on the menu at Sizzler (¿Qué hay de menú en Sizzler?) <br> Show me Applebee's menu (Mostrarme el menú de Applebee)
RatePlace | Calificar un sitio. | 4 star rating for Maxi's pizza in Kimberly. (Clasificación de cuatro estrellas para la pizza de Maxi en Kimberly) <br> Give 4-star to Bonefish on Tripadvisor. (Dar 4 estrellas a Bonefish en Tripadvisor) <br> Create a 4 star review for La Hacienda. (Crear una reseña de 4 estrellas para La Hacienda)
AddFavoritePlace | El usuario quiere agregar una ubicación a su lista de favoritos o MVP. | Save this location to my favorites. (Guardar esta ubicación en mis favoritos) <br> Add Best Buy to my favorites. (Agregar Best Buy a mis favoritos)

### <a name="entities"></a>**Entidades**
Entidades LUIS | Tipo de entidad | DESCRIPCIÓN | Ejemplos | Ejemplos de expresiones
--------------|-------------|-------------|----------|-------------------
AbsoluteLocation | simple | La ubicación o dirección de un lugar. | Palo Alto <br> 300 112th Ave SE <br> Seattle | **1020 Middlefield Rd.** in **Palo Alto** (en Palo Alto) <br> bird seed stores in **Seattle** (tiendas de semillas de aves en Seattle) <br> Get the distance from here to **300 112th Ave SE**. (Obtener la distancia desde aquí a 300 112th Ave SE).
Amenities | simple | Características y ventajas objetivas de un lugar público. | Frente al mar <br> Aparcamiento gratuito | Kirkland **waterfront** seafood restaurants. (Marisquerías Kirkland frente al mar) <br> Is there any **free parking** near me? (¿Hay algún aparcamiento gratuito cerca?)
Cuisine | simple | Un tipo de comida, plato o gastronomía típica. | Chino <br> Italiano <br> Sushi <br> Noodle <br> | Help me find a **Chinese** restaurant. (Ayudarme a buscar un restaurante chino) <br> What are the opening hours of the **Sushi** restaurant? (¿A qué hora abre el restaurante de sushi?) <br> Where is the nearest **steak** house? (¿Cuál es el asador más cercano?)
Date | datetime | Fecha y hora o duración para la fecha de la ubicación de destino. | tomorrow (mañana) <br> today (hoy) <br> 6 p. m. | What time does aquarium close **tomorrow**? (¿A qué hora cierra el acuario mañana?) <br> the Closest bike shop that is open after **6 p.m.** (La tienda de bicicletas más cercana está abierta después de las 6 p. m.)
Distancia | dimensión | La distancia a un lugar público desde la posición actual del usuario. | 15 millas <br> 10 miles (10 millas) | a clothing store within **15 miles** (un almacén de ropa dentro de 15 millas) <br> A kids' restaurant that is only **10 miles** away (Un restaurante de niños está solo a 10 millas de distancia)
MealType | List | Tipo de comida como desayuno o almuerzo. | Desayuno <br> Cena | Search **breakfast** Greenwood Seattle (Buscar desayuno en Greenwood Seattle) <br> Find me a place to have my **lunch**. (Encontrarme un lugar para comer)
Nearby | List | Describir un lugar cercano sin una ubicación o dirección absoluta. | nearest (más cercano) <br> in this area (en esta área) <br> around me (a mi alrededor) | Find **nearest** indian restaurant. (Buscar el restaurante indio más cercano) <br> Where is my **local** Wetherspoon? (Dónde está mi Wetherspoon local?) <br> Any good restaurants **around me**? (¿Hay buenos restaurantes a mi alrededor?)
OpenStatus | List | Indica si un lugar está abierto o cerrado. | abierto <br> closed | What time does Yogurt Land **closed** today? (¿Cuándo cierra hoy Yogurt Land?) <br> What are the **opening** hours for Costco? (¿A qué hora abre Costco?)
PlaceName | simple | El nombre de un destino que es un negocio, restaurante, atracción pública o institución. El nombre del lugar podría contener una entidad PlaceType si se usa habitualmente. | Central Park <br> Safeway <br> Walmart| What time does the **Safeway** pharmacy open? (¿Cuándo abre la farmacia Safeway?) <br> Is **Walmart** open? (¿Está Walmart abierto?)
PlaceType | simple | El tipo de un destino que es un negocio local, restaurante, atracción pública o institución. | restaurante <br> ópera <br> cine | **cinemas** in Cambridge (Cines en Cambridge) <br> Is there a **restaurant** near me? (¿Hay algún restaurante cerca?)
PriceRange | simple | El intervalo de precios de los productos o servicios de la ubicación. | cheap (barato) <br> cost effective (rentable) <br> expensive (caro) | Find **affordable** appliance repair (Buscar una tienda de reparación de electrodomésticos asequible) <br> What's a **cheap** pizza place that's open now? (¿Qué pizzería barata está abierta ahora?)
Producto | simple | El producto que ofrece un lugar. | clothes (ropa) <br> televisions (televisores) | Where's the best place to get **food**? (¿Dónde se encuentra el mejor lugar para conseguir comida?) <br> Find me east kilbride looking for **televisions**. (Encontrar East Kilbride al buscar televisores)
Rating | simple | La clasificación de un lugar. | 5 estrellas <br> top <br> good (bien) | Are there any **good** places for me to go out and eat tomorrow (¿Hay algún buen lugar cerca de aquí para salir y comer mañana?) <br> **best** Amsterdam restaurants (Los mejores restaurantes de Ámsterdam) <br> List **top** three pizza shops. (Listar las tres primeras pizzerías)


## <a name="restaurantreservation"></a>**RestaurantReservation**
El dominio de reserva de restaurante admite intenciones para controlar las reservas de restaurantes.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
Reservar | Solicitar una reserva en un restaurante. | Reserve at Zucca for two for tonight. (Reservar en Zucca para dos para esta noche) <br> Book a table for tomorrow. (Reservar una mesa para mañana) <br> Table for 3 in Palo Alto at 7. (Mesa para tres en Palo Alto para las 7:00) <br> Make a reservation for 3 at Red Lobster. (Hacer una reserva para tres en Red Lobster)
DeleteReservation | Cancelar o eliminar una reserva de un restaurante. | Cancel the reservation at Zucca tomorrow night. (Cancelar la reserva en Zucca mañana por la noche) <br> Forget about my reservation for red lobster at 7:00 p.m. next Friday. (Olvídese de mi reserva para Red Lobster a las 7:00 p. m. el próximo viernes) <br> I don't need the reservation for Zucca, cancel it. (No necesito la reserva para Zucca, cancélela)
ChangeReservation | Change the time, place or number of people for an existing restaurant reservation. (Cambiar la hora, el lugar o el número de personas para una reserva de restaurante existente) | Change my reservation to 9 p.m. (Cambiar mi reserva a las 9 p. m.) <br> Change my reservation from Zucca to Red Lobster. (Cambiar mi reserva de Zucca a Red Lobster) <br> 7 people instead of 6 for the reservation at Zucca. (7 personas en lugar de 6 para la reserva en Zucca)
Confirm | Confirmar una acción relacionada con la reserva. <br> ***Aviso**: Esta intención incluye principalmente la acción "Confirm" para el escenario Restaurant Reservation. Si necesita expresiones más generales en "Confirm", aproveche la intención "Confirm" en el dominio **Utilities**.* | Yes, I've made the reservations for tonight at 8 o'clock at the pasta maker. (Sí, he realizado las reservas para esta noche a las 8 en punto en el italiano de pasta) <br> Yes, just book it. (Sí, solo tiene que reservarlo) <br> Confirm the reservation at Sushi Bar. (Confirmar la reserva en el Sushi Bar)
FindReservationDetail | Mostrar la información detallada de una reserva existente. | Find my reservation at renaissance San Diego (Buscar mi reserva en Renaissance San Diego) <br> Show the reservation tomorrow. (Mostrar la reserva de mañana) <br> Display details of my reservation for Zucca. (Mostrar los detalles de mi reserva de Zucca)
FindReservationWhere | Comprobar la ubicación absoluta de la reserva. | Where is the location of Zucca in my reservation? (¿Dónde está la ubicación de Zucca de mi reserva?) <br> Show the locale of my reservation tomorrow. (Mostrar la ubicación de mi reserva mañana)
FindReservationWhen | Comprobar la hora exacta de la reserva | When is the reservation of Zucca for tomorrow? (Cuándo es la reserva de Zucca para mañana?) <br> The time for my reservation at Red Lobster. (La hora de la reserva de Red Lobster)
Reject | El usuario rechaza el asistente virtual propuesto al administrar una reserva. <br> ***Aviso**: Consulte el dominio **Utilities** para más información sobre la expresiones generales de Reject.* | Not need to set the event. (No es necesario establecer el evento) | No, I don't want to change the reservation. (No, no deseo cambiar la reserva) <br> No, don't book it, I made a mistake. (No, no lo reservo, me he equivocado)

### <a name="entities"></a>**Entidades**
Entidad de LUIS | Tipo de entidad | DESCRIPCIÓN | Ejemplos
-------|------|---------|-------------------
Dirección | simple | La ubicación o dirección de un evento para una reserva. | Palo Alto<br>300 112th Ave SE<br>Seattle
Atmosphere | List | Una descripción del ambiente de un restaurante. | Romántico<br>Casual<br>Ideal para grupos<br>nice (bonito)
Cuisine | simple | Un tipo de comida, plato o gastronomía típica. | Chino<br>Italiano<br>Mejicano<br>Sushi<br>Noodle<br>steak (carne)
MealType | List | El tipo de comida asociado a una reserva. | Desayuno<br>Cena<br>Almuerzo<br>Refrigerio
PlaceName | simple | El nombre de un restaurante | Zucca<br>Cheesecake Factory<br>red lobster
Rating | simple | La clasificación de un lugar o restaurante. | 5 estrellas<br>3 estrellas<br>4 estrellas
NumberPeople | simple | Número de personas de la reserva | 3<br>six (seis)
Hora | datetime| La hora de la reserva del restaurante. | tomorrow (mañana)<br>tonight (esta noche)<br>7:00 p. m.<br>Christmas Eve (Fin de año)


## <a name="todo"></a>**ToDo**
El dominio _ToDo_ proporciona listas de tipos de tareas para que los usuarios agreguen, marquen y eliminen sus elementos de tareas pendientes.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
AddToDo | El usuario desea agregar elementos de la tarea para cualquiera de los tipos de lista. |  Remind me to buy milk. (Recordarme que tengo que comprar leche) <br> Add an item called "buy milk" to my to-do list (Agregar un elemento llamado "comprar leche" a mi lista de tareas pendientes)
Confirm | El usuario quiere confirmar una acción en curso. La expresión contiene una señal explícita como "Yes" (Sí) para confirmar una operación. <br> <br> ***Aviso**: Esta intención incluye principalmente la acción "Confirm" para el escenario ToDo. Si necesita expresiones más generales en "Confirm", aproveche la intención "Confirm" en el dominio **Utilities**.* | Please delete the task. (Elimine la tarea) <br> I'm sure I want to add this task. (Estoy seguro que quiero agregar esta tarea) <br> Yes, I want to do that. (Sí, deseo hacerlo)
Cancelar | El usuario quiere cancelar la acción en curso.  <br> <br> ***Aviso**: Esta intención incluye principalmente la acción "Confirm" para el escenario Restaurant Reservation. Si necesita expresiones más generales en "Confirm", aproveche la intención "Confirm" en el dominio **Utilities**.* | No, forget it. (No, olvídelo) <br> just cancel the task. (simplemente cancele la tarea) <br> No, don't add it. (No, no lo agregue)
DeleteToDo | Eliminar un elemento de la lista de tareas haciendo referencia a su orden o elimina todos los elementos pendientes. | Remove all tasks. (Quitar todas las etiquetas) <br> Help me to delete the second one. (Ayúdeme a eliminar el segundo)
MarkToDo | Marcar una tarea como finalizada o hacer referencia al orden o contenido de la tarea. | Mark the task "buy milk" as finished. (Marcar la tarea "comprar leche" como terminada) <br> Complete task reading. (Completar la lectura de la tarea) <br> Complete all (Completar todo)
ShowNextPage | Una lista se dividirá en varias páginas para mostrarla. Show list items on the next page for the user. (Mostrar los elementos de la lista en la página siguiente para el usuario) | Can you show next page in the shopping list? (¿Se puede mostrar la página siguiente de la lista de la compra?) <br> What’s on the next? (¿Qué es lo siguiente?) <br> Pasos siguientes
ShowPreviousPage | Mostrar al usuario los elementos de la lista de la página anterior. | show previous. (mostrar anterior) <br> I need to check previous tasks. (Tengo que comprobar las tareas anteriores)
ShowToDo | Mostrar todos los elementos de la lista de tareas pendientes. | Show my shopping list (Mostrar mi lista de la compra) <br> Show my grocery list. (Mostrar mi lista de la compra)

### <a name="entities"></a>**Entidades**
Entidad de LUIS | Tipo de entidad | DESCRIPCIÓN | Ejemplos
-------|------|---------|-------------------
ContainsAll | simple | Representar todos los elementos o alguno de la lista de tareas | can you help to remove **all** tasks. (¿Me puede ayudar a quitar todas las tareas?) <br> Finish **everything**. (Finalizar todo)
ordinal | ordinal | Una referencia ordinal o numérica a un elemento. | Mark the **third** one as completed. (Marcar la tercera como completada) <br> Delete the **first** task. (Eliminar la primera tarea)
ListType | simple | Tipo de lista de tareas.  | Add shoes to my **shopping** list. (Agregar zapatos a mi lista de la compra)
FoodOfGrocery | List | Detectar una lista de elementos de comida. | Remind me to buy **milk**. (Recordarme que tengo que comprar leche) <br> Add **beef** to my grocery list. (Agregar la carne a mi lista de la compra)
TaskContent | simple, pattern.any | Detectar el contenido de una tarea. | Remind me to **call my mother** please. (Recordarme llamar a mi madre) <br> Add **celebrate John’s birthday** to my to-do list (Agregar "celebrar cumpleaños de John" a mi lista de tareas pendientes)


## <a name="utilities"></a>**Utilities**
El dominio _Utilities_ es un dominio general entre todos los modelos pregenerados de _LUIS_ que contiene intenciones y expresiones comunes en diferentes escenarios.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
 Cancelar | Cancelar una acción, solicitud, tarea o servicio. | Cancel it. (Cancelarlo) <br> Never mind, forget it. (No se preocupe, olvídese)
 Confirm | Confirmar una acción. | Por supuesto <br> Yes, please (Sí, claro) <br> Confirme.
 Reject | El usuario rechaza el asistente virtual propuesto. | Sin
 FinishTask | Finalizar una tarea que el usuario inició. | I am done. (Ya he terminado) <br> That's all. (Esto es todo) <br> Finish. (Finalizar)
 Goback | Retroceder un paso o volver al paso anterior. | Go back a step. (Retroceder un paso) <br> Go back. (Volver)
 Ayuda | Pedir ayuda. | Please help. (Ayuda, por favor) <br> Open help. (Abrir la ayuda)
 Repetir | Repetir una acción. | Say it again. (Volver a decirlo)
 ShowNext | Mostrar o indicar los siguientes elementos. | Mostrar lo siguiente.
 ShowPrevious | Mostrar el elemento anterior de una serie. | Show previous one. (Mostrar el anterior)
 StartOver | Reiniciar la aplicación o iniciar una nueva sesión. | Restart. (Reiniciar)
 Stop | Indicar al asistente virtual que detenga la conversación.  | Stop saying this please. (Dejar de decir eso)
 SelectAny | El usuario pide que seleccione cualquier elemento o resultado mostrado en la pantalla. | Any of it. (Cualquiera de ellos) <br> Select any one. (Seleccionar cualquiera)
 SelectNone | El usuario no selecciona ninguno de los elementos existentes y pide más opciones. | Give me other suggestions. (Proporcionar más sugerencias) <br> None of them. (Ninguna de ellas)
  SelectItem | El usuario pide que se seleccione un elemento o resultado que se muestra en la pantalla. | Select the third one. (Seleccionar el tercero) <br> Select the top right one. (Seleccionar el de la parte superior derecha)
 Escalate | Solicitar servicio de atención al cliente para resolver los problemas. | Can I talk to a person? (¿Puedo hablar con una persona?)
 ReadAloud | Leer algo en voz alta al usuario. | Read this page. (Leer esta página) <br> Read it aloud. (Leer en voz alta)

### <a name="entities"></a>**Entidades**
Entidad de LUIS | Tipo de entidad | DESCRIPCIÓN | Ejemplos
------------|-------------|-------------|---------
ordinal | ordinal | Una referencia ordinal o numérica a un elemento. | The **second** one. (El segundo) <br> **Next** one. (El siguiente)
número | número | Número de elementos que quiere el usuario | The next **3** items (Los tres elementos siguientes)
DirectionalReference | simple | Un punto de referencia en el que se encuentra un elemento en la pantalla. | The right one (El de la derecha) <br> upper

## <a name="weather"></a>**Weather**
El dominio Tiempo se centra en la comprobación y las advertencias de las condiciones meteorológicas con la ubicación y la hora, o en la comprobación de la hora por las condiciones meteorológicas.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
 CheckWeatherValue | Solicitar el clima o un factor relacionado con el clima para una ubicación en el pronóstico o información pasada. <br> Los factores relacionados con el tiempo incluyen: <li> temperatura </li> <li> lluvia, nieve, precipitación </li> <li> seco, húmedo, humedad </li> <li> luz </li> <li> índice UV </li> <li> pulgadas de lluvia o nieve </li> | What is the Air Quality Index in Beijing? (¿Cuál es el índice de calidad del aire en Pekín?) <br> How much rain is expected in Seattle in March? (¿Cuánta lluvia se espera en Seattle en marzo?) <br> Record highest temperature of Hawaii. (Registrar la temperatura más alta de Hawai)
 CheckWeatherTime | Preguntar por la hora de los factores pronosticados o históricos relacionados con el clima para una ubicación. | When is it expected to rain? (¿Cuándo se espera que llueva?) <br> A good time to go to Canada (Un buen momento para ir a Canadá) <br> When is the hottest month in Minnesota? (¿Cuándo es el mes más caluroso de Minnesota?)
 QueryWeather | Preguntar por las condiciones meteorológicas o los factores relacionados con el clima para un lugar específico para el que se espera una respuesta de "sí, no o tal vez", o pedir consejo sobre actividades que dependan de las condiciones meteorológicas. | Will it rain tomorrow? (¿Lloverá mañana?) <br> Is it sunny today? (¿Está soleado hoy?) <br> Is May too early to go to Alaska? (¿Es mayo muy pronto para ir a Alaska?)
 ChangeTemperatureUnit | Cambiar la unidad de tiempo, incluidos Celsius, Kelvin y Fahrenheit. | Convert in celsius. (Convertir en Celsius) <br> Can i get that in Kelvin? (¿Puedo obtenerlo en Kelvin?)
 GetWeatherAdvisory | Obtener información meteorológica o alertas de una ubicación específica. | Are there weather advisories in Memphis? (¿Hay información meteorológica en Memphis?) <br> Are there any weather alerts for my area? (¿Hay alguna alerta meteorológica en mi área?)

### <a name="entities"></a>**Entidades**
Entidad de LUIS | Tipo de entidad | DESCRIPCIÓN | Ejemplos
------------|-------------|-------------|---------
Location | geography | La ubicación absoluta o implícita de una solicitud de tiempo. | Palo Alto<br>Shanghai<br>Seattle<br>Delvina<br>
Fecha y hora   | datetime | Fecha y hora o duración para consultar el tiempo. | November (Noviembre)<br>Cada hora<br>morning (por la mañana)<br>This weekend (Esta semana)<br>10 day (10 días)<br>
AdditionalWeatherCondition | list | Palabra descriptiva adicional para el clima, como la velocidad o dirección del viento. | dirección<br>Fast (rápido)<br>velocidad
Historic | simple | Palabras descriptivas de los datos climáticos históricos, incluido el promedio de casos límites en el período pasado. | past (pasado)<br>historical/history (historial o histórico)<br>seasonal (estacional)<br>best time (mejor clima)<br>ever recorded (jamás registrado)
PrecipitationUnit | dimensión | La precipitación de nieve o lluvia. | 5 inches (5 pulgadas)<br>6 cm
SuitableFor | simple | La descripción de una actividad humana bajo una condición climática, que es común cuando los usuarios consultan el consejo de actividad que depende de las condiciones climáticas. | jacket (chaqueta)<br>umbrella (paraguas)<br>swimming (nadar)
TemperatureUnit |temperatura | temperatura | 18 Celsius<br>7 Kelvin degrees (7 grados Kelvin)
WeatherRange | simple | Condición específica de temperatura, viento y otras condiciones meteorológicas en un período de tiempo | maximum<br>alta<br>bajo<br>average high (promedio alto)<br>Highest (el más alto)
WeatherCondition | simple | Descripción de la condición meteorológica | sunny (soleado)<br>rain (lluvia)<br>raining (lloviendo)<br>temperatura<br>snow (nieve)<br>hot
WindDirectionUnit | list | Las palabras relativas a la dirección del viento | norte<br>south (sur)<br>east (este)<br>west (oeste)<br>northeast (nordeste)


## <a name="web"></a>**Web**
El dominio Web proporciona la intención y las entidades para la búsqueda de un sitio web.
### <a name="intents"></a>**Intenciones**
Nombre de la intención | DESCRIPCIÓN | Ejemplos
---------|----------|---------
 WebSearch | Una solicitud para navegar a un sitio web específico o buscar en un motor de búsqueda. | Search surface in google.com. (Buscar superficie en google.com) <br> Find happy birthday song on web (Buscar la canción de cumpleaños feliz en la web) <br> Go to www.twitter.com. (Ir a www.twitter.com)

### <a name="entities"></a>**Entidades**
Entidad de LUIS | Tipo de entidad | DESCRIPCIÓN | Ejemplos
------------|-------------|-------------|---------
SearchEngine | List | El motor de búsqueda que el usuario quiere usar. | Bing <br> Google
SearchText | simple, pattern.Any | El texto que el usuario quiere buscar. <br> _Etiquetar "amigos de Facebook" como SearchText si el sitio web después de "in" no es un motor de búsqueda. La dirección URL también debe etiquetarse como SearchText._ | Película <br> Aprendizaje profundo <br> Tom Cruise
Vínculo | url | El vínculo del sitio web. | www.twitter.com

