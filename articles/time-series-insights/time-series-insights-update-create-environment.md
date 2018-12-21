---
title: 'Configuración en la versión preliminar de Azure Time Series Insights: Tutorial de configuración de un entorno en la versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Obtenga información sobre cómo configurar el entorno en la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 9ad957d6378b1279f1ca51939eb4802b0ce7d78f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322625"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configuración de un entorno en la versión preliminar de Azure Time Series Insights

Este tutorial le guiará en el proceso de creación de un entorno en la versión preliminar de pago por uso (PAYG) de Azure Time Series Insights. En este tutorial, aprenderá a:

* Crear un entorno en la versión preliminar de Azure Time Series Insights.
* Conectar el entorno de versión preliminar de Azure Time Series Insights a un centro de eventos en Azure Event Hubs.
* Ejecutar una simulación de un parque eólico para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
* Realizar un análisis básico de los datos.
* Definir el tipo y la jerarquía de un modelo de serie temporal y asociarlo con sus instancias.

# <a name="create-a-device-simulation"></a>Creación de una simulación de dispositivo

En esta sección, creará tres dispositivos simulados que enviarán datos a un centro de IoT Hub.

1. Vaya a la [página principal de aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/Accelerators). La página principal de aceleradores de soluciones de Azure IoT muestra varios ejemplos pregenerados. Inicie sesión con la cuenta de Azure. A continuación, seleccione **Simulación de dispositivo**.

   ![Página principal de aceleradores de soluciones de Azure IoT][1]

   Por último, haga clic en **Probar ahora**.

1. Escriba los parámetros necesarios en la página **Crear la solución Simulación de dispositivo**:

   | Parámetro | DESCRIPCIÓN |
   | --- | --- |
   | Nombre de la solución |    Un valor único que se utiliza para la creación de un nuevo grupo de recursos. Los recursos de Azure enumerados se | han creado y asignado al grupo de recursos. |
   | Subscription | Especifique la misma suscripción usada para la creación del entorno de TSI. |
   | Region |   Especifique la misma región utilizada para la creación de su entorno de TSI. |
   | Implementación de recursos opcionales de Azure    | Deje activado IoT Hub, ya que los dispositivos simulados lo utilizarán para conectarse y transmitir datos. |

   Después de escribir los parámetros necesarios, haga clic en **Crear solución**. Espere aproximadamente 10-15 minutos para implementar su solución.

   ![Creación de una solución de simulación de dispositivos][2]

1. En el **panel del acelerador de soluciones**, haga clic en el botón **Iniciar**:

   ![Inicio de la solución de simulación de dispositivo][3]

1. Se le redirigirá a la página de **simulación de dispositivos IoT de Microsoft Azure**. Haga clic en **+ Nueva simulación** en la esquina superior derecha de la pantalla.

   ![Página de simulación de Azure IoT][4]

1.  Rellene los parámetros necesarios como sigue:

    ![Parámetros para rellenar][5]

    |||
    | --- | --- |
    | **Nombre** | Escriba un nombre único para un simulador. |
    | **Descripción** | Escriba una definición. |
    | **Duración de la simulación** | Establézcala en `Run indefinitely` |
    | **Modelo de dispositivo** | **Nombre**: escriba `Chiller` **Cantidad**: escriba `3`. |
    | **Target IoT Hub** (IoT Hub de destino) | Establézcala en `Use pre-provisioned IoT Hub` |

    Después de rellenar los parámetros necesarios, haga clic en **Iniciar simulación**.

1. En el panel de simulación de dispositivos, consulte los **dispositivos activos** y los **mensajes por segundo**.

    ![Panel de simulación de Azure IoT][6]

## <a name="list-device-simulation-properties"></a>Enumeración de propiedades de simulación de dispositivos

Antes de crear un entorno de Azure Time Series Insights, necesitará los nombres del centro de IoT Hub, de la suscripción y del grupo de recursos.

1. Vaya al **panel del acelerador de soluciones** e inicie sesión con la misma cuenta de la suscripción de Azure. Encuentre la simulación del dispositivo que creó en los pasos anteriores.

