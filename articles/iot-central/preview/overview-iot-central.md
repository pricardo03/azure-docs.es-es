---
title: Introducción a Azure IoT Central | Microsoft Docs
description: Azure IoT Central es una plataforma de aplicaciones de IoT que simplifica la creación de soluciones de IoT y ayuda a reducir la carga y el costo de las operaciones de administración de IoT y el desarrollo. En este artículo se proporciona información general sobre las características de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 12/10/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 52504fb8333b286407b3f2df8f962da59b80ac53
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434846"
---
# <a name="what-is-azure-iot-central-preview-features"></a>¿Qué es Azure IoT Central (características en versión preliminar)?

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

> [!WARNING]
> Las funcionalidades de [IoT Plug and Play](../../iot-pnp/overview-iot-plug-and-play.md) en Azure IoT Central se encuentran actualmente en versión preliminar pública. No use una IoT Central [plantilla de aplicación](../core/concepts-app-templates.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) habilitada para IoT Plug and Play para cargas de trabajo de producción. En entornos de producción, use una aplicación de IoT central creada a partir de una [plantilla de aplicación](../core/concepts-app-templates.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) actual, disponible con carácter general.

IoT Central es una plataforma de aplicaciones de IoT que reduce la carga y el costo del desarrollo, la administración y el mantenimiento de soluciones de IoT de nivel empresarial. La elección de compilar con IoT Central ofrece la oportunidad de centrar el tiempo, el dinero y la energía en transformar su negocio con datos de IoT, en lugar de simplemente mantener y actualizar una infraestructura de IoT compleja y continuamente en constante evolución.

La interfaz de usuario web le permite supervisar las condiciones del dispositivo, crear reglas y administrar millones de dispositivos y sus datos a lo largo de su ciclo de vida. Además, le permite actuar sobre la información del dispositivo mediante la extensión de IoT Intelligence en aplicaciones de línea de negocio.

Para IoT Central, en este artículo se describe lo siguiente:

- Los roles típicos asociados a un proyecto.
- Cómo crear una aplicación.
- Cómo conectar los dispositivos a la aplicación.
- Cómo administrar una aplicación.
- Azure IoT Edge funcionalidades en IoT Central.
- Cómo conectar los dispositivos del entorno de ejecución de Azure IoT Edge a la aplicación.

## <a name="known-issues"></a>Problemas conocidos

> [!Note]
> Estos problemas conocidos solo se aplican a las aplicaciones de versión preliminar de IoT Central.

- La exportación continua de datos no admite el formato Avro (incompatibilidad).
- GeoJSON no se admite actualmente.
- El icono del mapa no se admite actualmente.
- Los trabajos no admiten tipos complejos.
- No se admiten los tipos de esquema de matriz.
- Solo se admiten el SDK de dispositivos de C y los SDK de dispositivos y servicios de Node.js.
- Solo disponible en ubicaciones de Estados Unidos y Europa.
- Los modelos de funcionalidad del dispositivo deben tener todas las interfaces definidas insertadas en el mismo archivo.

## <a name="personas"></a>Personas

La documentación IoT Central hace referencia a cuatro personas que interactúan con una aplicación IoT Central:

- Un _generador de soluciones_ es responsable de definir los tipos de dispositivos que se conectan a la aplicación y de personalizar la aplicación para el operador.
- Un _operador_, que administra los dispositivos conectados a la aplicación.
- Un _administrador_ es responsable de las tareas administrativas, como la administración de [roles de usuario y permisos](howto-administer.md) dentro de la aplicación.
- Un _desarrollador de dispositivos_ crea el código que se ejecuta en un dispositivo o un módulo de IoT Edge conectado a la aplicación.

## <a name="create-your-iot-central-application"></a>Creación de la aplicación IoT Central

Como generador de soluciones, use IoT Central para crear una solución de IoT personalizada hospedada en la nube para su organización. Normalmente, una solución de personalizada IoT consta de:

