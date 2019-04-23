---
title: Azure Notification Hubs y la migración de Google Firebase Cloud Messaging (FCM)
description: Describe cómo Azure Notification Hubs aborda el GCM, Google para la migración de FCM.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/10/2019
ms.author: jowargo
ms.openlocfilehash: 4cbfc67bc66e84b4743f3326db40872241e5d474
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787022"
---
# <a name="azure-notification-hubs-and-the-google-firebase-cloud-messaging-fcm-migration"></a>Azure Notification Hubs y la migración de Google Firebase Cloud Messaging (FCM)

## <a name="current-state"></a>Estado actual

Cuando Google anunció su migración de Google Cloud Messaging (GCM) para Firebase Cloud Messaging (FCM), servicios de inserción, como las nuestras tenían que ajustar cómo se envían notificaciones a dispositivos Android para acomodar el cambio.

Hemos actualizado nuestro servicio back-end y luego publica actualizaciones a nuestras API y SDK según sea necesario. Con nuestra implementación, hemos tomado la decisión de mantener la compatibilidad con los esquemas existentes de la notificación de GCM para minimizar el impacto en el cliente. Esto significa que actualmente nosotros enviamos notificaciones a dispositivos Android mediante FCM en modo heredado de FCM. En última instancia, deseamos agregar compatibilidad con true FCM, incluidas las nuevas características y el formato de carga. Es un cambio a largo plazo y la migración actual se centra en mantener la compatibilidad con las aplicaciones existentes y los SDK. Puede usar el GCM o FCM SDK en la aplicación (junto con nuestro SDK) y nos aseguramos de que la notificación se envía correctamente.

Algunos clientes recientemente reciben un mensaje de advertencia de Google acerca de las aplicaciones con un punto de conexión GCM para las notificaciones. Esto es simplemente una advertencia y nada se interrumpe: notificaciones de Android de la aplicación todavía se envían a Google para el procesamiento y Google todavía los procesa. Algunos clientes que especifican el punto de conexión GCM explícitamente en la configuración de servicio todavía estaban utilizando el punto de conexión en desuso. Se hayan identificado ya esta brecha y estaba trabajando en solucionar el problema cuando Google envió el correo electrónico.

Reemplazamos ese punto de conexión en desuso y se implementa la solución.

## <a name="going-forward"></a>A partir de ahora

P+F de FCM de Google dice que no tiene que hacer nada. En el [FCM preguntas más frecuentes sobre](https://developers.google.com/cloud-messaging/faq), Google dice "SDK de cliente y los tokens GCM seguirán funcionando indefinidamente. Sin embargo, no podrá tener como destino la versión más reciente de Google Play Services en su aplicación Android a menos que migre a FCM."

Si su aplicación usa la biblioteca GCM, continúe y siga las instrucciones de Google para actualizar a la biblioteca FCM de la aplicación. Nuestro SDK es compatible con cualquiera, por lo que no tendrá que actualizar nada en la aplicación en nuestro bando (siempre y cuando esté al día con nuestra versión SDK).

## <a name="questions-and-answers"></a>Preguntas y respuestas

Le mostramos algunas respuestas a preguntas habituales a los clientes nos han comentado:

**P:** ¿Qué es necesario hacer para ser compatible por la fecha límite (Google actual de la fecha límite es 29 de mayo y se puede cambiar)?

**R:** Nada. Se mantendrá la compatibilidad con el esquema de notificación de GCM existente. La clave GCM seguirán funcionando con normalidad, así como cualquier SDK de GCM y las bibliotecas usadas por la aplicación.

Cuando se decide actualizar a las bibliotecas y SDK de FCM para aprovechar las nuevas características, la clave GCM seguirá funcionando. Puede cambiar al uso de una clave FCM si desea, pero asegúrese de que va a agregar Firebase a su proyecto GCM existente al crear el nuevo proyecto de Firebase. Esta forma garantiza la compatibilidad con versiones anteriores con los clientes que ejecutan versiones anteriores de la aplicación que aún utilizan bibliotecas y SDK de GCM.

Si está creando un nuevo proyecto FCM y no se adjunta al proyecto GCM existente, una vez que actualice Notification Hubs con el nuevo secreto FCM perderá la capacidad para enviar notificaciones push a sus instalaciones de aplicación actual, dado que la nueva clave FCM no tiene ningún vínculo a la antiguo GCM proyecto.

**P:** ¿Por qué recibo este correo electrónico sobre los extremos GCM anteriores que se va a usar? ¿Qué tengo que hacer?

**R:** Nada. Se han migrado a los nuevos extremos y habrá terminados pronto, por lo que es necesario ningún cambio. Nada se interrumpe, nuestro punto de conexión se perdió un causado simplemente los mensajes de advertencia de Google.

**P:** ¿Cómo puedo cambiar al nuevo SDK de FCM y bibliotecas sin interrumpir a los usuarios existentes?

R: Actualizar en cualquier momento. Google no ha anunciado aún ninguna degradación de bibliotecas y SDK existente de GCM. Para asegurarse de no romper las notificaciones de inserción a los usuarios existentes, asegúrese de que cuando cree el nuevo proyecto de Firebase que se va a asociar con el proyecto existente de GCM. Así asegurará de Firebase nuevos secretos funcionará para los usuarios que ejecutan las versiones anteriores de la aplicación con los SDK de GCM y bibliotecas, así como los nuevos usuarios de su aplicación con el SDK de FCM y bibliotecas.

**P:** ¿Cómo se utiliza las nuevas características FCM y esquemas para mis notificaciones?

**R:** Una vez que se publica una actualización a nuestras API y SDK, esté atento: se espera que tiene algo para usted en los próximos meses.
