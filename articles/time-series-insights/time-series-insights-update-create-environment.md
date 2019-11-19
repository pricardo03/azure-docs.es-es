---
title: 'Tutorial: Configuración de un entorno en la versión preliminar de Azure Time Series Insights'
description: 'Tutorial: Obtenga información sobre cómo configurar el entorno en la versión preliminar de Azure Time Series Insights.'
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/11/2019
ms.custom: seodec18
ms.openlocfilehash: 49960ae91688b97a9d38a65e1b17fd277508b996
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114574"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configuración de un entorno en la versión preliminar de Azure Time Series Insights

Este tutorial le guía en el proceso de crear un entorno de pago por uso (PAYG) de la versión preliminar de Azure Time Series Insights.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Crear un entorno en la versión preliminar de Azure Time Series Insights.
> * Conectar el entorno de versión preliminar de Azure Time Series Insights a una instancia de IoT Hub.
> * Ejecutar un ejemplo del acelerador de soluciones para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
> * Realizar un análisis básico de los datos.
> * Definir el tipo y la jerarquía de un modelo de Time Series y asociarlos con sus instancias.
> * Use el conector de Power BI y visualice los datos en Power BI.

>[!TIP]
> Los [aceleradores de soluciones de IoT](https://www.azureiotsolutions.com/Accelerators) proporcionan soluciones preconfiguradas de nivel empresarial que se pueden usar puede usar para acelerar el desarrollo de soluciones de IoT personalizadas.

Regístrese para obtener [suscripción gratuita a Azure](https://azure.microsoft.com/free/), en caso de que no tenga ninguna.

## <a name="prerequisites"></a>Requisitos previos

* Como mínimo, debe tener el rol **Colaborador** para la suscripción de Azure. Para más información, consulte [Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="create-a-device-simulation"></a>Creación de una simulación de dispositivo

En esta sección, creará tres dispositivos simulados que envían datos a una instancia de Azure IoT Hub.

1. Vaya a la [página de aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/Accelerators). En la página se muestran varios ejemplos creados previamente. Inicie sesión con la cuenta de Azure. A continuación, seleccione **Simulación de dispositivo**.

   [![Página de aceleradores de soluciones de Azure IoT](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

1. En la página siguiente, seleccione **Probar ahora**.

   [![Página Simulación de dispositivo](media/v2-update-provision/device-two-try.png)](media/v2-update-provision/device-two-try.png#lightbox)

1. Escriba los parámetros necesarios en la página **Crear la solución Simulación de dispositivos**:

    | Parámetro | . |
    | --- | --- |
    | **Nombre de la implementación** | Escriba un valor único para un nuevo grupo de recursos. Los recursos de Azure enumerados se crean y se asigna al grupo de recursos. |
    | **Suscripción de Azure** | Seleccione la suscripción donde creará el entorno de Time Series Insights. |
    | **Ubicación de Azure** | Seleccione la región donde pretende almacenar el entorno de Time Series Insights. Tenga en cuenta que el simulador de dispositivos solo se ofrece en un número limitado de regiones; por lo tanto, si no ve la región deseada, puede seleccionar una ubicación solo para el tutorial y, después, crear un nuevo entorno de TSI cuando esté listo para pasar a la siguiente fase de incorporación.  |
    | **Opciones de implementación** | Seleccione **Provision new IoT Hub** (Aprovisionar nuevo centro de IoT). |

    1. Seleccione **Crear**.
    [![Página Simulación de dispositivo](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. Después de unos 20 minutos, el acelerador de la solución estará listo.

    [![Página Simulación de dispositivo](media/v2-update-provision/device-two-ready.png)](media/v2-update-provision/device-two-ready.png#lightbox)

## <a name="create-a-preview-payg-environment"></a>Creación de un entorno de pago por uso (versión preliminar)

En esta sección se describe cómo crear un entorno dee Azure Time Series Insights en versión preliminar y conectarlo al centro de IoT que el Acelerador de soluciones de IOT mediante [Azure Portal](https://portal.azure.com/).

1. Inicie sesión en Azure Portal con la cuenta de su suscripción.

1. Seleccione **Crear un recurso** > **Internet de las cosas** > **Time Series Insights**.

   [![Seleccionar Internet de las cosas y, luego, Time Series Insights](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. En el panel **Creación del entorno de Time Series Insights**, en la pestaña **Aspectos básicos**, establezca los parámetros siguientes:

    | Parámetro | . |
    | --- | ---|
    | **Nombre del entorno** | Especifique un nombre único para el entorno de la versión preliminar de Azure Time Series Insights. |
    | **Suscripción** | Especifique la suscripción donde desea crear el entorno de versión preliminar de Azure Time Series Insights. Es un procedimiento recomendado usar la misma suscripción que el resto de los recursos de IoT creados por el simulador de dispositivos. |
    | **Grupos de recursos** | Seleccione un grupo de recursos existente o cree un nuevo grupo de recursos para el recurso de entorno de versión preliminar de Azure Time Series Insights. Un grupo de recursos es un contenedor para recursos de Azure. Un procedimiento recomendado es usar el mismo grupo de recursos que el resto de los recursos de IoT creados por el simulador de dispositivos. |
    | **Ubicación** | Seleccione una región del centro de datos para su entorno de versión preliminar de Azure Time Series Insights. Para evitar una latencia adicional, es mejor crear el entorno de versión preliminar de Azure Time Series Insights en la misma región que el centro de IoT creado por el simulador de dispositivo. |
    | **Nivel** |  Seleccione **Pago por uso** *.* Esta es la SKU del producto de versión preliminar de Azure Time Series Insights. |
    | **Id. de propiedad** | Especifique un valor que identifique de forma única su instancia de serie temporal. El valor que especifique en el cuadro **Id. de propiedad** no se puede cambiar más tarde. En este tutorial, escriba **iothub-connection-device-id**. Para obtener más información acerca del identificador de Time Series, consulte [Procedimientos recomendados al elegir un id. de serie temporal](./time-series-insights-update-how-to-id.md). |
    | **Nombre de cuenta de almacenamiento** | Escriba un nombre único global para la nueva cuenta de almacenamiento.|
    |**Habilitar almacenamiento intermedio**|Seleccione **sí** para habilitar el almacenamiento intermedio.|
    |**Retención de datos (en días)**|Elija la opción predeterminada de 7 días. |

    Seleccione **Siguiente: Origen de eventos**.

   [![Panel para crear un entorno de Time Series Insights](media/v2-update-provision/payg-two-1-create.png)](media/v2-update-provision/payg-two-1-create.png#lightbox)
   [![Panel para crear un entorno de Time Series Insights](media/v2-update-provision/payg-two-2-create.png)](media/v2-update-provision/payg-two-2-create.png#lightbox)

1. En la pestaña **Origen del evento**, puede establecer los siguientes parámetros:

   | Parámetro | . |
   | --- | --- |
   | **Create an event source?** ¿Crear un origen del evento? | Seleccione **Sí**.|
   | **Nombre** | Escriba un valor único para el nombre del origen de eventos. |
   | **Tipo de origen** | Seleccione **IoT Hub**. |
   | **Selección de un centro** | Elija **Seleccionar existente**. |
   | **Suscripción** | Seleccione la suscripción que usó para el simulador de dispositivos. |
   | **Nombre de la instancia de IoT Hub** | Seleccione el nombre del centro de IoT que creó para el simulador de dispositivos. |
   | **Directiva de acceso de IoT Hub** | seleccione **iothubowner**. |
   | **Grupo de consumidores de IoT Hub** | Seleccione **New** (Nuevo), escriba un nombre único y, a continuación, seleccione **Add** (Agregar). El grupo de consumidores debe ser un valor único en la versión preliminar de Azure Time Series Insights. |
   | **Propiedad de marca de tiempo** | Este campo se utiliza para identificar la propiedad de **Marca de tiempo** en los datos de telemetría entrantes. En este tutorial, deje este cuadro vacío. Este simulador usa la marca de tiempo entrante de IoT Hub, que Time Series Insights tiene como valor predeterminado. |

   Seleccione **Revisar + crear**.

   [![Configuración de un origen del evento](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Seleccione **Crear**.

    [![Página Review + Create (Revisar + crear), con el botón Create (Crear)](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

    Puede ver el estado de la implementación:

    [![Notificación de que la implementación ha finalizado](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Tendrá acceso a su entorno de versión preliminar de Azure Time Series Insights de forma predeterminada si es propietario de la suscripción de Azure. Compruebe que tiene acceso:

   1. Busque el grupo de recursos y seleccione el entorno de versión preliminar de Azure Time Series Insights creado recientemente: 
      [![Entorno seleccionado](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   1. En la página de la versión preliminar de Azure Time Series Insights, vaya a **Directivas de acceso a datos**: [![Directivas de acceso a datos](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   1. Compruebe que aparecen las credenciales:

      [![Credenciales mostradas](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Si no se muestran sus credenciales, deberá otorgarse permiso para acceder al entorno. Para ello, seleccione Agregar y busque sus credenciales. Para más información sobre cómo establecer permisos, lea [Concesión de acceso a datos](./time-series-insights-data-access.md).

## <a name="stream-data"></a>Datos de streaming

Ahora que ha implementado el entorno de Time Series Insights, comience a transmitir datos para su análisis.

1. Vaya a la [página de aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/Accelerators). Busque la solución en el panel del acelerador de soluciones y seleccione **Iniciar**:

    [![Inicio de la solución de simulación de dispositivo](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. Seleccione **Go to your solution accelerator** (Ir al acelerador de soluciones).

    [![Inicio de la solución de simulación de dispositivo](media/v2-update-provision/device-accelerator.png)](media/v2-update-provision/device-accelerator.png#lightbox)

1. Se le redirigirá a la página de **simulación de dispositivos IoT de Microsoft Azure**. Seleccione **+ New simulation** (+Nueva simulación) en la esquina superior derecha.

    [![Página de simulación de Azure IoT](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. En el panel del **programa de instalación de la simulación**, establezca los parámetros siguientes:

    | Parámetro | . |
    | --- | --- |
    | **Nombre** | Escriba un nombre único para un simulador. |
    | **Descripción** | Escriba una definición. |
    | **Simulation duration** (Duración de la simulación) | Establézcala en **Run indefinitely** (Ejecutar indefinidamente). |
    | **Device model** (Modelo de dispositivo) | Haga clic en +**Add a device type** (Agregar un tipo de dispositivo). <br />**Nombre**: Escriba **Elevator**. <br />**Amount** (Cantidad): Escriba **3**. <br /> Deje los valores predeterminados restantes. |
    | **Target IoT Hub** (IoT Hub de destino) | Establézcalo en **Use pre-provisioned IoT Hub** (Usar IoT Hub aprovisionado previamente). |

    [![Parámetros para especificar](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    Seleccione **Start simulation** (Iniciar simulación).

    En el panel de simulación de dispositivos, verá **Dispositivos activos** y **Total de mensajes**.

    [![Panel de simulación de Azure IoT](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="analyze-data"></a>Análisis de datos

En esta sección, se va realizar un análisis básico de los datos de la serie temporal mediante el [explorador de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).

1. Vaya al explorador de la versión preliminar de Azure Time Series Insights; para ello, seleccione la dirección URL de la página de recursos de [Azure Portal](https://portal.azure.com/).

    [![La dirección URL del explorador de la versión preliminar de Azure Time Series Insights](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. En el explorador de Time Series Insights, verá una barra que abarca la parte superior de la pantalla. Este es el selector de disponibilidad. Asegúrese de tener al menos dos 2m seleccionados y, si es necesario, expanda el intervalo de tiempo seleccionando los controladores de selector y arrastrándolos a la izquierda y a la derecha.

1. Podrá ver **Instancias de Time Series** a la izquierda.


    [![Lista de instancias no primarias](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Seleccione la primera instancia de la serie temporal. A continuación, seleccione **Show pressure** (Mostrar presión).

    [![Instancia de Time Series seleccionada con el comando de menú para mostrar la presión media](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

    Aparece un gráfico con las series temporales. Cambie el **Intervalo** a **30s**.

    [![Gráfico de serie temporal](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Repita el paso 3 con las otras dos instancias de Time Series para ver los tres, tal y como se muestra en este gráfico:

    [![Gráfico de todas las series temporales](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Elija el selector de intervalo en la esquina superior derecha. Aquí puede seleccionar horas de inicio y finalización específicas hasta el milisegundo o elegir entre opciones preconfiguradas, como la última hora. También puede cambiar la zona horaria predeterminada.

    [![Establezca el intervalo de tiempo en una hora](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

    A continuación se muestra una captura de pantalla del panel de gráficos después de ejecutar la simulación durante una hora:

    [![Panel de gráficos](media/v2-update-provision/analyze-seven-time.png)](media/v2-update-provision/analyze-seven-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Definir y aplicar un modelo

En esta sección, aplicará un modelo para estructurar los datos. Para completar el modelo, definirá los tipos, las jerarquías y las instancias. Para más información sobre el modelado de datos, vea [Modelo de Time Series](./time-series-insights-update-tsm.md).

1. En el explorador, seleccione la pestaña **Modelo**:

   [![Pestaña Model (Modelo) en el explorador](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. En la pestaña **Tipos**, seleccione **Agregar**.

   [![El botón Add (Agregar) para tipos](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Escriba los siguientes parámetros:

    | Parámetro | . |
    | --- | ---|
    | **Nombre** | Escriba **Elevator**. |
    | **Descripción** | Escriba **This is a type definition for Elevator** (Esta es una definición de tipo de Elevator). |

    [![El botón Add (Agregar) para tipos](media/v2-update-provision/define-three-properties.png)](media/v2-update-provision/define-three-properties.png#lightbox)

1. A continuación, seleccione la pestaña **Variables**. [![Seleccionar la pestaña Variables](media/v2-update-provision/define-four-variables.png)](media/v2-update-provision/define-four-variables.png#lightbox)

1. Seleccione **+ Agregar variable** y rellene los siguientes valores para la primera variable del tipo Elevator. Creará tres variables en total.

    | Parámetro | . |
    | --- | --- |
    | **Nombre** | Escriba **Avg Temperature** (Temperatura media). |
    | **Variante** | Seleccione **Numérico**. |
    | **Valor** | Seleccione un valor preestablecido: Seleccione **temperatura (doble)** . <br /> Nota: Es posible que el campo de **valor** tarde unos minutos en rellenarse automáticamente después de que la versión preliminar de Azure Time Series Insights comience a recibir eventos.|
    | **Operación de agregación** | Expanda **Opciones avanzadas**. <br /> Seleccione **AVG** (MEDIA). |

    [![Selecciones para definir la temperatura](media/v2-update-provision/define-five-variable.png)](media/v2-update-provision/define-five-variable.png#lightbox)

    Seleccione **Aplicar**.

    Vuelva a seleccionar **+ Agregar variable** y establezca los valores siguientes:

    | Parámetro | . |
    | --- | --- |
    | **Nombre** | Escriba **Avg Vibration**. |
    | **Variante** | Seleccione **Numérico**. |
    | **Valor** | Seleccione un valor preestablecido: Seleccione **vibration (Double)** (vibración [doble]). <br /> Nota: Es posible que el campo de **valor** tarde unos minutos en rellenarse automáticamente después de que la versión preliminar de Azure Time Series Insights comience a recibir eventos.|
    | **Operación de agregación** | Expanda **Opciones avanzadas**. <br /> Seleccione **AVG** (MEDIA). |

    [![Selecciones para definir la vibración](media/v2-update-provision/define-five-vibration.png)](media/v2-update-provision/define-five-vibration.png#lightbox)

    Seleccione **Aplicar**.

    Vuelva a seleccionar **+ Agregar variable** y establezca los valores siguientes para la tercera y última variable:

    | Parámetro | . |
    | --- | --- |
    | **Nombre** | Escriba **Floor**. |
    | **Variante** | Seleccione **Categórico**. |
    | **Valor** | Seleccione un valor preestablecido: Seleccione **Floor (Double)** (Planta [doble]). <br /> Nota: Es posible que el campo de **valor** tarde unos minutos en rellenarse automáticamente después de que la versión preliminar de Azure Time Series Insights comience a recibir eventos.|
    | **Categorías** | <span style="text-decoration: underline">Etiqueta </span>  - <span style="text-decoration: underline">Valores</span> <br /> Inferior: 1,2,3,4 <br /> Central: 5,6,7,8,9 <br /> Superior: 10,11,12,13,14,15 |
    | **Categoría predeterminada** | Escriba **Desconocido**. |

    [![Selecciones para definir la vibración](media/v2-update-provision/define-five-floor.png)](media/v2-update-provision/define-five-floor.png#lightbox)

    Seleccione **Aplicar**. Debe ver que se hayan creado tres variables:

    [![Selecciones para definir la vibración](media/v2-update-provision/define-five-floor2.png)](media/v2-update-provision/define-five-floor2.png#lightbox)

   Seleccione **Guardar**. Verá el **Tipo** creado:

    [![Selecciones para definir la vibración](media/v2-update-provision/define-five-floor3.png)](media/v2-update-provision/define-five-floor3.png#lightbox)

1. Seleccione la pestaña **Jerarquías**. Seleccione **+Agregar**.

    [![Pestaña Hierarchies (Jerarquías) con el botón Add (Agregar)](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. En el panel **Edit Hierarchy** (Editar jerarquía), establezca los parámetros siguientes:

   | Parámetro | . |
   | --- | ---|
   | **Nombre** | Especifique el valor de **Location Hierarchy** (Jerarquía de ubicación). |
   |**Niveles**| Escriba **Country** como nombre del primer nivel. <br> Seleccione **+ Agregar nivel**. <br> Escriba **City** para el segundo nivel y, después, seleccione **+ Agregar nivel**. <br> Escriba **Building** como nombre del tercer y último nivel. |

   Seleccione **Guardar**.

    [![Campos de jerarquía con el botón Create (Crear)](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

   Puede ver la jerarquía que ha creado:

    [![Información sobre la jerarquía](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Vaya a **Instancias**. En **Acciones** en el extremo derecho, seleccione el icono de lápiz para editar la primera instancia con los valores siguientes:

    | Parámetro | . |
    | --- | --- |
    | **Tipo** | Seleccione **Elevator**. |
    | **Nombre** | Escriba **Elevator 1**.|
    | **Descripción** | Escriba **Instance for Elevator 1**. |

    [![Selección del botón Edit (Editar) para una instancia](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

    Vaya a **Campos de instancia** y escriba lo siguiente:

    | Parámetro | . |
    | --- | --- |
    | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
    | **País** | Escriba **USA**. |
    | **Ciudad** | Escriba **Seattle**. |
    | **Edificio** | Escriba **Space Needle**. |

    [![Selección del botón Edit (Editar) para una instancia](media/v2-update-provision/define-ten2-edit.png)](media/v2-update-provision/define-ten2-edit.png#lightbox)

    Seleccione **Guardar**.

1. Repita el paso 8 con las otras dos instancias con los siguientes valores:

    <span style="text-decoration: underline;">Para Elevator 2</span>:

    | Parámetro | . |
    | --- | --- |
    | **Tipo** | Seleccione **Elevator**. |
    | **Nombre** | Escriba **Elevator 2**.|
    | **Descripción** | Escriba **Instance for Elevator 2**. |
    | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
    | **País** | Escriba **USA**. |
    | **Ciudad** | Escriba **Seattle**. |
    | **Edificio** | Escriba **Pacific Science Center**. |

    <span style="text-decoration: underline;">Para Elevator 3</span>:

    | Parámetro | . |
    | --- | --- |
    | **Tipo** | Seleccione **Elevator**. |
    | **Nombre** | Escriba **Elevator 3**.|
    | **Descripción** | Escriba **Instance for Elevator 3**. |
    | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
    | **País** | Escriba **USA**. |
    | **Ciudad** | Escriba **New York**. |
    | **Edificio** | Escriba **Empire State Building**. |

1. Vuelva a la pestaña **Analizar** para ver el panel de gráficos. En **Location Hierarchy** (Jerarquía de ubicación), expanda todos los niveles de jerarquía para mostrar las instancias de la serie temporal:

   [![La pestaña Analyze (Analizar)](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. En **Pacific Science Center**, seleccione la instancia de Time Series **Elevator 2** y, a continuación, seleccione **Show Average Temperature** (Mostrar temperatura media).

    [![Instancia Show Average Temperature de Time Series](media/v2-update-provision/define-eleven-temperature.png)](media/v2-update-provision/define-eleven-temperature.png#lightbox)

1. En la misma instancia, **Elevator 2**, seleccione **Show Floor** (Mostrar planta).

    [![Instancia Show Floor de Time Series](media/v2-update-provision/define-twelve-floor.png)](media/v2-update-provision/define-twelve-floor.png#lightbox)

    Con la variable de categoría, puede determinar cuánto tiempo empleó el ascensor en el planta superior, inferior y central.

## <a name="clean-up-resources"></a>Limpieza de recursos

Ahora que ha completado el tutorial, limpie los recursos que ha creado:

1. En el menú de la izquierda de [Azure Portal](https://portal.azure.com), seleccione **Todos los recursos** y busque el grupo de recursos de Azure Time Series Insights.
1. Seleccione **Eliminar** para eliminar todo el grupo de recursos (y todos los recursos que contiene), o quite los recursos de uno en uno.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:  

* Crear y usar un acelerador de simulación de dispositivos.
* Crear un entorno de pago por uso de la versión preliminar de Azure Time Series Insights.
* Conecte el entorno de versión preliminar de Azure Time Series Insights a una instancia de IoT Hub.
* Ejecutar un ejemplo del acelerador de soluciones para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
* Realizar un análisis básico de los datos.
* Definir el tipo y la jerarquía de un modelo de Time Series y asociarlos con sus instancias.

Ahora que sabe cómo crear su propio entorno de versión preliminar de Azure Time Series Insights, obtenga más información sobre los conceptos clave de Azure Time Series Insights.

Lea sobre la configuración de almacenamiento de Azure Time Series Insights:

> [!div class="nextstepaction"]
> [Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)

Obtenga más información sobre los modelos de serie temporal:

> [!div class="nextstepaction"]
> [Modelado de datos de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-tsm.md)
