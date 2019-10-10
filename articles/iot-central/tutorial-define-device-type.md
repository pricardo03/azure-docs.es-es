---
title: Definición de un nuevo tipo de dispositivo en Azure IoT Central | Microsoft Docs
description: Este tutorial le muestra, como desarrollador, cómo definir un nuevo tipo de dispositivo en la aplicación de Azure IoT Central. Se definen la telemetría, el estado, las propiedades y la configuración del tipo.
author: dominicbetts
ms.author: dobett
ms.date: 06/07/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: a3faa76c1506664a075648edc7d57fbba542b011
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960545"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Tutorial: Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Este tutorial le muestra, como desarrollador, cómo usar una plantilla de dispositivo para definir un nuevo tipo de dispositivo en la aplicación de Microsoft Azure IoT Central. Una plantilla de dispositivo define la telemetría, el estado, las propiedades y la configuración del tipo de dispositivo.

Para poder probar la aplicación antes de conectar un dispositivo real, IoT Central genera un dispositivo simulado a partir de la plantilla de dispositivo en el momento de la creación.

En este tutorial, creará una plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado). Un dispositivo acondicionador de aire conectado:

* Envía datos de telemetría, como la temperatura y la humedad.
* Indica su estado, como activado o desactivado.
* Tiene propiedades de dispositivo, como la versión de firmware y el número de serie.
* Tiene opciones de configuración como la temperatura de destino.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una nueva plantilla de dispositivo
> * Agregar telemetría al dispositivo
> * Ver datos de telemetría simulados
> * Definir la medida de eventos
> * Ver eventos simulados
> * Definir la medida de estado
> * Ver el estado simulado
> * Usar la configuración y las propiedades
> * Usar comandos
> * Ver el dispositivo simulado en el panel

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Si realizó la guía de inicio rápido [Creación de una aplicación de Azure IoT Central](quick-deploy-iot-central.md), puede volver a usar la aplicación que creó en dicha guía. En caso contrario, complete los pasos siguientes para crear una aplicación de Azure IoT Central vacía:

1. Vaya al sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral).

2. Escriba la dirección de correo electrónico y la contraseña que usa para acceder a la suscripción de Azure:

    ![Incorporación de la cuenta de la organización](./media/tutorial-define-device-type/sign-in.png)

3. Para empezar a crear una aplicación de Azure IoT Central, seleccione **New Application** (Nueva aplicación):

    ![Página del administrador de aplicaciones de Azure IoT Central](./media/tutorial-define-device-type/iotcentralhome.png)

4. Para crear una nueva aplicación de Azure IoT Central:
    
   * Elija **Versión de evaluación**. No necesita una suscripción de Azure para crear una aplicación de evaluación.
    
      Para más información acerca de los directorios y las suscripciones, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
    
   * Elija **Custom Application** (Aplicación personalizada).
    
   * Puede elegir un nombre de aplicación descriptivo, como **Contoso Air Conditioners**. Azure IoT Central genera un prefijo de dirección URL único. Puede cambiar este prefijo de dirección URL por algo más fácil de recordar.
    
   * Seleccione **Crear**.

     ![Página de creación de una aplicación de Azure IoT Central](./media/tutorial-define-device-type/iotcentralcreate.png)

     Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).

## <a name="create-a-device-template"></a>Creación de una plantilla de dispositivo

Como desarrollador, puede crear y editar las plantillas de dispositivo de la aplicación. Cuando se crea una plantilla de dispositivo, Azure IoT Central genera un dispositivo simulado a partir de la plantilla. El dispositivo simulado genera datos de telemetría que le permiten probar el comportamiento de la aplicación antes de conectar un dispositivo real.

Para agregar una nueva plantilla de dispositivo a la aplicación, debe ir a la página **Device Templates** (Plantillas de dispositivo). Para ello, seleccione **Device Templates** (Plantillas de dispositivo) en el menú de navegación izquierdo.

![Página Plantillas de dispositivo](./media/tutorial-define-device-type/devicetemplates.png)

