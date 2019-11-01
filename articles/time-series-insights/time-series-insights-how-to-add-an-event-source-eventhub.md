---
title: Adición de un origen de eventos de Event Hubs a Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo agregar un origen del evento que esté conectado a Azure Event Hubs a su entorno de Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/09/2019
ms.custom: seodec18
ms.openlocfilehash: 3adf8b71d264b01f13f5aac7002b7ec455a31d60
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990082"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Incorporación de un origen del evento de centro de eventos al entorno de Time Series Insights

Este artículo describe cómo usar Azure Portal para agregar un origen de eventos que lee datos de Azure Event Hubs en su entorno de Azure Time Series Insights.

> [!NOTE]
> Los pasos que se describen en este artículo son válidos tanto en entornos de Time Series Insights con disponibilidad general como en entornos en versión preliminar.

## <a name="prerequisites"></a>Requisitos previos

- Cree un entorno de Time Series Insights según lo explicado en [Creación de un entorno de Azure Time Series Insights: Creación de un entorno de Azure Time Series Insights](./time-series-insights-update-create-environment.md).
- Cree un centro de eventos. Vea [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](../event-hubs/event-hubs-create.md).
- El centro de eventos tiene que tener eventos de mensajes activos que se le hayan enviado. Encontrará más información en [Envío de eventos a Azure Event Hubs mediante .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Cree un grupo de consumidores dedicado en el centro de eventos para que el entorno de Time Series Insights los consuma. Cada origen del evento de Time Series Insights tiene que tener su propio grupo de consumidores dedicado que no se comparte con ningún otro consumidor. Si varios lectores consumen eventos desde el mismo grupo de consumidores, es probable que todos los lectores vean errores. Hay un límite de 20 grupos de consumidores por centro de eventos. Si desea información más detallada, consulte la [Guía de programación de Event Hubs](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Incorporación de un grupo de consumidores al centro de eventos

Las aplicaciones usan grupos de consumidores para extraer datos de Azure Event Hubs. Para leer datos de manera confiable del centro de eventos, proporcione un grupo de consumidores dedicado, solo para su uso en este entorno de Time Series Insights.

Para agregar un nuevo grupo de consumidores al centro de eventos:

1. En [Azure Portal](https://portal.azure.com), busque y abra el centro de eventos desde el espacio de nombres del centro de eventos.

    [![Apertura del espacio de nombres del centro de eventos](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-hub-namespace.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-hub-namespace.png#lightbox)

1. En **Entidades** seleccione **Grupos de consumidores** y, a continuación **Grupo de consumidores**.

   [![Centro de eventos: Incorporación de un grupo de consumidores](media/time-series-insights-how-to-add-an-event-source-eventhub/2-event-hub-consumer-group.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/2-event-hub-consumer-group.png#lightbox)

1. En la página **Grupos de consumidores**, proporcione un nuevo valor exclusivo para **Nombre**.  Use este mismo nombre al crear un nuevo origen del evento en el entorno de Time Series Insights.

1. Seleccione **Crear**.

## <a name="add-a-new-event-source"></a>Adición de un nuevo origen del evento

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Busque su entorno de Time Series Insights existente. En el menú izquierdo, seleccione **Todos los recursos**y después el entorno de Time Series Insights.

1. En **Topología del entorno** seleccione **Orígenes de eventos**y, a continuación, seleccione **Agregar**.

   [![En Orígenes de eventos, seleccione el botón Agregar](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png#lightbox)

1. Especifique un valor para **Nombre de origen de evento** que sea exclusivo para este entorno de Time Series Insights, como **event-stream**.

1. En **Origen**, seleccione **Centro de eventos**.

1. Seleccione los valores apropiados para **Opción de importación**:

   * Cuando ya tenga un centro de eventos en una de sus suscripciones seleccione **Use Event Hub from available subscriptions** (Usar el centro de eventos de las suscripciones disponibles). Esta opción presenta el enfoque más sencillo.

     [![Selección de una opción de importación de origen de eventos](media/time-series-insights-how-to-add-an-event-source-eventhub/4-select-an-option.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/4-select-an-option.png#lightbox)

    *  En la siguiente tabla se explican las propiedades necesarias para la opción **Use Event Hub from available subscriptions** (Usar el centro de eventos de las suscripciones disponibles):

       [![Detalles del centro de eventos y la suscripción](media/time-series-insights-how-to-add-an-event-source-eventhub/5-create-button.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/5-create-button.png#lightbox)

       | Propiedad | DESCRIPCIÓN |
       | --- | --- |
       | Subscription | La suscripción a la que pertenece la instancia y el espacio de nombres del centro de eventos deseado. |
       | Espacio de nombres del Centro de eventos | El espacio de nombres del centro de eventos al que pertenece la instancia de centro de eventos deseada. |
       | Nombre del centro de eventos | Nombre de la instancia de centro de eventos deseada. |
       | Valor de la directiva del centro de eventos | Seleccione la directiva de acceso compartido deseada. Puede crear la directiva de acceso compartido en el centro de eventos en la pestaña **Configurar**. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. La directiva de acceso compartido para el origen de eventos *debe* tener permisos de **lectura**. |
       | Clave de la directiva del Centro de eventos | Se rellena previamente a partir del valor de la directiva del centro de eventos seleccionado. |

    * Si el centro de eventos es externo a sus suscripciones o si quiere elegir opciones avanzadas, seleccione **Indicar manualmente la configuración del Centro de eventos**.

       En la siguiente tabla se explican las propiedades necesarias para la opción **Indicar manualmente la configuración del Centro de eventos**:
 
       | Propiedad | DESCRIPCIÓN |
       | --- | --- |
       | Id. de suscripción | La suscripción a la que pertenece la instancia y el espacio de nombres del centro de eventos deseado. |
       | Resource group | El grupo de recursos al que pertenece la instancia y el espacio de nombres del centro de eventos deseado. |
       | Espacio de nombres del Centro de eventos | El espacio de nombres del centro de eventos al que pertenece la instancia de centro de eventos deseada. |
       | Nombre del centro de eventos | Nombre de la instancia de centro de eventos deseada. |
       | Valor de la directiva del centro de eventos | Seleccione la directiva de acceso compartido deseada. Puede crear la directiva de acceso compartido en el centro de eventos en la pestaña **Configurar**. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. La directiva de acceso compartido para el origen de eventos *debe* tener permisos de **lectura**. |
       | Clave de la directiva del Centro de eventos | La clave de acceso compartido que se usa para autenticar el acceso al espacio de nombres de Service Bus. Especifique la clave principal o secundaria aquí. |

    * Ambas opciones comparten las siguientes opciones de configuración:

       | Propiedad | DESCRIPCIÓN |
       | --- | --- |
       | Grupo de consumidores de Event Hubs | El grupo de consumidores que lee eventos del centro de eventos. Es muy recomendable usar un grupo de consumidores dedicado para el origen del evento. |
       | Formato de serialización de eventos | Actualmente, JSON es el único formato de serialización disponible. Los mensajes de eventos tienen que estar en este formato, de lo contrario no se podrá leer ningún dato. |
       | Nombre de la propiedad de marca de tiempo | Para determinar este valor, es necesario que comprenda el formato de mensaje de los datos del mensaje que se envía al centro de eventos. Este valor es el **nombre** de la propiedad específica del evento en los datos del mensaje que quiere usar como marca de tiempo del evento. El valor distingue mayúsculas de minúsculas. Si se deja en blanco, se usa la **hora de puesta en cola del evento** como marca de tiempo del evento en el origen del evento. |

1. Agregue el nombre del grupo de consumidores dedicado de Time Series Insights que agregó al centro de eventos.

1. Seleccione **Crear**.

   Una vez que se haya creado el origen del evento, Time Series Insights iniciará automáticamente la transmisión de datos al entorno.

## <a name="next-steps"></a>Pasos siguientes

* [Defina las directivas de acceso a datos](time-series-insights-data-access.md) para proteger los datos.

* [Envíe eventos](time-series-insights-send-events.md) al origen de eventos.

* Acceso al entorno en el [explorador de Time Series Insights](https://insights.timeseries.azure.com).
