---
title: 'Administración de la versión preliminar de Azure Time Series: cómo aprovisionar y administrar la versión preliminar de Azure Time Series. | Microsoft Docs'
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
ms.openlocfilehash: 1834a59ff675f109267a406e3c2b03411e3bb50b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273524"
---
# <a name="how-to-provision-and-manage-azure-time-series-insights-preview"></a>Cómo aprovisionar y administrar la versión preliminar de Azure Time Series Insights

En este documento se describe cómo crear y administrar un entorno en la versión preliminar de Azure Time Series Insights con [Azure Portal](https://portal.azure.com/).

## <a name="overview"></a>Información general

A continuación encontrará una breve descripción general de cómo aprovisionar un entorno de versión preliminar de Azure Time Series Insights:

* Los entornos de versión preliminar de Azure Time Series Insights son entornos **PAYG**.
  * Como parte del proceso de creación, deberá proporcionar un id. de serie temporal. Los id. de serie temporal pueden tener hasta tres claves. Para obtener más información sobre cómo seleccionar un id. de serie temporal consulte [How to choose a Time Series ID](./time-series-insights-update-how-to-id.md) (Cómo elegir un id. de serie temporal).
  * Al aprovisionar un entorno de versión preliminar de Azure Time Series Insights, creará dos recursos de Azure:

    * Un entorno de la versión preliminar de Azure Time Series Insights.  
    * Una cuenta de uso general V1 de Azure Storage.
  
    Aprenda [a planificar su entorno ](./time-series-insights-update-plan.md).

    >[!IMPORTANT]
    > Para los clientes que usan cuentas V2, no habilite las propiedades de archivado o esporádicas de la cuenta de almacenamiento que usará.

* Cada entorno de la versión preliminar de Azure Time Series Insights puede estar asociado (opcionalmente) con un origen de evento. Consulte [How to add an Event Hub source](./time-series-insights-how-to-add-an-event-source-eventhub.md) (Cómo agregar un origen de Event Hub) y [How to add an IoT Hub source](./time-series-insights-how-to-add-an-event-source-iothub.md) (Cómo agregar un origen de IoT Hub) para obtener más información.
  * Deberá proporcionar una propiedad de id. de marca de tiempo y un grupo de consumidores único durante este paso. Al hacerlo, garantiza que el entorno obtenga acceso a los eventos apropiados.

* Una vez completado el aprovisionamiento, puede modificar sus directivas de acceso y otros atributos del entorno para adaptarse a sus requisitos comerciales.

## <a name="new-environment-creation"></a>Crear un nuevo entorno

Los siguientes pasos describen cómo crear un entorno de la versión preliminar de Azure Time Series Insights:

1. Seleccione el botón **PAYG** en el menú **SKU**. Proporcione un nombre de entorno y designe qué grupo de suscripción y qué grupo de recursos va a usar. A continuación, seleccione una ubicación admitida para el entorno en el que se hospedará.

1. Agregar un id. de serie temporal

    >[!NOTE]
    > * El id. de serie temporal distingue entre mayúsculas y minúsculas y es inmutable (esto es, no se puede cambiar después de configurarse).
    > * Los id. de serie temporal pueden tener hasta tres claves.
    > * Para obtener más información sobre cómo seleccionar un id. de serie temporal, consulte [How to choose a Time Series ID](./time-series-insights-update-how-to-id.md) (Cómo elegir un id. de serie temporal).

1. Cree una cuenta de Azure Storage seleccionando un nombre de cuenta de Azure Storage y designando una opción de replicación. Al hacerlo, se crea automáticamente una cuenta de uso general V1 de Azure Storage. Se creará en la misma región que el entorno de versión preliminar de Azure Time Series Insights que seleccionó anteriormente.

    ![Cree una instancia de Azure Time Series Insights.][1]

1. Opcionalmente, puede agregar un origen del evento.

   * Time Series Insights admite [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) y [Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) como opciones. Si bien solo puede agregar un único origen del evento en el momento de la creación del entorno, puede agregar un origen del evento adicional más adelante. Es mejor crear un grupo de consumidores único para asegurarse de que todos los eventos sean visibles en la instancia de la versión preliminar Azure Time Series Insights. Puede seleccionar un grupo de consumidores existente o crear un nuevo grupo de consumidores al agregar el origen del evento.

   * También debe designar la propiedad Timestamp apropiada. De forma predeterminada, Azure Time Series Insights usa el tiempo de puesta en cola del mensaje para cada origen del evento.

     > [!TIP]
     > Es posible que el tiempo de puesta en cola del mensaje no sea la mejor opción de configuración para usar en escenarios de carga de datos históricos o eventos por lotes. Asegúrese de comprobar correctamente la decisión de usar o no usar una propiedad Timestamp en tales casos.

    ![Agregue un origen del evento a su instancia.][2]

1. Revisar y crear

    ![Agregue un origen del evento a su instancia.][3]

Confirme que el entorno se ha aprovisionado con las opciones que quiere.

## <a name="management"></a>Administración

Tiene la capacidad de administrar su entorno de versión preliminar de Azure Time Series Insights con Azure Portal. Estas son las principales diferencias al administrar un entorno **PAYG** de la versión preliminar de Azure Time Series Insights en lugar de un entorno S1 o S2 con Azure Portal:

* La hoja *Introducción* de Azure Portal permanece sin cambios en Azure Time Series Insights, excepto en los siguientes casos:
  * La Capacidad se elimina ya que este concepto no es relevante para entornos **PAYG**.
  * Se ha agregado la propiedad de id. de serie temporal. Determina cómo se particionan los datos.
  * Se eliminan los conjuntos de datos de referencia.
  * La dirección URL mostrada le dirige al [Explorador de versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).
  * Su nombre de la cuenta de Azure Storage se agrega a la lista.

* La hoja *Configurar* de Azure Portal se eliminó en la versión preliminar de Azure Time Series Insights, ya que los entornos **PAYG** no son configurables.

* La hoja de datos *Referencia* de Azure Portal se ha eliminado de la versión preliminar de Azure Time Series Insights, ya que los datos de referencia no son un componente de los entornos **PAYG**.

![Compruebe su instancia.][4]

## <a name="next-steps"></a>Pasos siguientes

Aprenda [a planificar su entorno ](./time-series-insights-update-plan.md).

Aprenda a [agregar un origen de Event Hub](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Aprenda a [agregar un origen de IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.png
[2]: media/v2-update-manage/manage_two.png
[3]: media/v2-update-manage/manage_three.png
[4]: media/v2-update-manage/manage_four.png
