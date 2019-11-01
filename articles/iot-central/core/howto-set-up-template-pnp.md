---
title: Configuración de una plantilla de dispositivo en una aplicación de Azure IoT Central | Microsoft Docs
description: Aprenda a configurar una plantilla de dispositivo con medidas, valores, propiedades, reglas y un panel.
author: sarahhubbard
ms.author: sahubbar
ms.date: 07/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 32c34827dc9cddfc89ccd1fd67f419da662b130a
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72941393"
---
# <a name="set-up-and-manage-a-device-template-preview-features"></a>Configuración y administración de una plantilla de dispositivo (características de versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Una plantilla de dispositivo es un plano técnico que define las características y los comportamientos de un tipo de dispositivo que se conecta a una aplicación de Azure IoT Central.

Por ejemplo, un generador puede crear una plantilla de dispositivo para un ventilador conectado que tenga las siguientes características:

- Envía datos de telemetría de temperatura.
- Envía la propiedad de ubicación.
- Envía eventos de error del motor del ventilador.
- Envía el estado de funcionamiento del ventilador.
- Propiedad de velocidad del ventilador que se puede escribir
- Comando para reiniciar el dispositivo
- Panel que ofrece una vista global del dispositivo

A partir de esta plantilla de dispositivo, un operador puede crear y conectar dispositivos de ventilador reales. Todos estos ventiladores tienen medidas, propiedades y comandos que los operadores utilizan para supervisar y administrar. Los operadores usan los paneles de dispositivos y los formularios para interactuar con los dispositivos de ventilador.

> [!NOTE]
> Solo los generadores y administradores pueden crear, editar y eliminar plantillas de dispositivo. Cualquier usuario puede crear dispositivos en la página **Devices** (Dispositivos) a partir de las plantillas de dispositivo existentes.

[IoT Plug and Play](../../iot-pnp/overview-iot-plug-and-play.md) permite a IoT Central integrar dispositivos sin necesidad de escribir ningún código de dispositivo integrado. En el núcleo de IoT Plug and Play se encuentra un esquema de modelo de funcionalidad del dispositivo que describe dichas funcionalidades. En una aplicación de la versión preliminar de IoT Central, las plantillas de dispositivo usan estos modelos de funcionalidad del dispositivo IoT Plug and Play.

Como generador, tiene varias opciones para crear plantillas de dispositivo:

