---
title: Definición de un nuevo tipo de dispositivo IoT en Azure IoT Central | Microsoft Docs
description: Este tutorial le muestra, como desarrollador, cómo crear una plantilla de dispositivo de Azure IoT en la aplicación de Azure IoT Central. Se definen la telemetría, el estado, las propiedades y los comandos del tipo.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 5642ce6065c4b76bdbd6d772c74fed894de0888f
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73892461"
---
# <a name="tutorial-define-a-new-iot-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definición de un nuevo tipo de dispositivo IoT en la aplicación de Azure IoT Central (características en versión preliminar)

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

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Siga este inicio rápido para [crear una aplicación de Azure IoT Central](quick-deploy-iot-central.md).

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

- **Commands** (Comandos) proporciona una vista con los comandos del dispositivo y permite que el operador los envíe al dispositivo.
- **Overview** (Información general) proporciona una vista con la telemetría del dispositivo, mostrando gráficos y métricas.
- **About** (Acerca de) proporciona una vista con información del dispositivo, que muestra sus propiedades.

Una vez que haya seleccionado **Generate default views** (Generar vistas predeterminadas), verá que se han agregado automáticamente en la sección **Views** (Vistas) de la plantilla de dispositivo.

## <a name="add-dashboards"></a>Adición de paneles

Agregue paneles a una plantilla de dispositivo para permitir que los operadores visualicen un dispositivo mediante gráficos y métricas. Puede tener varios paneles para una plantilla de dispositivo.

Para agregar un panel a una plantilla de dispositivo:

- Vaya a la plantilla de dispositivo y seleccione **Views** (Vistas).
- Después, elija **Visualizing the Device** (Visualización del dispositivo).
- Escriba un nombre para el panel en **Dashboard Name** (Nombre del panel).
- Agregue iconos al panel en la lista de iconos estáticos, de propiedades, de propiedades en la nube, de telemetría y de comandos. Arrastre y coloque los iconos que le gustaría agregar al panel.
- Para trazar varios valores de telemetría en un único icono de gráfico, seleccione los valores de telemetría y, a continuación, seleccione **Combine** (Combinar).
- Configure los iconos que agrega para personalizar cómo muestra los datos. Para ello, seleccione el icono de engranaje o seleccione el botón **Change configuration** (Cambiar configuración) en el icono del gráfico.
- Organice y cambie el tamaño de los iconos en el panel.
- Guarde los cambios.

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

Después de publicar una plantilla de dispositivo, solo puede realizar cambios limitados en el modelo de funcionalidad del dispositivo. Para modificar una interfaz, debe [crear y publicar una nueva versión](./howto-version-device-template.md).

Para publicar una plantilla de dispositivo, vaya a su plantilla de dispositivo y seleccione **Publish** (Publicar).

Después de publicar una plantilla de dispositivo, el operador puede ir a la página **Devices** (Dispositivos) y agregar dispositivos reales o simulados que usan la plantilla de dispositivo. Puede seguir modificando y guardando la plantilla de dispositivo mientras realiza los cambios; sin embargo, si desea enviar estos cambios al operador para verlos en la página **Devices** (Dispositivos), debe seleccionar **Publish** (Publicar) cada vez.

## <a name="define-a-new-iot-gateway-device-type-preview-features"></a>Definición de un nuevo tipo de dispositivo de puerta de enlace de IoT (características de vista previa)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Este tutorial le muestra, como desarrollador, cómo usar una plantilla de dispositivo de puerta de enlace para definir un nuevo tipo de dispositivo Azure IoT en la aplicación de Azure IoT Central. 

En esta sección, creará una plantilla de dispositivo **Smart Building**. Un dispositivo de puerta de enlace de edificio inteligente:

* Envía datos de telemetría, como la temperatura y la ocupación.
* Responde a las propiedades que se escriben cuando se actualiza en la nube, como el intervalo de envío de telemetría.
* Responde a comandos como el restablecimiento de la temperatura.
* Permite relaciones con otros modelos de funcionalidad del dispositivo

### <a name="create-iot-device-templates"></a>Creación de plantillas de dispositivo IoT

Va a crear plantillas de dispositivo IoT. 

Haga clic en Device Templates (Plantillas de dispositivo) en el panel izquierdo, haga clic en **+ New** (+ Nueva), seleccione el icono **IoT Device** (Dispositivo IoT) y seleccionen el icono de sensor de ocupación; luego, haga clic en **Next: Customize** (Siguiente: Personalizar).

![Dispositivo IoT](./media/tutorial-define-iot-device-type/gateway-downstream-new.png)

