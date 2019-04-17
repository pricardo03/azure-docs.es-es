---
title: Uso de temas de Service Bus de Azure con Python| Microsoft Docs
description: Aprenda a usar los temas de Azure Service Bus y las suscripciones de Python.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: 102fe85916194648501be3d2cb39d8bcda9e9f5c
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59607082"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Uso de temas y suscripciones de Service Bus con Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

En este artículo se describe cómo usar los temas y las suscripciones de Service Bus. Los ejemplos están escritos en Python y usan el [paquete del SDK de Azure para Python][Azure Python package]. Los escenarios descritos incluyen:

- Creación de temas y suscripciones 
- Creación de filtros de suscripción 
- Envío de mensajes a un tema 
- Recepción de mensajes de una suscripción
- Eliminación de temas y suscripciones

## <a name="prerequisites"></a>Requisitos previos
1. Una suscripción de Azure. Para completar este tutorial, deberá tener una cuenta de Azure. Puede activar su [las ventajas de suscriptor de Visual Studio o MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) o registrarse para obtener un [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Siga los pasos de la [inicio rápido: Usar el portal de Azure para crear un tema de Service Bus y las suscripciones al tema](service-bus-quickstart-topics-subscriptions-portal.md) para crear un Bus de servicio **espacio de nombres** y obtenga el **cadena de conexión**.

    > [!NOTE]
    > Creará un **tema** y un **suscripción** al tema mediante el uso de **Python** en este inicio rápido. 
3. Instalar [paquete Python Azure][Azure Python package]. Consulte la [Guía de instalación de Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>de un tema

El objeto **ServiceBusService** le permite trabajar con temas. Agregue el siguiente código cerca de la parte superior de todo archivo Python en el que desee obtener acceso a Service Bus mediante programación:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

El siguiente código crea un objeto **ServiceBusService**. Reemplace `mynamespace`, `sharedaccesskeyname` y `sharedaccesskey` por el espacio de nombres real, el valor y el nombre de clave de la firma de acceso compartido (SAS).

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Puede obtener los valores para el valor y el nombre de clave SAS en [Azure Portal][Azure portal].

```python
bus_service.create_topic('mytopic')
```

El método `create_topic` también admite opciones adicionales, lo que permite invalidar la configuración predeterminada de los temas, como el período de vida de los mensajes o el tamaño máximo de los temas. En el siguiente ejemplo, se establece el tamaño máximo de los temas en 5 GB y el valor del período de vida (TTL) en un minuto:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Creación de suscripciones

Las suscripciones a los temas también se crean con el objeto **ServiceBusService**. A las suscripciones se les asigna un nombre y pueden tener un filtro opcional que restrinja el conjunto de mensajes que se entregan a su cola virtual.

> [!NOTE]
> Las suscripciones son permanentes y seguirán existiendo hasta que se eliminen, o se elimine el tema al que están asociadas.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Creación de una suscripción con el filtro predeterminado (MatchAll)

Se usa el filtro predeterminado **MatchAll** en caso de que no se haya especificado ninguno al crear una nueva suscripción. Al usar el filtro **MatchAll**, todos los mensajes publicados en el tema se colocan en la cola virtual de la suscripción. En el ejemplo siguiente se crea una suscripción llamada `AllMessages` que usa el filtro predeterminado **MatchAll**.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Creación de suscripciones con filtros

También se pueden definir filtros que permitan especificar qué mensajes enviados a un tema deben mostrarse en una suscripción a un tema concreto.

El tipo de filtro más flexible compatible con las suscripciones es **SqlFilter**, que implementa un subconjunto de SQL92. Los filtros de SQL operan en las propiedades de los mensajes que se publican en el tema. Para más información sobre las expresiones que se pueden usar con un filtro de SQL, vea la sintaxis de [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Se pueden agregar filtros a una suscripción, y para ello es preciso usar el método **create\_rule** del objeto **ServiceBusService**. Este método permite agregar nuevos filtros a una suscripción existente.

> [!NOTE]
> Debido a que el filtro predeterminado se aplica automáticamente a todas las suscripciones nuevas, debe eliminar primero el filtro predeterminado, si no quiere que **MatchAll** invalide los demás filtros que especifique. Puede eliminar la regla predeterminada utilizando el método `delete_rule` del objeto **ServiceBusService**.
> 
> 

En el ejemplo siguiente, se crea una suscripción denominada `HighMessages` con un objeto **SqlFilter** que solo selecciona los mensajes con una propiedad `messagenumber` personalizada cuyo valor es mayor que 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Del mismo modo, en el ejemplo que aparece a continuación, se crea una suscripción denominada `LowMessages` con un filtro **SqlFilter** que solo selecciona los mensajes cuyo valor de la propiedad `messagenumber` es menor o igual que 3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Cuando se envía un mensaje a `mytopic`, este se entrega siempre a los receptores suscritos al tema **AllMessages**, y se entrega de forma selectiva a los receptores suscritos a los temas **HighMessages** y **LowMessages** (según el contenido del mensaje).

## <a name="send-messages-to-a-topic"></a>Envío de mensajes a un tema

Para enviar un mensaje a un tema de Service Bus, la aplicación debe utilizar el método `send_topic_message` del objeto **ServiceBusService**.

En el ejemplo siguiente se demuestra cómo enviar cinco mensajes de prueba a `mytopic`. El valor de la propiedad `messagenumber` de cada mensaje varía en función de la iteración del bucle (lo que determina qué suscripciones lo reciben):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

El tamaño máximo de mensaje que admiten los temas de Service Bus es de 256 KB en el [nivel Estándar](service-bus-premium-messaging.md) y de 1 MB en el [nivel Premium](service-bus-premium-messaging.md). El encabezado, que incluye propiedades de la aplicación estándar y personalizadas, puede tener un tamaño máximo de 64 KB. No hay límite para el número de mensajes que contiene un tema, pero hay un tope para el tamaño total de los mensajes contenidos en un tema. El tamaño de los temas se define en el momento de la creación (el límite máximo es de 5 GB). Para obtener más información sobre las cuotas, vea [Cuotas de Service Bus][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Recepción de mensajes de una suscripción

Los mensajes se reciben de una suscripción utilizando el método `receive_subscription_message` del objeto **ServiceBusService**:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Los mensajes se eliminan de la suscripción a medida que se leen cuando el parámetro `peek_lock` está establecido en **False**. Puede leer y bloquear los mensajes sin eliminarlos de la cola si establece el parámetro opcional `peek_lock` en **True**.

El comportamiento de lectura y eliminación del mensaje como parte de la operación de recepción es el modelo más sencillo y el que mejor funciona en aquellas situaciones en las que una aplicación puede tolerar que no se procese un mensaje en caso de error. Para entender este comportamiento, pongamos una situación en la que un consumidor emite la solicitud de recepción que se bloquea antes de procesarla. Como Service Bus ha marcado el mensaje como consumido, cuando la aplicación se reinicie y empiece a consumir mensajes de nuevo, habrá perdido el mensaje que se consumió antes del bloqueo.

Si el parámetro `peek_lock` está establecido en **True**, el proceso de recepción se convierte en una operación en dos fases que permite admitir aplicaciones que no toleran la pérdida de mensajes. Cuando Service Bus recibe una solicitud, busca el siguiente mensaje que se va a consumir, lo bloquea para impedir que otros consumidores lo reciban y, a continuación, lo devuelve a la aplicación. Una vez que la aplicación termina de procesar el mensaje (o lo almacena de forma confiable para su futuro procesamiento), completa la segunda fase del proceso de recepción llamando al método `delete` del objeto **Message**. El método `delete` marca el mensaje como consumido y lo elimina de la suscripción.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
print(msg.body)
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Actuación ante errores de la aplicación y mensajes que no se pueden leer

Service Bus proporciona una funcionalidad que le ayuda a superar sin problemas los errores de la aplicación o las dificultades para procesar un mensaje. Si por cualquier motivo una aplicación de recepción es incapaz de procesar el mensaje, entonces puede llamar al método `unlock` del objeto **Message**. Este método hace que Service Bus desbloquee el mensaje de la suscripción y lo ponga a disposición para que pueda volver a recibirse, ya sea por la misma aplicación que lo consume o por otra.

También hay otro tiempo de expiración asociado a un mensaje bloqueado en la suscripción y, si la aplicación no puede procesar el mensaje antes de que expire el tiempo de espera del bloqueo (por ejemplo, si la aplicación se bloquea), Service Bus desbloquea el mensaje automáticamente y hace que esté disponible para que pueda volver a recibirse.

En caso de que la aplicación sufra un error después de procesar el mensaje y antes de llamar al método `delete`, entonces el mensaje se volverá a entregar a la aplicación cuando esta se reinicie. Este comportamiento se suele denominar Procesamiento de al menos una vez\*; es decir, cada mensaje se procesará al menos una vez, pero en ciertas situaciones a entregarse el mismo mensaje. Si el escenario no puede tolerar el procesamiento duplicado, entonces los desarrolladores de la aplicación deberían agregar lógica adicional a su aplicación para solucionar la entrega de mensajes duplicados. Esto se hace con la propiedad **MessageId** del mensaje, que permanece constante en todos los intentos de entrega.

## <a name="delete-topics-and-subscriptions"></a>Eliminación de temas y suscripciones

Los temas y las suscripciones son permanentes, por lo que deben eliminarse explícitamente a través del [Azure Portal][Azure portal] o mediante programación. En el ejemplo siguiente se muestra cómo eliminar el tema denominado `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Al eliminar un tema también se eliminan todas las suscripciones que estén registradas con él. También se pueden eliminar las suscripciones de forma independiente. El código siguiente muestra cómo eliminar una suscripción denominada `HighMessages` del tema `mytopic`:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce los fundamentos de los temas de Service Bus, siga estos vínculos para obtener más información.

* Vea [Colas, temas y suscripciones][Queues, topics, and subscriptions].
* Referencia para [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