## <a name="add-a-device-template"></a>Incorporación de una plantilla de dispositivo

Los pasos siguientes muestran cómo crear una nueva plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado) para los dispositivos que envían datos de telemetría de temperatura a la aplicación:

1. En la página **Device Templates** (Plantillas de dispositivo), seleccione **+ New** (+ Nuevo):

    ![Página Plantillas de dispositivo, crear plantilla de dispositivo](./media/tutorial-define-device-type/newtemplate.png)

2. La página muestra las plantillas que puede elegir.

    ![Biblioteca de plantillas de dispositivo](./media/tutorial-define-device-type/devicetemplatelibrary.png)

3. Seleccione **Custom** (Personalizada ), escriba **Connected Air Conditioner** (Aire acondicionado conectado) como nombre de la plantilla del dispositivo y seleccione **Create** (Crear). También puede cargar una imagen del dispositivo que estará visible para los operadores en el explorador de dispositivos:

    ![Dispositivo personalizado](./media/tutorial-define-device-type/createcustomdevice.png)

4. En la plantilla del dispositivo **Connected Air Conditioner**, asegúrese de que se encuentra en la pestaña **Medidas**, donde se define la telemetría. Cada plantilla de dispositivo que se define tiene pestañas independientes para que pueda:

   * Especificar las _medidas_, como la telemetría, el evento y el estado, enviadas por el dispositivo.

   * Definir los _valores_ que se usan para controlar el dispositivo.

   * Definir las _propiedades_ que son los metadatos del dispositivo.

   * Definir los _comandos_ que se ejecutarán directamente en el dispositivo.

   * Definir las _reglas_ asociadas con el dispositivo.

   * Personalizar el _panel_ del dispositivo de los operadores.

     ![Medidas del acondicionador de aire](./media/tutorial-define-device-type/airconmeasurements.png)

     > [!NOTE]
     > Para cambiar el nombre de la plantilla de dispositivo, selecciónelo en la parte superior de la página.

