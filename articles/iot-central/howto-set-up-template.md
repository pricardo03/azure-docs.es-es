---
title: Configuración de una plantilla de dispositivo en una aplicación de Azure IoT Central | Microsoft Docs
description: Aprenda a configurar una plantilla de dispositivo con medidas, valores, propiedades, reglas y un panel.
author: viv-liu
ms.author: viviali
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d1704220a13b6d6b5a48b7167d7912a38057127d
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466507"
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

1. Vaya a la página **Plantillas de dispositivo**.

2. Para crear una plantilla, empiece seleccionando **+ nuevo**.

3. Para empezar a trabajar, elija entre las plantillas predefinidas existentes. En caso contrario, seleccione **personalizado**, escriba un nombre y haga clic en **crear** para crear su propia plantilla desde cero.

   ![Biblioteca de plantillas de dispositivo](./media/howto-set-up-template/newtemplate.png)

4. Cuando se crea una plantilla personalizada, verá el **detalles del dispositivo** página de la nueva plantilla de dispositivo. IoT Central crea automáticamente un dispositivo simulado al crear una plantilla de dispositivo. Un dispositivo simulado permite probar el comportamiento de la aplicación antes de conectar un dispositivo real.

En las secciones siguientes se describen cada una de las pestañas de la página **Plantilla de dispositivo**.

## <a name="measurements"></a>Medidas

Las medidas son los datos que proceden del dispositivo. Puede agregar varias medidas a la plantilla de dispositivo para que coincidan con las funcionalidades de dicho dispositivo.

- Las medidas de tipo **Telemetría** son los puntos de datos numéricos que el dispositivo recopila a lo largo del tiempo. Se representan como un flujo de datos continuo. Un ejemplo es la temperatura.
- Las medidas de tipo **Evento** son los datos en un momento dado que representan algo significativo en el dispositivo. Un nivel de gravedad representa la importancia de un evento. Un ejemplo es un error del motor del ventilador.
- Las medidas de tipo **Estado** representan el estado del dispositivo o sus componentes durante un período de tiempo. Por ejemplo, se puede definir que el modo de ventilador tenga dos posibles estados: **Operating** (En funcionamiento) y **Stopped** (Detenido).

### <a name="create-a-telemetry-measurement"></a>Creación de una medida de tipo Telemetría

Para agregar una nueva medición de datos de telemetría, seleccione **+ nueva medición**, elija **telemetría** como la medida escriba y especifique los detalles en el formulario.

> [!NOTE]
> Los nombres de campo en la plantilla de dispositivo deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente para que la medida de telemetría se muestre en la aplicación cuando se conecte un dispositivo real. Haga lo mismo cuando configure los ajustes, las propiedades del dispositivo y los comandos mientras continúa definiendo la plantilla de dispositivo en las secciones siguientes.
.png, por ejemplo, puede agregar una nueva medición de datos de telemetría de temperatura:

| Nombre para mostrar        | Nombre de campo    |  Unidades    | Mín.   |Máx.|
| --------------------| ------------- |-----------|-------|---|
| Temperatura         | temp          |  degC     |  0    |100|

![Formulario "Create Telemetry" (Crear telemetría) con detalles de la medida de temperatura](./media/howto-set-up-template/measurementsform.png)

Después de seleccionar **Save** (Guardar), le medida **Temperature** aparece en la lista de medidas. En breve, podrá ver los datos de temperatura desde el dispositivo simulado.

Al mostrar los datos de telemetría, puede elegir entre las siguientes opciones de agregación: Promedio, mínimo, máximo, Sum y Count. **Promedio** está seleccionada como la agregación predeterminada en el gráfico. 

> [!NOTE]
> El tipo de datos de la medida de telemetría es un número de punto flotante.

### <a name="create-an-event-measurement"></a>Creación de una medida de tipo Evento

Para agregar una nueva medición del evento, seleccione **+ nueva medición** y seleccione **eventos** como el tipo de medida. Escriba los detalles en el formulario **Crear evento**.

Rellene los siguientes campos con los datos del evento: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Severity** (Gravedad). Puede elegir entre los tres niveles disponibles de gravedad: **Error**, **Advertencia** e **Información**.

