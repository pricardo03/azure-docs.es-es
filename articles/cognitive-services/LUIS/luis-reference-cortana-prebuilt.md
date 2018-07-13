---
title: Referencia de la aplicación precompilada Cortana | Microsoft Docs
description: Referencia del asistente personal Cortana, una aplicación precompilada de Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: cd808c30a6781fc0252992c13ba247486e35ad44
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380374"
---
# <a name="cortana-prebuilt-app-reference"></a>Referencia de la aplicación precompilada Cortana
Esta referencia muestra las intenciones y entidades que reconoce la aplicación precompilada Cortana.

## <a name="cortana-prebuilt-intents"></a>Intenciones de la aplicación precompilada Cortana
La aplicación precompilada de asistente personal puede identificar las intenciones siguientes:

| Intención | Expresiones de ejemplo |
|--------| ------------------|
| builtin.intent.alarm.alarm_other|update my 7:30 alarm to be eight o'clock (actualizar mi alarma de las 7:30 a las ocho en punto) <br/>change my alarm from 8am to 9am (cambiar mi alarma de las 8 a las 9 de la mañana) |
| builtin.intent.alarm.delete_alarm|delete an alarm (eliminar una alarma) <br/>delete my alarm "wake up" (eliminar mi alarma de despertar)|
| builtin.intent.alarm.find_alarm|what time is my wake-up alarm set for? (¿a qué hora está puesta mi alarma de despertar?) <br/> is my wake-up alarm on? (está activada mi alarma de despertar?) |
| builtin.intent.alarm.set_alarm|turn on my wake-up alarm (activar mi alarma de despertar)<br/>can you set an alarm for 12 called take antibiotics? (¿uedes poner una alarma a las 12 llamada tomar antibióticos?)|
| builtin.intent.alarm.snooze|snooze alarm for 5 minutes (posponer alarma 5 minutos)<br/>snooze alarm (posponer alarma)|
| builtin.intent.alarm.time_remaining| how much longer do I have until "wake-up"? (¿cuánto me queda hasta despertar?) <br/> how much time until my next alarm? (¿cuánto tiempo hasta la siguiente alarma?)|
| builtin.intent.alarm.turn_off_alarm | turn off my 7am alarm (desactivar la alarma de las 7 de la mañana) <br/> turn off my wake-up alarm (desactivar mi alarma de despertar) |
| builtin.intent.calendar.change_calendar_entry| change an appointment (cambiar una cita)<br/>move my meeting from today to tomorrow (cambiar mi reunión de hoy a mañana)|
|builtin.intent.calendar.check_availability|is tim busy on Friday? (¿Tim está ocupado el viernes?)<br/>is brian free on Saturday (¿Brian está disponible el sábado?)|
|builtin.intent.calendar.connect_to_meeting|connect to a meeting (conectar con una reunión)<br/>join the meeting online (unirse a la reunión en línea)|
|builtin.intent.calendar.create_calendar_entry|I need to schedule an appointment with tony for Friday (necesito programar una cita con Tony el viernes)<br/>make an appointment with lisa at 2pm on Sunday (concertar una cita con Lisa a las 14:00 horas el domingo)|
|builtin.intent.calendar.delete_calendar_entry|delete my appointment (eliminar mi cita)<br/>remove the meeting at 3 pm tomorrow from my calendar (quitar la reunión de mañana a las 3 de la tarde de mi calendario)|
|builtin.intent.calendar.find_calendar_entry|show my calendar (mostrar mi calendario)<br/>display my weekly calendar (mostrar mi calendario semanal)|
|builtin.intent.calendar.find_calendar_when|when is my next meeting with jon? (¿cuándo es mi siguiente reunión con Jon?)<br/>what time is my appointment with larry on Monday? (¿a qué hora es mi cita con Larry el lunes?)|
|builtin.intent.calendar.find_calendar_where|show me the location of my 6 o'clock meeting (mostrar la ubicación de mi reunión de las seis en punto)<br/>where is that meeting with jon? (¿dónde es esa reunión con Jon?)|
|builtin.intent.calendar.find_calendar_who|who is this meeting with? (¿cón quién es esa reunión?)<br/>who else is invited? (¿quién más está invitado?)|
|builtin.intent.calendar.find_calendar_why|what are the details of my 11 o'clock meeting? (¿cuáles son los detalles de Mi reunión 11 en punto?)<br/>what is that meeting with jon about? (¿cuál es el tema de la reunión con Jon?)|
|builtin.intent.calendar.find_duration|how long is my next meeting (¿cuánto dura mi siguiente reunión?)<br/>how many minutes long is my meeting this afternoon (¿cuántos minutos es mi reunión de esta tarde?)|
|builtin.intent.calendar.time_remaining|how much time until my next appointment? (¿cuánto tiempo hasta la siguiente cita?)<br/>how much more time do I have before the meeting? (¿cuánto tiempo tengo antes de la siguiente reunión?)|
|builtin.intent.communication.add_contact|save this number and put the name as jose (guardar este número y llamarlo José)<br/>put jason in my contacts list (poner a Jason en mi lista de contactos)|
|builtin.intent.communication.answer_phone|answer (responder)<br/>answer the call (responder la llamada)|
|builtin.intent.communication.assign_nickname|edit nickname for nickolas (editar alias de nickolas)<br/>add a nickname for john doe (agregar alias para john doe)|
|builtin.intent.communication.call_voice_mail|voice mail (buzón de voz)<br/>call voicemail (llamar al buzón de voz)|
|builtin.intent.communication.find_contact|show me my contacts (mostrar mis contactos)<br/>find contacts (buscar contactos)|
|builtin.intent.communication.forwarding_off|stop forwarding my calls (dejar de desviar mis llamadas)<br/>switch off call forwarding (desactivar el desvío de llamadas)|
|builtin.intent.communication.forwarding_on|set call forwarding to send calls to home phone (configurar el reenvío de llamadas para enviar las llamadas al teléfono de casa)<br/>forward calls to home phone (desviar llamadas al teléfono de casa)|
|builtin.intent.communication.ignore_incoming|do not answer that call (no responder esa llamada)<br/>not now, I'm busy (ahora no, estoy ocupado)|
|builtin.intent.communication.ignore_with_message|don't answer that call but send a message instead (no responder esa llamada y enviar un mensaje en su lugar)<br/>ignore and send a text back (ignorar y enviar SMS)|
|builtin.intent.communication.make_call|call bob and john (llamar a rob y john)<br/>call mom and dad (llamar a mamá y papá)|
|builtin.intent.communication.press_key|dial star (marcar asterisco)<br/>press the 1 2 3 (pulsar 1 2 3)|
|builtin.intent.communication.read_aloud|read text (leer SMS)<br/>what did she say in the message (¿qué dijo en el mensaje?)|
|builtin.intent.communication.redial|redial my last call (volver a marcar mi última llamada)<br/>redial (volver a marcar)|
|builtin.intent.communication.send_email|email to mike waters mike that dinner last week was splendid (enviar correo electrónico a mike waters para decirle que la cena de la semana pasada fue magnífica)<br/>send an email to bob (enviar un correo electrónico a bob)|
|builtin.intent.communication.send_text|send text to bob and john (enviar SMS a bob y john)<br/>Mensaje|
|builtin.intent.communication.speakerphone_off|take me off speaker (desactivar altavoz)<br/>turn off speakerphone (desactivar el altavoz del teléfono)|
|builtin.intent.communication.speakerphone_on|speakerphone mode (modo de altavoz)<br/>put on speakerphone (poner en altavoz)|
|builtin.intent.mystuff.find_attachment|find the document john emailed me yesterday (buscar el documento que john me envió ayer por correo electrónico) <br/>find the doc from john (buscar el documento de john)|
|builtin.intent.mystuff.find_my_stuff|I want to edit my shopping list from yesterday (quiero editar la lista de la compra de ayer)<br/>find my chemistry notes from September (buscar mis notas de química de septiembre)
|builtin.intent.mystuff.search_messages|open message (abrir mensaje) <br/>messages|
|builtin.intent.mystuff.transform_my_stuff|share my shopping list with my husband (compartir mi lista de la compra con mi marido)<br/>delete my shopping list (eliminar mi lista de la compra)|
|builtin.intent.ondevice.open_setting|open cortana settings (abrir la configuración de Cortana) <br/>jump to notifications (saltar a notificaciones)|
|builtin.intent.ondevice.pause|turn off music (apagar la música)<br/>music off (música apagada)|
|builtin.intent.ondevice.play_music|I want to hear owner of a lonely heart (quiero oír owner of a lonely heart)<br/>play some gospel music (reproducir algo de gospel)|
|builtin.intent.ondevice.recognize_song|tell me what this song is (dime de quién es esta canción)<br/>analyze and retrieve title of song (analizar y recuperar el título de la canción)|
|builtin.intent.ondevice.repeat|repeat this track (repetir este tema)<br/>play this song again (reproducir de nuevo esta canción)|
|builtin.intent.ondevice.resume|restart music (reiniciar la música)<br/>start music again (empezar la música de nuevo)|
|builtin.intent.ondevice.skip_back|back up a track (subir un tema)<br/>previous song (canción anterior)|
|builtin.intent.ondevice.skip_forward|next song (siguiente canción)<br/>skip ahead a track (saltar un tema)|
|builtin.intent.ondevice.turn_off_setting|wifi off (wifi desactivada)<br/>disable airplane mode (deshabilitar el modo avión)|
|builtin.intent.ondevice.turn_on_setting|wifi on (wifi activada)<br/>turn on bluetooth (activar bluetooth)|
|builtin.intent.places.add_favorite_place|add this address to my favorites (agregar esta dirección a mis favoritos)<br/>save this location to my favorites (guardar esta ubicación en mis favoritos)|
|builtin.intent.places.book_public_transportation|buy a train ticket (comprar un billete de tren)<br/>book a tram pass (reservar un billete de tranvía)|
|builtin.intent.places.book_taxi|can you find me a ride at this hour? (¿puedes buscarme transporte a esta hora?)<br/>find a taxi (buscar un taxi)|
|builtin.intent.places.check_area_traffic|what's the traffic like on 520 (cómo está el tráfico en la 520)<br/>how is the traffic on i-5 (¿cómo está el tráfico en la i-5?)|
|builtin.intent.places.check_into_place|check into joya (entrar en joya)<br/>check in here (entrar aquí)|
|builtin.intent.places.check_route_traffic|show me the traffic on the way to kirkland (múestrame el tráfico hasta kirkland)<br/>how is the traffic to seattle? (¿cómo está el tráfico en seattle?)|
|builtin.intent.places.find_place|where do I live (¿dónde vivo?) <br/>give me the top 3 Japanese restaurants (dime los tres mejores restaurantes japoneses)|
|builtin.intent.places.get_address|show me the address of guu on robson street (muéstrame la dirección de guu en robson street)<br/>what is the address of the nearest starbucks? (¿cuál es la dirección del starbucks más cercano?)|
|builtin.intent.places.get_coupon|find deals on tvs in my area (buscar ofertas en TV en mi zona)<br/>discounts in mountain view (descuentos en mountain view)|
|builtin.intent.places.get_distance|how far away is holiday inn? (¿a qué distancia está el holiday inn?)<br/>what's the distance to tahoe (¿a qué distancia está tahoe?)|
|builtin.intent.places.get_hours|what are bar del corso's hours on Mondays? (¿qué horario tiene el bar del corso?)<br/>when is the library open? (¿cuál es el horario de la biblioteca?)|
|builtin.intent.places.get_menu|show me applebee's menu (muéstrame el menú del applebee)<br/>what's on the menu at sizzler (¿qué hay de menú en sizzler?)|
|builtin.intent.places.get_phone_number|give the number for home depot (dame el número de home depot)<br/>what is the phone number of the nearest starbucks? (¿cuál es el número del starbucks más cercano?)|
|builtin.intent.places.get_price_range|how much does dinner at red lobster cost (¿cuánto cuesta cenar en el red lobster?)<br/>how expensive is purple cafe (¿es muy caro el café purple?)|
|builtin.intent.places.get_reviews|show me reviews for local hardware stores (muéstrame las opiniones de las ferreterías locales)<br/>I want to see restaurant reviews (quiero ver opiniones de restaurantes)|
|builtin.intent.places.get_route|give me directions to (dime cómo llegar a)|it is possible to get to pizza hut on foot (¿se puede llegar a pizza hut a pie?)|
|builtin.intent.places.get_star_rating|how many stars does the French laundry have? (¿cuántas estrellas tiene la lavandería French?)<br/>is the aquarium in monterrey good? (el acuario de monterrey es bueno?)|
|builtin.intent.places.get_transportation_schedule|what time does the san francisco ferry leave? (¿a qué hora sale el ferry para san francisco?)<br/>when is the latest train to seattle? (¿a qué hora sale el último tren a seattle?)|
|builtin.intent.places.get_travel_time|what's the driving time to denver from SF (¿cuánto se tarda conduciendo a denver desde san francisco?)<br/>how long will it take to get to san francisco from here (¿cuánto se tarda en llegar a san francisco desde aquí?)|
|builtin.intent.places.make_call|call Dr. smith in bellevue (llamar al dr. smith en bellevue)<br/>call the home depot on 1st street (llamar a home depot en 1st street)|
|builtin.intent.places.rate_place|give a rating for this restaurant (puntuar este restaurante)<br/>rate il fornaio 5 stars on yelp (puntuar il fornaio con 5 estrellas en yelp)|
|builtin.intent.places.show_map|what's my current location (¿cuál es mi ubicación actual?) <br/>what's my location (cuál es mi ubicación)|
|builtin.intent.places.takes_reservations|is it possible to make a reservation at the olive garden (¿se puede hacer una reserva en the olive garden?)<br/>does the art gallery accept reservations (¿la galería de arte acepta reservas?)|
|builtin.intent.reminder.change_reminder|change a reminder (cambiar un aviso)<br/>move up my picture day reminder 30 minutes (retrasar 30 minutos el aviso del día de mi imagen)|
|builtin.intent.reminder.create_single_reminder|remind me to wake up at 7 am (recordarme que me levante a las 7 a.m.)<br/>remind me to go trick or treating with luca at 4:40pm (recordarme ir a truco o trato con luca a las 4:40 p.m.)|
|builtin.intent.reminder.delete_reminder|delete this reminder (eliminar este aviso)<br/>delete my picture reminder (eliminar el aviso de mi imagen)|
|builtin.intent.reminder.find_reminder|do I have any reminders set up for today (¿tengo avisos configurados para hoy?)<br/>do I have a reminder set for luca's party (¿tengo configurado un aviso para la fiesta de luca?)|
|builtin.intent.reminder.read_aloud|read reminder out loud (leer el aviso en voz alta)<br/>read that reminder (leer ese aviso)|
|builtin.intent.reminder.snooze|snooze that reminder until tomorrow (posponer ese aviso hasta mañana)<br/>snooze this reminder (posponer este aviso)|
|builtin.intent.reminder.turn_off_reminder|turn off reminder (desactivar aviso)<br/>dismiss airport pickup reminder (descargar aviso de recogida en aeropuerto)|
|builtin.intent.weather.change_temperature_unit|change from fahrenheit to kelvin (cambiar de fahrenheit a kelvin) <br/>change from fahrenheit to celsius (cambiar de fahrenheit a celsius)|
|builtin.intent.weather.check_weather|show me the forecast for my upcoming trip to seattle (mostrarme la previsión de mi próximo viaje a seattle)<br/>how will the weather be this weekend (¿qué tiempo hará este fin de semana?)|
|builtin.intent.weather.check_weather_facts|what is the weather like in hawaii in December? (¿qué tiempo hará en hawaii en diciembre?)<br/>what was the temperature this time last year? (¿cuál fue la temperatura en esta época el año pasado?)|
|builtin.intent.weather.compare_weather|give me a comparison between the temperature high and lows of dallas and houston, tx (comparar las temperaturas máximas y mínimas de dallas y houston, tx)<br/>how does the weather compare to slc and nyc (¿qué diferencia de tiempo hay entre slc y nyc?)|
|builtin.intent.weather.get_frequent_locations|give me my most frequent location (dime mi ubicación más frecuente)<br/>show most often stops (muéstrame mis paradas más frecuentes)|
|builtin.intent.weather.get_weather_advisory|weather warnings (advertencias meteorológicas)<br/>show weather advisory for sacramento (muéstrame los avisos meteorológicos para sacramento)|
|builtin.intent.weather.get_weather_maps|display a weather map (mostrar un mapa meteorológico)<br/>show me weather maps for africa (muéstrame mapas meteorológicos de áfrica)|
|builtin.intent.weather.question_weather|will it be foggy tomorrow morning? (¿habrá niebla mañana por la mañana?)<br/>will I need to shovel snow this weekend? (¿necesitaré una pala de nieve este fin de semana?)|
|builtin.intent.weather.show_weather_progression|show local weather radar (mostrar el radar meteorológico local)<br/>begin radar (comenzar radar)|
|builtin.intent.none|how old are you (cuántos años tienes)<br/>open camera (abrir cámara)|

