---
title: Configuración de una plantilla de dispositivo en una aplicación de Azure IoT Central | Microsoft Docs
description: Aprenda a configurar una plantilla de dispositivo con medidas, valores, propiedades, reglas y un panel.
author: viv-liu
ms.author: viviali
ms.date: 10/26/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 61bc9da45ac420e5683be1ea3ad253eae9c0ba5a
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158948"
---
# <a name="set-up-a-device-template"></a>Configuración de una plantilla de dispositivo

Una plantilla de dispositivo es un plano técnico que define las características y los comportamientos de un tipo de dispositivo que se conecta a una aplicación de Azure IoT Central.

Por ejemplo, un generador puede crear una plantilla de dispositivo para un ventilador conectado a IoT que tenga:

- Medida de telemetría de temperatura

- Medida de eventos de error del motor del ventilador

- Medida del estado de funcionamiento del ventilador

- Configuración de la velocidad del ventilador

- Propiedad de ubicación

- Reglas que envían alertas

- Panel que ofrece una vista global del dispositivo

A partir de esta plantilla de dispositivo, un operador puede crear y conectar dispositivos de ventilador reales con nombres, como **ventilador-1** y **ventilador-2**. Todos estos ventiladores tienen medidas, valores, propiedades, reglas y un panel que los usuarios de la aplicación pueden supervisar y administrar.

> [!NOTE]
> Solo los generadores y administradores pueden crear, editar y eliminar plantillas de dispositivo. Cualquier usuario puede crear dispositivos en la página **Device Explorer** a partir de las plantillas de dispositivo existentes.

## <a name="create-a-device-template"></a>Creación de una plantilla de dispositivo

1. Vaya a la página **Application Builder** (Generador de aplicaciones).

2. Para crear una plantilla en blanco, seleccione **Create Device Template** (Crear plantilla de dispositivo) y luego **Custom** (Personalizado).

3. Escriba un nombre (por ejemplo, Refrigerator-1) para la nueva plantilla de dispositivo y seleccione **Create** (Crear).

   ![Página de detalles del dispositivo con "Refrigerator" como nombre de plantilla](./media/howto-set-up-template/devicedetailspage.png)

4. Ahora está en la página **Detalles del dispositivo** de un nuevo dispositivo simulado. Al crear una plantilla de dispositivo se crea automáticamente un dispositivo simulado. Informa de los datos y se puede controlar como un dispositivo real.

Ahora examinemos cada una de las pestañas de la página **Detalles del dispositivo**.

## <a name="measurements"></a>Medidas

Las medidas son los datos que proceden del dispositivo. Puede agregar varias medidas a la plantilla de dispositivo para que coincidan con las funcionalidades de dicho dispositivo.

- Las medidas de tipo **Telemetría** son los puntos de datos numéricos que el dispositivo recopila a lo largo del tiempo. Se representan como un flujo de datos continuo. Un ejemplo es la temperatura.
- Las medidas de tipo **Evento** son los datos en un momento dado que representan algo significativo en el dispositivo. Un nivel de gravedad representa la importancia de un evento. Un ejemplo es un error del motor del ventilador.
- Las medidas de tipo **Estado** representan el estado del dispositivo o sus componentes durante un período de tiempo. Por ejemplo, se puede definir que el modo de ventilador tenga dos posibles estados: **Operating** (En funcionamiento) y **Stopped** (Detenido).

### <a name="create-a-telemetry-measurement"></a>Creación de una medida de tipo Telemetría
Para agregar una nueva medida de telemetría, seleccione **Editar plantilla** y haga clic en el botón **+ New Measurement** (Nueva medida). Seleccione **Telemetry** (Telemetría) como tipo de telemetría y escriba los detalles en el formulario **Create Telemetry** (Crear telemetría).

> [!NOTE]
> Los nombres de campo en la plantilla de dispositivo deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente para que la medida de telemetría se muestre en la aplicación cuando se conecte un dispositivo real. Haga lo mismo cuando configure los ajustes, las propiedades del dispositivo y los comandos mientras continúa definiendo la plantilla de dispositivo en las secciones siguientes.