- Una aplicación basada en la nube que recibe los datos de telemetría de los dispositivos y le permite administrar los dispositivos.
- Varios dispositivos que ejecutan código personalizado y que están conectados a la aplicación en la nube.

Puede implementar rápidamente una aplicación IoT Central nueva y, luego, personalizarla según sus requisitos específicos en el explorador. Como generador de soluciones, se usan las herramientas web para crear una _plantilla de dispositivo_ para los dispositivos que se conectan a la aplicación. Una plantilla de dispositivo es el plano técnico que define las características y el comportamiento de un tipo de dispositivo, por ejemplo:

- La telemetría que envía.
- Las propiedades empresariales que un operador puede modificar.
- Las propiedades del dispositivo que se establecen mediante un dispositivo y que son de solo lectura en la aplicación.
- Propiedades, que un operador establece, que determinan el comportamiento del dispositivo.

Esta plantilla de dispositivo incluye:

- Un _modelo de funcionalidad del dispositivo_ que describe las funcionalidades que debe implementar un dispositivo, como los datos de telemetría que envía y las propiedades que notifica.
- Propiedades de la nube que no están almacenadas en el dispositivo.
- Personalizaciones, paneles y formularios que forman parte de la aplicación IoT Central.

### <a name="create-device-templates"></a>Creación de plantillas de dispositivo

[IoT Plug and Play](../../iot-pnp/overview-iot-plug-and-play.md) permite a IoT Central integrar dispositivos sin necesidad de escribir ningún código de dispositivo integrado. En el núcleo de IoT Plug and Play se encuentra un esquema de modelo de funcionalidad del dispositivo que describe dichas funcionalidades. En una aplicación de la versión preliminar de IoT Central, las plantillas de dispositivo usan estos modelos de funcionalidad de dispositivo de IoT Plug and Play.

Como generador de soluciones, tiene varias opciones para crear plantillas de dispositivo:

