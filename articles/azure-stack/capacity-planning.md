---
title: Planeamiento de capacidad de Azure Stack | Microsoft Docs
description: Obtenga más información sobre el planeamiento de capacidad para las implementaciones de Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.openlocfilehash: ad76e19e324c29c277e72f1e93d2b505984b50ba
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369769"
---
# <a name="azure-stack-capacity-planning"></a>Planeamiento de capacidad de Azure Stack
Al evaluar una solución de Azure Stack, existen opciones de configuración de hardware que tienen un impacto directo en la capacidad total de la nube de Azure Stack. Estas son las opciones clásicas de CPU, densidad de memoria, configuración de almacenamiento y escala de solución global o número de servidores. A diferencia de una solución de virtualización tradicional, la aritmética simple de estos componentes no es aplicable a la hora de determinar la capacidad utilizable. El primer motivo es que Azure Stack se ha diseñado para hospedar los componentes de infraestructura o administración dentro de la propia solución. El segundo motivo es que parte de la capacidad de la solución está reservada en apoyo de resistencia; la actualización del software de la solución de forma que minimiza la interrupción de las cargas de trabajo de inquilino.

> [!IMPORTANT]
> La información del planeamiento de capacidad que se ha proporcionado, así como la hoja de cálculo que lo acompaña, pretenden ser únicamente una guía que le ayudará a tomar decisiones de configuración y planeamiento de Azure Stack. No están pensados para que actúen como un sustituto de su propia investigación y análisis. 

## <a name="compute-and-storage-capacity-planning"></a>Planeamiento de la capacidad de almacenamiento y proceso
Se genera una solución de Azure Stack como un clúster hiperconvergido de proceso, red y almacenamiento. Esto permite un uso eficaz o un uso compartido de toda la capacidad de hardware del clúster, a lo que se hace referencia como unidad de escalado para Azure Stack, de forma que proporciona disponibilidad y escalabilidad. Todo el software de infraestructura se hospeda dentro de un conjunto de máquinas virtuales (VM) y comparte los mismos servidores físicos que las máquinas virtuales del inquilino. Las tecnologías de agrupación en clústeres de Windows Server de la unidad de escalado y las instancias individuales de Hyper-V administran todas las máquinas virtuales. Este enfoque simplifica la adquisición y administración de una solución de Azure Stack y permite el movimiento y la escalabilidad de todos los servicios (inquilino e infraestructura) en la totalidad de la unidad de escalado.

El único recurso físico que no se aprovisiona en exceso en una solución de Azure Stack es la memoria del servidor. Los otros recursos, los núcleos de CPU, el ancho de banda de red y la capacidad de almacenamiento se aprovisionarán en exceso para usar de la mejor forma posible los recursos disponibles. Al calcular la capacidad disponible para una solución, la memoria del servidor físico es el principal colaborador. El uso de otros recursos consiste en comprender la proporción del aprovisionamiento en exceso que es posible y qué se aceptará para la carga de trabajo prevista.

Aproximadamente, se utilizan 28 máquinas virtuales para hospedar la infraestructura de Azure Stack y, en total, consumen alrededor de 208 GB de memoria y 124 núcleos virtuales.  La razón de este número de máquinas virtuales es satisfacer la separación del servicio que se necesita para cumplir los requisitos de seguridad, escalabilidad, servicio y aplicación de revisión. Esta estructura de servicio interno permite la futura introducción de nuevos servicios de infraestructura mientras se desarrollan.

