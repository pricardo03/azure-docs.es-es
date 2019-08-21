---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: a98a941477fc83a104b55ed91f457c5a48f90d59
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029862"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances

Con una instancia reservada de máquina virtual optimizada para conseguir flexibilidad en el tamaño de la instancia, la reserva que adquiera se puede aplicar a los tamaños de las máquinas virtuales del mismo grupo de flexibilidad de tamaño de instancia. Por ejemplo, si compra una reserva para un tamaño de máquina virtual de la serie DSv2 como, por ejemplo, Standard_DS5_v2, el descuento por la reserva se puede aplicar a los otros cuatro tamaños que aparecen en el mismo grupo de flexibilidad de tamaño de instancia:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Pero ese descuento de reserva no se aplica a los tamaños de máquinas virtuales que aparecen en grupos de flexibilidad de tamaño de instancia diferentes, como las SKU en la serie DSv2 de memoria alta: Standard_DS11_v2, Standard_DS12_v2, etc.

Dentro del grupo de flexibilidad de tamaño de instancia, el número de máquinas virtuales al que se aplica el descuento por la reserva depende del tamaño de máquina virtual que elija al comprar una reserva. También depende de los tamaños de las máquinas virtuales que tenga en ejecución. La columna de relación compara la superficie relativa para cada tamaño de máquina virtual en ese grupo de flexibilidad de tamaño de instancia. Use el valor de relación para calcular cómo se aplica el descuento por la reserva a las máquinas virtuales que tiene en ejecución.

## <a name="examples"></a>Ejemplos

Los ejemplos siguientes usan los tamaños y relaciones en la tabla de la serie DSv2.

Va a comprar una instancia reservada de máquina virtual con el tamaño Standard_DS4_v2 en la que la relación o superficie relativa comparada con los otros tamaños de esa serie es 8.

- Escenario 1: Ejecución de ocho máquinas virtuales de tamaño Standard_DS1_v2 con una relación de 1. El descuento por la reserva se aplica a las ocho máquinas virtuales.
- Escenario 2: Ejecución de máquinas virtuales de tamaño Standard_DS2_v2 con una relación de 2 cada una. También se ejecuta una máquina virtual con tamaño Standard_DS3_v2 con una relación de 4. La superficie total es 2+2+4=8. Por tanto, el descuento por la reserva se aplica a las tres máquinas virtuales.
- Escenario 3: Ejecución de una máquina virtual de tamaño Standard_DS5_v2 con una relación de 16. El descuento por la reserva se aplicaría a la mitad del costo de proceso de esa máquina virtual.

En las siguientes secciones se muestra qué tamaños están en el mismo grupo de serie de tamaño cuando compra una instancia reservada de máquina virtual optimizada con flexibilidad de tamaño.

## <a name="instance-size-flexibility-ratio-for-vms"></a>Relación de flexibilidad de tamaño de instancia para máquinas virtuales 

El siguiente CSV contiene los grupos, ArmSkuName y las relaciones de flexibilidad de tamaño de la instancia.  

[Relaciones de flexibilidad de tamaño de instancia](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

La dirección URL del archivo y el esquema se mantendrán fijos para que pueda consumir este archivo mediante programación. En breve, los datos también estarán disponibles a través de la API.
