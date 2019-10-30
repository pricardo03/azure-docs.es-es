---
title: Importancia de las notificaciones de envío en las aplicaciones móviles con Visual Studio App Center y Azure Notification Hubs
description: Obtenga información sobre los servicios, como App Center, que le permite interactuar con los usuarios de la aplicación móvil.
author: elamalani
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 05a9bc6ccd26aaecd3e0bf615880e0543b8604f7
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795216"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>Interacción con los usuarios de la aplicación enviando notificaciones push 

Tanto si va a compilar aplicaciones empresariales como de consumidor, quiere que los usuarios usen activamente su aplicación. A menudo, hay ocasiones en las que quiere compartir noticias de última hora, actualizaciones de juegos, ofertas emocionantes, actualizaciones de productos o cualquier información relevante con los usuarios. Para aumentar la interacción con los usuarios y que sigan volviendo a su aplicación, necesita una manera de comunicarse con ellos en tiempo real.

Las notificaciones push ofrecen una manera rápida y eficaz de comunicarse con los usuarios de la aplicación. Puede ponerse en contacto con los usuarios en el momento adecuado y notificarles la información que quiera, normalmente en un cuadro de diálogo o emergente en un dispositivo móvil, independientemente de lo que hagan.

## <a name="value-of-push-notifications"></a>Valor de las notificaciones push
Las notificaciones push proporcionan valor a los usuarios finales y a la empresa.

Las empresas pueden:
- **Comunicarse directamente con sus usuarios** enviándoles mensajes en la plataforma compatible en el momento adecuado.
- **Aumentar la involucración del usuario** mediante el envío de actualizaciones y recordatorios en tiempo real a los usuarios, animándoles a interactuar con la aplicación de forma periódica.
- **Conservar usuarios** y volver a interactuar con usuarios inactivos que hayan descargado la aplicación pero no la usen para volver a activarlos.
- **Aumentar las tasas de conversión** mediante el análisis del comportamiento del usuario final, la segmentación de los usuarios finales y el aprovechamiento de campañas basadas en notificaciones de inserción en el dispositivo móvil.
- **Promover productos y servicios** mediante el envío de mensajes de inserción y actualizaciones oportunas a los usuarios.
- **Dirigirse a los usuarios** mediante el envío de mensajes de inserción personalizados.

Para los usuarios de la aplicación, las notificaciones push:
- **Ofrecen valor e información** en tiempo real.
- **Recuerdan a los usuarios** que usen la aplicación.

Use los servicios siguientes para habilitar notificaciones push en sus aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
El servicio [App Center Push](/appcenter/push/) le permite interactuar con los usuarios mediante el envío de mensajes dirigidos a los usuarios de iOS, Android y Windows sin tener que administrar el proceso de envío de notificaciones a los dispositivos con Servicios de notificaciones push (PNS). Basado en Azure Notification Hubs, este servicio elimina las complejidades asociadas a la inserción de notificaciones de forma manual, ya que proporciona un panel eficaz.

**Principales características**
- **Envíe notificaciones push a dispositivos móviles** a través de diversas plataformas.
- Use notificaciones para enviar datos a una aplicación, mostrar un mensaje al usuario o desencadenar una acción a través de la aplicación.
- Destinos de las notificaciones: 
    - Envíe mensajes de difusión a **todos los dispositivos registrados**.
    - Envíe notificaciones a **audiencias** creadas en función de la información del dispositivo y de las propiedades personalizadas.
    - Envíe notificaciones push a **usuarios específicos**.
    - Envíe notificaciones push a **dispositivos específicos**.
- **Telemetría enriquecida** al enviar cambios, el dispositivo y el error están disponibles en el portal de App Center.
- **Compatibilidad con plataformas**: iOS, Android, macOS, Xamarin, React Native, Unity y Cordova.

**Referencias**
- [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Push](/appcenter/push/)

## <a name="azure-notification-hubs"></a>Azure Notification Hubs
[Notification Hubs](/azure/notification-hubs/notification-hubs-push-notification-overview) proporciona un motor de inserción de escalabilidad horizontal y fácil de usar que le permite enviar notificaciones a cualquier plataforma (iOS, Android, Windows, Kindle, Baidu, etc.) desde cualquier back-end (en la nube o local).

**Principales características**
- **Envíe notificaciones push** a cualquier plataforma desde cualquier back-end, en la nube o local.
- **Rápida difusión** de notificaciones push a millones de dispositivos móviles con una única llamada de API.
- Adapte las notificaciones push en función del cliente, el idioma y la ubicación.
- Defina y notifique dinámicamente los **segmentos de clientes**.
- **Escale al momento** a millones de dispositivos móviles.
- **Compatibilidad con plataformas**: iOS, Android, Windows, Kindle y Baidu.
        
**Referencias**
- [Azure Portal](https://portal.azure.com) 
- [Introducción a Azure Notification Hubs](/azure/notification-hubs/)   
- [Guías de inicio rápido](/azure/notification-hubs/create-notification-hub-portal)
- [Muestras](/azure/notification-hubs/samples)
