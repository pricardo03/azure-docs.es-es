---
title: Introducción a Azure IoT Central | Microsoft Docs
description: Azure IoT Central es una plataforma de aplicaciones de IoT que simplifica la creación de soluciones de IoT. En este artículo se proporciona información general sobre las características de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 08/26/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 30e7b4c39c24f4271c53f7a9f8940e4fb3c2e298
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048670"
---
<!---
Purpose of an Overview article: 
1. To give a TECHNICAL overview of a service/product: What is it? Why should I use it? It's a "learn" topic that describes key benefits and our competitive advantage. It's not a "do" topic.
2. To help audiences who are new to service but who may be familiar with related concepts. 
3. To compare the service to another service/product that has some similar functionality, ex. SQL Database / SQL Data Warehouse, if appropriate. This info can be in a short list or table. 
-->

# <a name="what-is-azure-iot-central"></a>¿Qué es Azure IoT Central?

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Azure IoT Central es una plataforma de aplicaciones de IoT que reduce la carga y el costo asociado con el desarrollo, la administración y el mantenimiento de soluciones de IoT de nivel empresarial. La elección de compilar con Azure IoT Central ofrece la oportunidad de centrar su tiempo, dinero y energía en transformar su negocio con datos de IoT, en lugar de simplemente mantener y actualizar una infraestructura de IoT compleja y continuamente en constante evolución.

La interfaz fácil de usar facilita la supervisión de las condiciones de los dispositivos, la creación de reglas y la administración de millones de dispositivos y sus datos a lo largo de su ciclo de vida. Además, le permite actuar sobre la información del dispositivo mediante la extensión de IoT Intelligence en aplicaciones de línea de negocio.

Para Azure IoT Central, en este artículo se describe lo siguiente:

- Los roles típicos asociados a un proyecto.
- Cómo crear una aplicación.
- Cómo conectar los dispositivos a la aplicación.
- Cómo administrar una aplicación.

## <a name="personas"></a>Personas

La documentación de Azure IoT Central hace referencia a los cuatro roles que interactúan con una aplicación de Azure IoT Central:

- Un _compilador_, que es responsable de definir los tipos de dispositivos que se conectan a la aplicación y de personalizar la aplicación para el operador.
- Un _operador_, que administra los dispositivos conectados a la aplicación.
- Un _administrador_ es responsable de tareas como la administración de [roles de usuario y permisos](howto-administer.md) dentro de la aplicación.
- Un _programador de dispositivos_, que crea el código que se ejecuta en un dispositivo conectado a la aplicación.

## <a name="create-your-azure-iot-central-application"></a>Creación de una aplicación de Azure IoT Central

Como compilador, usará Azure IoT Central para crear una solución de IoT personalizada y hospedada en la nube para su organización. Normalmente, una solución de personalizada IoT consta de:

- Una aplicación basada en la nube que recibe los datos de telemetría de los dispositivos y le permite administrar los dispositivos.
- Varios dispositivos que ejecutan código personalizado y que están conectados a la aplicación en la nube.

Puede implementar rápidamente una nueva aplicación de Azure IoT Central y, después, ajustarla a sus requisitos específicos en el explorador. Como compilador, use herramientas web para crear una _plantilla de dispositivo_ para los dispositivos que se conectan a la aplicación. Una plantilla de dispositivo es el plano técnico que define las características y el comportamiento de un tipo de dispositivo, por ejemplo:

- La telemetría que envía.
- Las propiedades empresariales que un operador puede modificar.
- Las propiedades del dispositivo que se establecen mediante un dispositivo y que son de solo lectura en la aplicación.
- Los umbrales en los que la aplicación responde.
- La configuración que determina el comportamiento del dispositivo.

Puede probar inmediatamente sus plantillas de dispositivo y la aplicación con datos simulados que Azure IoT Central genera automáticamente.

Como compilador, también puede personalizar la interfaz de usuario de la aplicación de Azure IoT Central para los operadores responsables del uso diario de la aplicación. Las personalizaciones que un compilador puede realizar son:

- Definir el diseño de las propiedades y la configuración en una plantilla de dispositivo.
- Configurar paneles personalizados para ayudar a los operadores a descubrir información y resolver los problemas con mayor rapidez.
- Configurar análisis personalizados para explorar los datos de series temporales de los dispositivos conectados.

## <a name="connect-your-devices"></a>Conectar dispositivos

Después de que el compilador define los tipos de dispositivos que pueden conectarse a la aplicación, un desarrollador de dispositivo crea el código que se ejecuta en los dispositivos. Como desarrollador de dispositivo, usará los [SDK de Azure IoT](https://github.com/Azure/azure-iot-sdks) de Microsoft, de código abierto, para crear el código del dispositivo. Estos SDK admiten numerosos lenguajes, plataformas y protocolos para satisfacer cualquier necesidad de conexión de dispositivos a una aplicación de Azure IoT Central. Los SDK le ayudan a implementar las siguientes funcionalidades de los dispositivos:

- Crear una conexión segura.
- Enviar datos de telemetría.
- Crear informes de estado.
- Recibir actualizaciones de configuración.

Para más información, consulte la entrada de blog [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don't](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Ventajas de usar los SDK de Azure IoT y riesgos que hay que evitar si no lo hace).

## <a name="manage-your-application"></a>Administración de una aplicación

Microsoft hospeda completamente las aplicaciones de Azure IoT Central, lo que reduce la sobrecarga de administración de las aplicaciones.

Como operador, la aplicación de Azure IoT Central se utiliza para administrar los dispositivos de su solución de Azure IoT Central. Los operadores realizan tareas como:

- Supervisar los dispositivos conectados a la aplicación.
- Solucionar problemas y errores de los dispositivos.
- Aprovisionar dispositivos nuevos.

Como compilador, puede definir reglas personalizadas y acciones que se aplican a la transmisión de datos desde los dispositivos conectados. Un operador puede habilitar o deshabilitar estas reglas en el nivel de dispositivo para controlar y automatizar las tareas dentro de la aplicación.

Los administradores administran el acceso a la aplicación con [permisos y roles de usuario](howto-administer.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya tiene información general de Azure IoT Central, estos son los próximos pasos sugeridos:

- Comprender las diferencias entre [Azure IoT Central y los aceleradores de soluciones de IoT de Azure](overview-iot-options.md).
- Familiarizarse con la [interfaz de usuario de Azure IoT Central](overview-iot-central-tour.md).
- [Crear una aplicación de Azure IoT Central](quick-deploy-iot-central.md) para empezar a trabajar.
- Esta serie de tutoriales, le muestran cómo:
  - [Como compilador, crear una plantilla de dispositivo](tutorial-define-device-type.md)
  - [Como compilador, agregar reglas para automatizar la solución](tutorial-configure-rules.md)
  - [Como compilador, personalizar la aplicación para los operadores](tutorial-customize-operator.md)
  - [Como operador, supervisar los dispositivos](tutorial-monitor-devices.md)
  - [Como operador, agregar un dispositivo real a la solución](tutorial-add-device.md)
  - [Como desarrollador de dispositivos, crear código para los dispositivos](tutorial-add-device.md#prepare-the-client-code)
