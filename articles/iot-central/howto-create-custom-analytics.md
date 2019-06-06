---
title: Ampliar Azure IoT Central con análisis personalizado | Microsoft Docs
description: Como desarrollador de soluciones, configurar una aplicación de IoT Central para realizar las visualizaciones y análisis personalizados. Esta solución utiliza Azure Databricks.
author: dominicbetts
ms.author: dobett
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e039e2b8d9c183b5bfee1bee47e4addc4c873bf7
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743446"
---
# <a name="extend-azure-iot-central-with-custom-analytics"></a>Ampliar Azure IoT Central con análisis personalizado

Esta guía muestra, como desarrollador de soluciones, cómo ampliar la aplicación IoT Central con visualizaciones y análisis personalizados. El ejemplo se usa un [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) área de trabajo para analizar el flujo de datos de telemetría de IoT Central y para generar visualizaciones como [cuadro trazados](https://wikipedia.org/wiki/Box_plot).

Esta guía muestra cómo ampliar IoT Central más allá de lo que puede hacer ya con la [herramientas de análisis integrado](howto-create-analytics.md).

En esta guía, obtendrá información sobre cómo:

* Stream telemetría desde una aplicación IoT Central mediante *exportación continua de datos*.
* Crear un entorno de Azure Databricks para analizar y representar los datos de telemetría de dispositivo.

## <a name="prerequisites"></a>Requisitos previos

Recuerde que para completar los pasos de esta guía paso a paso, necesita una suscripción activa a Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

### <a name="iot-central-application"></a>Aplicación de IoT Central

Crear una aplicación IoT Central desde la [Azure IoT Central - mis aplicaciones](https://aka.ms/iotcentral) página con la configuración siguiente:

| Configuración | Valor |
| ------- | ----- |
| Plan de pago | Pay-As-You-Go |
| Plantilla de la aplicación | Ejemplo Contoso |
| Nombre de la aplicación | Acepte el valor predeterminado o elija su propio nombre |
| URL | Acepte el valor predeterminado o elija su propio prefijo de dirección URL única |
| Directorio | El inquilino de Azure Active Directory |
| Suscripción de Azure | Su suscripción de Azure |
| Region | Este de EE. UU |

Los ejemplos y capturas de pantalla en este artículo utilizan la **East US** región. Elija una ubicación cercana a usted y asegúrese de que crear todos los recursos en la misma región.

### <a name="resource-group"></a>Grupos de recursos

Use la [Azure portal para crear un grupo de recursos](https://portal.azure.com/#create/Microsoft.ResourceGroup) llamado **IoTCentralAnalysis** para contener los otros recursos que cree. Cree los recursos de Azure en la misma ubicación que la aplicación IoT Central.

### <a name="event-hubs-namespace"></a>Espacio de nombres de Event Hubs

Use la [Azure portal para crear un espacio de nombres de Event Hubs](https://portal.azure.com/#create/Microsoft.EventHub) con las siguientes opciones:

| Configuración | Valor |
| ------- | ----- |
| Name    | Elija el nombre de espacio de nombres |
| Plan de tarifa | Básica |
| Subscription | Su suscripción |
| Grupos de recursos | IoTCentralAnalysis |
| Ubicación | Este de EE. UU |
| Unidades de procesamiento | 1 |

### <a name="azure-databricks-workspace"></a>El área de trabajo de Azure Databricks

Use la [Azure portal para crear un servicio de Azure Databricks](https://portal.azure.com/#create/Microsoft.Databricks) con las siguientes opciones:

| Configuración | Valor |
| ------- | ----- |
| Nombre del área de trabajo    | Elija el nombre de área de trabajo |
| Subscription | Su suscripción |
| Grupos de recursos | IoTCentralAnalysis |
| Ubicación | Este de EE. UU |
| Nivel de precios | Estándar |

Cuando haya creado los recursos necesarios, su **IoTCentralAnalysis** grupo de recursos se parece a la captura de pantalla siguiente:

![Grupo de recursos de análisis de IoT Central](media/howto-create-custom-analytics/resource-group.png)

## <a name="create-an-event-hub"></a>Creación de un centro de eventos

Puede configurar una aplicación IoT Central para exportar continuamente datos de telemetría a un centro de eventos. En esta sección, creará un centro de eventos para recibir datos de telemetría desde su aplicación IoT Central. El centro de eventos entrega los datos de telemetría a su trabajo de Stream Analytics para su procesamiento.

1. En el portal de Azure, vaya al espacio de nombres de Event Hubs y seleccione **+ centro de eventos**.
1. Nombre de su centro de eventos **centralexport**y seleccione **crear**.
1. En la lista de event hubs en el espacio de nombres, seleccione **centralexport**. A continuación, elija **directivas de acceso compartido**.
1. Seleccione **+ agregar**. Crear una directiva denominada **escuchar** con el **escuchar** de notificación.
1. Cuando la directiva esté lista, selecciónelo en la lista y, a continuación, copie el **clave principal de la cadena de conexión** valor.
1. Tome nota de esta cadena de conexión, se usará más adelante cuando configure su cuaderno de Databricks para leer desde el centro de eventos.

El espacio de nombres de Event Hubs se parece a la captura de pantalla siguiente:

![Espacio de nombres de Event Hubs](media/howto-create-custom-analytics/event-hubs-namespace.png)

## <a name="configure-export-in-iot-central"></a>Configurar la exportación en IoT Central

Navegue hasta la [aplicación IoT Central](https://aka.ms/iotcentral) creó a partir de la plantilla de Contoso. En esta sección, configurará la aplicación para transmitir los datos de telemetría desde sus dispositivos simulados al centro de eventos. Para configurar la exportación:

1. Navegue hasta la **exportación continua de datos** página, seleccione **+ nuevo**y, a continuación, **Azure Event Hubs**.
1. Utilice la siguiente configuración para configurar la exportación y luego seleccione **guardar**:

    | Configuración | Valor |
    | ------- | ----- |
    | Nombre para mostrar | Exportar a Event Hubs |
    | Enabled | Por |
    | Espacio de nombres de Event Hubs | El nombre del espacio de nombres de Event Hubs |
    | Centro de eventos | centralexport |
    | Medidas | Por |
    | Dispositivos | Off |
    | Plantillas de dispositivo | Off |

![Configuración de exportación continua de datos](media/howto-create-custom-analytics/cde-configuration.png)

Espere hasta que el estado de exportación **ejecutando** antes de continuar.

## <a name="configure-databricks-workspace"></a>Configurar el área de trabajo de Databricks

En el portal de Azure, vaya al servicio de Azure Databricks y seleccione **iniciar área de trabajo**. Una nueva pestaña en el explorador se abre y se inicia sesión en el área de trabajo.

### <a name="create-a-cluster"></a>Creación de un clúster

En el **Azure Databricks** página, en la lista de tareas comunes, seleccionadas **nuevo clúster**.

Use la información de la tabla siguiente para crear el clúster:

| Configuración | Valor |
| ------- | ----- |
| Cluster Name | centralanalysis |
| Modo de clúster | Estándar |
| Versión del Runtime de Databricks | 5.3 (Scala 2.11, Spark 2.4.0) |
| versión de Python | 3 |
| Habilitar escalado automático | Sin |
| Finalizar después de minutos de inactividad | 30 |
| Tipo de trabajo | Standard_DS3_v2 |
| Trabajos | 1 |
| Tipo de controlador | Igual que trabajo |

Creación de un clúster puede tardar varios minutos, espere a que la creación del clúster completar antes de continuar.

### <a name="install-libraries"></a>Instalar bibliotecas

En el **clústeres** página, espere a que el estado del clúster es **ejecutando**.

Los pasos siguientes muestran cómo importar la biblioteca necesita su ejemplo en el clúster:

1. En el **clústeres** página, espere hasta que el estado de la **centralanalysis** clúster interactivo es **ejecutando**.

1. Seleccione el clúster y, a continuación, elija el **bibliotecas** ficha.

1. En el **bibliotecas** ficha, elija **instalar nuevos**.

1. En el **instalar biblioteca** página, elija **Maven** como el origen de la biblioteca.

1. En el **coordina** cuadro de texto, escriba el siguiente valor: `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`

1. Elija **instalar** para instalar la biblioteca en el clúster.

1. El estado de la biblioteca es ahora **instalado**:

    ![Biblioteca instalada](media/howto-create-custom-analytics/cluster-libraries.png)

### <a name="import-a-databricks-notebook"></a>Importar un cuaderno de Databricks

Use los pasos siguientes para importar un cuaderno de Databricks que contiene el código de Python para analizar y visualizar los datos de telemetría de IoT Central:

1. Navegue hasta la **área de trabajo** página en su entorno de Databricks. Seleccione la lista desplegable situada junto a su nombre de cuenta y, a continuación, elija **importación**.

1. Elija esta opción Importar desde una dirección URL y escriba la siguiente dirección: [https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true](https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true)

1. Para importar el Bloc de notas, elija **importar**.

1. Seleccione el **área de trabajo** para ver el Bloc de notas importada:

    ![Bloc de notas importada](media/howto-create-custom-analytics/import-notebook.png)

1. Edite el código en la primera celda de Python para agregar la cadena de conexión de Event Hubs que guardó anteriormente:

    ```python
    from pyspark.sql.functions import *
    from pyspark.sql.types import *

    ###### Event Hub Connection strings ######
    telementryEventHubConfig = {
      'eventhubs.connectionString' : '{your Event Hubs connection string}'
    }
    ```

## <a name="run-analysis"></a>Ejecutar el análisis

Para ejecutar el análisis, debe asociar el Bloc de notas para el clúster:

1. Seleccione **Detached** y, a continuación, seleccione el **centralanalysis** clúster.
1. Si el clúster no se está ejecutando, inícielo.
1. Para iniciar el Bloc de notas, seleccione el botón de ejecución.

Puede ver un error en la última celda. Si es así, compruebe que las celdas anteriores se están ejecutando, espere un minuto para algunos datos se escriban en el almacenamiento y, a continuación, ejecute de nuevo la última celda.

### <a name="view-smoothed-data"></a>Ver datos suavizadas

En el Bloc de notas, desplácese hacia abajo hasta la celda de la 14 a ver un gráfico de la humedad Media acumulada por tipo de dispositivo. Este trazado se actualiza continuamente a medida que llega la telemetría de streaming:

![Suaviza el trazado de la telemetría](media/howto-create-custom-analytics/telemetry-plot.png)

Puede cambiar el tamaño del gráfico en el Bloc de notas.

### <a name="view-box-plots"></a>Vista de cajas

En el Bloc de notas, desplácese hacia abajo hasta la celda 20 para ver el [cuadro trazados](https://en.wikipedia.org/wiki/Box_plot). Los trazados de cuadro se basan en datos estáticos, por lo que para actualizarlos debe volver a ejecutar la celda:

![Diagramas de cajas](media/howto-create-custom-analytics/box-plots.png)

Puede cambiar el tamaño de los trazados en el Bloc de notas.

## <a name="tidy-up"></a>Limpieza

Para limpiar después de este tema de procedimientos y evitar costos innecesarios, eliminar el **IoTCentralAnalysis** grupo de recursos en el portal de Azure.

Puede eliminar la aplicación IoT Central desde la **administración** página dentro de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

En esta guía paso a paso, ha aprendido lo siguiente:

* Stream telemetría desde una aplicación IoT Central mediante *exportación continua de datos*.
* Crear un entorno de Azure Databricks para analizar y representar los datos de telemetría.

Ahora que sabe cómo crear análisis personalizado, es el siguiente paso sugerido obtener información sobre cómo [visualizar y analizar los datos de Azure IoT Central en un panel de Power BI](howto-connect-powerbi.md).
