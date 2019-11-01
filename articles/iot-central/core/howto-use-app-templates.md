---
title: Uso de las plantillas de aplicación en Azure IoT Central | Microsoft Docs
description: Como operador, cómo usar conjuntos de dispositivos en la aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: d682c49aa833b9e11dbbddc5e9f6afd52cbb6e84
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942313"
---
# <a name="use-application-templates"></a>Uso de plantillas de aplicación

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

En este artículo se describe cómo puede crear y utilizar plantillas de aplicación si es administrador de soluciones.

Cuando crea una aplicación de Azure IoT Central, tiene la posibilidad de usar plantillas de ejemplo integradas. También puede crear sus propias plantillas de aplicación a partir de las aplicaciones existentes de IoT Central. A continuación, puede usar sus propias plantillas de aplicación al crear nuevas aplicaciones.

Cuando crea una plantilla de aplicación, esta incluye los siguientes elementos de su aplicación existente:

- El panel de la aplicación predeterminado, incluidos el diseño del panel y todos los iconos que ha definido.
- Las plantillas del dispositivo, incluidas las medidas, la configuración, las propiedades, los comandos y el panel.
- Las reglas. Se incluyen todas las definiciones de las reglas. Sin embargo, no se incluyen las acciones, excepto las de correo electrónico.
- Los conjuntos de dispositivos, incluidas sus condiciones y paneles.

> [!WARNING]
> Si un panel incluye iconos que muestran información acerca de dispositivos específicos, estos iconos muestran el mensaje **No se encontró el recurso solicitado** en la nueva aplicación. Debe volver a configurar estos iconos para mostrar información acerca de los dispositivos en la nueva aplicación.

Cuando crea una plantilla de aplicación, no incluye los siguientes elementos:

- Dispositivos
- Usuarios
- Definiciones de trabajos
- Definiciones de exportaciones de datos continuas

Agregue estos elementos manualmente a cualquier aplicación creada a partir de una plantilla de aplicación.

## <a name="create-an-application-template"></a>Elija una plantilla de aplicación.

Para crear una plantilla de aplicación a partir de una aplicación existente de IoT Central:

1. Vaya a la sección **Administración** en su aplicación.
1. Seleccione **Application Template Export** (Exportación de la plantilla de aplicación).
1. En la página **Application Template Export** (Exportación de la plantilla de aplicación), escriba un nombre y una descripción para la plantilla.
1. Seleccione el botón **Exportar** para crear la plantilla de aplicación. Ahora puede copiar el **vínculo que se puede compartir** que permite que otras personas puedan crear una nueva aplicación a partir de la plantilla:

![Elija una plantilla de aplicación.](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>Uso de una plantilla de aplicación

Para usar una plantilla de aplicación para crear una nueva aplicación de IoT Central, necesita un **vínculo que se puede compartir** que haya creado anteriormente. Pegue el **vínculo que se puede compartir** en la barra de direcciones de su explorador. La página **Crear una aplicación**  se muestra con su plantilla de aplicación personalizada seleccionada:

![Crear una aplicación a partir de una plantilla](media/howto-use-app-templates/create-app.png)

Seleccione el plan de pago y rellene los demás campos del formulario. A continuación, seleccione **Crear** para crear una nueva aplicación de IoT Central a partir de la plantilla de aplicación.

## <a name="manage-application-templates"></a>Administración de las plantillas de aplicaciones

En la página **Application Template Export** (Exportación de la plantilla de aplicación), puede eliminar o actualizar la plantilla de aplicación.

Si elimina una plantilla de aplicación, ya no podrá usar el vínculo que se puede compartir generado anteriormente para crear nuevas aplicaciones.

Para actualizar la plantilla de aplicación, cambie el nombre o la descripción de la plantilla en la página **Application Template Export** (Exportación de la plantilla de aplicación). A continuación, vuelva a seleccionar el botón **Exportar**. Esta acción genera un nuevo **vínculo que se puede compartir** e invalida cualquier URL anterior de **vínculo que se puede compartir vínculo**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar las plantillas de aplicaciones, le sugerimos el paso siguiente para aprender a [administrar IoT Central desde Azure Portal](howto-manage-iot-central-from-portal.md).