Para admitir la actualización automatizada de todos los componentes de software de la infraestructura o del servidor físico, o bien para las revisiones y actualizaciones, las selecciones de ubicación de la máquina virtual del usuario y de la infraestructura no consumirán todos los recursos de memoria de la unidad de escalado. Se dejará sin asignar una cantidad de la memoria total de todos los servidores de una unidad de escalado a favor de los requisitos de resistencia de la solución. Por ejemplo, cuando se actualiza la imagen de Windows Server del servidor físico, las máquinas virtuales hospedadas en el servidor se mueven a otro lugar de la unidad de escalado mientras se actualizan las imágenes de Windows Server del servidor. Una vez completada la actualización, el servidor se reinicia y vuelve a las cargas de trabajo del servicio. El objetivo de la revisión y de la actualización de una solución de Azure Stack es evitar la necesidad de detener las máquinas virtuales hospedadas. Tratando de cumplir dicho objetivo, se deja sin asignar el mínimo indispensable de la capacidad de memoria de un servidor para permitir el movimiento de máquinas virtuales dentro de la unidad de escalado. Esta selección de ubicación y movimiento se aplica a las máquinas virtuales de la infraestructura y a las máquinas virtuales creadas en nombre del usuario o inquilino de la solución de Azure Stack. El resultado final de la implementación es tal que la cantidad de memoria reservada para admitir el movimiento necesario de la máquina virtual puede ser mucho más que la capacidad de un único servidor debido a que los desafíos de selección de ubicación de las máquinas virtuales tienen diferentes requisitos de memoria. Una sobrecarga adicional en la categoría de uso de memoria es la de la propia instancia de Windows Server. La instancia del sistema operativo base de cada servidor usará memoria para el sistema operativo y sus tablas de la página virtual, junto con la memoria utilizada por Hyper-V en la administración de cada una de las máquinas virtuales hospedadas.

Más adelante en esta sección, se ofrece una descripción detallada de las complejidades de los cálculos de capacidad. En esta introducción, se proporcionan los ejemplos siguientes para ayudar a comprender la capacidad disponible de los diferentes tamaños de la solución. Se trata de cálculos estimados y, como resultado, se realizan suposiciones sobre el uso de memoria de la máquina virtual del inquilino que puede que no sean reales para las instalaciones de producción. Para esta tabla, se utiliza el tamaño de máquina virtual de Azure Estándar D2. Las máquinas virtuales Estándar D2 de Azure se definen con 2 CPU virtuales y 7 GB de memoria.

|     |Por capacidad del servidor|| Capacidad de la unidad de escalado|  |  |||
|-----|-----|-----|-----|-----|-----|-----|-----|
|     | Memoria | Núcleos de CPU | Número de servidores | Memoria | Núcleos de CPU | Máquinas virtuales del inquilino<sup>1</sup>     | Relación del núcleo<sup>2</sup>    |
|Ejemplo 1|256 GB|28|4|1.024 GB| 112 | 54 |4:3|
|Ejemplo 2|512 GB|28|4|2024 GB|112|144|4:1|
|Ejemplo 3|384 GB|28|12|4608 GB|336|432|3:1|
|     |     |     |     |     |     |     |     |

> <sup>1</sup>Máquinas virtuales Estándar D2.

> <sup>2</sup>Relación del núcleo virtual y núcleo físico.

Como se mencionó anteriormente, la capacidad de la máquina virtual viene determinada por la memoria disponible. Las relaciones de núcleos virtuales y núcleos físicos ejemplifican cómo la densidad de la máquina virtual cambiará la capacidad de CPU disponible, a menos que la solución se construya con un mayor número de núcleos físicos (se elige otra CPU). Lo mismo puede decirse de la capacidad de almacenamiento y de la capacidad de memoria caché de almacenamiento.

Los ejemplos anteriores de densidad de la máquina virtual tienen una finalidad meramente explicativa. La complejidad aumenta en los cálculos de soporte técnico. Es necesario contactar con Microsoft o con un asociado de soluciones para entender mejor las opciones de planeamiento de capacidad y la capacidad disponible resultante.

En el resto de esta sección se describen los requisitos de implementación de Azure Stack de proceso y almacenamiento. Utilice esta información para entender de forma básica cuáles son los componentes necesarios y sus valores mínimos de configuración. También se proporciona información adicional para describir cómo se configura la solución con respecto a la capacidad disponible y los posibles límites del sistema con respecto a la capacidad de rendimiento y la capacidad del inquilino.

> [!NOTE]
> Los requisitos de planeamiento de capacidad de red son mínimos, ya que solo se puede configurar el tamaño de la dirección VIP pública. Para obtener información sobre cómo agregar más direcciones IP públicas en Azure Stack, consulte [Add Public IP Addresses](azure-stack-add-ips.md) (Adición de direcciones IP públicas).


## <a name="next-steps"></a>Pasos siguientes
[Planeamiento de la capacidad de proceso](capacity-planning-compute.md)