## <a name="prebuilt-entities"></a>Entidades precompiladas 

Estos son algunos ejemplos de entidades que la aplicación de asistente personal precompilada puede identificar:

|Entidad |Ejemplo de entidad en una expresión |
|-------|------------------|
|builtin.alarm.alarm_state | turn `off` my wake-up alarm (activar mi alarma de despertar)    <br/> is my wake-up alarm `on` (¿está activada mi alarma de despertar?)  | 
|builtin.alarm.duration |snooze for `10 minutes` (posponer 10 minutos)    <br/> snooze alarm for `5 minutes` (posponer alarma 5 minutos)  |
|builtin.alarm.start_date | set an alarm for `monday` at 7 am (establecer una alarma para el lunes a las 7 a.m.)   <br/> set an alarm for `tomorrow` at 7 am (establecer una alarma para mañana a las 7 a.m.)   |
|builtin.alarm.start_time | create an alarm for `30 minutes` (crear una alarma para 30 minutos)    <br/> set the alarm to go off `in 20 minutes` (establecer la alarma para que se apague en 20 minutos)   |
|builtin.alarm.title | is my `wake up` alarm on (¿mi alarma de despertar está activada?) <br/> can you set an alarm for quarter to 12 Monday to Friday called `take antibiotics` (¿puedes establecer una alarma a las doce menos cuarto de lunes a viernes llamada tomar antibióticos?) |
|builtin.calendar.absolute_location | create an appointment for tomorrow at `123 main street` (crear una cita para mañana en 123 main streen)   <br/> the meeting will take place in `cincinnati` on the 5th of june (la reunión tendrá lugar en cincinnati el 5 de junio)    |
|builtin.calendar.contact_name|put a marketing meeting on my calendar and be sure that `joe` is there (poner una reunión de marketing en mi calendario y asegurarme de que joe esté allí) <br/>I want to set up a lunch at il fornaio and invite `paul` (quiero convocar una comida en il fornaio e invitar a paul) |   
|builtin.calendar.destination_calendar|add this to my `work` schedule (agregar esto a mi calendario de trabajo)   <br/>put this on my `personal` calendar (poner esto en mi calendario personal) |
|builtin.calendar.duration| set up an appointment for `an hour` at 6 tonight (concertar una cita de una hora a las 6 esta tarde) <br/>  book a `2 hour` meeting with joe (reservar una reunión de 2 horas con joe) |  
|builtin.calendar.end_date | create a calendar entry called vacation from tomorrow until `next monday` (crear una entrada en el calendario llamada vacaciones desde mañana hasta el próximo lunes) <br/>    block my time as busy until `monday, october 5th` (marcar mi tiempo como ocupado hasta el lunes 5 de octubre) | 
|builtin.calendar.end_time | the meeting ends at `5:30 PM` (la reunión termina a las 5:30 p.m.) <br/> schedule it from 11 to `noon` (programarlo de 11 a 12 del mediodía)  |   
|builtin.calendar.implicit_location | cancel the appointment at the dmv (cancelar la cita en dmv) <br/> change the location of miles' birthday to `poppy restaurant` (cambiar la ubicación del cumpleaños de miles al restaurante poppy) |    
|builtin.calendar.move_earlier_time | push the meeting forward `an hour` (retrasar la reunión una hora) <br/> move the dentist's appointment up `30 minutes` (adelantar la cita con el dentista 30 minutos)       |
|builtin.calendar.move_later_time | move my dentist appointment `30 minutes` (cambiar la cita con el dentista 30 minutos)    <br/> push the meeting out `an hour` (retrasar la reunión una hora) |  
|builtin.calendar.original_start_date | reschedule my appointment at the barber from 'Tuesday' to Wednesday (cambiar mi cita con el peluquero del martes al miércoles) <br/> move my meeting with ken from `monday` to Tuesday (cambiar mi reunión con ken del lunes al martes) |
|builtin.calendar.original_start_time | reschedule my meeting from `2:00` to 3 (cambiar mi reunión de las 2 a las 3)  <br/> change my dentist appointment from `3:30` to 4 (cambiar mi cita con el dentista de las 3:30 a las 4) |  
|builtin.calendar.start_date | what time does my party start on `flag day` (¿a qué hora empieza mi fiesta el día de la bandera?) <br/> schedule lunch for the `friday after next` at noon (concertar comida dentro de dos viernes a mediodía) 
|builtin.calendar.start_time | I want to schedule it for `this morning` (quiero programarlo para esta mañana) <br/> I want to schedule it in the `morning` (quiero programarlo durante la mañana) |  
|builtin.calendar.title | `vet appointment`  <br/> `dentist` Tuesday (dentista el martes) |
|builtin.communication.audio_device_type | make the call using `bluetooth` (llamar usando bluetooth)  <br/> call using my `headset` (llamar con los auriculares) | 
|builtin.communication.contact_name | text `bob jones` (enviar SMS a bob jones)  <br/> | call `sarah` (llamar a sarah)|
|builtin.communication.destination_platform | call dave in `london` (llamar a dave en londres) <br/> call his `work line` (llamar a su número de trabajo) |    
|builtin.communication.from_relationship_name | show calls from my `daughter` (mostrar las llamadas de mi hija) <br/> read the email from `mom` (leer el correo de mamá)   |   
|builtin.communication.key | dial `star` (marcar asterisco) <br/>  press the `hash` key (pulsar la tecla almohadilla)    |
|builtin.communication.message | email carly to say `i'm running late` (enviar correo electrónico a carly para decir que llego tarde) <br/> text angus smith `good luck on your exam` (enviar SMS a angus buena suerte con tu examen) |    
|builtin.communication.message_category | new email marked for `follow up` (nuevo correo electrónico marcado para seguimiento)  <br/> new email marked `high priority` (nuevo correo electrónico marcado como prioridad alta) |    
|builtin.communication.message_type | send an `email` (enviar correo electrónico)   <br/> read my `text` messages aloud (leer mis SMS en voz alta) |
|builtin.communication.phone_number | I want to dial `1-800-328-9459` (quiero marcar 1-800-3289549) <br/>     call `555-555-5555` (llamar al 555-555-5555) |   
|builtin.communication.relationship_name | text my `husband` (enviar SMS a mi marido) <br/>  email `family` (enviar correo electrónico a familia)| 
|builtin.communication.slot_attribute | change the `recipient` (cambiar el destinatario) <br/>    change the `text` (cambiar el texto) | 
|builtin.comminication.source_platform | call him from `skype` (llamarle desde skype) <br/> call him from my `personal line` (llamarle desde mi número personal) |
|builtin.mystuff.attachment| with documents `attached` (con documentos adjuntos) <br/> find the email `attachment` bob sent (buscar el adjunto de correo que bob envió) |
|builtin.mystuff.contact_name| find the spreadsheet lisa sent to `me` (buscar la hoja de cálculo que lisa me envió) <br/> where's the document i sent to `susan` last night (dónde está el documento que envié a susan anoche) |
|builtin.mystuff.data_source | `c:\dev\` <br/> my `desktop` (mi escritorio) <br/> |`"resolution": { "resolution_type": "metadataItems", "metadataType": CanonicalEntity", "value": "desktop" }`|
|builtin.mystuff.data_type | locate the `document` i worked on last night (buscar el documento en el que trabajé anoche) <br/> |`"resolution": {"resolution_type": "metadataItems", "metadataType": "CanonicalEntity", "value":Document" }` <br/> bring up mike's `visio diagram` (mostrar el diagrama de visio de mike)  |
|builtin.mystuff.end_date| show me the docs i worked on `between yesterday and today` (mostrarme los documentos en los que trabajé entre ayer y hoy)   <br/> find what doc i was working on `before thursday the 31st` (buscar el documento en el que estuve trabajando entre el jueves y el día 31) |
|builtin.mystuff.end_time| find files i saved `before noon` (buscar los archivos que guardé antes de mediodía) <br/> find what doc i was working on `before 4pm` (buscar el documento en el que estuve trabajando antes de las 4 p.m.)|      
|builtin.mystuff.file_action| open the spreadsheet i `saved` yesterday (abrir la hoja de cálculo que guardé ayer) <br/> find the spreadsheet kevin `created` (buscar la hoja de cálculo que kevin creó)| 
|builtin.mystuff.from_contact_name | find the proposal `jason` sent me (buscar la propuesta que jason me envió) <br/> open `isaac` 's last email (abrir el último correo electrónico de isaac) |
|builtin.mystuff.keyword | show me the `french conjugation` files (mostrarme los archivos de conjugación de francés) <br/> find the `marketing plan` i drafted yesterday (buscar el plan de marketing que redacté ayer) |
|builtin.mystuff.location | the document i edited at `work` (el documento que edité en el trabajo) <br/> photos i took in `paris` (las fotos que hice en parís) |
|builtin.mystuff.message_category | look for my `new` emails (buscar mis correos electrónicos nuevos) <br/> search for my `high priority` email (buscar mi correo de prioridad alta) |
|builtin.mystuff.message_type | check my `email` (comprobar mi correo electrónico) <br/>  show me my `text` messages (mostrarme mis mensajes SMS)  |
|builtin.mystuff.source_platform | search my `hotmail` email for email from john (buscar en mi correo electrónico de hotmail los mensajes de john)    <br/> find the document i sent from `work` (buscar el documento que envié desde el trabajo) |
|builtin.mystuff.start_date | find notes from `january` (buscar las notas de enero) <br/> find the email i send rob `after january 1st` (buscar el correo electrónico que envié a rob después del 1 de enero) |
|builtin.mystuff.start_time | find that email i sent rob sometime before 2pm but `after noon`? (buscar el correo electrónico que envié a rob en algún momento antes de las 2 p.m. pero después de las 12 p.m.) <br/> find the worksheet kristin sent to me that I edited `last night` (buscar la hoja de cálculo que kristin me envió y que edité anoche) | 
|builtin.mystuff.title | `c:\dev\mystuff.txt` <br/> `*.txt` |
|builtin.mystuff.transform_action | `download` the file john sent me (desccargar el archivo que john me envió) <br/> `open` my annotation guidelines doc (abrir el documento con las directrices de anotación) |
|builtin.note.note_text | create a grocery list including `pork chops, applesauce and milk` (crear una lista de la compra que incluya chuletas de cerdo, salsas de manzana y leche) <br/> make a note to `buy milk` (crear nota para comprar leche) |
|builtin.note.title | make a note called `grocery list` (crear una nota llamada lista de la compra) <br/> make a note called `people to call` (crear una nota llamada gente para llamar) |
|builtin.ondevice.music_artist_name | play everything by `rufus wainwright` (reproducir todo lo de rufus wainwright) <br/> play `garth brooks` music (reproducir la música de garth brooks) |   
|builtin.ondevice.music_genre | show `classic rock` songs (mostrar canciones de rock clásico) <br/> play my `classical` music from the baroque period (reproducir mi música clásica del período barroco) |
|builtin.ondevice.music_playlist | shuffle all britney spears from `workout` playlist (reproducir aleatoriamente todo lo de britney spears de la lista de reproducción ejercicio) <br/> play `breakup` playlist (reproducir la lista de reproducción ruptura)
|builtin.ondevice.music_song_name | play `summertime` (reproducir verano) <br/> play `me and bobby mcgee` (reproducir me and bobby mcgee) | 
|builtin.ondevice.setting_type | `quiet hours` <br/> `airplane mode` |
|builtin.places.absolute_location | take me to the intersection of `5th and pike` (llévame a la intersección de la quinta con pike) <br/> no, I want directions to `1 microsoft way redmond wa 98052` (no, quiero indicaciones para llegar a 1 microsoft way redmond wa 98052) |
|builtin.places.atmosphere | look for `interesting` places to go out (buscar lugares interesantes donde ir)    <br/> where can I find a `casual` restaurant (dónde puedo encontrar un restaurante informal) |  
|builtin.places.audio_device_type |call the post office on `hands free` (llamar a la oficina de correos con el manos libres) <br/> call papa john's with `speakerphone` (llamar a papa john con el altavoz) |    
|builtin.places.avoid_route | avoid the `toll road` (evitar el peaje) <br/> get me to san francisco avoiding the `construction on 101` (llévame a san francisco evitando las obras de la 101) |  
|builtin.places.cuisine | `halal` deli near mountain view (tienda de embutidos halal cerca de mountain view)   <br/> `kosher` fine dining on the peninsula (cena kosher de calidad en la península) |
|builtin.places.date | make a reservation for `next friday the 12th` (realizar una reserva para el próximo viernes 12) <br/>  is mashiko open on `mondays`? (¿está abierto mashiko los lunes?) |
|builtin.places.discount_type | find a `coupon` for macy's (buscar un cupón para macy's) <br/> find me a `coupon` (búscame un cupón) |
|builtin.places.distance | is there a good diner `within 5 miles` of here (¿hay buenos sitios para cenar a menos de 5 kilómetros de aquí?) <br/> find ones `within 15 miles` (buscar a menos de 5 kilómetros) |   
|builtin.places.from_absolute_location | directions from `45 elm street` to home (indicaciones desde 45 elm street a casa) <br/> get me directions from `san francisco` to palo alto (indícame como llegar desde san francisco a palo alto)  |
|builtin.places.from_place_name | driving from the `post office` to 56 center street (conducir desde la oficina de correos hasta el 56 de center street) <br/> get me directions from `home depot` to lowes (indícame cómo llegar desde home depot a lowes) |
|builtin.places.from_place_type | directions to downtown from `work` (indicaciones para ir al centro desde el trabajo) <br/>  get me directions from the `drug store` to home (indícame cómo llegar desde la droguería a casa) |
|builtin.places.meal_type | nearby places for `dinner` (lugares cercanos para cenar) <br/> find a good place for a business `lunch` (buscar un buen lugar para una comida de negocios) | 
|builtin.places.nearby | show me some cool shops `near` me (muéstrame buenas tiendas cerca)  <br/> are there any good Lebanese restaurants `around` here? (¿hay buenos restaurantes libaneses cerca de aquí?) |
|builtin.places.open_status | when is the mall `closed` (¿cuándo cierra el centro comercial?) <br/> get me the `opening` hours of the store (dime el horario de apertura de la tienda) | 
|builtin.places.place_name | take me to `central park` (llévame a central park) <br/> look up the `eiffel tower` (buscar la torre eiffel) |   
|builtin.places.place_type | `atms` <br/> `post office` |
|builtin.places.prefer_route | show directions by the `shortest` route (mostrar indicaciones para llegar por la ruta más corta) <br/> take the `fastest` route (tomar la ruta más rápida) | 
|builtin.places.price_range| give me places that are `moderately affordable` (indícame lugares que sean moderadamente asequibles) <br/>  I want an `expensive` one (quiero uno caro)   |
|builtin.places.product | where can I get `fresh fish` around here (dónde puedo comprar pescado fresco por aquí cerca)  <br/> where around here sells `bare minerals` (¿dónde venden por aquí minerales sin procesar?) |
|builtin.places.public_transportation_route | bus schedule for the `m2` bus (horarios del bus m2) <br/> bus route `3x` (ruta del bus 3x) |  
|builtin.places.rating | show `3 star` restaurants (mostrar restaurantes de 3 estrellas) <br/> show results that are `3 stars or higher` (mostrar resultados con 3 estrellas o más)  |
|builtin.places.reservation_number | book a table for `seven` people (reservar una mesa para siete personas) <br/>  make a reservation for `two` at il fornaio (realizar una reserva para dos en il fornaio) |
|builtin.places.results_number | show me the `10` coffee shops closest to here (mostrarme las 10 cafeterías más próximas) <br/> show me top `3` aquariums (mostrarme los 3 principales acuarios)  
|builtin.places.service_provided | where can I go to `whale watch` by bus? (dónde puedo ir a ver ballenas en autobús?) <br/> I need a mechanic to `fix my brakes` (necesito un mecánico para arreglar los frenos) |    
|builtin.places.time | I want places that are open on Saturday at `8 am` (quiero lugares que estén abiertos el sábado a las 8 a.m.)| is mashiko open `now` (¿está mashiko abierto ahora?) |
|builtin.places.transportation_company | train schedules for `new jersey transit` (horarios de tren para new jersey) <br/> can I get there on `bart` (¿puedo llegar allí en bart) | 
|builtin.places.transportation_type | where is a music store i can get to `on foot`? (¿dónde hay una tienda de música a la que pueda ir a pie?) <br/>  give me `biking` directions to mashiko (dime cómo llegar a mashiko en bicicleta)|
|builtin.places.travel_time | I want to be able to drive `less than 15 minutes` (quiero poder conducir menos de 15 minutos) <br/>   I want somewhere I can get to in `under 15 minutes` (quiero algún sitio al que pueda llegar en menos de 15 minutos) |   
|builtin.reminder.absolute_location | remind me to call my dad when i land in `chicago` (recordarme que llame a mi padre cuando aterrice en chicago) <br/> when I get back to `seattle` remind me to get gas (recuérdame que eche gasolina cuando vuelva a seattle) |
|builtin.reminder.contact_name | when `bob` calls, remind me to tell him the joke (cuando bob llame, recordarme que le cuente el chiste) <br/> create a reminder to mention the school bus when I talk to `arthur` (crear un aviso para mencionar el bus escolar cuando hable con arthur) |
|builtin.reminder.leaving_absolute_location | reminder pick up craig when leaving `1200 main` (aviso para recoger a craig cuando salga de 1200 main) |
|builtin.reminder.leaving_implicit_location | remind me to get gas when I leave `work` (recordarme que eche gasolina cuando salga de trabajar)  |
|builtin.reminder.original_start_date | change the reminder about the lawn from `saturday` to Sunday (cambiar el aviso sobre el césped del sábado al domingo) <br/> move my reminder about school from `monday` to Tuesday (cambiar el aviso sobre la escuela del lunes al martes)   |
|builtin.reminder.relationship_name | when my `husband` calls, remind me to tell him about the pta meeting (cuando llame mi marido, recordarme que le cuente sobre la reunión de padres) <br/> remind me again when `mom` calls (volver a recordármelo cuando llame mamá)|
|builtin.reminder.reminder_text | can you remind me to `bring up my small spot of patchy skin` when dr smith'calls (puedes recordarme que comente lo de la mancha en la piel cuando llame el dr. smith)  <br/> remind me to `pick up dry cleaning` at 4:40 (recordarme recoger la ropa seca a las 4:40)   |
|builtin.reminder.start_date | remind me the `thursday after next` at 8 pm (recordármelo dentro de dos jueves a las 8 p.m.)  <br/> remind me `next thursday the 18th` at 8 pm (recordármelo el próximo jueves 18 a las 8 p.m.)    |
|builtin.reminder.start_time | create a reminder `in 30 minutes` (crear un aviso en 30 minutos) <br/> create a reminder to water the plants `this evening at 7` (crear un aviso para regar las plantas esta tarde a las 7) |  
|builtin.weather.absolute_location | will it rain in `boston` (¿lloverá en boston?) <br/> what's the forecast for `seattle`? (¿cuál es la previsión para seattle?)  |
|builtin.weather.date_range | weather in nyc `this weekend` (tiempo en nyc este fin de semana) <br/>   look up the `five day` forecast in hollywood florida (mirar la previsión a cinco días en hollywood florida) |
|builtin.weather.suitable_for | can I go `hiking` in shorts this weekend? (¿puedo ir a hacer senderismo en pantalón corto este fin de semana?)   <br/> will it be nice enough to `walk` to the game today? (¿hará bueno hoy para caminar hasta el partido?) | 
|builtin.weather.temperature_unit | what is the temperature today in `kelvin` (¿cuál es la temperatura hoy en kelvin?)   <br/> show me the temps in `celsius` (mostrarme la temperatura en celsius) |  
|builtin.weather.time_range | does it look like it will snow `tonight`? (¿parece que va a nevar esta noche?) <br/> is it windy right `now`? (¿hace viento ahora mismo?)  |
|builtin.weather.weather_condition | show `precipitation` (mostrarme las precipitaciones) <br/> how thick is the `snow` at lake tahoe now? (¿qué grosor de nieve hay ahora en el lago tahoe?)  |

