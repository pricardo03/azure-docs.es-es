---
title: Introducción a los aceleradores de soluciones de Azure IoT | Microsoft Docs
description: Información acerca de los aceleradores de soluciones de IoT de Azure. Los aceleradores de soluciones de IoT son elementos completos y preparados para implementar soluciones de IoT.
author: dominicbetts
ms.author: dobett
ms.date: 06/07/2018
ms.topic: overview
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: timlt
ms.openlocfilehash: 2a4f0b035ce80809a678731a50921791fc0db928
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097995"
---
# <a name="what-are-azure-iot-solution-accelerators"></a>¿Qué son los aceleradores de soluciones de Azure IoT?

Una solución de IoT en la nube normalmente usa varios servicios basados en la nube y código personalizado para administrar la conectividad de los dispositivos, el procesamiento y el análisis de los datos, y su presentación.

Los aceleradores de soluciones de IoT son una colección de soluciones de IoT completas y listas para implementarse, que abordan escenarios comunes de IoT, como la supervisión remota, la fábrica conectada y el mantenimiento predictivo. Al implementar un acelerador de soluciones, la implementación incluye todos los servicios basados en la nube necesarios, junto con el código de aplicación requerido.

Los aceleradores de soluciones son puntos de partida para sus propias soluciones de IoT. El código fuente para todos los aceleradores de soluciones es código abierto y está disponible en GitHub. Le recomendamos que descargue y [personalice](iot-accelerators-remote-monitoring-customize.md) los aceleradores de soluciones para cumplir sus requisitos.

También puede utilizarlos como herramientas de aprendizaje antes de crear una solución de IoT personalizada desde cero. Implementan prácticas probadas para soluciones de IoT basadas en la nube de modo que pueda realizar su seguimiento.

El código de aplicación en cada acelerador de soluciones incluye un panel que le permite administrar dicho acelerador. Por ejemplo, puede usar un panel para ver los datos de telemetría desde los dispositivos conectados, aprovisionar los dispositivos nuevos o actualizar el firmware en los dispositivos conectados:

[![Panel de soluciones](./media/about-iot-accelerators/dashboard-inline.png)](./media/about-iot-accelerators/dashboard-expanded.png#lightbox)

## <a name="supported-iot-scenarios"></a>Escenarios de IoT admitidos

Actualmente, hay cuatro aceleradores de soluciones disponibles para implementar:

### <a name="remote-monitoring"></a>Supervisión remota

Use este acelerador de soluciones a fin de recopilar datos de telemetría procedentes de varios dispositivos remotos y para controlarlos. Algunos dispositivos de ejemplo serían los sistemas de refrigeración instalados en las instalaciones de los clientes o las válvulas instaladas en estaciones de bombeo remotas.

### <a name="connected-factory"></a>Fábrica conectada

Use este acelerador de soluciones para recopilar datos de telemetría de recursos industriales con una interfaz [OPC Unified Architecture](https://opcfoundation.org/about/opc-technologies/opc-ua/) y para controlarlos. Los recursos industriales podrían incluir estaciones de prueba y ensamblado en una línea de producción de una fábrica.

### <a name="predictive-maintenance"></a>Mantenimiento predictivo

Use este acelerador de soluciones para predecir cuándo se espera que un dispositivo remoto sufra un error, de modo que pueda realizar tareas de mantenimiento antes de que ocurra el error predicho. Este acelerador de soluciones utiliza algoritmos de aprendizaje automático para predecir los errores de la telemetría de los dispositivos. Algunos dispositivos de ejemplo podrían ser los motores de un avión o los ascensores.

### <a name="device-simulation"></a>Simulación de dispositivo

Use este acelerador de soluciones para ejecutar varios dispositivos simulados que generen datos de telemetría realistas. Puede utilizar este acelerador de soluciones para probar el comportamiento de los demás aceleradores de soluciones o para probar sus propias soluciones personalizadas de IoT.

## <a name="design-principles"></a>Principios de diseño

Todos los aceleradores de soluciones siguen los mismos principios de diseño y objetivos. Están diseñados para ser:

* **Escalables**, lo que le permite conectar y administrar millones de dispositivos conectados.
* **Extensibles**, lo que le permite personalizarlos para satisfacer sus requisitos.
* **Comprensibles**, lo que le permite comprender cómo funcionan y cómo se implementan.
* **Modulares**, lo que le permite intercambiar servicios alternativos.
* **Seguros**, combinando la seguridad de Azure con características de seguridad integradas de dispositivos y de conectividad.

## <a name="architectures-and-languages"></a>Arquitecturas y lenguajes

Los aceleradores de soluciones originales se escribieron con .NET usando una arquitectura Model-View-Controller (MVC). Microsoft está actualizando los aceleradores de soluciones con una nueva arquitectura basada en microservicios. Tanto las versiones [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) como [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) de cada microservicio están disponibles en GitHub. En la siguiente tabla se muestra el estado actual de los aceleradores de soluciones:

| Acelerador de soluciones   | Arquitectura  | Lenguajes     |
| ---------------------- | ------------- | ------------- |
| Supervisión remota      | Microservicios | Java y .NET |
| Mantenimiento predictivo | MVC           | .NET          |
| Fábrica conectada      | MVC           | .NET          |

Para más información sobre las arquitecturas de microservicios, consulte [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (Arquitectura de la aplicación .NET) y [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservicios: una revolución de aplicaciones con la tecnología de la nube).

## <a name="deployment-options"></a>Opciones de implementación

Puede implementar los aceleradores de soluciones basados en microservicios en las siguientes configuraciones:

* **Estándar:** implementación ampliada de la infraestructura para desarrollar una implementación de producción. Azure Container Service implementa los microservicios en varias máquinas virtuales de Azure. Kubernetes orquesta los contenedores de Docker que hospedan los microservicios individuales.
* **Básica:** versión de menor costo para ver una demostración o probar una implementación. Todos los microservicios se implementan en una máquina virtual de Azure.
* **Local:** implementación de la máquina local para desarrollo y pruebas. Este enfoque implementa los microservicios en un contenedor Docker local y se conecta a IoT Hub, Azure Cosmos DB y los servicios de almacenamiento de Azure en la nube.

El costo que supone ejecutar un acelerador de soluciones es una suma del costo de los [servicios de Azure subyacentes](https://azure.microsoft.com/pricing). Consulte los detalles de los servicios de Azure utilizados al elegir las opciones de implementación.

## <a name="next-steps"></a>Pasos siguientes

Para probar un acelerador de soluciones de IoT, consulte la guía de inicio rápido [Implementación de una solución de supervisión remota basada en la nube](quickstart-remote-monitoring-deploy.md).