Por ejemplo, puede agregar un nuevo evento denominado **Fan Motor Error**.

| Nombre para mostrar        | Nombre de campo    |  Gravedad predeterminada |
| --------------------| ------------- |-----------|
| Fan Motor Error     | fanmotorerror |  Error    |

![Formulario "Create Evento" (Crear evento) formulario con detalles de un evento del motor del ventilador](./media/howto-set-up-template/eventmeasurementsform.png)

Después de seleccionar **Save** (Guardar), le medida **Fan Motor Error** aparece en la lista de medidas. En breve, podrá ver los datos del evento desde el dispositivo simulado.

Para ver más detalles sobre un evento, seleccione el icono de evento en el gráfico:

![Detalles del evento "Fan Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
> El tipo de datos de la medida de tipo Evento es cadena.

### <a name="create-a-state-measurement"></a>Creación de una medida de tipo Estado

Para agregar una nueva medida de estado, seleccione el **+ nueva medición** y seleccione **estado** como el tipo de medida. Escriba los detalles en el formulario **Crear estado**.

Rellene los siguientes campos con los datos del estado: **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y **Values** (Valores). Cada valor también puede tener un nombre para mostrar que se usará cuando el valor aparezca en gráficos y tablas.

Por ejemplo, puede agregar un nuevo estado denominado **Fan Mode** que tenga dos valores posibles que el dispositivo puede enviar: **Operating** (En funcionamiento) y **Stopped** (Detenido).

| Nombre para mostrar | Nombre de campo    |  Valor 1   | Nombre para mostrar | Valor 2    |Nombre para mostrar  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Fan Mode     | fanmode       |  1         | Operating    |     0      | Stopped      |

![Formulario de "Edit State" con los detalles del modo de ventilador](./media/howto-set-up-template/statemeasurementsform.png)

Después de seleccionar **Save** (Guardar), le medida del estado **Fan Mode** aparece en la lista de medidas. En breve, podrá ver los datos del estado desde el dispositivo simulado.

Si el dispositivo envía demasiados puntos de datos en un intervalo pequeño, la medida de estado aparece se muestra con otro objeto visual diferente. Seleccione el gráfico para ver todos los puntos de datos dentro de ese período de tiempo en orden cronológico. También puede reducir el intervalo de tiempo para ver la medida trazada en el gráfico.

> [!NOTE]
> El tipo de datos de la medida de tipo Estado es cadena.

## <a name="settings"></a>Configuración

La configuración controla un dispositivo. Permite a los operadores proporcionar entradas al dispositivo. Puede agregar varias configuraciones a la plantilla de dispositivo que aparecen como iconos en la pestaña **Configuración** para que los operadores los utilicen. Puede agregar muchos tipos de valores: número, texto, fecha, alternar, lista de selección y etiqueta de sección.

La configuración puede tener uno de estos tres estados. El dispositivo notifica estos estados.

- **Synced** (Sincronizado): el dispositivo ha cambiado para reflejar el valor de la configuración.

- **Pending**: el dispositivo está actualmente cambiando al valor de la configuración.

- **Error**: el dispositivo ha devuelto un error.

Por ejemplo, puede agregar una nueva configuración de la velocidad del ventilador seleccionando **configuración** y escribir en el nuevo **número** configuración:

| Nombre para mostrar  | Nombre de campo    |  Unidades  | Decimals |Inicial|
| --------------| ------------- |---------| ---------|---- |
| Fan Speed     | fanSpeed      | RPM     | 2        | 0   |

![Formulario "Configure Number" con detalles de los valores de velocidad](./media/howto-set-up-template/settingsform.png)

Después de seleccionar **Guardar**, la opción **Velocidad del ventilador** aparece como un icono. Un operador puede usar esta opción en la página **Device Explorer** para cambiar la velocidad del ventilador del dispositivo.

## <a name="properties"></a>Properties (Propiedades)

Las propiedades son metadatos asociados al dispositivo, como la ubicación y el número de serie de dicho dispositivo. Agregue varias propiedades a la plantilla de dispositivo que aparecen como iconos en la pestaña **Propiedades**. Una propiedad puede tener un tipo como número, texto, fecha, botón de alternancia, propiedad del dispositivo, etiqueta o ubicación. Un operador puede especificar los valores de las propiedades cuando crea un dispositivo y puede editar estos valores en cualquier momento. Las propiedades del dispositivo son de solo lectura y se envían desde el dispositivo a la aplicación. Un operador no puede cambiarlas. Cuando se conecta un dispositivo real, el icono de la propiedad del dispositivo se actualiza en la aplicación.

Existen dos categorías de propiedades:

- _Propiedades del dispositivo_ que el dispositivo informa a la aplicación de IoT Central. Las propiedades del dispositivo son valores de solo lectura que informa el dispositivo y que se actualizan en la aplicación cuando se conecta un dispositivo real.
- _Propiedades de la aplicación_ que se almacenan en la aplicación y que el operador puede editar. El dispositivo no reconoce las propiedades de la aplicación.

Por ejemplo, puede agregar la última fecha de mantenimiento del dispositivo como una nueva propiedad **Fecha** (una propiedad de la aplicación) en la pestaña **Propiedades**:

| Nombre para mostrar  | Nombre de campo | Valor inicial   |
| --------------| -----------|-----------------|
| Última revisión      | lastServiced        | 01/29/2019     |

![Formulario "Configurar último mantenimiento" en la pestaña "Propiedades"](./media/howto-set-up-template/propertiesform.png)

Después de seleccionar **Guardar**, la última fecha de mantenimiento del dispositivo aparece como un icono.

Después de crear el icono, puede cambiar el valor de propiedad de la aplicación en **Device Explorer**.

### <a name="create-a-location-property-through-azure-maps"></a>Creación de una propiedad de ubicación con Azure Maps

Puede dar contexto geográfico a los datos de ubicación en Azure IoT Central y asignar cualquier coordenada de latitud y de longitud de una dirección. O bien puede asignar las coordenadas de latitud y longitud. Azure Maps habilita esta funcionalidad en IoT Central.

Puede agregar dos tipos de propiedades de ubicación:

- **Ubicación como una propiedad de la aplicación**, que se almacena en la aplicación. El dispositivo no reconoce las propiedades de la aplicación.
- **Ubicación como una propiedad del dispositivo**, que el dispositivo notifica a la aplicación.

#### <a name="add-location-as-an-application-property"></a>Agregar ubicación como una propiedad de la aplicación

Puede crear una propiedad de ubicación como una propiedad de la aplicación mediante Azure Maps en la aplicación IoT Central. Por ejemplo, puede agregar la dirección de la instalación de dispositivos:

1. Vaya a la pestaña **Propiedades**.

2. En la biblioteca, seleccione **Location** (Ubicación).

3. Configure los valores de los campos **Display Name** (Nombre para mostrar), **Field Name** (Nombre de campo) y (opcionalmente) **Initial Value** (Valor inicial) de la ubicación.

    | Nombre para mostrar  | Nombre de campo | Valor inicial |
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

    | Nombre para mostrar  | Nombre de campo | Tipo de datos |
    | --------------| -----------|-----------|
    | Ubicación del dispositivo | deviceLocation | ubicación  |

   > [!NOTE]
   > Los nombres de campo deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente.

   ![Formulario "Configure Device Properties" (Configurar propiedades del dispositivo) con detalles de ubicación](./media/howto-set-up-template/locationdeviceproperty2.png)

Una vez que se conecta el dispositivo real, la ubicación que agregó como propiedad del dispositivo se actualiza con el valor que el dispositivo envió. Ahora que ha configurado la propiedad de ubicación, puede [agregar un mapa para visualizar la ubicación en el panel del dispositivo](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Comandos:

Los comandos se usan para administrar un dispositivo de forma remota. Permiten a los operadores ejecutar comandos en el dispositivo. Puede agregar varios comandos a la plantilla de dispositivo que aparecen como iconos en la pestaña **Commands** (Comandos) para que los operadores los utilicen. Como generador del dispositivo, tiene la flexibilidad de definir comandasen función de sus requisitos.

¿En qué se diferencia un comando de un valor?

* **Configuración**: una configuración es un ajuste que desea aplicar a un dispositivo. Desea que el dispositivo conserve dicha configuración hasta que la cambie. Por ejemplo, desea establecer la temperatura del congelador y desea dicho valor aun cuando se reinicie el congelador.

* **Comando**: los comandos se usan para ejecutar al instante un comando en el dispositivo de forma remota desde IoT Central. Si un dispositivo no está conectado, se agota el tiempo de espera del comando y se produce un error. Por ejemplo, desea reiniciar un dispositivo.

Por ejemplo, puede agregar un nuevo **Echo** comando seleccionando el **comandos** ficha, a continuación, seleccione **+ nuevo comando**y escriba los detalles del nuevo comando:

| Nombre para mostrar  | Nombre de campo | Tiempo de espera predeterminado | Tipo de datos |
| --------------| -----------|---------------- | --------- |
| Comando Echo  | echo       |  30             | text      |

![Formulario "Configure Command" (Configurar comando) con detalles del eco](./media/howto-set-up-template/commandsecho1.png)

Después de seleccionar **Guardar**, el comando **Eco** aparece como un icono y está listo para usarse en **Device Explorer** una vez que se conecta el dispositivo real. Los nombres de campo del comando deben coincidir con los nombres de propiedad en el código de dispositivo correspondiente para que los comandos se ejecuten correctamente.

## <a name="rules"></a>Reglas

Las reglas permiten a los operadores supervisar los dispositivos prácticamente en tiempo real. Las reglas invocan automáticamente acciones, como el envío de un correo electrónico cuando se desencadena la regla. Actualmente hay un solo tipo de regla disponible:

- **Regla de telemetría**, que se desencadena cuando la telemetría del dispositivo seleccionado supera un umbral especificado. [Más información acerca de las reglas de telemetría](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>panel

El panel es donde puede ir un operador para ver información sobre un dispositivo. Como generador, puede agregar iconos a esta página que ayuden a los operadores a saber cómo se comporta el dispositivo. Puede agregar varios iconos de panel a la plantilla de dispositivo. Puede agregar muchos tipos de iconos del panel, como imagen, gráfico de líneas, gráfico de barras, indicador clave de rendimiento (KPI), configuración y propiedades y etiqueta.

Por ejemplo, puede agregar un icono **Settings and Properties** (Configuración y propiedades) para mostrar una selección de los valores actuales de la configuración y las propiedades; para ello, seleccione la pestaña **Panel** y el icono de la biblioteca:

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](./media/howto-set-up-template/dashboardsettingsandpropertiesform1.png)

Ahora, cuando un operador acceda al panel en **Device Explorer**, verá el icono.

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Adición de una ubicación de Azure Maps al panel

Si ha configurado una propiedad de ubicación, puede visualizar la ubicación mediante un mapa en el panel del dispositivo.

1. Vaya a la pestaña **Panel**.

1. En el panel del dispositivo, seleccione **Mapa** en la Biblioteca.

1. Asigne un título al mapa. El siguiente ejemplo tiene el título **Installation Location** (Ubicación de instalación). A continuación, elija la propiedad de ubicación configurada anteriormente en la pestaña **Propiedades**. En el ejemplo siguiente, se ha seleccionado **Installation address** (Dirección de instalación).

   ![Formulario "Configure Map" (Configurar mapa) con detalles del título y las propiedades](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Seleccione **Guardar**. El mosaico muestra la ubicación que ha seleccionado.

Puede cambiar el tamaño del mapa hasta obtener el tamaño deseado. Ahora, cuando un operador acceda al panel en **Device Explorer**, se mostrarán todos los iconos del panel que se han configurado, incluido un mapa de ubicación.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar una plantilla de dispositivo en una aplicación de Azure IoT Central puede:

> [!div class="nextstepaction"]
> [Crear una nueva versión de plantilla de dispositivo](howto-version-devicetemplate.md)
> [conectar un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central](howto-connect-devkit.md)
> [conectar una aplicación cliente genérico a los de Azure Aplicación de IoT Central (Node.js)](howto-connect-nodejs.md)