---
title: 'Sistemas operativos y motores de contenedor compatibles: Azure IoT Edge | Microsoft Docs'
description: Obtenga información sobre qué sistemas operativos pueden ejecutar el demonio y el entorno de ejecución de Azure IoT Edge y los motores de contenedor admitidos para los dispositivos de producción
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/18/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5bc133e81f9917aafb406a6bfb27922cdba48ef5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612222"
---
# <a name="azure-iot-edge-supported-systems"></a>Sistemas compatibles con Azure IoT Edge

Hay varias maneras de buscar compatibilidad para el producto de Azure IoT Edge.

**Notificación de errores**: la mayor parte del desarrollo que lleva el producto de Azure IoT Edge se produce en el proyecto de código abierto de IoT Edge. Los errores se pueden notificar en la [página de problemas](https://github.com/azure/iotedge/issues) del proyecto. Las correcciones pasan pronto del proyecto a las actualizaciones de producto.

**Equipo de soporte técnico al cliente de Microsoft**: los usuarios que dispongan de un [plan de soporte técnico](https://azure.microsoft.com/support/plans/) pueden ponerse en contacto con este equipo creando una incidencia de soporte técnico directamente en [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Solicitudes de características**: el producto Azure IoT Edge realiza un seguimiento de las solicitudes de características a través de la [página de voz del usuario](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Motores de contenedor
Azure IoT Edge necesita un motor de contenedores para iniciar los módulos desde el momento en que se implementan como contenedores. Microsoft ofrece un motor de contenedor, moby-engine, para satisfacer este requisito. Se basa en el proyecto de código abierto de Moby. Docker CE y Docker EE son otros motores de contenedores conocidos. También se basan en el proyecto de código abierto de Moby y son compatibles con Azure IoT Edge. Microsoft proporciona el mejor soporte técnico posible para los sistemas que usan esos motores de contenedores pero, sin embargo, no puede proporcionar soluciones para los problemas de ellos. Por esta razón, Microsoft recomienda el uso de moby-engine en sistemas de producción.

<br>
<center>

![MOBY como en tiempo de ejecución del contenedor](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>Sistemas operativos
Azure IoT Edge se ejecuta en la mayoría de los sistemas operativos que se pueden ejecutar contenedores; Sin embargo, todos estos sistemas son igualmente incompatibles. Los sistemas operativos se agrupan en niveles que representan el nivel de compatibilidad que los usuarios pueden esperar.
* Los sistemas de nivel 1 se pueden considerar como oficialmente compatibles. Para los sistemas de nivel 1, Microsoft:
    * tiene este sistema operativo en pruebas automatizadas
    * proporciona paquetes de instalación para ellos
* Los sistemas de nivel 2 se pueden considerar como compatible con Azure IoT Edge y pueden usarse con relativa facilidad. Para los sistemas de nivel 2:
    * Microsoft ha realizado pruebas ad hoc en las plataformas o sabe de un asociado que se ejecutan correctamente Azure IoT Edge en la plataforma
    * Los paquetes de información de otras plataformas pueden funcionar en estas plataformas
    
La familia del sistema operativo host siempre debe coincidir con la familia del sistema operativo invitado que se utilice en el contenedor de un módulo. En otras palabras, solo se pueden usar contenedores de Linux en Linux y contenedores de Windows en Windows. Cuando se usa Windows, el proceso solo se admiten contenedores aislados, no en Hyper-V aislado contenedores.  

<br>
<center>

![Sistema operativo invitado coincide con el sistema operativo del host](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Nivel 1
Disponibilidad general

| Sistema operativo | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Raspbian-stretch | Sin  | Sí|
| Ubuntu Server 16.04 | Sí | Sin  |
| Ubuntu Server 18.04 | Sí | Sin  |

Versión preliminar pública

| Sistema operativo | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core, compilación 17763 | Sí | Sin  |
| Windows 10 IoT Enterprise, compilación 17763 | Sí | Sin  |
| Windows Server 2019 | Sí | Sin  |

Los sistemas operativos de Windows enumerados anteriormente son los requisitos para los dispositivos que ejecutan los contenedores de Windows en Windows. Esta configuración es la única configuración admitida para la producción. Los paquetes de instalación de Azure IoT Edge para Windows permiten el uso de contenedores de Linux en Windows; Sin embargo, esta configuración es únicamente para desarrollo y pruebas. No se permite utilizar contenedores de Linux en Windows en el entorno de producción. En este escenario de desarrollo, puede utilizarse cualquier versión de Windows 10 compilación 14393 y Windows Server 2016 o versiones posteriores.

### <a name="tier-2"></a>Nivel 2

| Sistema operativo | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Sí | Sí |
| Debian 8 | Sí | Sí |
| Debian 9 | Sí | Sí |
| RHEL 7.5 | Sí | Sí |
| Ubuntu 18.04 | Sí | Sí |
| Ubuntu 16.04 | Sí | Sí |
| Wind River 8 | Sí | Sin  |
| Yocto | Sí | Sin  |


## <a name="virtual-machines"></a>Virtual Machines
Azure IoT Edge puede ejecutarse en máquinas virtuales. Uso de una máquina virtual como un IoT Edge dispositivo es común cuando los clientes desean mejorar la infraestructura existente con inteligencia perimetral. La familia del sistema operativo de la máquina virtual host debe coincidir con la familia del sistema operativo invitado que se utiliza en el contenedor de un módulo. Este requisito es el mismo que cuando Azure IoT Edge se ejecuta directamente en un dispositivo. Azure IoT Edge es independiente de la tecnología de virtualización subyacente y funciona en máquinas virtuales que incorporan plataformas como Hyper-V y vSphere.

<br>
<center>

![Azure IoT Edge en una máquina virtual](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Requisitos mínimos del sistema
Azure IoT Edge funciona perfectamente en dispositivos tan pequeños como Raspberry Pi3 y en hardware de nivel de servidor. Elegir el hardware adecuado para su escenario depende de las cargas de trabajo que desea ejecutar. Tomar la decisión final sobre el dispositivo que se va a utilizar puede resultar complicado; sin embargo, puede empezar creando un prototipo de una solución en portátiles o equipos de escritorio tradicionales.

La experiencia que adquiera mientras crea ese prototipo le ayudará a realizar la selección final del dispositivo. Preguntas que debe considerar son: 

* ¿Cuántos módulos están en la carga de trabajo?
* ¿Cuántas capas compartir los contenedores de los módulos?
* ¿En qué idioma se escriben los módulos? 
* ¿Cuántos datos los módulos procesará?
* ¿Los módulos es necesario ningún hardware especializado para acelerar sus cargas de trabajo?
* ¿Cuáles son las características de rendimiento deseado de la solución?
* ¿Cuál es su presupuesto para hardware?
