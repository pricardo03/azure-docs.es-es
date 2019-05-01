---
title: Procedimientos recomendados de configuración de dispositivos para Azure IoT Hub | Microsoft Docs
description: Procedimientos recomendados para configurar dispositivos IoT a escala
author: chrisgre
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: c97395981ea3af90c7b0c590cb049fccc7392304
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734837"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Procedimientos recomendados para la configuración de dispositivos en una solución de IoT

La administración automática de dispositivos en Azure IoT Hub automatiza muchas de las tareas repetitivas y complejas de administración de grandes flotas de dispositivos durante su ciclo de vida completo. En este artículo se definen muchos de los procedimientos recomendados para los diversos roles que intervienen en el desarrollo y el funcionamiento de una solución de IoT.

* **Integrador/fabricante de hardware de IoT:** Fabricantes de hardware de IoT, integradores que ensamblan hardware de diversos fabricantes o proveedores que proporcionan hardware para una implementación de IoT que fabrican o integran otros proveedores. Intervienen en el desarrollo y la integración de firmware, sistemas operativos integrados y software integrado.

* **Desarrollador de soluciones de IoT:** El desarrollo de una solución de IoT se suele realizar mediante un programador de soluciones. Este desarrollador puede formar parte de un equipo interno o ejercer como integrador de sistemas especializado en esta actividad. El desarrollador de soluciones de IoT puede desarrollar diversos componentes de la solución de IoT desde cero, integrar varios componentes estándar o de código abierto o personalizar un [acelerador de soluciones de IoT](/azure/iot-accelerators/).

* **Operador de soluciones de IoT:** Después de implementa la solución de IoT, requiere operaciones a largo plazo, supervisión, actualizaciones y mantenimiento. De estas tareas puede encargarse un equipo interno formado por especialistas en tecnología de la información, equipos de operaciones de hardware y mantenimiento y especialistas en dominios que supervisan que la infraestructura de IoT funciona de manera correcta globalmente.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>Administración automática de dispositivos para configurar dispositivos IoT a escala

La administración automática de dispositivos incluye las muchas ventajas de los [dispositivos gemelos](iot-hub-devguide-device-twins.md) y los [módulos gemelos](iot-hub-devguide-module-twins.md) para sincronizar los estados deseados y notificados entre los dispositivos y la nube. [Configuraciones automáticas de dispositivos](iot-hub-auto-device-config.md) automáticamente la actualización de grandes conjuntos de gemelos y resumir el progreso y compatibilidad. En los siguientes pasos generales se describe cómo se desarrolla y usa la administración automática de dispositivos:

* El **integrador o fabricante de hardware de IoT** implementa las características de administración de dispositivos dentro de una aplicación integrada mediante [dispositivos gemelos](iot-hub-devguide-device-twins.md). Estas características pueden incluir actualizaciones de firmware, instalación y actualizaciones de software y administración de configuraciones.

* El **desarrollador de soluciones de IoT** implementa la capa de administración de las operaciones de administración de dispositivos mediante [dispositivos gemelos](iot-hub-devguide-device-twins.md) y [configuraciones automáticas de dispositivos](iot-hub-auto-device-config.md). La solución debe incluir la definición de una interfaz de operador para llevar a cabo las tareas de administración de dispositivos.

* El **operador de soluciones de IoT** usa la solución de IoT para realizar tareas de administración de dispositivos, en concreto, para agrupar los dispositivos, iniciar cambios de configuración (como actualizaciones de firmware), supervisar el progreso y solucionar los problemas que surjan.

## <a name="iot-hardware-manufacturerintegrator"></a>Integrador/fabricante de hardware IoT

Los siguientes procedimientos recomendados van dirigidos a los fabricantes e integrados de hardware que se ocupan del desarrollo de software integrado:

