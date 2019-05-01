---
title: Aprovisionamiento y administración de la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Aprenda a aprovisionar y administrar la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: a73be313daa3b45cabc2adb07bb3d2592c7d307e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725463"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Aprovisionamiento y administración de la versión preliminar de Azure Time Series Insights

En este artículo se describe cómo crear y administrar un entorno en la versión preliminar de Azure Time Series Insights con [Azure Portal](https://portal.azure.com/).

## <a name="overview"></a>Información general

Los entornos de versión preliminar de Azure Time Series Insights son entornos de pago por uso (PAYG).

Al aprovisionar un entorno de versión preliminar de Azure Time Series Insights, creará dos recursos de Azure:

* Un entorno de la versión preliminar de Azure Time Series Insights.  
* Una cuenta de uso general v1 de Azure Storage.
  
Aprenda [a planificar su entorno ](./time-series-insights-update-plan.md).

>[!IMPORTANT]
> Para la versión preliminar, asegúrese de que está usando un almacenamiento de Azure de uso general v1 (GPv1) cuenta.  Soporte técnico para uso general v2 y versiones posteriores se agregarán en un futuro próximo.  

Opcionalmente, puede asociar cada entorno de la versión preliminar de Azure Time Series Insights con un origen de evento. Para más información, consulte [Agregar un origen de centro de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md) y [Agregar un origen de centro de IoT](./time-series-insights-how-to-add-an-event-source-iothub.md). Deberá proporcionar una propiedad de id. de marca de tiempo y un grupo de consumidores único durante este paso. Al hacerlo, garantiza que el entorno obtenga acceso a los eventos apropiados.

Una vez completado el aprovisionamiento, puede modificar sus directivas de acceso y otros atributos del entorno para adaptarse a sus requisitos comerciales.

## <a name="create-the-environment"></a>Creación del entorno

Los siguientes pasos describen cómo crear un entorno de la versión preliminar de Azure Time Series Insights:

1. Seleccione el botón **PAYG** en el menú **SKU**. Proporcione un nombre de entorno y elija el grupo de suscripción y el grupo de recursos que va a usar. A continuación, seleccione una ubicación admitida para el entorno en el que se hospedará.

   ![Cree una instancia de Azure Time Series Insights.][1]

1. Introduzca un id. de serie temporal.

    >[!NOTE]
    > * El id. de serie temporal es inmutable y distingue mayúsculas de minúsculas. (Una vez establecido, no se puede cambiar).
    > * Los id. de serie temporal pueden tener hasta tres claves.
    > * Para obtener más información sobre cómo seleccionar un id. de serie temporal, consulte [Elegir un id. de serie temporal](./time-series-insights-update-how-to-id.md).

1. Cree una cuenta de Azure Storage seleccionando un nombre de cuenta de almacenamiento y designando una opción de replicación. Al hacerlo, se crea automáticamente una cuenta de uso general v1 de Azure Storage. Se creará en la misma región que el entorno de versión preliminar de Azure Time Series Insights que seleccionó anteriormente.

    ![Creación de una cuenta de Azure Storage para la instancia][5]

1. Opcionalmente, puede agregar un origen del evento.

   * Time Series Insights admite [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) y [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) como opciones. Si bien solo puede agregar un único origen del evento en el momento de la creación del entorno, puede agregar otro origen del evento más adelante. Es mejor crear un grupo de consumidores único para asegurarse de que todos los eventos sean visibles en la instancia de la versión preliminar Azure Time Series Insights. Puede seleccionar un grupo de consumidores existente o crear un nuevo grupo de consumidores al agregar el origen del evento.

   * También debe elegir la propiedad Timestamp apropiada. De forma predeterminada, Azure Time Series Insights usa el tiempo de puesta en cola del mensaje para cada origen del evento.

     > [!TIP]
     > Es posible que el tiempo de puesta en cola del mensaje no sea la mejor opción de configuración para usar en escenarios de carga de datos históricos o eventos por lotes. Asegúrese de comprobar correctamente la decisión de usar o no usar una propiedad Timestamp en tales casos.

     ![Pestaña de origen de eventos][2]

1. Confirme que el entorno se ha aprovisionado con las opciones que quiere.

    ![Pestaña Revisar y crear][3]

## <a name="manage-the-environment"></a>Administrar el entorno

Puede administrar su entorno de versión preliminar de Azure Time Series Insights con Azure Portal. Estas son las principales diferencias al administrar un entorno PAYG de la versión preliminar de Azure Time Series Insights en lugar de un entorno S1 o S2 con Azure Portal:

* La hoja **Introducción** de Azure Portal permanece sin cambios en Azure Time Series Insights, excepto en los siguientes casos:
  * La Capacidad se elimina ya que este concepto no es relevante para entornos PAYG.
  * Se ha agregado la propiedad de id. de serie temporal. Determina cómo se particionan los datos.
  * Se eliminan los conjuntos de datos de referencia.
  * La dirección URL mostrada le dirige al [Explorador de versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).
  * Su nombre de la cuenta de Azure Storage se agrega a la lista.

* La hoja **Configurar** de Azure Portal se eliminó en la versión preliminar de Azure Time Series Insights, porque los entornos PAYG no son configurables.

* La hoja de datos **Referencia** de Azure Portal se ha eliminado de la versión preliminar de Azure Time Series Insights, porque los datos de referencia no son un componente de los entornos PAYG.

![Entorno de la versión preliminar de Time Series Insights en Azure Portal][4]

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Planeamiento del entorno](./time-series-insights-update-plan.md).

- Obtenga información sobre cómo [agregar un origen de centro de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Configurar [un origen de IoT hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.PNG
[2]: media/v2-update-manage/manage_two.PNG
[3]: media/v2-update-manage/manage_three.PNG
[4]: media/v2-update-manage/manage_four.PNG
[5]: media/v2-update-manage/manage_five.PNG