1. Haga clic en el simulador de dispositivos y haga clic en **Iniciar**. Haga clic en el vínculo del **Portal de administración de Azure** que se muestra en el lado derecho.

    ![Anuncios del simulador][7]

1. Anote los nombres del centro de IoT Hub, de la suscripción y del grupo de recursos.

    ![Azure Portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Creación de un entorno de pago por uso en la versión preliminar de Time Series Insights

En esta sección se describe cómo crear un entorno en la versión preliminar de Azure Time Series Insights con [Azure Portal](https://portal.azure.com/).

1. Inicie sesión en Azure Portal con la cuenta de su suscripción.

1. Seleccione **Crear un recurso**.

1. Seleccione la categoría **Internet de las cosas** y, después, elija **Time Series Insights**.

   ![Seleccione Crear un recurso, luego seleccione Internet de las cosas y Time Series Insights.][9]

1. Rellene los campos de la página como sigue:

   | | |
   | --- | ---|
   | **Nombre del entorno** | Elija un nombre único para el entorno de la versión preliminar de Azure Time Series Insights. |
   | **Suscripción** | Especifique la suscripción donde desea crear el entorno de versión preliminar de Azure Time Series Insights. Es un procedimiento recomendado usar la misma suscripción que el resto de los recursos de IoT creados por el simulador de dispositivos. |
   | **Grupo de recursos** | Un grupo de recursos es un contenedor para recursos de Azure. Elija un grupo de recursos existente o cree uno, para el recurso de entorno de versión preliminar de Azure Time Series Insights. Es un procedimiento recomendado usar el mismo grupo de recursos que el resto de los recursos de IoT creados por el administrador de dispositivos. |
   | **Ubicación** | Elija una región del centro de datos para su entorno de versión preliminar de Azure Time Series Insights. Para evitar costos de ancho de banda y una latencia agregados, es mejor mantener el entorno de la versión preliminar de Azure Time Series Insights en la misma región que otros recursos de IoT. |
   | **Nivel** |  Seleccione `PAYG`, que significa pago por uso. Se trata de la SKU del producto de versión preliminar de Azure Time Series Insights. |
   | **Id. de propiedad** | Identifica la serie temporal de manera exclusiva. Tenga en cuenta que este campo es inmutable y no se puede cambiar más adelante. Para este tutorial, establezca el campo en `iothub-connection-device-id`. Para más información sobre el Id. de serie de tiempo, lea [cómo elegir un identificador de serie de tiempo](./time-series-insights-update-how-to-id.md). |
   | **Nombre de cuenta de almacenamiento** | Escriba un nombre único global para la nueva cuenta de almacenamiento que se va a crear. |

   Después de rellenar los campos anteriores, haga clic en **Siguiente: Origen de eventos**.

   ![Haga clic en Siguiente: Origen de eventos][10]

1. En la página, rellene los campos como sigue:

   | | |
   | --- | --- |
   | **Creación de un origen de eventos** | Escriba `Yes`.|
   | **Nombre** | Requiere un valor único, que se utiliza para denominar el origen del evento.|
   | **Tipo de origen** | Escriba `IoT Hub`. |
   | **Selección de un centro** | Escriba `Select Existing`. |
   | **Suscripción** | Escriba la suscripción que usó para el simulador de dispositivos. |
   | **Nombre de la instancia de IoT Hub** | Escriba el nombre del centro de IoT que creó para el simulador de dispositivos. |
   | **Directiva de acceso de IoT Hub** | Escriba `iothubowner`. |
   | **Grupo de consumidores de IoT Hub** | Necesita un grupo de consumidores exclusivo para una versión preliminar de Azure Time Series Insights. |
   | **Timestamp** | Este campo se utiliza para identificar la propiedad de marca de tiempo en los datos de telemetría entrantes. Para este tutorial, no rellene el campo. Este simulador usa la marca de tiempo entrante desde IoT Hub que Time Series Insights tiene como valor predeterminado.|

   Para crear un grupo de consumidores exclusivo:

   1. Haga clic en **Nuevo** junto al campo **Grupo de consumidores del IoT Hub**:

      ![Haga clic en Siguiente: Origen de eventos][11]

   1. Asigne al grupo de consumidores un nombre único y haga clic en **Agregar**:

      ![Haga clic en Agregar][12]

   Después de rellenar los campos anteriores, haga clic en **Revisar y crear**.

      ![Revisar y crear][13]

1. Revise todos los campos en la página Revisar y haga clic en **Crear**.

   ![Crear][14]

1. Puede ver el estado de la implementación.

   ![Implementación completada][15]

1. Debe obtener acceso al entorno de serie temporal si es propietario del inquilino. Para asegurarse de que tiene acceso:

   * Vaya al entorno de versión preliminar de Azure Time Series Insights recién creado. Para ello, puede buscar el grupo de recursos. A continuación, haga clic en el entorno de Time Series:

      ![Implementación completada][16]

   * En la página de la versión preliminar de Azure Time Series Insights, vaya a **Directivas de acceso de datos**.

     ![Directivas de acceso a datos][17]

   * Compruebe que aparecen las credenciales.

     ![Comprobar las credenciales][18]

   Si no se muestran sus credenciales, deberá darse permiso para acceder al entorno. Lea [Concesión de acceso a los datos](./time-series-insights-data-access.md) para obtener más información sobre cómo establecer permisos.

## <a name="analyze-data-in-your-environment"></a>Análisis de datos en el entorno

En esta sección, se va realizar un análisis básico de los datos de la serie temporal mediante el [explorador de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).

1. Haga clic en la dirección URL de la página de recursos de [Azure Portal](https://portal.azure.com/) para acceder al explorador de la versión preliminar de Azure Time Series Insights.

   ![Dirección URL del explorador de Azure Time Series Insights][19]

1. En el explorador, seleccione los nodos **Unparented Instances** (Instancias no primarias) para ver todas las instancias de la versión preliminar de Azure Time Series Insights en el entorno.

   ![Lista de instancias no primarias][20]

1. En la serie temporal que se muestra, haga clic en la primera instancia. A continuación, haga clic en **Mostrar presión media**.

   ![Mostrar presión media][21]

1. Debe aparecer un gráfico de serie temporal a la derecha:

   ![Gráfico de serie temporal][22]

1. Repita el **paso 3** con las otras dos series temporales. Todas las series temporales se pueden ver como se muestra a continuación:

   ![Gráfico de todas las series temporales][23]

1. Modifique el **intervalo de tiempo** para ver las tendencias de serie temporal durante la última hora. Seleccione el cuadro de opción **Desde** como se muestra a continuación:

   ![Seleccionar la opción Desde][24]

1. Cambie la hora en el cuadro de opción **Desde** para mostrar los eventos de la última hora:

   ![Seleccionar la opción Desde][25]

1. A continuación, puede comparar la presión de los tres dispositivos durante la última hora:

   ![Seleccionar la opción Desde][26]

## <a name="define-and-apply-a-model"></a>Definir y aplicar un modelo

En esta sección, se aplicará un modelo para estructurar los datos. Para completar el modelo, definirá los tipos, las jerarquías y las instancias. Para obtener más información sobre el modelado de datos, vaya a [Modelos de serie temporal](./time-series-insights-update-tsm.md).

1. En el explorador, seleccione la pestaña **Modelo**:

   ![Seleccionar la pestaña Modelo][27]

1. A continuación, haga clic en **+ Agregar** para agregar un tipo. En el lado derecho, se abrirá un editor de tipos.

   ![Haga clic en Agregar][28]

1. A continuación, defina tres variables: presión, temperatura y humedad en un tipo. Defina los siguientes campos:

   | | |
   | --- | ---|
   | **Nombre** | Escriba `Chiller`. |
   | **Descripción** | Escriba `This is a type definition of Chiller`. |

   * Ahora, defina la presión con tres variables:

      | | |
      | --- | ---|
      | **Nombre** | Escriba `Avg Pressure`. |
      | **Valor** | Seleccione **presión (doble)**. Tenga en cuenta que este campo puede tardar unos minutos en rellenarse después de que Azure Time Series Insights empiece a recibir eventos. |
      | **Operación de agregación** | Seleccionar `AVG` |

      ![Agregar una variable][29]

      Haga clic en **+ Variable** para agregar la variable siguiente.

   * Ahora, defina la temperatura:

      | | |
      | --- | ---|
      | **Nombre** | Escriba `Avg Temperature`. |
      | **Valor** | Seleccione **temperatura (doble)**. Tenga en cuenta que este campo puede tardar unos minutos en rellenarse después de que Azure Time Series Insights empiece a recibir eventos. |
      | **Operación de agregación** | Seleccionar `AVG`|

      ![Definir temperatura][30]

   * Ahora, defina la humedad:

      | | |
      | --- | ---|
      | **Nombre** | Escriba `Max Humidity`. |
      | **Valor** | Seleccione **humedad (doble)**. Tenga en cuenta que este campo puede tardar unos minutos en rellenarse después de que Azure Time Series Insights empiece a recibir eventos. |
      | **Operación de agregación** | Seleccionar `MAX`|

      ![Definir temperatura][31]

   Después de definir las variables, haga clic en **Crear**.

1. Puede ver el tipo agregado:

   ![Ver el tipo agregado][32]

1. El siguiente paso es agregar una jerarquía. En la sección **Jerarquías**, seleccione **+ Agregar** para crear una jerarquía:

   ![Agregar una jerarquía][33]

1. Defina la jerarquía. Rellene los campos como sigue:

   | | |
   | --- | ---|
   | **Nombre** | Escriba `Location Hierarchy`. |
   | **Nivel 1** | Escriba `Country`. |
   | **Nivel 2** | Escriba `City`. |
   | **Nivel 3** | Escriba `Building`. |

   Después de rellenar los campos anteriores, haga clic en **Crear**.

   ![Definir una jerarquía][34]

1. Puede ver la jerarquía creada:

   ![Ver la jerarquía][35]

1. Después de definir la jerarquía, haga clic en **Instancias** a la izquierda. Cuando aparezcan las instancias, haga clic en la primera instancia y seleccione **Editar**:

   ![Editar una instancia][36]

1. En el lado derecho se abrirá un editor de texto. Agregue los campos siguientes:

   | | |
   | --- | --- |
   | **Tipo** | Seleccionar `Chiller` |
   | **Descripción** | Escriba `Instance for Chiller-01.1`. |
   | **Jerarquías** | Habilite `Location Hierarchy`. |
   | **País** | Escriba `USA`. |
   | **Ciudad** | Escriba `Seattle`. |
   | **Edificio** | Escriba `Space Needle`. |

    Después de rellenar los campos anteriores, haga clic en **Guardar**.

   ![Guardar un refrigerador][37]

1. Repita el paso anterior para los otros sensores. Use los siguientes campos:

   * Para el refrigerador 01.2:

     | | |
     | --- | --- |
     | **Tipo** | Seleccionar `Chiller` |
     | **Descripción** | Escriba `Instance for Chiller-01.2`. |
     | **Jerarquías** | Habilite `Location Hierarchy`. |
     | **País** | Escriba `USA`. |
     | **Ciudad** | Escriba `Seattle`. |
     | **Edificio** | Escriba `Pacific Science Center`. |

   * Para el refrigerador 01.3:

     | | |
     | --- | --- |
     | **Tipo** | Seleccionar `Chiller` |
     | **Descripción** | Escriba `Instance for Chiller-01.1`. |
     | **Jerarquías** | Habilite `Location Hierarchy`. |
     | **País** | Escriba `USA`. |
     | **Ciudad** | Escriba `New York`. |
     | **Edificio** | Escriba `Empire State Building`. |

1. Vaya a la pestaña **Analizar** y actualice la página. Expanda todos los niveles de jerarquía para buscar la serie temporal.

   ![Ver la pestaña Analizar][38]

1. Para explorar la serie temporal durante la última hora, cambie **Tiempos rápidos** a última hora:

   ![Explorar la última hora][39]

1. Haga clic en la serie temporal en **Pacific Science Center** y, después, en **Mostrar humedad máxima**.

   ![Mostrar humedad máxima][40]

1. Se abrirá la serie temporal para **Humedad máxima** con un tamaño de intervalo de 1 minuto. Haga clic en una región para filtrar un intervalo. A continuación, haga clic con el botón derecho y acerque para analizar eventos durante el período de tiempo:

   ![Ver, filtrar y acercar][41]

   ![Ver, filtrar y acercar][42]

1. También puede hacer clic en una región y después hacer clic con el botón derecho para ver los detalles del evento:

   ![Ver, filtrar y acercar][43]

   ![Ver, filtrar y acercar][44]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:  

* Crear y usar un acelerador de simulación de dispositivos.
* Crear un entorno de pago por uso de la versión preliminar de Azure Time Series Insights.
* Conectar el entorno de versión preliminar de Azure Time Series Insights a un centro de eventos.
* Ejecutar una simulación de un parque eólico para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
* Realizar un análisis básico de los datos.
* Definir la jerarquía y el tipo de modelo de serie temporal y asociarlos con sus instancias.

Ahora que sabe cómo crear su propio entorno de versión preliminar de Azure Time Series Insights, obtenga más información sobre los conceptos clave de Azure Time Series Insights.

Lea sobre la configuración de almacenamiento de Azure Time Series Insights:

> [!div class="nextstepaction"]
> [Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)

Obtenga más información sobre los modelos de serie temporal:

> [!div class="nextstepaction"]
> [Modelado de datos de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/device-one-accelerator.png
[2]: media/v2-update-provision/device-two-create.png
[3]: media/v2-update-provision/device-three-launch.png
[4]: media/v2-update-provision/device-four-iot-sim-page.png
[5]: media/v2-update-provision/device-five-params.png
[6]: media/v2-update-provision/device-six-listings.png
[7]: media/v2-update-provision/device-seven-dashboard.png
[8]: media/v2-update-provision/device-eight-portal.png

[9]: media/v2-update-provision/payg-one-azure.png
[10]: media/v2-update-provision/payg-two-create.png
[11]: media/v2-update-provision/payg-three-new.png
[12]: media/v2-update-provision/payg-four-add.png
[13]: media/v2-update-provision/payg-five-event-source.png
[14]: media/v2-update-provision/payg-six-review.png
[15]: media/v2-update-provision/payg-seven-deploy.png
[16]: media/v2-update-provision/payg-eight-environment.png
[17]: media/v2-update-provision/payg-nine-data-access.png
[18]: media/v2-update-provision/payg-ten-verify.png

[19]: media/v2-update-provision/analyze-one-portal.png
[20]: media/v2-update-provision/analyze-two-unparented.png
[21]: media/v2-update-provision/analyze-three-show-pressure.png
[22]: media/v2-update-provision/analyze-four-chart.png
[23]: media/v2-update-provision/analyze-five-chart.png
[24]: media/v2-update-provision/analyze-six-from.png
[25]: media/v2-update-provision/analyze-seven-change-from.png
[26]: media/v2-update-provision/analyze-eight-all.png

[27]: media/v2-update-provision/define-one-model.png
[28]: media/v2-update-provision/define-two-add.png
[29]: media/v2-update-provision/define-three-variable.png
[30]: media/v2-update-provision/define-four-avg.png
[31]: media/v2-update-provision/define-five-humidity.png
[32]: media/v2-update-provision/define-six-type.png
[33]: media/v2-update-provision/define-seven-hierarchy.png
[34]: media/v2-update-provision/define-eight-add-hierarchy.png
[35]: media/v2-update-provision/define-nine-created.png
[36]: media/v2-update-provision/define-ten-edit.png
[37]: media/v2-update-provision/define-eleven-chiller.png
[38]: media/v2-update-provision/define-twelve.png
[39]: media/v2-update-provision/define-thirteen-explore.png
[40]: media/v2-update-provision/define-fourteen-show-max.png
[41]: media/v2-update-provision/define-fifteen-filter.png
[42]: media/v2-update-provision/define-sixteen.png
[43]: media/v2-update-provision/define-seventeen.png
[44]: media/v2-update-provision/define-eighteen.png