Se le presentará la página de revisión. Haga clic en el botón **Crear**. 

![Dispositivo IoT](./media/tutorial-define-iot-device-type/gateway-downstream-review.png)

Se crea una plantilla de dispositivo. 

![Dispositivo IoT](./media/tutorial-define-iot-device-type/occupancy-sensor.png)

Creará una plantilla de dispositivo para el sensor S1. 

Haga clic en Device Templates (Plantillas de dispositivo) en el panel izquierdo, haga clic en **+ New** (+ Nueva), seleccione el icono **IoT Device** (Dispositivo IoT) y seleccionen el icono de sensor de ocupación; luego, haga clic en **Next: Customize** (Siguiente: Personalizar).

![Dispositivo IoT](./media/tutorial-define-iot-device-type/s1-sensor.png)

Se le presentará la página de revisión. Haga clic en el botón **Crear**. 

![Dispositivo de nivel inferior](./media/tutorial-define-iot-device-type/s1-review.png)

Se crea una plantilla de dispositivo. 

![Dispositivo de nivel inferior](./media/tutorial-define-iot-device-type/s1-template.png)

## <a name="create-an-iot-gateway-device-template"></a>Creación de una plantilla de dispositivo de puerta de enlace de IoT

Puede optar por crear una plantilla de dispositivo de puerta de enlace de IoT. El dispositivo de puerta de enlace tendrá relaciones con dispositivos de nivel inferior que se conecten a IoT Central a través de él. 

### <a name="downstream-device-relationships-with-gateway-device"></a>Relaciones de dispositivos de nivel inferior con el dispositivo de puerta de enlace

Los dispositivos IoT pueden conectarse al dispositivo de puerta de enlace de Azure IoT. 

![Página Central Application (Aplicación central)](./media/tutorial-define-iot-device-type/gatewaypattern.png)

Como desarrollador, puede crear y editar plantillas de dispositivo de puerta de enlace de Azure IoT en la aplicación. Después de publicar una plantilla de dispositivo, puede conectar dispositivos reales que implementan la plantilla de dispositivo.

### <a name="select-device-template-type"></a>Selección del tipo de plantilla de dispositivo 

Para agregar una nueva plantilla de dispositivo a la aplicación, debe ir a la página **Device Templates** (Plantillas de dispositivo). Para ello, seleccione la pestaña **Device Templates** (Plantillas de dispositivo) en el panel izquierdo.

![Página Central Application (Aplicación central)](./media/tutorial-define-iot-device-type/devicetemplate.png)

Haga clic en **+ New** (+ Nueva) para empezar a crear una plantilla de dispositivo.

![Plantillas de dispositivo: nueva](./media/tutorial-define-iot-device-type/devicetemplatenew.png)

![Selección de plantillas de dispositivo: puerta de enlace](./media/tutorial-define-iot-device-type/gateway-review.png)

Llegará a la página de selección del tipo de plantilla de dispositivo. Seleccione el icono **Azure IoT** y haga clic en el botón **Next: Customize** (Siguiente: Personalizar) en la parte inferior.

Active la casilla Gateway (Puerta de enlace) y haga clic en **Create** (Crear). 

![Selección de plantillas de dispositivo: puerta de enlace](./media/tutorial-define-iot-device-type/gateway-customize.png)

En la página de revisión que aparece, haga clic en **Create** (Crear). 

![Plantilla de dispositivo: puerta de enlace](./media/tutorial-define-iot-device-type/gateway-review.png)

Como nombre de la plantilla de puerta de enlace, escriba **Smart Building Gateway**. Haga clic en el icono **Custom** (Personalizar).

Agregue una interfaz estándar **Device Information** (Información del dispositivo).

### <a name="add-relationships"></a>Adición de relaciones

Puede agregar relaciones de nivel inferior a los modelos de funcionalidad de los dispositivos que se conectarán al dispositivo de puerta de enlace.

Cree relaciones con modelos de funcionalidad del dispositivo de nivel inferior. Haga clic en **Guardar**

![Plantilla de dispositivo: puerta de enlace](./media/tutorial-define-iot-device-type/gateway-occupancy-s1-rel.png)

### <a name="add-cloud-properties"></a>Adición de propiedades de nube

Una plantilla de dispositivo puede incluir propiedades de la nube. Las propiedades de la nube solo existen en la aplicación IoT Central y nunca se envían a un dispositivo ni se reciben de él.

