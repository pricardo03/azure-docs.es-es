---
title: Configuración de una plantilla de dispositivo en una aplicación de Azure IoT Central | Microsoft Docs
description: Aprenda a configurar una plantilla de dispositivo con medidas, valores, propiedades, reglas y un panel.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: f71d4c7dc94fedfd598ab87c51366ba9fb1f184a
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063042"
---
# <a name="set-up-a-device-template"></a>Configuración de una plantilla de dispositivo

Una plantilla de dispositivo es un plano técnico que define las características y los comportamientos de un tipo de dispositivo que se conecta a una aplicación de Microsoft Azure IoT Central.

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

3. Escriba el nombre de la nueva plantilla de dispositivo y seleccione **Create** (Crear).

   ![Página de detalles del dispositivo con "Refrigerator" como nombre de plantilla](./media/howto-set-up-template/devicedetailspage.png)

4. Ahora está en la página **Detalles del dispositivo** de un nuevo dispositivo simulado. Al crear una plantilla de dispositivo se crea automáticamente un dispositivo simulado. Informa de los datos y se puede controlar como un dispositivo real.

Ahora examinemos cada una de las pestañas de la página **Detalles del dispositivo**.

## <a name="measurements"></a>Medidas

Las medidas son los datos que proceden del dispositivo. Puede agregar varias medidas a la plantilla de dispositivo para que coincidan con las funcionalidades de dicho dispositivo.

- Las medidas de tipo **Telemetría** son los puntos de datos numéricos que el dispositivo recopila a lo largo del tiempo. Se representan como un flujo de datos continuo. Un ejemplo es la temperatura.
- Las medidas de tipo **Evento** son los datos en un momento dado que representan algo significativo en el dispositivo. Un nivel de gravedad representa la importancia de un evento. Un ejemplo es un error del motor del ventilador.
- Las medidas de tipo **Estado** representan el estado del dispositivo o sus componentes durante un período de tiempo. Por ejemplo, se puede definir que el modo de ventilador tenga dos posibles estados: **Operating** (En funcionamiento) y **Stopped** (Detenido).

### <a name="create-a-telemetry-measurement"></a>Creación de una medida de tipo Telemetría
Para agregar una nueva medida de tipo Telemetría, seleccione el botón **+ New Measurement** (Nueva medida). Seleccione **Telemetry** (Telemetría) como tipo de telemetría y escriba los detalles en el formulario **Create Telemetry** (Crear telemetría).

> [!NOTE]
> Cuando se conecte un dispositivo real, preste atención al nombre de la medida que el dispositivo notifica. Dicho nombre debe coincidir exactamente con la entrada de **Nombre de campo** de una medida.

Por ejemplo, puede agregar una nueva medida de telemetría de temperatura:

![Formulario "Create Telemetry" (Crear telemetría) con detalles de la medida de temperatura](./media/howto-set-up-template/measurementsform.png)

Después de seleccionar **Save** (Guardar), le medida **Temperature** aparece en la lista de medidas. Un operador puede ver los datos de temperatura que recopila el dispositivo.

