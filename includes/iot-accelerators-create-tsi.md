---
title: archivo de inclusión
description: archivo de inclusión
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/20/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 8c114ed137089e70899e601ebdc1d4d39f562601
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383100"
---
## <a name="create-a-consumer-group"></a>Creación de un grupo de consumidores

Debe crear un grupo de consumidores dedicado en IoT Hub para usarlo en el streaming de datos de telemetría a Time Series Insights. Un origen del evento de Time Series Insights debe tener el uso exclusivo de un grupo de consumidores de IoT Hub.

Los pasos siguientes utilizan la CLI de Azure en Azure Cloud Shell para crear el grupo de consumidores:

1. IoT Hub es uno de varios recursos que creó cuando implementó el acelerador de soluciones de simulación de dispositivos. Ejecute el siguiente comando para buscar el nombre de su instancia de IoT Hub; recuerde que debe usar el nombre de su acelerador de soluciones:

    ```azurecli-interactive
    az resource list --resource-group contoso-simulation -o table
    ```

    IoT Hub es el recurso de tipo **Microsoft.Devices/IotHubs**.

1. Agregue un grupo de consumidores llamado **devicesimulationtsi** al centro. En el siguiente comando se usa el nombre de su acelerador de soluciones y el del centro:

    ```azurecli-interactive
    az iot hub consumer-group create --hub-name contoso-simulation7d894 --name devicesimulationtsi --resource-group contoso-simulation
    ```

    Ya puede cerrar Azure Cloud Shell.

## <a name="create-a-new-time-series-insights-environment"></a>Creación de un entorno de Time Series Insights

[Azure Time Series Insights](../articles/time-series-insights/time-series-insights-overview.md) es un servicio de análisis, almacenamiento y visualización totalmente administrado que permite administrar los datos de serie temporal a escala de IoT en la nube. Para crear un entorno de Time Series Insights:

1. Inicie sesión en el [Azure Portal](http://portal.azure.com/).

1. Seleccione **Crear un recurso** > **Internet de las cosas** > **Time Series Insights**.

    ![Nuevo entorno de Time Series Insights](./media/iot-accelerators-create-tsi/new-time-series-insights.png)

1. Para crear un entorno de Time Series Insights en el mismo grupo de recursos que su acelerador de soluciones, use los valores de la tabla siguiente:

    | Configuración | Valor |
    | ------- | ----- |
    | Nombre del entorno | En la captura de pantalla siguiente se usa el nombre **Contoso-TSI**. Elija un nombre exclusivo de su elección al completar este paso. |
    | Subscription | Seleccione la suscripción de Azure en el menú desplegable. |
    | Grupos de recursos | **contoso-simulation**. Use el nombre del acelerador de la soluciones. |
    | Ubicación | En este ejemplo se utiliza **Este de EE. UU.**. Cree el entorno en la misma región que el acelerador de simulación de dispositivos. |
    | SKU |**S1** |
    | Capacity | **1** |

    ![Creación del entorno de Time Series Insights](./media/iot-accelerators-create-tsi/new-time-series-insights-create.png)

    > [!NOTE]
    > Agregar el entorno de Time Series Insights al mismo grupo de recursos que el acelerador de soluciones significa que se eliminará cuando elimine el acelerador de soluciones.

1. Haga clic en **Create**(Crear). El entorno puede tardar unos minutos en crearse.

## <a name="create-event-source"></a>Creación de un origen de eventos

Cree un origen del evento para conectarlo a IoT Hub. Asegúrese de utilizar el grupo de consumidores creado en los pasos anteriores. Un origen del evento de Time Series Insights requiere que cada servicio tenga un grupo de consumidores dedicado que no esté en uso por otro servicio.

1. En Azure Portal, vaya al nuevo entorno de Time Series.

1. En el lado izquierdo, seleccione **Orígenes de eventos**:

    ![Visualización de orígenes de eventos](./media/iot-accelerators-create-tsi/time-series-insights-event-sources.png)

1. Haga clic en **Agregar**:

    ![Adición de orígenes de eventos](./media/iot-accelerators-create-tsi/time-series-insights-event-sources-add.png)

1. Para configurar el centro de IoT como un nuevo origen del evento, use los valores de la tabla siguiente:

    | Configuración | Valor |
    | ------- | ----- |
    | Nombre de origen de eventos | En la captura de pantalla siguiente se usa el nombre **contoso-iot-hub**. Use un nombre exclusivo de su elección al completar este paso. |
    | Origen | **IoT Hub** |
    | Opción de importación | **Usar IoT Hub desde suscripciones disponibles** |
    | Id. de suscripción | Seleccione la suscripción de Azure en el menú desplegable. |
    | Nombre de IoT Hub | **contoso-simulation7d894**. Use el nombre de su IoT Hub en el acelerador de soluciones del simulador de dispositivos. |
    | Nombre de directiva de IoT Hub | **iothubowner** |
    | Clave de la directiva de IoT Hub | Este campo se rellena automáticamente. |
    | Grupo de consumidores de IoT Hub | **devicesimulationtsi** |
    | Formato de serialización de eventos | **JSON** |
    | Nombre de la propiedad de marca de tiempo | Déjelo en blanco |

    ![Creación de un origen del evento](./media/iot-accelerators-create-tsi/time-series-insights-event-source-create.png)

1. Haga clic en **Create**(Crear).

> [!NOTE]
> También puede [conceder acceso a usuarios adicionales](../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) al explorador de Time Series Insights.