1. Seleccione **Cloud Properties** (Propiedades de nube) y, luego, **+ Add Cloud Property** (+ Agregar propiedad de la nube). Use la información de la tabla siguiente para agregar una propiedad de la nube a la plantilla de dispositivo.

    | Display Name (Nombre para mostrar)      | Tipo semántico | Schema |
    | ----------------- | ------------- | ------ |
    | Fecha de la última revisión | None          | Date   |
    | Nombre del cliente     | None          | Cadena |

2. Seleccione **Save** (Guardar) para guardar los cambios.

### <a name="add-customizations"></a>Adición de personalizaciones

Use personalizaciones cuando necesite modificar una interfaz o agregar características específicas de IoT Central a una funcionalidad que no requiera controlar la versión del modelo de funcionalidad del dispositivo. Se pueden personalizar los campos cuando el modelo de funcionalidad esté en estado borrador o publicado. Y solo se pueden personalizar aquellos que no interrumpan la compatibilidad de la interfaz. Por ejemplo, puede:

- Personalizar el nombre para mostrar y las unidades de una funcionalidad.
- Agregar un color predeterminado para usarlo cuando el valor aparezca en un gráfico.
- Especificar los valores inicial, mínimo y máximo de una propiedad.

No se puede personalizar el tipo o el nombre de la funcionalidad. Haga clic en **Guardar**

### <a name="create-views"></a>Creación de vistas

Como desarrollador, puede personalizar la aplicación para mostrar información pertinente sobre el dispositivo sensor ambiental a un operador. Las personalizaciones permiten al operador administrar estos dispositivo conectados a la aplicación. Puede crear dos tipos de vistas para que un operador interactúe con los dispositivos:

* Formularios para ver y editar las propiedades del dispositivo y la nube
* Paneles para visualizar los dispositivos

### <a name="generate-default-views"></a>Generación de vistas predeterminadas

En este tutorial, haga clic en Generate default views (Generar vistas predeterminadas). Se generan los paneles Overview (Información general) y About (Acerca de). 

## <a name="publish-device-template"></a>Publicación de la plantilla de dispositivo

Antes de poder crear un sensor ambiental simulado o de conectar un sensor ambiental real, debe publicar la plantilla de dispositivo.

Para publicar una plantilla de dispositivo, siga estos pasos:

1. Vaya a la plantilla de dispositivo en la página **Device Templates** (Plantillas de dispositivo).

2. Seleccione **Publicar**.

3. En el cuadro de diálogo **Publish a Device Template** (Publicar una plantilla de dispositivo), elija **Publish** (Publicar):

Una vez publicada una plantilla de dispositivo, es visible en la página **Devices** (Dispositivos) y para el operador. En una plantilla de dispositivo publicada, no se puede editar un modelo de funcionalidad del dispositivo sin crear una versión. Sin embargo, puede hacer actualizaciones en las propiedades de la nube, las personalizaciones y las vistas en una plantilla de dispositivo publicada sin control de versiones. Después de realizar los cambios, seleccione **Publish** (Publicar) para enviar esos cambios al operador.

## <a name="create-gateway-simulated-device"></a>Creación de un dispositivo simulado de puerta de enlace

En el explorador de dispositivos, cree una puerta de enlace de edificio inteligente simulada. 

![Plantilla de dispositivo: puerta de enlace](./media/tutorial-define-iot-device-type/smartbuildingdevice.png)

## <a name="create-downstream-simulated-devices"></a>Creación de dispositivos simulados de nivel inferior

En el explorador de dispositivos, cree un sensor de ocupación simulado. 

![Plantilla de dispositivo: ocupación](./media/tutorial-define-iot-device-type/occupancydevice.png)

En el explorador de dispositivos, cree un sensor s1 simulado. 

![Plantilla de dispositivo: s1](./media/tutorial-define-iot-device-type/s1device.png)

## <a name="add-downstream-devices-relationships-to-gateway-device"></a>Adición de relaciones de dispositivos de nivel inferior al dispositivo de puerta de enlace

Seleccione el sensor S1 y el sensor de ocupación y haga clic en **Connect to gateway** (Conectar con la puerta de enlace). 

![Plantilla de dispositivo: s1](./media/tutorial-define-iot-device-type/connecttogateway.png)

Seleccione la plantilla de dispositivo de puerta de enlace y la instancia de dispositivo de puerta de enlace y haga clic en **Join** (Unir).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Crear una puerta de enlace de IoT como plantilla de dispositivo
* Crear propiedades de la nube
* Crear personalizaciones
* Definir una visualización para la telemetría del dispositivo
* Agregar relaciones
* Publicar la plantilla de dispositivo

Este es el siguiente paso recomendado:

> [!div class="nextstepaction"]
> [Conexión de un dispositivo](tutorial-connect-pnp-device.md)