5. Para agregar la medida de telemetría de temperatura, seleccione **+ New Measurement** (+ Nueva medida). A continuación, elija **Telemetry** (Telemetría) como el tipo de medida:

    ![Medidas del dispositivo Aire acondicionado conectado](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Cada tipo de telemetría que define para una plantilla de dispositivo incluye [opciones de configuración](howto-set-up-template.md) como:

   * Opciones de visualización.

   * Detalles de telemetría.

   * Parámetros de simulación.

     Para configurar la telemetría **Temperature** (Temperatura), use la información de la tabla siguiente:

     | Configuración              | Valor         |
     | -------------------- | -----------   |
     | Display Name (Nombre para mostrar)         | Temperatura   |
     | Nombre del campo           | temperatura   |
     | Unidades                | F             |
     | Min                  | 60            |
     | max                  | 110           |
     | Posiciones decimales       | 0             |

     También puede elegir un color para la presentación de la telemetría. Para guardar la definición de la telemetría, seleccione **Save** (Guardar):

     ![Configurar la simulación de temperatura](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Poco después, la pestaña **Medidas** muestra un gráfico de la telemetría de temperatura del dispositivo acondicionador de aire conectado simulado. Use los controles para administrar la visibilidad, la agregación o para editar la definición de telemetría:
 
    > [!NOTE]
    > En el caso de la telemetría, **Average** (promedio) se establece como la agregación predeterminada. 

    ![Ver la simulación de temperatura](./media/tutorial-define-device-type/viewsimulation.png)

8. También puede personalizar el gráfico mediante los controles **Line** (Línea), **Stacked** (Apilado) y **Edit Time Range** (Editar intervalo de tiempo):

    ![Personalizar el gráfico](./media/tutorial-define-device-type/customizechart.png)

## <a name="add-an-event-measurement"></a>Incorporación de una medida de tipo Evento

Los eventos se usan para definir los datos en un momento dado que el dispositivo envía cuando se produce un evento, como un error o el error de algún componente. Azure IoT Central puede simular eventos de dispositivo que permitan probar el comportamiento de una aplicación antes de conectarla un dispositivo real. Las medidas de eventos del tipo de dispositivo se definen en la vista **Medidas**.

1. Para agregar la medida del evento **Fan Motor Error** (Error del motor del ventilador), elija **+ New Measurement** (+ Nueva medida). A continuación, elija **Event** (Evento) como el tipo de medida:

    ![Medidas del dispositivo Aire acondicionado conectado](./media/tutorial-define-device-type/eventnew.png)

2. Cada tipo de evento que define para una plantilla de dispositivo incluye [opciones de configuración](howto-set-up-template.md) como:

   * Nombre para mostrar.

   * Nombre del campo.

   * Gravedad.

     Para configurar el evento **Fan Motor Error** (Error del motor del ventilador), use la información de la tabla siguiente:

     | Configuración              | Valor             |
     | -------------------- | -----------       |
     | Display Name (Nombre para mostrar)         | Fan Motor Error   |
     | Nombre del campo           | fanmotorerr       |
     | severity             | Error             |

     Para guardar la definición del evento, haga clic en **Save** (Guardar):

     ![Configurar la medida de eventos](./media/tutorial-define-device-type/eventconfiguration.png)

3. Poco después, la pestaña **Medidas** muestra un gráfico de los eventos generados aleatoriamente del dispositivo acondicionador de aire conectado simulado. Use los controles para administrar la visibilidad o para editar la definición del evento:

    ![Simulación de la visualización del evento](./media/tutorial-define-device-type/eventview.png)

1. Para ver más detalles del evento, selecciónelo en el gráfico:

    ![Ver detalles del evento](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-a-state-measurement"></a>Definición de una medida de tipo Estado

Estado se puede usar para definir y visualizar el estado del dispositivo o de su componente durante un período de tiempo. Azure IoT Central puede simular el estado del dispositivo, lo que permite probar el comportamiento de una aplicación antes de conectarla un dispositivo real. Puede definir medidas de estado para el tipo de dispositivo en la vista **Measurements** (Medidas).

1. Para agregar un medida del estado **Fan Mode** (Modo ventilador), seleccione **+ New Measurement** (+ Nueva medida). A continuación, elija **State** (Estado) como el tipo de medida:

    ![Medidas de estado del dispositivo Aire acondicionado conectado](./media/tutorial-define-device-type/statenew.png)

2. Cada tipo de estado que se define para una plantilla de dispositivo incluye [opciones de configuración](howto-set-up-template.md) como:

   * Nombre para mostrar.

   * Nombre del campo.

   * Valores con etiquetas de presentación opcionales.

   * Color para cada valor.

     Para configurar el estado **Fan Mode** (Modo del ventilador), use la información de la tabla siguiente:

     | Configuración              | Valor             |
     | -------------------- | -----------       |
     | Display Name (Nombre para mostrar)         | Fan Mode          |
     | Nombre del campo           | fanmode           |
     | Valor                | 1                 |
     | Etiqueta para mostrar        | Operating         |
     | Valor                | 0                 |
     | Etiqueta para mostrar        | Detenido           |

     Para guardar la definición de la medida de estado, seleccione **Save** (Guardar):

     ![Configurar la medida de estado](./media/tutorial-define-device-type/stateconfiguration.png)

3. Poco después, la pestaña **Medidas** muestra un gráfico de los estados generados aleatoriamente del dispositivo acondicionador de aire conectado simulado. Use los controles para administrar la visibilidad o para editar la definición del estado:

    ![Ver simulación del estado](./media/tutorial-define-device-type/stateview.png)

4. Si el dispositivo ha enviado demasiados puntos de datos en un intervalo pequeño, la medida del estado se muestra con un otro objeto visual. Seleccione el gráfico para ver todos los puntos de datos de ese período en orden cronológico. También puede reducir el intervalo de tiempo para ver las medidas con más detalle.

## <a name="settings-properties-and-commands"></a>Configuración, propiedades y comandos

La configuración, las propiedades y los comandos son los diferentes valores definidos en las plantillas de dispositivo que se asocian a cada dispositivo individual:

* La _configuración_ se usa para enviar datos de configuración a un dispositivo desde la aplicación. Por ejemplo, un operador podría utilizar una configuración para cambiar el intervalo de telemetría del dispositivo de dos segundos a cinco segundos. Cuando un operador cambia un valor, la configuración se marca como pendiente en la interfaz de usuario hasta que el dispositivo responde con una confirmación.

* Use las _propiedades_ para definir los metadatos asociados al dispositivo. Existen dos categorías de propiedades:
    
  * Las _propiedades de la aplicación_ se usan para registrar información sobre el dispositivo en la aplicación. Por ejemplo, puede usarlas para registrar la ubicación de un dispositivo y la fecha de la última revisión. Dichas propiedades se almacenan en la aplicación y no se sincronizan con el dispositivo. Un operador puede asignar valores a las propiedades.

  * Las _propiedades del dispositivo_ se usan para permitir a un dispositivo enviar valores de propiedad a la aplicación. Solo el dispositivo puede actualizar estas propiedades. Para un operador, las propiedades del dispositivo son de solo lectura. En este escenario de una máquina de aire acondicionado conectada, el número de serie del dispositivo y el de versión de firmware son propiedades del dispositivo que este notifica.
    
    Para más información, consulte [Propiedades](howto-set-up-template.md#properties) en la guía paso a paso para la configuración de plantillas de dispositivo.

* Use _comandos_ para administrar de forma remota su dispositivo desde la aplicación. Puede ejecutar directamente los comandos en el dispositivo desde la nube para controlar los dispositivos. Por ejemplo, un operador puede ejecutar comandos como el de reinicio para reiniciar al instante el dispositivo.

## <a name="use-settings"></a>Usar la configuración

La *configuración* se usa para permitir a un operador enviar datos de configuración a un dispositivo. En esta sección, agregará una configuración para la plantilla de dispositivo **Connected Air Conditioner** que permite a un operador establecer la temperatura de destino del acondicionador de aire conectado.

1. Vaya la pestaña **Configuración** de la plantilla del dispositivo **Connected Air Conditioner**.

2. Puede crear una configuración de tipos diferentes, como números o texto. Elija **Number** (Número) para agregar un valor numérico al dispositivo.

3. Para configurar la configuración **Set Temperature** (Establecer temperatura), use la información de la tabla siguiente:

    | Campo                | Valor           |
    | -------------------- | -----------     |
    | Display Name (Nombre para mostrar)         | Set Temperature |
    | Nombre del campo           | setTemperature  |
    | Unidad de medida      | F               |
    | Decimal Places       | 1               |
    | Valor mínimo        | 20              |
    | Valor máximo        | 200             |
    | Valor inicial        | 80              |
    | DESCRIPCIÓN          | Establezca la temperatura de destino del acondicionador de aire |

    Después, seleccione **Save** (Guardar):

    ![Configurar la opción Set Temperature (Establecer temperatura)](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Cuando el dispositivo confirma el cambio de configuración, el estado de la configuración se cambia a **synced** (sincronizado).

4. Para personalizar el diseño de la pestaña **Configuración**, mueva los iconos de configuración y cambie su tamaño:

    ![Personalizar el diseño de la configuración](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Uso de las propiedades

Las *propiedades de la aplicación* se usan para almacenar información sobre el dispositivo en la aplicación. En esta sección se agregan propiedades de la aplicación a la plantilla de dispositivo **Connected Air Conditioner** para almacenar la ubicación del dispositivo y la fecha de la última revisión. Estas propiedades se pueden editar en la aplicación. El dispositivo también informa de propiedades como el número de serie y la versión del firmware, que son de solo lectura en la aplicación.

1. Vaya la pestaña **Propiedades** de la plantilla del dispositivo **Connected Air Conditioner**.

1. Puede crear propiedades de dispositivo de tipos diferentes, como números o texto. Para agregar una propiedad de ubicación a la plantilla de dispositivo, elija **Location**. Para configurar la propiedad de ubicación, use la información de la tabla siguiente:

    | Campo                | Valor                |
    | -------------------- | -------------------- |
    | Display Name (Nombre para mostrar)         | Location             |
    | Nombre del campo           | location             |
    | Valor inicial        | Seattle, WA          |
    | DESCRIPCIÓN          | Ubicación del dispositivo      |

    Deje los otros campos con sus valores predeterminados.

    ![Configurar las propiedades del dispositivo](./media/tutorial-define-device-type/configureproperties.png)

    Seleccione **Guardar**.

1. Para agregar una propiedad de fecha de la última revisión a la plantilla de dispositivo, elija **Date**.

1. Para configurar la propiedad de fecha de la última revisión, use la información de la tabla siguiente:

    | Campo                | Valor                   |
    | -------------------- | ----------------------- |
    | Display Name (Nombre para mostrar)         | Fecha de la última revisión       |
    | Nombre del campo           | serviceDate             |
    | Valor inicial        | 1/1/2019                |
    | DESCRIPCIÓN          | Última revisión           |

    ![Configurar las propiedades del dispositivo](./media/tutorial-define-device-type/configureproperties2.png)

    Seleccione **Guardar**.

1. Para personalizar el diseño de la pestaña **Propiedades**, mueva los iconos de propiedades y cambie su tamaño.

1. Para agregar una propiedad del dispositivo, como la versión de firmware, a la plantilla de dispositivo, elija **Device Property** (Propiedad del dispositivo).

1. Para configurar la versión de firmware, use la información de la siguiente tabla:

    | Campo                | Valor                   |
    | -------------------- | ----------------------- |
    | Display Name (Nombre para mostrar)         | Firmware version        |
    | Nombre del campo           | firmwareVersion         |
    | Tipo de datos            | text                    |
    | DESCRIPCIÓN          | Versión de firmware de la máquina de aire acondicionado |

    ![Configuración de la versión de firmware](./media/tutorial-define-device-type/configureproperties3.png)

    Seleccione **Guardar**.

1. Para agregar una propiedad del dispositivo, como el número de serie, a la plantilla de dispositivo, elija **Device Property** (Propiedad del dispositivo).

1. Para configurar el número de serie, use la información de la siguiente tabla:

    | Campo                | Valor                   |
    | -------------------- | ----------------------- |
    | Display Name (Nombre para mostrar)         | Serial number           |
    | Nombre del campo           | serialNumber            |
    | Tipo de datos            | text                    |
    | DESCRIPCIÓN          | Número de serie de la máquina de aire acondicionado  |

    ![Configuración del número de serie](./media/tutorial-define-device-type/configureproperties4.png)

    Seleccione **Guardar**.

    > [!NOTE]
    > La propiedad del dispositivo se envía del dispositivo a la aplicación. Los valores de versión de firmware y número de serie se actualizarán cuando el dispositivo real se conecte a IoT Central.

## <a name="use-commands"></a>Usar comandos

Los _comandos_ se usan para permitir que un operador ejecute comandos directamente en el dispositivo. En esta sección se agrega un comando a la plantilla de dispositivo **Connected Air Conditioner** que permite al operador difundir determinado mensaje de eco en la máquina de aire acondicionado conectada.

1. Vaya a la pestaña **Comandos** de la plantilla del dispositivo **Connected Air Conditioner** para editar la plantilla.

1. Seleccione **+ New Command** (+ Nuevo comando) para agregar un comando al dispositivo y empezar a configurarlo.

1. Para configurar el nuevo comando, use la información de la tabla siguiente:

    | Campo                | Valor           |
    | -------------------- | -----------     |
    | Display Name (Nombre para mostrar)         | Comando Echo    |
    | Nombre del campo           | echo            |
    | Tiempo de espera predeterminado      | 30              |
    | Tipo de datos         | text            |
    | DESCRIPCIÓN          | Comando de dispositivo  |  

    Para agregar entradas adicionales al comando, seleccione **+** en **Input Fields** (Campos de entrada).

    ![Prepararse para agregar una configuración](./media/tutorial-define-device-type/commandsecho1.png)

     Seleccione **Guardar**.

1. Para personalizar el diseño de la pestaña **Comandos**, mueva los iconos de configuración y cambie su tamaño.

## <a name="view-your-simulated-device"></a>Visualización del dispositivo simulado

Tras definir la plantilla de dispositivo **Connected Air Conditioner**, puede personalizar su **panel** para incluir las medidas, la configuración y las propiedades que ha definido. A continuación, puede obtener una vista previa del panel como un operador:

1. Vaya a la pestaña **Panel** de la plantilla del dispositivo **Connected Air Conditioner**.

1. Elija **Line Chart** (Gráfico de líneas) para agregar el componente al **panel**.

1. Configure el componente **Line Chart** (Gráfico de líneas) según se indica en la tabla siguiente:

    | Configuración      | Valor       |
    | ------------ | ----------- |
    | Título        | Temperatura |
    | Intervalo de tiempo   | Los últimos 30 minutos |
    | Medidas     | Temperature (Temperatura) (seleccione **Visibility** [Visibilidad] junto a **Temperature**[Temperatura]) |

    ![Configuración del gráfico de línea](./media/tutorial-define-device-type/linechartsettings.png)

    Después, seleccione **Guardar**.

1. Seleccione el componente **Event History** (Historial de eventos) y utilice la información de la siguiente tabla:

    | Configuración      | Valor       |
    | ------------ | ----------- |
    | Título        | Fan Motor Events (Eventos de motor del ventilador) |
    | Intervalo de tiempo   | Los últimos 30 minutos |
    | Medidas     | Fan Motor Error (Error de motor de ventilador) (haga clic en **Visibility** [Visibilidad] junto a **Fan Motor Error** [Error de motor de ventilador]) |

    ![Configuración del gráfico del evento](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Después, seleccione **Guardar**.

1. Configure el componente **State History** (Historial de estados) según se indica en la siguiente tabla:

    | Configuración      | Valor       |
    | ------------ | ----------- |
    | Título        | Fan Mode |
    | Intervalo de tiempo   | Los últimos 30 minutos |
    | Medidas | Fan Mode (Modo ventilador) (elija **Visibility** [Visibilidad] junto a **Fan Mode** [Modo ventilador]) |

    ![Configuración del gráfico de línea](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Después, seleccione **Guardar**.

1. Para agregar la configuración y las propiedades del dispositivo al panel, elija **Propiedades y configuración**. Seleccione **Add/Remove** (Agregar o eliminar) para agregar la configuración o las propiedades que le gustaría ver en el panel.

1. Configure el componente **Settings and Properties** (Configuración y propiedades) según se indica en la tabla siguiente:

    | Configuración                 | Valor         |
    | ----------------------- | ------------- |
    | Título                   | Propiedades del dispositivo |
    | Configuración y propiedades | Set Temperature<br/>Serial number<br/>Firmware version |

    La configuración y las propiedades que ha definido previamente en **Propiedades y configuración** se muestran en **Columnas disponibles**.

    ![Configuración de la propiedad Set temperature](./media/tutorial-define-device-type/propertysettings4.png)

    Después, seleccione **Guardar**.

1. Ya puede ver los datos simulados de Connected Air Conditioner en el panel. Los iconos y el diseño del panel se pueden editar:

    ![Visualización del panel](./media/tutorial-define-device-type/dashboard.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Creación de una nueva plantilla de dispositivo
> * Agregar telemetría al dispositivo
> * Ver datos de telemetría simulados
> * Definir los eventos del dispositivo
> * Ver eventos simulados
> * Definir el estado
> * Ver el estado simulado
> * Usar la configuración y las propiedades
> * Usar comandos
> * Ver el dispositivo simulado en el panel

Ahora que ha definido una plantilla de dispositivo en la aplicación de Azure IoT Central, estos son los siguientes pasos que se sugieren:

* [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules.md)
* [Personalización de la vista del operador](tutorial-customize-operator.md)