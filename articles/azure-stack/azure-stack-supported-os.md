---
title: Sistemas operativos invitados compatibles con Azure Stack | Microsoft Docs
description: Estos sistemas operativos invitados pueden usarse en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 056c44de4f89b04149c275b1508c7ea8f397d6e6
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53810786"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Sistemas operativos invitados compatibles con Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

## <a name="windows"></a> Windows

Azure Stack admite los sistemas operativos invitados Windows que se indican en la siguiente tabla:

| Sistema operativo | DESCRIPCIÓN | Disponible en Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server, versión 1709 | 64 bits | Core con Containers |
| Windows Server 2016 | 64 bits |  Datacenter, Datacenter Core, Datacenter con Containers |
| Windows Server 2012 R2 | 64 bits |  Centro de datos |
| Windows Server 2012 | 64 bits |  Centro de datos |
| Windows Server 2008 R2 SP1 | 64 bits |  Centro de datos |
| Windows Server 2008 SP2 | 64 bits |  Traiga su propia imagen |
| Windows 10 *(vea la nota 1)* | 64 bits, Pro y Enterprise | Traiga su propia imagen |

> [!NOTE]
> Para implementar sistemas operativos cliente Windows 10 en Azure Stack, debe tener una [licencia por usuario de Windows](https://www.microsoft.com/en-us/Licensing/product-licensing/windows10.aspx) o adquirir a través de un proveedor de servicios de hosting cualificado multiinquilino ([QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)).

Las imágenes de Marketplace están disponibles para licencias de pago por uso o BYOL (EA/SPLA). No se admite el uso de ambas en una sola instancia de Azure Stack. Durante la implementación, Azure Stack inserta una versión adecuada del agente invitado en la imagen.

Las ediciones de Datacenter están disponibles en Marketplace para su descarga; los clientes pueden traer sus propias imágenes de servidor, incluidas otras ediciones. Las imágenes del cliente Windows no están disponibles en Marketplace.

## <a name="linux"></a>Linux

Las distribuciones Linux indicadas como disponibles en Marketplace incluyen el agente Linux de Microsoft Azure (WALA) necesario. Si trae su propia imagen para Azure Stack, siga las instrucciones de [Agregar imágenes de Linux a Azure Stack](azure-stack-linux.md).

> [!NOTE]
> Las imágenes personalizadas deben compilarse con la última versión pública de WALA. Las versiones anteriores a la 2.2.18 pueden no funcionar correctamente en Azure Stack.
>
> No se admite [cloud-init](https://cloud-init.io/) en Azure Stack en este momento.

| Distribución | DESCRIPCIÓN | Publicador | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Basado en CentOS 6.9 | 64 bits | Rogue Wave | SÍ |
| Basado en CentOS 7.4 | 64 bits | Rogue Wave | SÍ |
| ClearLinux | 64 bits | ClearLinux.org | SÍ |
| Container Linux |  64 bits | CoreOS | Stable |
| Debian 8 "Jessie" | 64 bits | credativ |  SÍ |
| Debian 9 "Stretch" | 64 bits | credativ | SÍ |
| Red Hat Enterprise Linux 7.x | 64 bits | Red Hat |Traiga su propia imagen |
| SLES 11SP4 | 64 bits | SUSE | SÍ |
| SLES 12SP3 | 64 bits | SUSE | SÍ |
| Ubuntu 14.04 LTS | 64 bits | Canonical | SÍ |
| Ubuntu 16.04 LTS | 64 bits | Canonical | SÍ |
| Ubuntu 18.04-LTS | 64 bits | Canonical | SÍ |

Para obtener información de soporte técnico de Red Hat Enterprise Linux, consulte [Red Hat and Azure Stack: Frequently Asked Questions](https://access.redhat.com/articles/3413531) (Red Hat y Azure Stack: preguntas más frecuentes).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Azure Stack Marketplace, vea los siguientes artículos:

- [Descarga de elementos de Marketplace](azure-stack-download-azure-marketplace-item.md)  
- [Creación y publicación de un producto en Marketplace](azure-stack-create-and-publish-marketplace-item.md)