* **Implemente [dispositivos gemelos](iot-hub-devguide-device-twins.md):** Los dispositivos gemelos permiten sincronizar la configuración deseada de la nube y para informar sobre la configuración actual y las propiedades del dispositivo. La mejor manera de implementar dispositivos gemelos en aplicaciones integradas es mediante los [SDK de Azure IoT](https://github.com/Azure/azure-iot-sdks). Los dispositivos gemelos resultan muy adecuados para la configuración debido a que:

    * Admiten la comunicación bidireccional.
    * Permiten estados de dispositivo conectado y desconectado.
    * Siguen el principio de coherencia final.
    * Se pueden consultar completamente en la nube.

* **Estructura del dispositivo gemelo para administración de dispositivos:** El dispositivo gemelo debe estar estructurado de modo que las propiedades de administración de dispositivos se agrupan en secciones. Al hacer esto, los cambios en la configuración se pueden aislar sin afectar a otras secciones del dispositivo gemelo. Por ejemplo, puede crear una sección dentro de las propiedades deseadas para firmware, otra sección para software y una tercera sección para configuración de red. 

* **Atributos de dispositivo de informes que son útiles para la administración de dispositivos:** Marca y modelo, firmware, el sistema operativo, el número de serie de atributos como dispositivo físico y otros identificadores son útiles para crear informes y como parámetros para dirigirse a los cambios de configuración.

* **Definir los principales Estados para los informes de estado y progreso:** Estados de nivel superior se deben enumerar para que se pueden notificar al operador. Por ejemplo, una actualización de firmware notificaría el estado como Actual, Descargando, Aplicando, En curso y Error. Defina campos adicionales para obtener más información sobre cada estado.

## <a name="iot-solution-developer"></a>Desarrollador de soluciones de IoT

Estos son los procedimientos recomendados para los desarrolladores de soluciones de IoT que crean sistemas basados en Azure:

* **Implemente [dispositivos gemelos](iot-hub-devguide-device-twins.md):** Los dispositivos gemelos permiten sincronizar la configuración deseada de la nube y para informar sobre la configuración actual y las propiedades del dispositivo. La mejor manera de implementar dispositivos gemelos dentro de las aplicaciones de las soluciones en la nube es usar los [SDK de Azure IoT](https://github.com/Azure/azure-iot-sdks). Los dispositivos gemelos resultan muy adecuados para la configuración debido a que:

    * Admiten la comunicación bidireccional.
    * Permiten estados de dispositivo conectado y desconectado.
    * Siguen el principio de coherencia final.
    * Se pueden consultar completamente en la nube.

* **Organizar los dispositivos mediante etiquetas del dispositivo gemelo:** La solución debe permitir el operador definir los anillos de calidad o a otros conjuntos de dispositivos basados en diversas estrategias de implementación, como el valor controlado. La organización de los dispositivos se puede implementar dentro de la solución mediante etiquetas de dispositivo gemelo y [consultas](iot-hub-devguide-query-language.md). La organización de los dispositivos es necesaria para permitir implementaciones de configuraciones con precisión y seguridad.

* **Implemente [configuraciones automáticas de dispositivos](iot-hub-auto-device-config.md):** Implementación configuraciones automáticas de dispositivos y configuración del monitor cambia a grandes conjuntos de dispositivos IoT a través de los dispositivos gemelos. Las configuraciones automáticas de dispositivos están dirigidas a conjuntos de dispositivos gemelos mediante la **condición de destino**, que es una consulta basada en etiquetas de dispositivo gemelo o propiedades notificadas. El **contenido de destino** es el conjunto de propiedades deseadas que se establecerán en los dispositivo gemelos dirigidos. El contenido de destino se debe alinear con la estructura de dispositivo gemelo definida por el fabricante o integrador de hardware de IoT.

   Las **métricas** son consultas basadas en las propiedades notificadas del dispositivo gemelo, y se deben alinear también con la estructura de dispositivo gemelo definida por el fabricante o integrador de hardware de IoT. Las configuraciones automáticas de dispositivos también tienen la ventaja de que IoT Hub realiza operaciones de dispositivo gemelo a una velocidad que nunca supera los [límites](iot-hub-devguide-quotas-throttling.md) de lecturas y actualizaciones de dispositivos gemelos.

* **Use la [servicio Device Provisioning](../iot-dps/how-to-manage-enrollments.md):** Los programadores de soluciones deben usar el servicio Device Provisioning para asignar las etiquetas del dispositivo gemelo a nuevos dispositivos, tal que los que se configuran automáticamente por **configuraciones automáticas de dispositivos** que estén destinados a los gemelos con esa etiqueta. 

## <a name="iot-solution-operator"></a>Operador de soluciones de IoT

Los siguientes procedimientos recomendados están destinados a los operadores de soluciones de IoT que usan una solución de IoT creada en Azure:

* **Organizar los dispositivos para la administración:** La solución de IoT debe definir o permitir la creación de anillos de calidad o a otros conjuntos de dispositivos basados en diversas estrategias de implementación, como el valor controlado. Los conjuntos de dispositivos se usarán para implementar los cambios de configuración y realizar otras operaciones de administración de dispositivos a escala.

* **Realice los cambios de configuración mediante una implementación por fases:**  Por fases es un proceso general que un operador implementa cambios en un amplio conjunto de dispositivos IoT. El objetivo consiste en realizar cambios gradualmente para reducir el riesgo de realizar cambios importantes a gran escala.  El operador debe usar la interfaz de la solución para crear una [configuración automática de dispositivos](iot-hub-auto-device-config.md), y la condición de destino debe tener como destino un conjunto inicial de dispositivos (por ejemplo, un grupo de valores controlados). El operador debe validar entonces el cambio de configuración en el conjunto inicial de dispositivos.

   Una vez completada la validación, el operador actualizará la configuración automática de dispositivos para que incluya un conjunto mayor de dispositivos. El operador también debe establecer la configuración con una prioridad más alta que otras configuraciones dirigidas actualmente a esos dispositivos. La implementación se puede supervisar mediante las métricas notificadas por la configuración automática de dispositivos.

* **Realice las reversiones en el caso de errores o configuraciones incorrectas:**  Configuración automática de dispositivos que provoca errores o configuraciones incorrectas se puede revertir cambiando el **condición de destino** para que los dispositivos ya no cumplen la condición de destino. Asegúrese de que otra configuración automática de dispositivos de prioridad inferior todavía está dirigida a esos dispositivos. Compruebe que la reversión se realizó correctamente mediante la visualización de las métricas: La configuración de revertida ya no debe mostrar el estado de dispositivos sin destino y las métricas de la configuración del segundo deben incluir ahora recuentos para los dispositivos que aún tienen como destino.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda sobre la implementación de dispositivos gemelos en [Dispositivos gemelos de IoT Hub](iot-hub-devguide-device-twins.md).

* Siga los pasos para crear, actualizar o eliminar una configuración automática de dispositivos en [Configuración y supervisión de dispositivos IoT a escala](iot-hub-auto-device-config.md).

* Implementar un patrón de actualización de firmware mediante dispositivos gemelos y configuraciones automáticas de dispositivos en [Tutorial: Implementar un proceso de actualización de firmware de dispositivo](tutorial-firmware-update.md).