![Gráfico de medidas](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Creación de una medida de tipo Evento
Para agregar una nueva medida de tipo Evento, seleccione el botón **+ New Measurement** (Nueva medida). Seleccione **Event** (Evento) como tipo de medida y escriba los detalles en el formulario **Create Event** (Crear evento).

Rellene los siguientes campos con los datos del evento: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Severity** (Gravedad). Puede elegir entre los tres niveles disponibles de gravedad: **Error**, **Warning** (Advertencia) e **Information** (Información).  

Por ejemplo, puede agregar un nuevo evento denominado **Fan Motor Error**.

![Formulario "Create Evento" (Crear evento) formulario con detalles de un evento del motor del ventilador](./media/howto-set-up-template/eventmeasurementsform.png)

Después de seleccionar **Save** (Guardar), le medida **Fan Motor Error** aparece en la lista de medidas. Un operador puede ver los datos de evento que envía el dispositivo.

![Gráfico de medidas de evento](./media/howto-set-up-template/eventmeasurementschart.png)

Para ver más detalles acerca del evento, seleccione el icono de evento en el gráfico:

![Detalles del evento "Fan Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Creación de una medida de tipo Estado
Para agregar una nueva medida de tipo Estado, seleccione el botón **+ New Measurement** (Nueva medida). Seleccione **State** (Estado) como tipo de medida y escriba los detalles en el formulario **Create State** (Crear estado).

Rellene los siguientes campos con los datos del estado: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Values** (Valores). Cada valor también puede tener un nombre para mostrar que se usará cuando el valor aparezca en gráficos y tablas.

Por ejemplo, puede agregar un nuevo estado denominado **Fan Mode** que tenga dos valores posibles que el dispositivo puede enviar: **Operating** (En funcionamiento) y **Stopped** (Detenido).

![Formulario de "Edit State" con los detalles del modo de ventilador](./media/howto-set-up-template/statemeasurementsform.png)

Después de seleccionar **Save** (Guardar), le medida del estado **Fan Mode** aparece en la lista de medidas. El operador puede ver los datos de estado que envía el dispositivo.

![Gráfico de medidas de estado](./media/howto-set-up-template/statemeasurementschart.png)

Si el dispositivo envía demasiados puntos de datos en un intervalo pequeño, la medida de estado aparece se muestra con otro objeto visual diferente, como se muestra en la siguiente captura de pantalla. Si hace clic en el gráfico, se muestran todos los puntos de datos dentro de ese período de tiempo en orden cronológico. También puede reducir el intervalo de tiempo para ver la medida trazada en el gráfico.

![Detalles de la medida del estado "Static Fan Mode"](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Settings

La configuración controla un dispositivo. Permite a los operadores de la aplicación proporcionar entradas al dispositivo. Puede agregar varias configuraciones a la plantilla de dispositivo que aparecen como iconos en la pestaña **Configuración** para que los operadores los utilicen. Puede agregar seis tipos de valores: número, texto, fecha, alternar, lista de selección y etiqueta de sección.

> [!NOTE]
> Cuando se conecte un dispositivo real, preste atención al nombre del valor que dicho dispositivo notifica. El nombre debe coincidir exactamente con la entrada de **Nombre de campo** de un valor.

La configuración puede tener uno de estos tres estados. El dispositivo notifica estos estados.

- **Synced** (Sincronizado): el dispositivo ha cambiado para reflejar el valor de la configuración.

- **Pending** (Pendiente): el dispositivo está actualmente cambiando al valor de la configuración.

- **Error**: el dispositivo ha devuelto un error.

Por ejemplo, puede agregar una nueva configuración de velocidad del ventilador:

![Formulario "Configure Number" con detalles de los valores de velocidad](./media/howto-set-up-template/settingsform.png)

Después de seleccionar **Guardar**, la configuración **Fan speed** (Velocidad del ventilador) aparece como un icono y está lista para usarse para cambiar la velocidad del ventilador del dispositivo.

Después de crear un icono, puede probar la nueva configuración. En primer lugar, desactive el modo de diseño en la parte superior derecha de la pantalla.

![Pestaña "Settings" (Configuración) con el conmutador "Design Mode" (Modo de diseño) del icono](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Properties (Propiedades)

Las propiedades son los metadatos de dispositivo asociados al dispositivo, como la ubicación y el número de serie de dicho dispositivo. Puede agregar varias propiedades a la plantilla de dispositivo que aparecen como iconos en la pestaña **Propiedades**. Un operador puede especificar los valores de las propiedades cuando crea un dispositivo y puede editar estos valores en cualquier momento. Puede agregar seis tipos de propiedades: número, texto, fecha, alternar, propiedad de dispositivo y etiqueta.

Existen dos categorías de propiedades:

- Propiedades del **dispositivo** que notifica el dispositivo.
- Propiedades de la **aplicación** que se almacenan exclusivamente en la aplicación. El dispositivo no reconoce las propiedades de la aplicación.

> [!NOTE]
> En lo que a las propiedades de dispositivo se refiere, cuando haya conectado un dispositivo real, preste atención al nombre de la propiedad que el dispositivo notifica. El nombre debe coincidir exactamente con la entrada de **Field Name** (Nombre de campo) de la propiedad. En lo que a las propiedades de aplicación se refiere, el nombre del campo puede ser cualquier cosa que desee, siempre que el nombre sea único en la plantilla de dispositivo.

Por ejemplo, puede agregar la ubicación del dispositivo como una nueva propiedad:

![Formulario "Configure Texto" (Configurar texto) de la pestaña "Properties" (Propiedades)](./media/howto-set-up-template/propertiesform.png)

Después de seleccionar **Save** (Guardar), la ubicación del dispositivo aparece como un icono:

![Icono de ubicación](./media/howto-set-up-template/propertiestile.png)

Después de crear un icono, puede cambiar el valor de propiedad. En primer lugar, desactive el modo de diseño en la parte superior derecha de la pantalla.

### <a name="create-a-location-property-through-azure-maps"></a>Creación de una propiedad de ubicación con Azure Maps
Puede dar contexto geográfico a los datos de ubicación en Azure IoT Central y asignar cualquier coordenada de latitud y de longitud de una dirección. O bien simplemente puede asignar las coordenadas de latitud y longitud. Azure Maps habilita esta funcionalidad en IoT Central.

Puede agregar dos tipos de propiedades de ubicación:
- **Ubicación como una propiedad de la aplicación**, que se almacena únicamente en la aplicación. El dispositivo no reconoce las propiedades de la aplicación.
- **Ubicación como una propiedad del dispositivo**, que el dispositivo notifica.

#### <a name="add-location-as-an-application-property"></a>Agregar ubicación como una propiedad de la aplicación 
Puede crear una propiedad de ubicación como una propiedad de la aplicación mediante Azure Maps en la aplicación IoT Central. Por ejemplo, puede agregar la dirección de la instalación de dispositivos. 

1. En la pestaña **Properties** (Propiedades), asegúrese de que **Design Mode** (Modo de diseño) está en **On** (Activado).

   ![Pestaña "Properties" (Propiedades) con Design mode (Modo de diseño) activado](./media/howto-set-up-template/locationcloudproperty1.png)

2. En la biblioteca, seleccione **Location** (Ubicación).
3. Configure los valores de los campos **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y (opcionalmente) **Initial Value** (Valor inicial) de la ubicación. 

   ![Formulario "Configure Location" (Configurar ubicación) con detalles de la ubicación](./media/howto-set-up-template/locationcloudproperty2.png)

   Se admiten dos formatos para agregar una ubicación:
   - **Ubicación como una dirección**
   - **Ubicación como coordenadas** 

4. Seleccione **Guardar**. 

   ![Propiedad de ubicación con la dirección de instalación agregada](./media/howto-set-up-template/locationcloudproperty3.png)

Ahora, un operador puede actualizar el valor de ubicación en el formulario de campo de ubicación. 

#### <a name="add-location-as-a-device-property"></a>Agregar la ubicación como una propiedad del dispositivo 

Puede crear una propiedad de ubicación como una propiedad de dispositivo que el dispositivo notifica. Por ejemplo, si desea realizar un seguimiento de la ubicación del dispositivo:

1. En la pestaña **Properties** (Propiedades), asegúrese de que **Design Mode** (Modo de diseño) está en **On** (Activado).

   ![Pestaña "Properties" (Propiedades) con Design mode (Modo de diseño) activado](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Seleccione **Device Property** (Propiedad del dispositivo) en la biblioteca.
3. Configure el nombre para mostrar y el nombre del campo, y seleccione **Location** (Ubicación) como tipo de datos. 

   > [!NOTE]
   > El nombre del campo debe coincidir exactamente con el nombre de la propiedad que el dispositivo notifica. 

   ![Formulario "Configure Device Properties" (Configurar propiedades del dispositivo) con detalles de ubicación](./media/howto-set-up-template/locationdeviceproperty2.png)

Ahora que ha configurado la propiedad de ubicación, puede [agregar un mapa para visualizar la ubicación en el panel del dispositivo](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Comandos:

Los comandos se usan para administrar un dispositivo de forma remota. Permiten a los operadores de la aplicación ejecutar al instante comandos en el dispositivo. Puede agregar varios comandos a la plantilla de dispositivo que aparecen como iconos en la pestaña **Commands** (Comandos) para que los operadores los utilicen. Como generador del dispositivo, tiene la flexibilidad de definir comandasen función de sus requisitos.

¿En qué se diferencia un comando de un valor? 

* **Valor**: un valor es una configuración que desea aplicar a un dispositivo y desea que el dispositivo conserve dicha configuración hasta que la cambie. Por ejemplo, desea establecer la temperatura del congelador y desea dicho valor aun cuando se reinicie el congelador. 

* **Comando**: los comandos se usan para ejecutar al instante un comando en el dispositivo de forma remota desde IoT Central. Si un dispositivo no está conectado, se agota el tiempo de espera del comando y se produce un error. Por ejemplo, desea reiniciar un dispositivo.  

Cuando se ejecuta un comando, puede estar en uno de los tres estados, en función de si el dispositivo ha recibido el comando. 

Por ejemplo, puede agregar una nuevo comando **Echo**:

![Formulario "Configure Command" (Configurar comando) con detalles del eco](./media/howto-set-up-template/commandsecho.png)

Después de seleccionar **Save** (Guardar), el comando **Echo** (Eco) aparece como un icono y está listo para usarse para mostrar el dispositivo.

Después de crear un icono, puede probar el nuevo comando.

## <a name="rules"></a>Reglas

Las reglas permiten a los operadores supervisar los dispositivos prácticamente en tiempo real. Las reglas invocan automáticamente acciones, como el envío de un correo electrónico cuando se desencadena la regla. Actualmente hay un solo tipo de regla disponible:

- **Regla de telemetría**, que se desencadena cuando la telemetría del dispositivo seleccionado supera un umbral especificado. [Más información acerca de las reglas de telemetría](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Panel

El panel es donde puede ir un operador para ver información sobre un dispositivo. Como generador, puede agregar iconos a esta página que ayuden a los operadores a saber cómo se comporta el dispositivo. Puede agregar varios iconos de panel a la plantilla de dispositivo. Puede agregar seis tipos de iconos de panel: imagen, gráfico de líneas, gráfico de barras, KPI, configuración y propiedades, y etiqueta.

Por ejemplo, puede agregar un icono **Settings and Properties** (Configuración y propiedades) para mostrar una selección de los valores actuales de la configuración y las propiedades:

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Ahora, cuando un operador vea el panel, podrá ver este icono que muestra las propiedades y la configuración del dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Adición de una ubicación de Azure Maps al panel

Si ha configurado una propiedad de la ubicación anteriormente en [Creación de una propiedad de ubicación con Azure Maps](#create-a-location-property-through-azure-maps), puede visualizar la ubicación mediante un mapa en el panel del dispositivo.

1. En la pestaña **Dashboard** (Panel), asegúrese de que **Design Mode** (Modo de diseño) está en **On** (Activado).

   ![Pestaña "Dashboard" (Panel) con Design mode (Modo de diseño) activado](./media/howto-set-up-template/locationcloudproperty4map.png)

2. En el panel del dispositivo, seleccione **Mapa** en la Biblioteca. 
3. Asigne un título y elija la propiedad de ubicación que se ha configurado anteriormente como parte de las propiedades del dispositivo.

   ![Formulario "Configure Map" (Configurar mapa) con detalles del título y las propiedades](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Seleccione **Guardar**. El mosaico muestra la ubicación que ha seleccionado. 

   ![Mosaico con ubicación seleccionada](./media/howto-set-up-template/locationcloudproperty6map.png) 

Puede cambiar el tamaño del mapa hasta obtener el tamaño deseado.

Cuando un operador vea el panel, puede ver todos los iconos del panel que ha configurado, incluido un mapa de ubicación.

![Iconos en el panel](./media/howto-set-up-template/locationcloudproperty7map.png) 

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar una plantilla de dispositivo en una aplicación de Azure IoT Central puede:

> [!div class="nextstepaction"]
> [Creación de una nueva versión de plantilla de dispositivo](howto-version-devicetemplate.md)