- Diseñe la plantilla de dispositivo en IoT Central y, después, implemente el modelo de funcionalidad del dispositivo en el código del dispositivo.
- Importe un modelo de funcionalidad del dispositivo desde el [catálogo de dispositivos Azure Certified for IoT](https://aka.ms/iotdevcat) y, a continuación, agregue las propiedades, las personalizaciones y los paneles en la nube que necesite la aplicación de IoT Central.
- Cree un modelo de funcionalidad del dispositivo mediante Visual Studio Code. Implemente el código del dispositivo a partir del modelo. Importe manualmente el modelo de funcionalidad del dispositivo en la aplicación de IoT Central y, a continuación, agregue las propiedades, las personalizaciones y los paneles en la nube que necesite la aplicación de IoT Central.
- Cree un modelo de funcionalidad del dispositivo mediante Visual Studio Code. Implemente el código del dispositivo desde el modelo y conecte el dispositivo real a la aplicación de IoT Central mediante una primera conexión del dispositivo. IoT Central busca e importa el modelo de funcionalidad del dispositivo desde el repositorio público. Después, puede agregar las propiedades, las personalizaciones y los paneles en la nube que la aplicación de IoT Central necesita a la plantilla del dispositivo.

## <a name="create-a-device-template-from-the-device-catalog"></a>Creación de una plantilla de dispositivo desde el catálogo de dispositivos

Como generador, puede empezar a compilar rápidamente la solución mediante un dispositivo certificado de IoT Plug and Play que aparece en el [catálogo de dispositivos Azure IoT](https://catalog.azureiotsolutions.com/alldevices). IoT Central se integra con el catálogo de dispositivos para permitirle importar un modelo de funcionalidad del dispositivo desde cualquiera de estos dispositivos certificados de IoT Plug and Play. Para crear una plantilla de dispositivo desde uno de estos dispositivos en IoT Central:

1. Vaya a la página **Device Templates** (Plantillas de dispositivo) en la aplicación de IoT Central.
1. Seleccione **+ New** (+Nuevo) y, a continuación, seleccione cualquiera de los dispositivos certificados de IoT Plug and Play en el catálogo que aparece a continuación. IoT Central crea una plantilla de dispositivo basada en este modelo de funcionalidad del dispositivo.
1. Agregue las propiedades, las personalizaciones o las vistas en la nube a la plantilla de dispositivo.
1. Seleccione **Publish** (Publicar) para publicar esta plantilla de dispositivo a fin de que esté disponible para que los operadores vean y conecten los dispositivos.

## <a name="create-a-device-template-from-scratch"></a>Creación de una plantilla de dispositivo desde cero

Una plantilla de dispositivo contiene:

- Un _modelo de funcionalidad del dispositivo_ que especifica la telemetría, las propiedades y los comandos que implementa el dispositivo. Estas funcionalidades están organizadas en una o varias interfaces.
- _Propiedades en la nube_ que definen la información que almacena la aplicación de IoT Central acerca de los dispositivos. Por ejemplo, una propiedad en la nube podría registrar los datos en los que se realizó la última revisión de un dispositivo. Esta información nunca se comparte con el dispositivo.
- Las _personalizaciones_ permiten que el generador reemplace algunas de las definiciones en el modelo de funcionalidad del dispositivo. Por ejemplo, el generador podría reemplazar el nombre de una propiedad de dispositivo. Los nombres de propiedad aparecen los paneles y formularios de IoT Central.
- Los _paneles y formularios_ permiten al generador crear una interfaz de usuario para que los operadores supervisen y administren los dispositivos conectados a la aplicación.

Para crear una plantilla de dispositivo en IoT Central:

1. Vaya a la página **Device Templates** (Plantillas de dispositivo) en la aplicación de IoT Central.
1. Seleccione **+ New** (+Nuevo) y después **Custom** (Personalizar).
1. Escriba un nombre para la plantilla, como **Sensor medioambiental**.
1. Presione **Entrar**. IoT Central crea una plantilla de dispositivo vacía.

## <a name="manage-a-device-template"></a>Administración de una plantilla de dispositivo

Puede cambiar el nombre o eliminar una plantilla desde la página principal de la plantilla.

Después de agregar un modelo de funcionalidad del dispositivo a la plantilla, puede publicarlo. No se puede conectar un dispositivo basado en esta plantilla para que los operadores puedan verlo en la página **Devices** (Dispositivos) hasta que se haya publicado la plantilla.

## <a name="create-a-capability-model"></a>Creación de un modelo de funcionalidad

Para crear un modelo de funcionalidad del dispositivo, puede:

- Usar IoT Central para crear un modelo personalizado desde cero.
- Importar un modelo desde un archivo JSON. El generador de dispositivos puede haber usado Visual Studio Code para crear un modelo de funcionalidad del dispositivo para la aplicación.
- Seleccione uno de los dispositivos en el catálogo de dispositivos. Esta opción importa el modelo de funcionalidad del dispositivo que el fabricante ha publicado para este dispositivo. Un modelo de funcionalidad del dispositivo importado como este se publica automáticamente.

## <a name="manage-a-capability-model"></a>Administración de un modelo de funcionalidad

Después de crear un modelo de funcionalidad del dispositivo, puede:

- Agregar interfaces al modelo. Un modelo debe tener al menos una interfaz.
- Editar los metadatos del modelo, como el identificador, el espacio de nombres y el nombre.
- Eliminar el modelo.

## <a name="create-an-interface"></a>Creación de una interfaz

Una funcionalidad del dispositivo debe tener al menos una interfaz. Una interfaz es una colección reutilizable de funcionalidades.

Para crear una interfaz:

1. Vaya al modelo de funcionalidad del dispositivo y elija **+ Add Interface** (+ Agregar interfaz).

1. En la página **Select an Interface** (Seleccione una interfaz), puede:

    - Crear una interfaz personalizada desde cero.
    - Importar una interfaz existente desde un archivo. El generador de dispositivos puede haber usado Visual Studio Code para crear una interfaz para el dispositivo.
    - Elija una de las interfaces estándar, como la interfaz **Device Information** (Información del dispositivo). Las interfaces estándar especifican las funcionalidades comunes a muchos dispositivos. Estas interfaces estándar están publicadas por Microsoft Azure IoT y no se pueden modificar ni editar.

1. Después de crear una interfaz, elija **Edit Identity** (Editar identidad) para cambiar el nombre para mostrar de la interfaz.

1. Si decide crear una interfaz personalizada desde cero, puede agregar las funcionalidades del dispositivo. Las funcionalidades del dispositivo son la telemetría, las propiedades y los comandos.

### <a name="telemetry"></a>Telemetría

La telemetría es un flujo de valores enviados desde el dispositivo, normalmente desde un sensor. Por ejemplo, un sensor podría informar de la temperatura ambiente.

En la siguiente tabla se muestran las opciones de configuración de una funcionalidad de telemetría:

| Campo | DESCRIPCIÓN |
| ----- | ----------- |
| Display Name (Nombre para mostrar) | El nombre para mostrar del valor de telemetría que se usa en los paneles y formularios. |
| NOMBRE | El nombre del campo en el mensaje de telemetría. IoT Central genera un valor para este campo a partir del nombre para mostrar, pero puede elegir su propio valor si es necesario. |
| Capability Type (Tipo de funcionalidad) | Telemetría. |
| Semantic Type (Tipo semántico) | El tipo semántico de la telemetría, como la temperatura, el estado o el evento. La elección del tipo semántico determina cuál de los campos siguientes está disponible. |
| Schema | El tipo de datos de telemetría, como doble, cadena o vector. Las opciones disponibles vienen determinadas por el tipo semántico. El esquema no está disponible para los tipos semánticos de evento y estado. |
| severity | Solo está disponible para el tipo semántico de evento. **Error**, **Information** (Información) o **Warning** (Advertencia). |
| State Values (Valores de estado) | Solo está disponible para el tipo semántico de estado. Defina los valores de estado posibles, cada uno de los cuales tiene el nombre para mostrar, el nombre, el tipo de enumeración y el valor. |
| Unidad | Una unidad para el valor de telemetría, como **mph**, **%** o **&deg;C**. |
| Display Unit (Unidad de visualización) | Una unidad de visualización para su uso en paneles y formularios. |
| Comentario | Cualquier comentario sobre la funcionalidad de telemetría. |
| DESCRIPCIÓN | Una descripción de la funcionalidad de telemetría. |

### <a name="properties"></a>properties (Propiedades)

Las propiedades representan valores a un momento dado. Por ejemplo, un dispositivo puede usar una propiedad para notificar la temperatura objetivo que está intentando alcanzar. Puede establecer las propiedades que se pueden escribir desde IoT Central.

En la siguiente tabla se muestran las opciones de configuración de una funcionalidad de propiedad:

| Campo | DESCRIPCIÓN |
| ----- | ----------- |
| Display Name (Nombre para mostrar) | El nombre para mostrar del valor de propiedad que se usa en los paneles y formularios. |
| NOMBRE | El nombre de la propiedad. IoT Central genera un valor para este campo a partir del nombre para mostrar, pero puede elegir su propio valor si es necesario. |
| Capability Type (Tipo de funcionalidad) | Propiedad. |
| Semantic Type (Tipo semántico) | El tipo semántico de la propiedad, como la temperatura, el estado o el evento. La elección del tipo semántico determina cuál de los campos siguientes está disponible. |
| Schema | El tipo de datos de la propiedad, como doble, cadena o vector. Las opciones disponibles vienen determinadas por el tipo semántico. El esquema no está disponible para los tipos semánticos de evento y estado. |
| Writeable (Grabable) | Si la propiedad no se puede escribir, el dispositivo puede notificar los valores de propiedad a IoT Central. Si la propiedad se puede escribir, el dispositivo puede notificar los valores de propiedad a IoT Central y este puede enviar actualizaciones de propiedades al dispositivo.
| severity | Solo está disponible para el tipo semántico de evento. **Error**, **Information** (Información) o **Warning** (Advertencia). |
| State Values (Valores de estado) | Solo está disponible para el tipo semántico de estado. Defina los valores de estado posibles, cada uno de los cuales tiene el nombre para mostrar, el nombre, el tipo de enumeración y el valor. |
| Unidad | Una unidad para el valor de propiedad, como **mph**, **%** o **&deg;C**. |
| Display Unit (Unidad de visualización) | Una unidad de visualización para su uso en paneles y formularios. |
| Comentario | Cualquier comentario sobre la funcionalidad de propiedad. |
| DESCRIPCIÓN | Una descripción de la funcionalidad de propiedad. |

### <a name="commands"></a>Comandos:

Puede llamar a los comandos de dispositivo desde IoT Central. Los comandos, opcionalmente, pasan parámetros al dispositivo y reciben una respuesta del dispositivo. Por ejemplo, puede llamar a un comando para reiniciar un dispositivo en 10 segundos.

En la tabla siguiente se muestran las opciones de configuración de una funcionalidad de comando:

| Campo | DESCRIPCIÓN |
| ----- | ----------- |
| Display Name (Nombre para mostrar) | El nombre para mostrar del comando que se usa en los paneles y formularios. |
| NOMBRE | El nombre del comando. IoT Central genera un valor para este campo a partir del nombre para mostrar, pero puede elegir su propio valor si es necesario. |
| Capability Type (Tipo de funcionalidad) | Get-Help |
| Get-Help | SynchronousExecutionType. |
| Comentario | Cualquier comentario sobre la funcionalidad del comando. |
| DESCRIPCIÓN | Una descripción de la funcionalidad del comando: |
| Solicitud | Si está habilitada, una definición del parámetro de solicitud que incluye lo siguiente: nombre, nombre para mostrar, esquema, unidad y unidad de visualización. |
| Response | Si está habilitada, una definición de la respuesta del comando que incluye lo siguiente: nombre, nombre para mostrar, esquema, unidad y unidad de visualización. |

## <a name="manage-an-interface"></a>Administración de una interfaz

Siempre que no haya publicado la interfaz, puede editar las funcionalidades definidas por la interfaz. Una vez publicada la interfaz, deberá crear una nueva versión de la plantilla de dispositivo y controlar la versión de la interfaz para realizar los cambios. Los cambios que no requieren el control de versiones, como nombres para mostrar o unidades, se pueden realizar en la sección **Customize** (Personalizar).

También puede exportar la interfaz como un archivo JSON si desea reutilizarla en otro modelo de funcionalidad.

## <a name="add-cloud-properties"></a>Adición de propiedades de nube

Use las propiedades en la nube para almacenar información acerca de los dispositivos en IoT Central. Las propiedades en la nube no se envían nunca a un dispositivo. Por ejemplo, puede usar las propiedades en la nube para almacenar el nombre del cliente que ha instalado el dispositivo o la fecha del último servicio del dispositivo.

En la tabla siguiente se muestran las opciones de configuración para una propiedad en la nube:

| Campo | DESCRIPCIÓN |
| ----- | ----------- |
| Display Name (Nombre para mostrar) | El nombre para mostrar del valor de propiedad en la nube que se usa en los paneles y formularios. |
| NOMBRE | El nombre de la propiedad en la nube. IoT Central genera un valor para este campo a partir del nombre para mostrar, pero puede elegir su propio valor si es necesario. |
| Semantic Type (Tipo semántico) | El tipo semántico de la propiedad, como la temperatura, el estado o el evento. La elección del tipo semántico determina cuál de los campos siguientes está disponible. |
| Schema | El tipo de datos de la propiedad en la nube, como doble, cadena o vector. Las opciones disponibles vienen determinadas por el tipo semántico. |

## <a name="add-customizations"></a>Adición de personalizaciones

Use las personalizaciones cuando necesite modificar una interfaz importada o agregar características específicas de IoT Central a una funcionalidad. Solo puede personalizar los campos que no interrumpan la compatibilidad de la interfaz. Por ejemplo, puede:

- Personalizar el nombre para mostrar y las unidades de una funcionalidad.
- Agregar un color predeterminado para usarlo cuando el valor aparezca en un gráfico.
- Especificar los valores inicial, mínimo y máximo de una propiedad.

No se puede personalizar el tipo o el nombre de la funcionalidad. Si hay cambios que no se pueden realizar en la sección **Customize** (Personalizar), deberá crear una versión de la plantilla de dispositivo y la interfaz para modificar la funcionalidad.

### <a name="generate-default-views"></a>Generación de vistas predeterminadas

La generación de vistas predeterminadas es una forma rápida de empezar a visualizar la información importante del dispositivo. Puede generar hasta tres vistas predeterminadas para la plantilla de dispositivo:

1. **Commands** (Comandos) proporciona una vista con los comandos del dispositivo y permite que el operador los envíe al dispositivo.
1. **Overview** (Información general) proporciona una vista con la telemetría del dispositivo, mostrando gráficos y métricas.
1. **About** (Acerca de) proporciona una vista con información del dispositivo, que muestra sus propiedades.

Una vez que haya seleccionado **Generate default views** (Generar vistas predeterminadas), verá que se han agregado automáticamente en la sección **Views** (Vistas) de la plantilla de dispositivo.

## <a name="add-dashboards"></a>Adición de paneles

Agregue paneles a una plantilla de dispositivo para permitir que los operadores visualicen un dispositivo mediante gráficos y métricas. Puede tener varios paneles para una plantilla de dispositivo.

Para agregar un panel a una plantilla de dispositivo:

1. Vaya a la plantilla de dispositivo y seleccione **Views** (Vistas).
1. Después, elija **Visualizing the Device** (Visualización del dispositivo).
1. Escriba un nombre para el panel en **Dashboard Name** (Nombre del panel).
1. Agregue iconos al panel en la lista de iconos estáticos, de propiedades, de propiedades en la nube, de telemetría y de comandos. Arrastre y coloque los iconos que le gustaría agregar al panel.
1. Para trazar varios valores de telemetría en un único icono de gráfico, seleccione los valores de telemetría y, a continuación, seleccione **Combine** (Combinar).
1. Configure los iconos que agrega para personalizar cómo muestra los datos. Para ello, seleccione el icono de engranaje o seleccione el botón **Change configuration** (Cambiar configuración) en el icono del gráfico.
1. Organice y cambie el tamaño de los iconos en el panel.
1. Guarde los cambios.

### <a name="configure-preview-device-to-view-dashboard"></a>Configuración del dispositivo de versión preliminar para ver el panel

Para ver y probar el panel, puede seleccionar **Configure preview device** (Configurar dispositivo de versión preliminar), que le permite ver el panel de la misma forma que lo ve el operador después de publicarlo. Esta opción permite validar que las vistas muestran los datos correctos. Puede elegir entre ningún dispositivo de versión preliminar, el dispositivo de prueba real que ha configurado para su plantilla de dispositivo o un dispositivo existente en su aplicación utilizando el identificador de dispositivo.

## <a name="add-forms"></a>Adición de formularios

Agregue formularios a una plantilla de dispositivo para permitir a los operadores administrar un dispositivo con la visualización y configuración de propiedades. Los operadores solo pueden editar las propiedades en la nube y las propiedades del dispositivo que se pueden escribir. Puede tener varios formularios para una plantilla de dispositivo.

Para agregar un formulario a una plantilla de dispositivo:

1. Vaya a la plantilla de dispositivo y seleccione **Views** (Vistas).
1. Después, seleccione **Editing Device and Cloud data** (Editar datos del dispositivo y la nube).
1. Escriba un nombre para el formulario en **Form Name** (Nombre de formulario).
1. Seleccione el número de columnas que se usará para diseñar el formulario.
1. Agregue propiedades a una sección existente del formulario o seleccione las propiedades y elija **Add section** (Agregar sección). Use las secciones para agrupar las propiedades del formulario. Puede agregar un título a una sección.
1. Configure cada propiedad en el formulario para personalizar su comportamiento.
1. Organice las propiedades en el formulario.
1. Guarde los cambios.

## <a name="publish-a-device-template"></a>Publicación de una plantilla de dispositivo

Antes de poder conectar un dispositivo que implementa el modelo de funcionalidad del dispositivo, debe publicar la plantilla del dispositivo.

Después de publicar una plantilla de dispositivo, solo puede realizar cambios limitados en el modelo de funcionalidad del dispositivo. Para modificar una interfaz, debe [crear y publicar una nueva versión](./howto-version-device-template-pnp.md).

Para publicar una plantilla de dispositivo, vaya a su plantilla de dispositivo y seleccione **Publish** (Publicar).

Después de publicar una plantilla de dispositivo, el operador puede ir a la página **Devices** (Dispositivos) y agregar dispositivos reales o simulados que usan la plantilla de dispositivo. Puede seguir modificando y guardando la plantilla de dispositivo mientras realiza los cambios; sin embargo, si desea enviar estos cambios al operador para verlos en la página **Devices** (Dispositivos), debe seleccionar **Publish** (Publicar) cada vez.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar una plantilla de dispositivo en una aplicación de Azure IoT Central puede:

> [!div class="nextstepaction"]
> [Crear una nueva versión de plantilla de dispositivo](howto-version-device-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
> [Conectar un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
