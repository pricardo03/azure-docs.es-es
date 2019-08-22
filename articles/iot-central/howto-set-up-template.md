---
title: Configuración de una plantilla de dispositivo en una aplicación de Azure IoT Central | Microsoft Docs
description: Aprenda a configurar una plantilla de dispositivo con medidas, valores, propiedades, reglas y un panel.
author: viv-liu
ms.author: viviali
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 63c0a04a6d18d6af850b1492d2efa9df9aa65219
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877380"
---
# <a name="set-up-a-device-template"></a>Configuración de una plantilla de dispositivo

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Una plantilla de dispositivo es un plano técnico que define las características y los comportamientos de un tipo de dispositivo que se conecta a una aplicación de Azure IoT Central.

Por ejemplo, un generador puede crear una plantilla de dispositivo para un ventilador conectado que tenga las siguientes características:

- Medida de telemetría de temperatura
- Medida de ubicación
- Medida de eventos de error del motor del ventilador
- Medida del estado de funcionamiento del ventilador
- Configuración de la velocidad del ventilador
- Reglas que envían alertas
- Panel que ofrece una vista global del dispositivo

A partir de esta plantilla de dispositivo, un operador puede crear y conectar dispositivos de ventilador reales con nombres, como **ventilador-1** y **ventilador-2**. Todos estos ventiladores tienen medidas, valores, propiedades, reglas y un panel que los usuarios de la aplicación pueden supervisar y administrar.

> [!NOTE]
> Solo los generadores y administradores pueden crear, editar y eliminar plantillas de dispositivo. Cualquier usuario puede crear dispositivos en la página **Device Explorer** a partir de las plantillas de dispositivo existentes.

## <a name="create-a-device-template"></a>Creación de una plantilla de dispositivo

1. Vaya a la página **Plantillas de dispositivo**.

2. Para crear una plantilla, empiece por seleccionar **+Nuevo**.

3. Para empezar a trabajar rápidamente, seleccione entre las plantillas pregeneradas existentes. De lo contrario, seleccione **Personalizado**, escriba un nombre y haga clic en **Crear** para crear su propia plantilla desde cero.

   ![Biblioteca de plantillas de dispositivo](./media/howto-set-up-template/newtemplate.png)

4. Cuando se crea una plantilla personalizada, se ve la página **Detalles del dispositivo** de la nueva plantilla de dispositivo. IoT Central crea automáticamente un dispositivo simulado al crear una plantilla de dispositivo. Un dispositivo simulado permite probar el comportamiento de la aplicación antes de conectar un dispositivo real.

En las secciones siguientes se describen cada una de las pestañas de la página **Plantilla de dispositivo**.

## <a name="measurements"></a>Medidas

Las medidas son los datos que proceden del dispositivo. Puede agregar varias medidas a la plantilla de dispositivo para que coincidan con las funcionalidades de dicho dispositivo.

- Las medidas de tipo **Telemetría** son los puntos de datos numéricos que el dispositivo recopila a lo largo del tiempo. Se representan como un flujo de datos continuo. Un ejemplo es la temperatura.
- Las medidas de tipo **Evento** son los datos en un momento dado que representan algo significativo en el dispositivo. Un nivel de gravedad representa la importancia de un evento. Un ejemplo es un error del motor del ventilador.
- Las medidas de tipo **Estado** representan el estado del dispositivo o sus componentes durante un período de tiempo. Por ejemplo, se puede definir que el modo de ventilador tenga dos posibles estados: **Operating** (En funcionamiento) y **Stopped** (Detenido).
- Las medidas de **ubicación** son las coordenadas de longitud y latitud del dispositivo durante un período de tiempo. Por ejemplo, un ventilador se puede mover de una ubicación a otra.

### <a name="create-a-telemetry-measurement"></a>Creación de una medida de tipo Telemetría

Para agregar una nueva medida de telemetría, seleccione **+ New Measurement** (Nueva medida), elija **Telemetría** como tipo de medida y escriba los detalles en el formulario.

> [!NOTE]
> Los nombres de campo en la plantilla de dispositivo deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente para que la medida de telemetría se muestre en la aplicación cuando se conecte un dispositivo real. Haga lo mismo cuando configure los ajustes, las propiedades del dispositivo y los comandos mientras continúa definiendo la plantilla de dispositivo en las secciones siguientes.

Por ejemplo, puede agregar una nueva medida de telemetría de temperatura:

| Display Name (Nombre para mostrar)        | Nombre del campo    |  Unidades    | Min   |max|
| --------------------| ------------- |-----------|-------|---|
| Temperatura         | temp          |  degC     |  0    |100|

