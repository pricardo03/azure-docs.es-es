---
title: Orígenes de eventos de Azure Event Grid
description: Describe los orígenes de eventos compatibles con Azure Event Grid
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: tomfitz
ms.openlocfilehash: adef9d2f2d859c62d3b3b3a542536698fa668f9a
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498307"
---
# <a name="event-sources-in-azure-event-grid"></a>Orígenes de eventos de Azure Event Grid

Un origen de evento es donde se produce el evento. Varios servicios de Azure se configuran automáticamente para enviar eventos. También puede crear aplicaciones personalizadas que envían eventos. Las aplicaciones personalizadas no necesitan estar hospedadas en Azure para usar Event Grid para la distribución de eventos.

Este artículo contiene vínculos a contenido para cada origen de eventos.

## <a name="azure-subscriptions"></a>Suscripciones de Azure

Suscríbase a eventos de suscripciones de Azure para responder a los cambios en los recursos de una suscripción de Azure.

|Título |DESCRIPCIÓN  |
|---------|---------|
| [Tutorial: Integración de Azure Automation con Event Grid y Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Cree una máquina virtual, que envía un evento. El evento desencadena un runbook de Automation que etiqueta la máquina virtual y genera un mensaje que se envía a un canal de Equipos de Microsoft. |
| [Suscripción a eventos mediante el portal](subscribe-through-portal.md) | Uso del portal para la suscripción a eventos desde una suscripción de Azure. |
| [CLI de Azure: suscripción a eventos desde una suscripción de Azure](./scripts/event-grid-cli-azure-subscription.md) |Script de ejemplo que crea una suscripción de Event Grid para una suscripción de Azure y envía eventos a un webhook. |
| [PowerShell: suscripción a eventos desde una suscripción de Azure](./scripts/event-grid-powershell-azure-subscription.md)| Script de ejemplo que crea una suscripción de Event Grid para una suscripción de Azure y envía eventos a un webhook. |
| [Esquema del evento](event-schema-subscriptions.md) | Muestra los campos en los eventos de suscripción de Azure. |

## <a name="container-registry"></a>Container Registry

Suscríbase a los eventos de Container Registry para responder a los cambios en las imágenes.

|Título |DESCRIPCIÓN  |
|---------|---------|
| [Guía de inicio rápido: Envío de eventos de Container Registry](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Explica cómo utilizar la CLI de Azure para enviar eventos de Container Registry. |
| [Esquema del evento](event-schema-container-registry.md) | Describe los campos de los eventos de Container Registry. |

## <a name="custom-topics"></a>Temas personalizados

Suscríbase a temas personalizados para responder a eventos de la aplicación.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Guía de inicio rápido: creación y enrutamiento de eventos personalizados con la CLI de Azure](custom-event-quickstart.md) | Muestra cómo utilizar la CLI de Azure para enviar eventos personalizados. |
| [Guía de inicio rápido: creación y enrutamiento de eventos personalizados con Azure PowerShell](custom-event-quickstart-powershell.md) | Muestra cómo utilizar Azure PowerShell para enviar eventos personalizados. |
| [Guía de inicio rápido: creación y enrutamiento de eventos personalizados con Azure Portal](custom-event-quickstart-portal.md) | Muestra cómo utilizar Azure Portal para enviar eventos personalizados. |
| [Guía de inicio rápido: enrutamiento de eventos personalizados a Azure Queue Storage](custom-event-to-queue-storage.md) | Describe cómo enviar eventos personalizados a una instancia de Queue Storage. |
| [Publicación en tema personalizado](post-to-custom-topic.md) | Se describe cómo publicar un evento en un tema personalizado. |
| [CLI de Azure: creación de un tema personalizado de Event Grid](./scripts/event-grid-cli-create-custom-topic.md)|Script de ejemplo que crea un tema personalizado. El script recupera el punto de conexión y una clave.|
| [CLI de Azure: suscripción a eventos de un tema personalizado](./scripts/event-grid-cli-subscribe-custom-topic.md)|Script de ejemplo que crea una suscripción a un tema personalizado. Envía eventos a un webhook.|
| [PowerShell: Creación de un tema personalizado de Event Grid](./scripts/event-grid-powershell-create-custom-topic.md)|Script de ejemplo que crea un tema personalizado. El script recupera el punto de conexión y una clave.|
| [PowerShell: suscripción a eventos de un tema personalizado](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Script de ejemplo que crea una suscripción a un tema personalizado. Envía eventos a un webhook.|
| [Plantilla de Resource Manager: tema personalizado y punto de conexión del webhook](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Plantilla de Resource Manager que crea un tema personalizado y una suscripción a ese tema personalizado. Envía eventos a un webhook. |
|
| [Plantilla de Resource Manager: tema personalizado y punto de conexión a Event Hubs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Plantilla de Resource Manager que crea una suscripción a un tema personalizado. Envía eventos a una instancia de Azure Event Hubs. |
| [Esquema del evento](event-schema.md) | Muestra los campos de eventos personalizados. |

## <a name="event-hubs"></a>Event Hubs

Suscríbase a eventos de Event Hubs para responder a eventos de archivos de captura. Event Hubs puede actuar como origen de eventos o como controlador de eventos. En los siguientes artículos se explica cómo utilizar Event Hubs como origen.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Tutorial: transmisión de macrodatos a un almacén de datos](event-grid-event-hubs-integration.md) | Cuando Event Hubs crea un archivo de captura, Event Grid envía un evento a una aplicación de función. La aplicación recupera el archivo de captura y migra los datos a un almacenamiento de datos. |
| [Esquema del evento](event-schema-event-hubs.md) | Muestra los campos de los eventos de Event Hubs. |

Para ejemplos de Event Hubs como controlador, consulte este artículo sobre el [controlador de Event Hubs](event-handlers.md#event-hubs).

## <a name="iot-hub"></a>IoT Hub

Suscríbase a eventos de IoT Hub para responder a eventos de dispositivo creado, eliminado, conectado y desconectado.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Envío de notificaciones por correo electrónico sobre eventos de Azure IoT Hub mediante Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Una aplicación lógica envía un correo electrónico de notificación cada vez que se agrega un dispositivo a IoT Hub. |
| [Reacción a eventos de IoT Hub usando Event Grid para desencadenar acciones](../iot-hub/iot-hub-event-grid.md) | Información general sobre la integración de IoT Hub con Event Grid. |
| [Esquema del evento](event-schema-iot-hub.md) | Muestra los campos de los eventos de IoT Hub. |
| [Ordenación de los eventos de dispositivo conectado y dispositivo desconectado](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Muestra cómo ordenar los eventos de estado de conexión del dispositivo. |

## <a name="media-services"></a>Media Services

Suscríbase a eventos de Media Services para responder a eventos de estado de trabajo.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Introducción: reacción ante eventos de Media Services](../media-services/latest/reacting-to-media-services-events.md) | Información general de la integración de Media Services con Event Grid. |
| [Tutorial: enrutamiento de eventos de Azure Media Services a un punto de conexión web personalizado mediante la CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo enviar eventos desde Media Services. |
| [Esquema del evento](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra los campos de los eventos de Media Services. |

## <a name="resource-groups"></a>Grupos de recursos

Suscríbase a eventos de grupos de recursos para responder a los cambios de los recursos de un grupo de recursos.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Tutorial: supervisión de los cambios en máquinas virtuales con Azure Event Grid y Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) | Una aplicación lógica supervisa los cambios realizados en una máquina virtual y envía mensajes de correo electrónico sobre dichos cambios. |
| [CLI de Azure: suscripción a eventos para un grupo de recursos](./scripts/event-grid-cli-resource-group.md)| Script de ejemplo que se suscribe a eventos para un grupo de recursos. Envía eventos a un webhook. |
| [CLI de Azure: suscripción a eventos de un grupo de recursos y filtrado de los eventos de un recurso](./scripts/event-grid-cli-resource-group-filter.md) | Script de ejemplo que se suscribe a eventos para un grupo de recursos y filtra los eventos para un recurso. |
| [PowerShell: suscripción a eventos para un grupo de recursos](./scripts/event-grid-powershell-resource-group.md) | Script de ejemplo que se suscribe a eventos para un grupo de recursos. Envía eventos a un webhook. |
| [PowerShell: suscripción a eventos para un grupo de recursos y filtrado de los eventos para un recurso](./scripts/event-grid-powershell-resource-group-filter.md) | Script de ejemplo que se suscribe a eventos para un grupo de recursos y filtra los eventos para un recurso. |
| [Plantilla de Resource Manager: suscripción de recursos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Se suscribe a eventos para una suscripción o grupo de recursos de Azure. Envía eventos a un webhook. |
| [Esquema del evento](event-schema-resource-groups.md) | Muestra los campos de eventos de grupos de recursos. |

## <a name="service-bus"></a>Azure Service Bus

Suscríbase a eventos de Service Bus para responder a mensajes sin un agente de escucha activo.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Tutorial: ejemplos de integración de Azure Service Bus en Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid envía mensajes de temas de Service Bus a la aplicación de función y a la aplicación lógica. |
| [Introducción: integración de Azure Service Bus en Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Información general de la integración de Service Bus con Event Grid. |
| [Esquema del evento](event-schema-service-bus.md) | Muestra los campos de los eventos de Service Bus. |

## <a name="storage"></a>Storage

Suscríbase a eventos de Blob Storage para responder a eventos creados y eliminados del blob.

|Título  |DESCRIPCIÓN  |
|---------|---------|
| [Guía de inicio rápido: enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con la CLI de Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar la CLI de Azure para enviar eventos de Blob Storage a un webhook. |
| [Guía de inicio rápido: enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar Azure PowerShell para enviar eventos de Blob Storage a un webhook. |
| [Guía de inicio rápido: creación y enrutamiento de eventos de Blob Storage con Azure Portal](blob-event-quickstart-portal.md) | Muestra cómo utilizar el portal para enviar eventos de Blob Storage a un webhook. |
| [CLI de Azure: suscripción a eventos de una cuenta de Blob Storage](./scripts/event-grid-cli-blob.md) | Script de ejemplo que se suscribe a un evento para una cuenta de Blob Storage. Envía el evento a un webhook. |
| [PowerShell: suscripción a eventos de una cuenta de Blob Storage](./scripts/event-grid-powershell-blob.md) | Script de ejemplo que se suscribe a un evento para una cuenta de Blob Storage. Envía el evento a un webhook. |
| [Plantilla de Resource Manager: creación de una cuenta de Blob Storage y una suscripción](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) | Implementa una cuenta de Azure Blob Storage y crea una suscripción a eventos de dicha cuenta. Envía eventos a un webhook. |
| [Introducción: reacción ante eventos de Blob Storage](../storage/blobs/storage-blob-event-overview.md) | Información general de la integración de Blob Storage con Event Grid. |
| [Esquema del evento](event-schema-blob-storage.md) | Muestra los campos de los eventos de Blob Storage. |

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
