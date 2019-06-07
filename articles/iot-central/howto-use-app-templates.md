---
title: Uso de plantillas de aplicación en Azure IoT Central | Microsoft Docs
description: Como operador, cómo usar conjuntos de dispositivos en la aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a26f70c5a61f3855a3de991072a7e84103e87b69
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499225"
---
# <a name="use-application-templates"></a>Uso de plantillas de aplicación

Este artículo se describe cómo, como un administrador de soluciones, crear y utilizar plantillas de aplicación.

Cuando se crea una aplicación de Azure IoT Central, tendrá la opción de plantillas de ejemplo integrados. También puede crear sus propias plantillas de aplicación de las aplicaciones existentes de IoT Central. A continuación, puede usar sus propias plantillas de aplicación al crear nuevas aplicaciones.

Cuando se crea una plantilla de aplicación, incluye los siguientes elementos de la aplicación existente:

- El panel de la aplicación de forma predeterminada, incluidos el diseño del panel y todos los iconos que ha definido.
- Plantillas de dispositivo, incluidas las medidas, configuración, propiedades, comandos y panel.
- Reglas. Se incluyen todas las definiciones de regla. Sin embargo, las acciones, excepto por las acciones de correo electrónico, no se incluyen.
- Conjuntos de dispositivos, incluidos sus condiciones y los paneles.

> [!WARNING]
> Si un panel incluye iconos que muestran información acerca de dispositivos específicos, estos iconos muestran **no se encontró el recurso solicitado** en la nueva aplicación. Debe volver a configurar estos iconos para mostrar información acerca de los dispositivos en la nueva aplicación.

Cuando se crea una plantilla de aplicación, no incluye los siguientes elementos:

- Dispositivos
- Usuarios
- Definiciones de trabajos
- Definiciones de exportación continua de datos

Agregar estos elementos manualmente a las aplicaciones que se crea a partir de una plantilla de aplicación.

## <a name="create-an-application-template"></a>Crear una plantilla de aplicación

Para crear una plantilla de aplicación de una aplicación IoT Central existente:

1. Vaya a la **administración** sección en la aplicación.
1. Seleccione **exportación de la plantilla de aplicación**.
1. En el **Exportar plantilla de aplicación** , escriba un nombre y una descripción para la plantilla.
1. Seleccione el **exportar** botón para crear la plantilla de aplicación. Ahora puede copiar el **vínculo compartible** que permite a alguien crear una nueva aplicación de la plantilla:

![Crear una plantilla de aplicación](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>Usar una plantilla de aplicación

Para usar una plantilla de aplicación para crear una nueva aplicación de IoT Central, debe creado anteriormente **que se pueda compartir vínculo**. Pegar la **vínculo compartible** en la barra de direcciones del explorador. El **crear una aplicación** página se muestra con la plantilla de aplicación personalizada seleccionada:

![Crear una aplicación desde una plantilla](media/howto-use-app-templates/create-app.png)

Seleccione el plan de pago y rellene los demás campos del formulario. A continuación, seleccione **crear** para crear una nueva aplicación de IoT Central desde la plantilla de aplicación.

## <a name="manage-application-templates"></a>Administrar plantillas de aplicación

En el **Exportar plantilla de aplicación** página, puede eliminar o actualizar la plantilla de aplicación.

Si elimina una plantilla de aplicación, ya no se puede usar el vínculo que se pueda compartir generado anteriormente para crear nuevas aplicaciones.

Para actualizar la plantilla de aplicación, cambie el nombre de la plantilla o la descripción en el **Exportar plantilla de aplicación** página. A continuación, seleccione el **exportar** nuevamente en el botón. Esta acción genera un nuevo **que se pueda compartir vínculo** e invalida cualquier anterior **que se pueda compartir vínculo** dirección URL.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar plantillas de aplicación, el siguiente paso sugerido es obtener información sobre cómo [administrar IoT Central desde el portal de Azure](howto-manage-iot-central-from-portal.md)