![Formulario "Create Telemetry" (Crear telemetría) con detalles de la medida de temperatura](./media/howto-set-up-template/measurementsform.png)

Después de seleccionar **Save** (Guardar), le medida **Temperature** aparece en la lista de medidas. En breve, podrá ver los datos de temperatura desde el dispositivo simulado.

Al mostrar la telemetría, puede elegir entre las siguientes opciones de agregación: Media, Mínimo, Máximo, Suma y Recuento. **Media** se selecciona como la agregación predeterminada en el gráfico.

> [!NOTE]
> El tipo de datos de la medida de telemetría es un número de punto flotante.

### <a name="create-an-event-measurement"></a>Creación de una medida de tipo Evento

Para agregar una nueva medida de evento, seleccione **+ New Measurement** (Nueva medida) y **Evento** como tipo de medida. Escriba los detalles en el formulario **Crear evento**.

Rellene los siguientes campos con los datos del evento: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Severity** (Gravedad). Puede elegir entre los tres niveles disponibles de gravedad: **Error**, **Advertencia** e **Información**.

Por ejemplo, puede agregar un nuevo evento denominado **Fan Motor Error**.

| Display Name (Nombre para mostrar)        | Nombre del campo    |  Gravedad predeterminada |
| --------------------| ------------- |-----------|
| Fan Motor Error     | fanmotorerror |  Error    |

![Formulario "Create Evento" (Crear evento) formulario con detalles de un evento del motor del ventilador](./media/howto-set-up-template/eventmeasurementsform.png)

Después de seleccionar **Save** (Guardar), le medida **Fan Motor Error** aparece en la lista de medidas. En breve, podrá ver los datos del evento desde el dispositivo simulado.

Para ver más detalles sobre un evento, seleccione el icono del evento en el gráfico:

