---
title: Creación de Azure Event Hubs habilitado para Apache Kafka| Microsoft Docs
description: Creación de un espacio de nombres de Azure Event Hubs habilitado para Kafka con Azure Portal
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: b72f6b190aad436398629c67f271020ffd0a8da9
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822082"
---
# <a name="create-apache-kafka-enabled-event-hubs"></a>Creación centros de eventos habilitados para Apache Kafka

Azure Event Hubs es una plataforma como servicio (PaaS) de streaming de macrodatos que ingiere millones de eventos por segundo, y proporciona baja latencia y alto rendimiento para análisis y visualización en tiempo real.

Azure Event Hubs proporciona un punto de conexión de Kafka. Este punto de conexión permite que el espacio de nombres de Event Hubs entienda el protocolo de mensajes y las API de [Apache Kafka](https://kafka.apache.org/intro). Con esta funcionalidad, puede comunicarse con las instancias de Event Hubs como lo haría con temas de Kafka sin cambiar los clientes de protocolo ni ejecutar sus propios clústeres. Admite [Apache Kafka versión 1.0.](https://kafka.apache.org/10/documentation.html) y versiones posteriores.

En este artículo se describe cómo crear un espacio de nombres de Event Hubs y cómo obtener la cadena de conexión necesaria para conectar las aplicaciones de Kafka con instancias de Event Hubs habilitadas para Kafka.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs habilitado para Kafka

1. Inicie sesión en [Azure Portal][Azure portal] y haga clic en **Crear un recurso** en la parte superior izquierda de la pantalla.

2. Busque Event Hubs y seleccione las opciones que se muestran aquí:
    
    ![Búsqueda de Event Hubs en el portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Proporcione un nombre único y habilite a Kafka en el espacio de nombres. Haga clic en **Create**(Crear).
    
    ![Creación de un espacio de nombres](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.jpg)
 
4. Una vez que se crea el espacio de nombres, en la pestaña **Configuración**, haga clic en **Directivas de acceso compartido** para obtener la cadena de conexión.

    ![Haga clic en Directivas de acceso compartido](./media/event-hubs-create/create-event-hub7.png)

5. Puede elegir la directiva **RootManageSharedAccessKey** predeterminada o agregar una nueva directiva. Haga clic en el nombre de la directiva y copie la cadena de conexión. 
    
    ![Selección de una directiva](./media/event-hubs-create/create-event-hub8.png)
 
6. Agregue esta cadena de conexión a la configuración de la aplicación de Kafka.

Ahora puede transmitir eventos desde las aplicaciones que usan el protocolo de Kafka en Event Hubs.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más acerca de Event Hubs, consulte estos vínculos:

* [Stream into Event Hubs from your Kafka applications](event-hubs-quickstart-kafka-enabled-event-hubs.md) (Transmitir en Event Hubs desde sus aplicaciones de Kafka)
* [Información sobre Event Hubs para Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Más información sobre Events Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
