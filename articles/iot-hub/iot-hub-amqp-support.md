---
title: Descripción de la compatibilidad de AMQP de Azure IoT Hub | Microsoft Docs
description: 'Guía del desarrollador: compatibilidad con dispositivos que se conectan a IoT Hub puntos de conexión accesibles desde el dispositivo y orientado al servicio mediante el protocolo AMQP. Incluye información sobre la compatibilidad integrada de AMQP en el SDK de dispositivo IoT de Azure.'
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: f39f184bdc09677e347a2691351309dd6483f467
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65965393"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Comunicarse con IoT hub mediante el protocolo AMQP

IoT Hub admite [AMQP versión 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) ofrecer una variedad de funcionalidades a través de puntos de conexión accesibles desde el dispositivo y orientado al servicio. Este documento describe el uso de AMQP de clientes se conecten a IoT Hub para usar la funcionalidad de IoT Hub.

## <a name="service-client"></a>Cliente del servicio

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Conexión y la autenticación en IoT Hub (cliente de servicio)
Para conectarse a IoT Hub mediante AMQP, un cliente puede utilizar el [seguridad en función de las notificaciones (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) o [autenticación Simple de autenticación y seguridad SASL (nivel)](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

La siguiente información es necesaria para el cliente del servicio:

| Información | Valor | 
|-------------|--------------|
| Nombre de host de Centro de IoT | `<iot-hub-name>.azure-devices.net` |
| Nombre de clave | `service` |
| Clave de acceso | Clave principal o secundaria asociada con el servicio |
| Firma de acceso compartido | SAS de corta duración en el siguiente formato: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (encontrará el código para generar esta firma [aquí](./iot-hub-devguide-security.md#security-token-structure)).


El fragmento de código siguiente usa [uAMQP biblioteca de Python](https://github.com/Azure/azure-uamqp-python) para conectarse a IoT hub a través de un vínculo del remitente.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Invocar los mensajes de nube a dispositivo (cliente de servicio)
Se describe el intercambio de mensajes de nube a dispositivo entre el servicio y IoT Hub, así como entre el dispositivo e IoT Hub [aquí](iot-hub-devguide-messages-c2d.md). El cliente de servicios utiliza dos vínculos, que se describe a continuación para enviar mensajes y recibir comentarios de los mensajes enviados anteriormente de dispositivos.

| Creado por | Tipo de vínculo | Ruta de acceso del vínculo | DESCRIPCIÓN |
|------------|-----------|-----------|-------------|
| Servicio | Vínculo del remitente | `/messages/devicebound` | Mensajes C2D destinados a los dispositivos se envían a este vínculo por el servicio. Los mensajes enviados a través de este vínculo tienen sus `To` propiedad establecida en la ruta de vínculo del receptor del dispositivo de destino: es decir, `/devices/<deviceID>/messages/devicebound`. |
| Servicio | Vínculo del receptor | `/messages/serviceBound/feedback` | Mensajes de comentarios finalización, el rechazo y abandono procedentes de dispositivos recibidos en este vínculo por servicio. Para obtener más información acerca de los mensajes de comentarios, vea [aquí](./iot-hub-devguide-messages-c2d.md#message-feedback). |

El siguiente fragmento de código muestra cómo crear un mensaje C2D y enviarla a un dispositivo mediante [uAMQP biblioteca de Python](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Para recibir comentarios, cliente de servicios crea un vínculo del receptor. El siguiente fragmento de código muestra cómo hacerlo mediante [uAMQP biblioteca de Python](https://github.com/Azure/azure-uamqp-python).

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Como se indicó anteriormente, un mensaje de comentarios C2D tiene el tipo de contenido de `application/vnd.microsoft.iothub.feedback.json` y propiedades en el cuerpo JSON se pueden usar para inferir el estado de entrega del mensaje original:
* Clave `statusCode` en comentarios cuerpo tiene cualquiera de estos valores: `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Clave `deviceId` en comentarios cuerpo tiene el Id. de dispositivo de destino.
* Clave `originalMessageId` en comentarios cuerpo tiene el identificador del mensaje C2D original enviado por el servicio. Esto puede usarse para correlacionar sus comentarios a los mensajes C2D.

### <a name="receive-telemetry-messages-service-client"></a>Recibir mensajes de telemetría (cliente de servicio)
De forma predeterminada, IoT Hub almacena los mensajes de telemetría de dispositivo introducidos en un centro de eventos integrado. El cliente de servicios puede usar el protocolo AMQP para recibir los eventos almacenados.

Para ello, el cliente del servicio en primer lugar debe conectarse al punto de conexión de IoT Hub y recibir una dirección de redirección a los centros de eventos integrados. Cliente de servicio, a continuación, usa la dirección proporcionada para conectarse al centro de eventos integrado.

En cada paso, el cliente debe presentar las siguientes piezas de información:
* Credenciales de servicio válido (token de SAS de servicio).
* Una ruta de acceso tiene el formato correcto tiene la intención de recuperar los mensajes de la partición de grupo de consumidores. Para un identificador de partición y de grupo de consumidores determinado, la ruta de acceso tiene el formato siguiente: `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` (el grupo de consumidores predeterminado es `$Default`).
* Un predicado de filtro opcional para designar un punto de partida en la partición (Esto puede ser en forma de una marca de tiempo de secuencia número, offset o puestos en cola).

El fragmento de código siguiente usa [uAMQP biblioteca de Python](https://github.com/Azure/azure-uamqp-python) para mostrar los pasos anteriores.

```python
import json
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified using endpiont_filter
# Valid predicates include:
# - amqp.annotation.x-opt-sequence-number
# - amqp.annotation.x-opt-offset
# - amqp.annotation.x-opt-enqueued-time
# Set endpoint_filter variable to None if no filter is needed
endpoint_filter = b'amqp.annotation.x-opt-sequence-number > 2995'

# Helper function to set the filtering predicate on the source URI
def set_endpoint_filter(uri, endpoint_filter=''):
  source_uri = uamqp.address.Source(uri)
  source_uri.set_filter(endpoint_filter)
  return source_uri

receive_client = uamqp.ReceiveClient(set_endpoint_filter(uri, endpoint_filter), debug=True)
try:
  batch = receive_client.receive_message_batch(max_batch_size=5)
except uamqp.errors.LinkRedirect as redirect:
  # Once a redirect error is received, close the original client and recreate a new one to the re-directed address
  receive_client.close()

  sas_auth = uamqp.authentication.SASTokenAuth.from_shared_access_key(redirect.address, policy_name, access_key)
  receive_client = uamqp.ReceiveClient(set_endpoint_filter(redirect.address, endpoint_filter), auth=sas_auth, debug=True)

# Start receiving messages in batches
batch = receive_client.receive_message_batch(max_batch_size=5)
for msg in batch:
  print('*** received a message ***')
  print(''.join(msg.get_data()))
  print('\t: ' + str(msg.annotations['x-opt-sequence-number']))
  print('\t: ' + str(msg.annotations['x-opt-offset']))
  print('\t: ' + str(msg.annotations['x-opt-enqueued-time']))
```

Para un identificador de dispositivo determinado, IoT Hub usa un hash del identificador de dispositivo para determinar qué partición para almacenar sus mensajes en. El fragmento de código anterior muestra la recepción de eventos de una sola partición de este tipo. Sin embargo, tenga en cuenta que una aplicación típica a menudo se necesita recuperar los eventos almacenados en todas las particiones de event hubs.


### <a name="additional-notes"></a>Notas adicionales
* Las conexiones AMQP podrían interrumpirse debido al problema o expiración del token de autenticación (generado en el código) de red. El cliente del servicio debe controlar estas circunstancias y volver a establecer la conexión y vínculos si es necesario. Para el caso de expiración del token de autenticación, el cliente también puede renovar el token antes de su expiración para evitar una disminución de la conexión.
* En algunos casos, el cliente debe ser capaz de controlar correctamente las redirecciones de enlace. Consulte la documentación del cliente AMQP sobre cómo controlar esta operación.

### <a name="receive-cloud-to-device-messages-device-and-module-client"></a>Recibir mensajes de nube a dispositivo (cliente de dispositivos y módulos)
Vínculos AMQP que se usan en el lado del dispositivo son los siguientes:

| Creado por | Tipo de vínculo | Ruta de acceso del vínculo | DESCRIPCIÓN |
|------------|-----------|-----------|-------------|
| Dispositivos | Vínculo del receptor | `/devices/<deviceID>/messages/devicebound` | Se reciben mensajes C2D destinados a dispositivos en este vínculo por cada dispositivo de destino. |
| Dispositivos | Vínculo del remitente | `/messages/serviceBound/feedback` | Comentarios de mensajes C2D enviados al servicio a través de este vínculo por los dispositivos. |
| Módulos | Vínculo del receptor | `/devices/<deviceID>/modules/<moduleID>/messages/devicebound` | Cada módulo de destino reciben mensajes C2D destinados a los módulos en este vínculo. |
| Módulos | Vínculo del remitente | `/messages/serviceBound/feedback` | Comentarios de mensajes C2D enviados al servicio a través de este vínculo por los módulos. |


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el protocolo AMQP, consulte el [especificación de AMQP v1.0](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Para obtener más información acerca de la mensajería de IoT Hub, consulte:

* [Mensajes de nube a dispositivo](./iot-hub-devguide-messages-c2d.md)
* [Compatibilidad con protocolos adicionales](iot-hub-protocol-gateway.md)
* [Compatibilidad con el protocolo MQTT](./iot-hub-mqtt-support.md)