![Detalles del evento "Fan Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
> El tipo de datos de la medida de tipo Evento es cadena.

### <a name="create-a-state-measurement"></a>Creación de una medida de tipo Estado

Para agregar una nueva medida de estado, seleccione el botón **+ New Measurement** (Nueva medida) y **Estado** como tipo de medida. Escriba los detalles en el formulario **Crear estado**.

Rellene los siguientes campos con los datos del estado: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Values** (Valores). Cada valor también puede tener un nombre para mostrar que se usará cuando el valor aparezca en gráficos y tablas.

Por ejemplo, puede agregar un nuevo estado denominado **Fan Mode** que tenga dos valores posibles que el dispositivo puede enviar: **Operating** (En funcionamiento) y **Stopped** (Detenido).

| Display Name (Nombre para mostrar) | Nombre del campo    |  Value 1   | Display Name (Nombre para mostrar) | Value 2    |Display Name (Nombre para mostrar)  |
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Fan Mode     | fanmode       |  1         | Operating    |     0      | Stopped      |

![Formulario de "Edit State" con los detalles del modo de ventilador](./media/howto-set-up-template/statemeasurementsform.png)

Después de seleccionar **Save** (Guardar), le medida del estado **Fan Mode** aparece en la lista de medidas. En breve, podrá ver los datos del estado desde el dispositivo simulado.

Si el dispositivo envía demasiados puntos de datos en un intervalo pequeño, la medida de estado aparece se muestra con otro objeto visual diferente. Seleccione el gráfico para ver todos los puntos de datos de ese período en orden cronológico. También puede reducir el intervalo de tiempo para ver la medida trazada en el gráfico.

> [!NOTE]
> El tipo de datos de la medida de tipo Estado es cadena.

### <a name="create-a-location-measurement"></a>Creación de una medida de ubicación

Para agregar una nueva medida de ubicación, seleccione **+ New Measurement** (+ Nueva medida), elija **Location** (Ubicación) como tipo de medida y escriba los detalles en el formulario **Create Measurement** (Crear medida).

Por ejemplo, puede agregar una nueva medida de telemetría de ubicación:

| Display Name (Nombre para mostrar)        | Nombre del campo    |
| --------------------| ------------- |
| Asset Location      |  assetloc     |

![Formulario "Create Location" (Crear ubicación) con detalles de la medida de ubicación](./media/howto-set-up-template/locationmeasurementsform.png)

Después de seleccionar **Save** (Guardar), la medida **Location** (Ubicación) aparece en la lista de medidas. En breve, podrá ver los datos de la ubicación desde el dispositivo simulado.

Cuando se muestre la ubicación, puede elegir entre las siguientes opciones: ubicación más reciente e historial de ubicación. El **historial de ubicación** solo se aplica durante el intervalo de tiempo seleccionado.

El tipo de datos de la medida de ubicación es un objeto que contiene la longitud, la latitud y una altitud opcional. El fragmento de código siguiente muestra la estructura de JavaScript:

```javascript
assetloc: {
  lon: floating point number,
  lat: floating point number,
  alt?: floating point number
}
```

Una vez que se conecta el dispositivo real, la ubicación que agregó como medida se actualiza con el valor que el dispositivo envió. Ahora que ha configurado la medida de ubicación, puede [agregar un mapa para visualizar la ubicación en el panel del dispositivo](#add-a-location-measurement-in-the-dashboard).

## <a name="settings"></a>Configuración

La configuración controla un dispositivo. Permite a los operadores proporcionar entradas al dispositivo. Puede agregar varias configuraciones a la plantilla de dispositivo que aparecen como iconos en la pestaña **Configuración** para que los operadores los utilicen. Puede agregar muchos tipos de valores: número, texto, fecha, alternancia y etiqueta de sección.

La configuración puede tener uno de estos tres estados. El dispositivo notifica estos estados.

- **Synced** (Sincronizado): el dispositivo ha cambiado para reflejar el valor de la configuración.

- **Pending**: el dispositivo está actualmente cambiando al valor de la configuración.

- **Error**: el dispositivo ha devuelto un error.

Por ejemplo, puede agregar un nuevo valor de velocidad del ventilador si selecciona **Configuración** y escribe el nuevo valor **Número**:

| Display Name (Nombre para mostrar)  | Nombre del campo    |  Unidades  | Decimals |Inicial|
| --------------| ------------- |---------| ---------|---- |
| Fan Speed     | fanSpeed      | RPM     | 2        | 0   |

![Formulario "Configure Number" con detalles de los valores de velocidad](./media/howto-set-up-template/settingsform.png)

Después de seleccionar **Guardar**, la opción **Velocidad del ventilador** aparece como un icono. Un operador puede usar esta opción en la página **Device Explorer** para cambiar la velocidad del ventilador del dispositivo.

## <a name="properties"></a>properties (Propiedades)

Las propiedades son metadatos asociados al dispositivo, como la ubicación fija del dispositivo y el número de serie. Agregue varias propiedades a la plantilla de dispositivo que aparecen como iconos en la pestaña **Propiedades**. Una propiedad tiene un tipo, como un número, texto, una fecha, un botón de alternancia, una propiedad del dispositivo, una etiqueta o una ubicación fija. Un operador especifica los valores de las propiedades cuando crea un dispositivo, y puede editar estos valores en cualquier momento. Las propiedades del dispositivo son de solo lectura y se envían desde el dispositivo a la aplicación. Un operador no puede cambiarlas. Cuando se conecta un dispositivo real, el icono de la propiedad del dispositivo se actualiza en la aplicación.

Existen dos categorías de propiedades:

- _Propiedades del dispositivo_ que el dispositivo informa a la aplicación de IoT Central. Las propiedades del dispositivo son valores de solo lectura que informa el dispositivo y que se actualizan en la aplicación cuando se conecta un dispositivo real.
- _Propiedades de la aplicación_ que se almacenan en la aplicación y que el operador puede editar. Las propiedades de la aplicación solo se almacenan en la aplicación, nunca se ven en un dispositivo.

Por ejemplo, puede agregar la última fecha de mantenimiento del dispositivo como una nueva propiedad **Fecha** (una propiedad de la aplicación) en la pestaña **Propiedades**:

| Display Name (Nombre para mostrar)  | Nombre del campo | Valor inicial   |
| --------------| -----------|-----------------|
| Última revisión      | lastServiced        | 01/29/2019     |

![Formulario "Configurar último mantenimiento" en la pestaña "Propiedades"](./media/howto-set-up-template/propertiesform.png)

Después de seleccionar **Guardar**, la última fecha de mantenimiento del dispositivo aparece como un icono.

Después de crear el icono, puede cambiar el valor de propiedad de la aplicación en **Device Explorer**.

### <a name="create-a-location-property"></a>Creación de una propiedad de ubicación

Puede proporcionar contexto geográfico a los datos de ubicación en Azure IoT Central y asignar cualquier coordenada de latitud y longitud o una dirección. Azure Maps habilita esta funcionalidad en IoT Central.

Puede agregar dos tipos de propiedades de ubicación:

- **Ubicación como una propiedad de la aplicación**, que se almacena en la aplicación. Las propiedades de la aplicación solo se almacenan en la aplicación, nunca se ven en un dispositivo.
- **Ubicación como una propiedad del dispositivo**, que el dispositivo notifica a la aplicación. Este tipo de propiedad es más adecuada para una ubicación estática.

> [!NOTE]
> La ubicación como propiedad no registra un historial. Si se desea un historial, se debe usar una medida de ubicación.

#### <a name="add-location-as-an-application-property"></a>Agregar ubicación como una propiedad de la aplicación

Puede crear una propiedad de ubicación como una propiedad de la aplicación mediante Azure Maps en la aplicación IoT Central. Por ejemplo, puede agregar la dirección de la instalación de dispositivos:

1. Vaya a la pestaña **Propiedades**.

2. En la biblioteca, seleccione **Location** (Ubicación).

3. Configure los valores de los campos **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y (opcionalmente) **Initial Value** (Valor inicial) de la ubicación.

    | Display Name (Nombre para mostrar)  | Nombre del campo | Valor inicial |
    | --------------| -----------|---------|
    | Dirección de la instalación | installAddress | Microsoft, 1 Microsoft Way, Redmond, WA 98052   |

   ![Formulario "Configure Location" (Configurar ubicación) con detalles de la ubicación](./media/howto-set-up-template/locationcloudproperty2.png)

   Se admiten dos formatos para agregar una ubicación:
   - **Ubicación como una dirección**
   - **Ubicación como coordenadas**

4. Seleccione **Guardar**. Un operador puede actualizar el valor de ubicación en **Device Explorer**.

#### <a name="add-location-as-a-device-property"></a>Agregar la ubicación como una propiedad del dispositivo

Puede crear una propiedad de ubicación como una propiedad de dispositivo que el dispositivo notifica. Por ejemplo, si desea realizar un seguimiento de la ubicación del dispositivo:

1. Vaya a la pestaña **Propiedades**.

2. Seleccione **Device Property** (Propiedad del dispositivo) en la biblioteca.

3. Configure el nombre para mostrar y el nombre del campo y seleccione **Ubicación** como tipo de datos:

    | Display Name (Nombre para mostrar)  | Nombre del campo | Tipo de datos |
    | --------------| -----------|-----------|
    | Ubicación del dispositivo | deviceLocation | location  |

   > [!NOTE]
   > Los nombres de campo deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente.

   ![Formulario "Configure Device Properties" (Configurar propiedades del dispositivo) con detalles de ubicación](./media/howto-set-up-template/locationdeviceproperty2.png)

Una vez que se conecta el dispositivo real, la ubicación que agregó como propiedad del dispositivo se actualiza con el valor que el dispositivo envió. Ahora que ha configurado la propiedad de ubicación, puede [agregar un mapa para visualizar la ubicación en el panel del dispositivo](#add-a-location-property-in-the-dashboard).

## <a name="commands"></a>Comandos:

Los comandos se usan para administrar un dispositivo de forma remota. Permiten a los operadores ejecutar comandos en el dispositivo. Puede agregar varios comandos a la plantilla de dispositivo que aparecen como iconos en la pestaña **Commands** (Comandos) para que los operadores los utilicen. Como generador del dispositivo, tiene la flexibilidad de definir comandasen función de sus requisitos.

¿En qué se diferencia un comando de un valor?

- **Configuración**: una configuración es un ajuste que desea aplicar a un dispositivo. Desea que el dispositivo conserve dicha configuración hasta que la cambie. Por ejemplo, desea establecer la temperatura del congelador y desea dicho valor aun cuando se reinicie el congelador.

- **Comando**: los comandos se usan para ejecutar al instante un comando en el dispositivo de forma remota desde IoT Central. Si un dispositivo no está conectado, se agota el tiempo de espera del comando y se produce un error. Por ejemplo, desea reiniciar un dispositivo.

Por ejemplo, puede agregar un nuevo comando **Eco** si selecciona la pestaña **Comandos**, **+ Nuevo comando** y escribe los detalles del nuevo comando:

| Display Name (Nombre para mostrar)  | Nombre del campo | Tiempo de espera predeterminado | Tipo de datos |
| --------------| -----------|---------------- | --------- |
| Comando Echo  | echo       |  30             | text      |

![Formulario "Configure Command" (Configurar comando) con detalles del eco](./media/howto-set-up-template/commandsecho1.png)

Después de seleccionar **Guardar**, el comando **Eco** aparece como un icono y está listo para usarse en **Device Explorer** una vez que se conecta el dispositivo real. Los nombres de campo del comando deben coincidir con los nombres de las propiedades del código de dispositivo correspondiente para que los comandos se ejecuten correctamente.

[Este es un vínculo al ejemplo de código de dispositivo de C.](https://github.com/Azure/iot-central-firmware/blob/ad40358906aeb8f2040a822ba5292df866692c16/MXCHIP/mxchip_advanced/src/AzureIOTClient.cpp#L34)

## <a name="rules"></a>Reglas

Las reglas permiten a los operadores supervisar los dispositivos prácticamente en tiempo real. Las reglas invocan automáticamente acciones, como el envío de un correo electrónico cuando se desencadena la regla. Actualmente hay un solo tipo de regla disponible:

- **Regla de telemetría**, que se desencadena cuando la telemetría del dispositivo seleccionado supera un umbral especificado. [Más información acerca de las reglas de telemetría](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>panel

El panel es donde puede ir un operador para ver información sobre un dispositivo. Como creador, puede agregar iconos a esta página que ayuden a los operadores a saber cómo se comporta el dispositivo. Puede agregar muchos tipos de iconos del panel, como imagen, gráfico de líneas, gráfico de barras, indicador clave de rendimiento (KPI), configuración y propiedades y etiqueta.

Por ejemplo, puede agregar un icono **Settings and Properties** (Configuración y propiedades) para mostrar una selección de los valores actuales de la configuración y las propiedades; para ello, seleccione la pestaña **Panel** y el icono de la biblioteca:

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](./media/howto-set-up-template/dashboardsettingsandpropertiesform1.png)

Ahora, cuando un operador acceda al panel en **Device Explorer**, verá el icono.

### <a name="add-a-location-measurement-in-the-dashboard"></a>Adición de una medida de ubicación al panel

Si ha configurado una medida de ubicación, puede visualizar la ubicación con un mapa en el panel del dispositivo. En el caso de las medidas de ubicación, tiene la opción de trazar el historial de ubicaciones.

1. Vaya a la pestaña **Panel**.

1. En el panel del dispositivo, seleccione **Mapa** en la Biblioteca.

1. Asigne un título al mapa. El siguiente ejemplo tiene el título **Device Current Location**. A continuación, elija la medida de ubicación que configuró anteriormente en la pestaña **Measurements** (Medidas). En el ejemplo siguiente, se selecciona la medida **Asset Location**:

   ![Formulario "Configure Map" (Configurar mapa) con detalles del título y las propiedades](./media/howto-set-up-template/locationcloudproperty5map.png)

1. Seleccione **Guardar**. El mosaico muestra la ubicación que ha seleccionado.

Puede cambiar el tamaño del icono del mapa. Ahora, cuando un operador acceda al panel en **Device Explorer**, se mostrarán todos los iconos del panel que se han configurado, incluido un mapa de ubicación.

### <a name="add-a-location-property-in-the-dashboard"></a>Adición de una propiedad de ubicación al panel

Si ha configurado una propiedad de ubicación, puede visualizar la ubicación en un mapa en el panel del dispositivo.

1. Vaya a la pestaña **Panel**.

1. En el panel del dispositivo, seleccione **Mapa** en la Biblioteca.

1. Asigne un título al mapa. El siguiente ejemplo tiene el título **Device Current Location**. A continuación, elija la propiedad de ubicación configurada anteriormente en la pestaña **Propiedades**. En el ejemplo siguiente, se selecciona la medida **Device Location**:

   ![Formulario Configure Map con detalles del título y las propiedades](./media/howto-set-up-template/locationcloudproperty6map.png)

1. Seleccione **Guardar**. El mosaico muestra la ubicación que ha seleccionado.

Puede cambiar el tamaño del icono del mapa. Ahora, cuando un operador acceda al panel en **Device Explorer**, se mostrarán todos los iconos del panel que se han configurado, incluido un mapa de ubicación.

Para más información sobre cómo usar los iconos en Azure IoT Central, consulte [Uso de iconos de panel](howto-use-tiles.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar una plantilla de dispositivo en una aplicación de Azure IoT Central puede:

- [Creación de una nueva versión de plantilla de dispositivo](howto-version-device-template.md)
- [Connect an MXChip IoT DevKit device to your Microsoft IoT Central application](howto-connect-devkit.md) (Conexión de un dispositivo MXChip IoT DevKit a una aplicación de Microsoft IoT Central)
- [Conexión de un aplicación cliente de Node.js genérica a una aplicación de Azure IoT Central (Node.js)](howto-connect-nodejs.md)