Por ejemplo, puede agregar una nueva medida de telemetría de temperatura:
| Display Name (Nombre para mostrar)        | Nombre del campo    |  Unidades    | Min   |max|
| --------------------| ------------- |-----------|-------|---|
| Temperatura         | temp          |  degC     |  0    |100|

![Formulario "Create Telemetry" (Crear telemetría) con detalles de la medida de temperatura](./media/howto-set-up-template/measurementsform.png)

Después de seleccionar **Listo**, aparece la medida **Temperatura** en la lista de medidas. En breve, podrá ver la visualización de los datos de temperatura que ha generado el dispositivo simulado. Cuando se crea una plantilla de dispositivo, se genera un dispositivo simulado desde la plantilla que le permite probar el comportamiento de la aplicación antes de que se conecte un dispositivo físico o real.


> [!NOTE]
  El tipo de datos de la medida de telemetría es un número de punto flotante.

### <a name="create-an-event-measurement"></a>Creación de una medida de tipo Evento
Para agregar una nueva medida de evento, seleccione **Editar plantilla** y haga clic en el botón **+ New Measurement** (Nueva medida). Seleccione **Event** (Evento) como tipo de medida y escriba los detalles en el formulario **Create Event** (Crear evento).

Rellene los siguientes campos con los datos del evento: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Severity** (Gravedad). Puede elegir entre los tres niveles disponibles de gravedad: **Error**, **Warning** (Advertencia) e **Information** (Información).  

Por ejemplo, puede agregar un nuevo evento denominado **Fan Motor Error**.

| Display Name (Nombre para mostrar)        | Nombre del campo    |  Gravedad predeterminada | 
| --------------------| ------------- |-----------|
| Fan Motor Error     | fanmotorerror |  Error    | 

![Formulario "Create Evento" (Crear evento) formulario con detalles de un evento del motor del ventilador](./media/howto-set-up-template/eventmeasurementsform.png)

Después de seleccionar **Listo**, la medida **Fan Motor Error** aparece en la lista de medidas. Un operador puede ver los datos de evento que envía el dispositivo.

![Gráfico de medidas de evento](./media/howto-set-up-template/eventmeasurementschart.png)

Para ver más detalles sobre el evento, seleccione el icono de evento en el gráfico.

