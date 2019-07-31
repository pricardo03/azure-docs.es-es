---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/28/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 9832ae6010f262322328ec22876a35c89e1bedca
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850669"
---
Si coloca las máquinas virtuales en una sola región, reducirá la distancia física entre las instancias. Además, si las coloca en una sola zona de disponibilidad, estarán todavía más cercanas físicamente. Aun así, a medida que la superficie de Azure crece, una sola zona de disponibilidad puede abarcar varios centros de datos físicos, lo que es posible que provoque una latencia de red que puede afectar al rendimiento de la aplicación. 

Para acercar las máquinas virtuales lo más posible con la menor latencia posible, debe implementarlas dentro de un grupo de selección de ubicación de proximidad.

Un grupo de selección de ubicación de proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Los grupos de selección de ubicación de proximidad son útiles para las cargas de trabajo en las que la latencia baja es un requisito.


- Baja latencia entre máquinas virtuales independientes.
- Baja latencia entre máquinas virtuales en un único conjunto de disponibilidad o conjunto de escalado de máquinas virtuales. 
- Baja latencia entre máquinas virtuales independientes, máquinas virtuales en varios conjuntos de disponibilidad o varios conjuntos de escalado. Puede tener varios recursos de proceso en un solo grupo de selección de ubicación para reunir una aplicación de varias capas. 
- Baja latencia entre varias capas de aplicación con diferentes tipos de hardware. Por ejemplo, la ejecución del back-end con la serie M en un conjunto de disponibilidad y el front-end en una instancia de la serie D, en un conjunto de escalado, en un solo grupo de selección de ubicación de proximidad.

> [!IMPORTANT]
> Los grupos de selección de ubicación de proximidad se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Los grupos de selección de ubicación de proximidad no están disponibles en estas regiones durante la versión preliminar: **Japón Oriental**, **Este de Australia** e **India central**.


## <a name="best-practices"></a>Procedimientos recomendados 
- Para la latencia más baja, use grupos de selección de ubicación de proximidad junto con redes aceleradas. Para obtener más información, consulte [Creación de una máquina virtual Linux con redes aceleradas](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) o [Creación de una máquina virtual Windows con redes aceleradas](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Implemente todos los tamaños de máquina virtual en una sola plantilla de Resource Manager. Para evitar llegar a hardware que no admite todas las SKU y tamaños de máquina virtual que necesita, incluya todas las capas de aplicación en una sola plantilla de modo que se implementen todas al mismo tiempo.
- Si va a crear scripts de la implementación mediante PowerShell, la CLI o el SDK, puede producirse un error de asignación `OverconstrainedAllocationRequest`. En este caso, debe detener o desasignar todas las máquinas virtuales existentes y cambiar la secuencia en el script de implementación para que empiece con la SKU o los tamaños de máquina virtual en los que se produjo el error. 
- Al reutilizar un grupo de selección de ubicación existente del que se han eliminado máquinas virtuales, espere a que la eliminación se complete antes de agregarle máquinas virtuales.
- Si su prioridad es la latencia, coloque las máquinas virtuales en un grupo de selección de ubicación de proximidad y toda la solución en una zona de disponibilidad. En cambio, si su prioridad es la resistencia, distribuya las instancias por varias zonas de disponibilidad (un solo grupo de selección de ubicación de proximidad no puede abarcar zonas).