- Diseñe la plantilla de dispositivo en IoT Central y, después, implemente el modelo de funcionalidad del dispositivo en el código del dispositivo.
- Importe un modelo de funcionalidad del dispositivo desde el [catálogo de dispositivos Azure Certified for IoT](https://aka.ms/iotdevcat) y, a continuación, agregue las propiedades, las personalizaciones y los paneles en la nube que necesite la aplicación de IoT Central.
- Cree un modelo de funcionalidad del dispositivo mediante Visual Studio Code. Implemente el código del dispositivo desde el modelo y conecte el dispositivo a la aplicación de IoT Central. IoT Central encuentra el modelo de funcionalidad del dispositivo en un repositorio y crea automáticamente una plantilla de dispositivo simple.
- Cree un modelo de funcionalidad del dispositivo mediante Visual Studio Code. Implemente el código del dispositivo a partir del modelo. Importe manualmente el modelo de funcionalidad del dispositivo en la aplicación de IoT Central y, a continuación, agregue las propiedades, las personalizaciones y los paneles en la nube que necesite la aplicación de IoT Central.

Como generador de soluciones, puede usar IoT Central para generar código para que los dispositivos de prueba validen las plantillas de dispositivo.

### <a name="customize-the-ui"></a>Personalización de la interfaz de usuario

Como generador de soluciones, también puede personalizar la interfaz de usuario de la aplicación IoT Central para los operadores responsables del uso cotidiano de la aplicación. Entre las personalizaciones que puede crear un generador de soluciones se incluyen:

- Definir el diseño de las propiedades y la configuración en una plantilla de dispositivo.
- Configurar paneles personalizados para ayudar a los operadores a descubrir información y resolver los problemas con mayor rapidez.
- Configurar análisis personalizados para explorar los datos de series temporales de los dispositivos conectados.

## <a name="connect-your-devices"></a>Conectar dispositivos

Después de que el compilador define los tipos de dispositivos que pueden conectarse a la aplicación, un desarrollador de dispositivo crea el código que se ejecuta en los dispositivos. Como desarrollador de dispositivo, usará los [SDK de Azure IoT](https://github.com/Azure/azure-iot-sdks) de Microsoft, de código abierto, para crear el código del dispositivo. Estos SDK tienen una amplia compatibilidad con el lenguaje, la plataforma y el protocolo para satisfacer sus necesidades de conexión de los dispositivos a la aplicación IoT Central. Los SDK le ayudan a implementar las siguientes funcionalidades de los dispositivos:

- Crear una conexión segura.
- Enviar datos de telemetría.
- Crear informes de estado.
- Recibir actualizaciones de configuración.

Para más información, consulte la entrada de blog [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don't](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Ventajas de usar los SDK de Azure IoT y riesgos que hay que evitar si no lo hace).

### <a name="azure-iot-edge-devices"></a>Dispositivos de Azure IoT Edge

Además de los dispositivos creados con el [SDK de IoT de Azure](https://github.com/Azure/azure-iot-sdks), también puede conectar [dispositivos Azure IoT Edge](../../iot-edge/about-iot-edge.md) a una aplicación IoT Central. Azure IoT Edge permite ejecutar la inteligencia de nube y la lógica personalizada directamente en dispositivos IoT administrados por IoT Central. El tiempo de ejecución de IoT Edge permite:

- Instalación y actualización de las cargas de trabajo en el dispositivo.
- Mantenimiento de los estándares de seguridad de Azure IoT Edge en el dispositivo.
- Garantía de que los módulos de IoT Edge están siempre en ejecución.
- Notificación del mantenimiento del módulo a la nube para la supervisión remota.
- Administración de la comunicación entre los dispositivos hoja descendentes y un dispositivo IoT Edge, entre los módulos de un dispositivo IoT Edge y entre un dispositivo IoT Edge y la nube.

Para más información, consulte [dispositivos Azure IoT Edge e IoT Central](./concepts-architecture.md#azure-iot-edge-devices).

## <a name="manage-your-application"></a>Administración de una aplicación

Las aplicaciones IoT Central están totalmente hospedadas por Microsoft, lo que reduce la sobrecarga de administración que suponen las aplicaciones.

Como operador, se usa la aplicación IoT Central para administrar los dispositivos de la solución de IoT Central. Los operadores realizan tareas como:

- Supervisar los dispositivos conectados a la aplicación.
- Solucionar problemas y errores de los dispositivos.
- Aprovisionar dispositivos nuevos.

Como generador de soluciones, puede definir reglas y acciones personalizadas que funcionan a través de la transmisión de datos desde dispositivos conectados. Un operador puede habilitar o deshabilitar estas reglas en el nivel de dispositivo para controlar y automatizar las tareas dentro de la aplicación.

Los administradores administran el acceso a la aplicación con [permisos y roles de usuario](howto-administer.md).

## <a name="quotas"></a>Cuotas

Cada suscripción de Azure tiene cuotas predeterminadas que pueden afectar al ámbito de la solución de IoT. Actualmente, IoT Central limita el número de aplicaciones que se pueden implementar en una suscripción a 10. Si quiere aumentar este límite, póngase en contacto con el [soporte técnico de Microsoft](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una visión general de IoT Central, estos son los siguientes pasos sugeridos:

- Conocer las [tecnologías y servicios de Azure disponibles para crear soluciones de IoT](../../iot-fundamentals/iot-services-and-technologies.md).
- Familiarizarse con la [interfaz de usuario de Azure IoT Central](overview-iot-central-tour.md).
- [Crear una aplicación de Azure IoT Central](quick-deploy-iot-central.md) para empezar a trabajar.
- Más información acerca de [IoT Plug and Play](../../iot-pnp/overview-iot-plug-and-play.md)
- Más información sobre cómo [Crear una plantilla de dispositivo Azure IoT Edge](./tutorial-define-edge-device-type.md)