![Detalles del evento "Fan Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
  El tipo de datos de la medida de tipo Evento es cadena.

### <a name="create-a-state-measurement"></a>Creación de una medida de tipo Estado
Para agregar una nueva medida de estado, seleccione **Editar plantilla** y haga clic en el botón **+ New Measurement** (Nueva medida). Seleccione **State** (Estado) como tipo de medida y escriba los detalles en el formulario **Create State** (Crear estado).

Rellene los siguientes campos con los datos del estado: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Values** (Valores). Cada valor también puede tener un nombre para mostrar que se usará cuando el valor aparezca en gráficos y tablas.

Por ejemplo, puede agregar un nuevo estado denominado **Fan Mode** que tenga dos valores posibles que el dispositivo puede enviar: **Operating** (En funcionamiento) y **Stopped** (Detenido).


| Display Name (Nombre para mostrar) | Nombre del campo    |  Value 1   | Display Name (Nombre para mostrar) | Value 2    |Display Name (Nombre para mostrar)  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Fan Mode     | fanmode       |  1         | Operating    |     0      | Stopped      |

![Formulario de "Edit State" con los detalles del modo de ventilador](./media/howto-set-up-template/statemeasurementsform.png)

Después de seleccionar **Listo**, la medida **Fan Mode** aparece en la lista de medidas. El operador puede ver los datos de estado que envía el dispositivo.

![Gráfico de medidas de estado](./media/howto-set-up-template/statemeasurementschart.png)

Si el dispositivo envía demasiados puntos de datos en un intervalo pequeño, la medida de estado aparece se muestra con otro objeto visual diferente, como se muestra en la siguiente captura de pantalla. Si hace clic en el gráfico, se muestran todos los puntos de datos dentro de ese período de tiempo en orden cronológico. También puede reducir el intervalo de tiempo para ver la medida trazada en el gráfico.

> [!NOTE]
  El tipo de datos de la medida de tipo Estado es cadena.

## <a name="settings"></a>Configuración

La configuración controla un dispositivo. Permite a los operadores de la aplicación proporcionar entradas al dispositivo. Puede agregar varias configuraciones a la plantilla de dispositivo que aparecen como iconos en la pestaña **Configuración** para que los operadores los utilicen. Puede agregar muchos tipos de valores: número, texto, fecha, alternar, lista de selección y etiqueta de sección. 

La configuración puede tener uno de estos tres estados. El dispositivo notifica estos estados.

- **Synced** (Sincronizado): el dispositivo ha cambiado para reflejar el valor de la configuración.

- **Pending** (Pendiente): el dispositivo está actualmente cambiando al valor de la configuración.

- **Error**: el dispositivo ha devuelto un error.

Por ejemplo, para agregar una nueva configuración de velocidad de ventilador puede seleccionar **Editar plantilla** y escribir la nueva configuración **Number**:

| Display Name (Nombre para mostrar)  | Nombre del campo    |  Unidades  | Decimals |Inicial|
| --------------| ------------- |---------| ---------|---- |
| Fan Speed     | fanSpeed      | RPM     | 2        | 0   |

![Formulario "Configure Number" con detalles de los valores de velocidad](./media/howto-set-up-template/settingsform.png)

Después de seleccionar **Guardar**, la configuración **Fan speed** (Velocidad del ventilador) aparece como un icono y está lista para usarse para cambiar la velocidad del ventilador del dispositivo.

Después de crear un icono, seleccione **Done** (Listo) en la parte superior derecha de la pantalla. Una vez que el dispositivo real está conectado a la aplicación, el valor de configuración cambiará a sincronizado.

![Pestaña "Settings" (Configuración) con el conmutador "Design Mode" (Modo de diseño) del icono](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Properties (Propiedades)

Las propiedades son los metadatos de dispositivo asociados al dispositivo, como la ubicación y el número de serie de dicho dispositivo. Puede agregar varias propiedades a la plantilla de dispositivo que aparecen como iconos en la pestaña **Propiedades**. Puede agregar muchos tipos de propiedades: número, texto, fecha, alternar, propiedad de dispositivo, etiqueta y ubicación. Un operador puede especificar los valores de las propiedades cuando crea un dispositivo y puede editar estos valores en cualquier momento. Sin embargo, las propiedades del dispositivo son de solo lectura y se envían desde el dispositivo a la aplicación y el operador no puede cambiarlas. Cuando se conecta el dispositivo real, el icono de propiedad del dispositivo se actualizará en la aplicación. 

Existen dos categorías de propiedades:

- Propiedades del **dispositivo** que el dispositivo informa a la aplicación de IoT Central. Son los valores de solo lectura que informa el dispositivo y se actualizarán en la aplicación cuando el dispositivo real se conecte. 
- Propiedades de la **aplicación** que se almacenan exclusivamente en la aplicación y el operador puede editar. El dispositivo no reconoce las propiedades de la aplicación.

Por ejemplo, puede agregar la ubicación de dispositivos como una nueva propiedad **Text** (una propiedad de la aplicación) si selecciona **Editar plantilla** y escribe la nueva propiedad:

| Display Name (Nombre para mostrar)  | Nombre del campo | Quitar espacios iniciales  | Quitar espacios finales  | Distinción entre mayúsculas y minúsculas| Longitud mínima | Longitud máxima |
| --------------| -----------|---------| ---------|---- |----|----|
| Ubicación      | loc        | Off     |  Off     | mixta  | 0 | 100|

![Formulario "Configure Texto" (Configurar texto) de la pestaña "Properties" (Propiedades)](./media/howto-set-up-template/propertiesform.png)

Después de seleccionar **Save** (Guardar), la ubicación del dispositivo aparece como un icono:

![Icono de ubicación](./media/howto-set-up-template/propertiestile.png)

Después de crear el icono, puede cambiar el valor de propiedad de la aplicación. Primero, seleccione **Listo** en la parte superior derecha de la pantalla.

### <a name="create-a-location-property-through-azure-maps"></a>Creación de una propiedad de ubicación con Azure Maps
Puede dar contexto geográfico a los datos de ubicación en Azure IoT Central y asignar cualquier coordenada de latitud y de longitud de una dirección. O bien simplemente puede asignar las coordenadas de latitud y longitud. Azure Maps habilita esta funcionalidad en IoT Central.

Puede agregar dos tipos de propiedades de ubicación:
- **Ubicación como una propiedad de la aplicación**, que se almacena únicamente en la aplicación. El dispositivo no reconoce las propiedades de la aplicación.
- **Ubicación como una propiedad del dispositivo**, que el dispositivo notifica a la aplicación.

#### <a name="add-location-as-an-application-property"></a>Agregar ubicación como una propiedad de la aplicación 
Puede crear una propiedad de ubicación como una propiedad de la aplicación mediante Azure Maps en la aplicación IoT Central. Por ejemplo, puede agregar la dirección de la instalación de dispositivos. 

1. En la pestaña **Propiedades**, seleccione **Editar plantilla**.

   ![Pestaña "Properties" (Propiedades) con Design mode (Modo de diseño) activado](./media/howto-set-up-template/locationcloudproperty1.png)

2. En la biblioteca, seleccione **Location** (Ubicación).
3. Configure los valores de los campos **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y (opcionalmente) **Initial Value** (Valor inicial) de la ubicación. 

    | Display Name (Nombre para mostrar)  | Nombre del campo | Valor inicial |
    | --------------| -----------|---------| 
    | Dirección de la instalación | insta_address | Microsoft, 1 Microsoft Way, Redmond, WA 98052   |

   ![Formulario "Configure Location" (Configurar ubicación) con detalles de la ubicación](./media/howto-set-up-template/locationcloudproperty2.png)

   Se admiten dos formatos para agregar una ubicación:
   - **Ubicación como una dirección**
   - **Ubicación como coordenadas** 

4. Seleccione **Guardar** y **Listo**. Ahora, un operador puede actualizar el valor de ubicación en el formulario de campo de ubicación. 

#### <a name="add-location-as-a-device-property"></a>Agregar la ubicación como una propiedad del dispositivo 

Puede crear una propiedad de ubicación como una propiedad de dispositivo que el dispositivo notifica. Por ejemplo, si desea realizar un seguimiento de la ubicación del dispositivo:

1. En la pestaña **Propiedades**, seleccione **Editar plantilla**.

   ![Pestaña "Properties" (Propiedades) con Design mode (Modo de diseño) activado](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Seleccione **Device Property** (Propiedad del dispositivo) en la biblioteca.
3. Configure el nombre para mostrar y el nombre del campo, y seleccione **Location** (Ubicación) como tipo de datos. 

    | Display Name (Nombre para mostrar)  | Nombre del campo | Tipo de datos |
    | --------------| -----------|-----------| 
    | Ubicación del dispositivo | deviceLoc| location  |

   > [!NOTE]
   > Los nombres de campo deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente.

   ![Formulario "Configure Device Properties" (Configurar propiedades del dispositivo) con detalles de ubicación](./media/howto-set-up-template/locationdeviceproperty2.png)

Una vez que se conecta el dispositivo real, la ubicación que agregó como propiedad de dispositivo se actualizará con el valor que el dispositivo envió. La dirección de instalación, la ubicación que agregó como una propiedad de la aplicación, es un icono editable. Ahora que ha configurado la propiedad de ubicación, puede [agregar un mapa para visualizar la ubicación en el panel del dispositivo](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Comandos:

Los comandos se usan para administrar un dispositivo de forma remota. Permiten a los operadores de la aplicación ejecutar al instante comandos en el dispositivo. Puede agregar varios comandos a la plantilla de dispositivo que aparecen como iconos en la pestaña **Commands** (Comandos) para que los operadores los utilicen. Como generador del dispositivo, tiene la flexibilidad de definir comandasen función de sus requisitos.

¿En qué se diferencia un comando de un valor? 

* **Valor**: un valor es una configuración que desea aplicar a un dispositivo y desea que el dispositivo conserve dicha configuración hasta que la cambie. Por ejemplo, desea establecer la temperatura del congelador y desea dicho valor aun cuando se reinicie el congelador. 

* **Comando**: los comandos se usan para ejecutar al instante un comando en el dispositivo de forma remota desde IoT Central. Si un dispositivo no está conectado, se agota el tiempo de espera del comando y se produce un error. Por ejemplo, desea reiniciar un dispositivo.  


Por ejemplo, puede agregar un nuevo comando **Eco** si selecciona **Editar plantilla**, hace clic en **+ Nuevo comando** y escribe el nuevo comando:

| Display Name (Nombre para mostrar)  | Nombre del campo | Tiempo de espera predeterminado | Tipo de datos |
| --------------| -----------|---------------- | --------- | 
| Comando Echo  | echo       |  30             | text      |

![Formulario "Configure Command" (Configurar comando) con detalles del eco](./media/howto-set-up-template/commandsecho.png)

Después de seleccionar **Guardar** y **Listo**, el comando **Eco** aparece como un icono y está listo para usarse para mostrar el dispositivo una vez que se conecta el dispositivo real. Los nombres de campo del comando deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente para que los comandos se ejecuten correctamente.


## <a name="rules"></a>Reglas

Las reglas permiten a los operadores supervisar los dispositivos prácticamente en tiempo real. Las reglas invocan automáticamente acciones, como el envío de un correo electrónico cuando se desencadena la regla. Actualmente hay un solo tipo de regla disponible:

- **Regla de telemetría**, que se desencadena cuando la telemetría del dispositivo seleccionado supera un umbral especificado. [Más información acerca de las reglas de telemetría](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>panel

El panel es donde puede ir un operador para ver información sobre un dispositivo. Como generador, puede agregar iconos a esta página que ayuden a los operadores a saber cómo se comporta el dispositivo. Puede agregar varios iconos de panel a la plantilla de dispositivo. Puede agregar muchos tipos de iconos del panel, como imagen, gráfico de líneas, gráfico de barras, indicador clave de rendimiento (KPI), configuración y propiedades y etiqueta.

Por ejemplo, puede agregar un icono **Settings and Properties** (Configuración y propiedades) para mostrar una selección de los valores actuales de la configuración y las propiedades. Para ello, seleccione **Editar plantilla** y elija el icono de la biblioteca:

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Ahora, cuando un operador vea el panel, podrá ver este icono que muestra las propiedades y la configuración del dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Adición de una ubicación de Azure Maps al panel

Si ha configurado una propiedad de la ubicación anteriormente en [Creación de una propiedad de ubicación con Azure Maps](#create-a-location-property-through-azure-maps), puede visualizar la ubicación mediante un mapa en el panel del dispositivo.

1. En la pestaña **Panel**, seleccione **Editar plantilla**.

   ![Pestaña "Dashboard" (Panel) con Design mode (Modo de diseño) activado](./media/howto-set-up-template/locationcloudproperty4map.png)

2. En el panel del dispositivo, seleccione **Mapa** en la Biblioteca. 
3. Asígnele un título. El ejemplo siguiente tiene el título Ubicación de la instalación, luego elija la propiedad de ubicación que configuró anteriormente en la pestaña Propiedades. En el ejemplo siguiente, se selecciona **Dirección de la instalación**.

   ![Formulario "Configure Map" (Configurar mapa) con detalles del título y las propiedades](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Seleccione **Guardar**. El mosaico muestra la ubicación que ha seleccionado. 

   ![Mosaico con ubicación seleccionada](./media/howto-set-up-template/locationcloudproperty6map.png) 

Puede cambiar el tamaño del mapa hasta obtener el tamaño deseado. Cuando un operador vea el panel, puede ver todos los iconos del panel que ha configurado, incluido un mapa de ubicación.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar una plantilla de dispositivo en una aplicación de Azure IoT Central puede:

> [!div class="nextstepaction"]
> [Creación de una nueva versión de plantilla de dispositivo](howto-version-devicetemplate.md)
