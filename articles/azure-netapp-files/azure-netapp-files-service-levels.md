---
title: Niveles de servicio de Azure NetApp Files | Microsoft Docs
description: Se describe el rendimiento de los niveles de servicio de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: c2086eb0c5529d8e570a545e35fc716f70c7541f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "56668351"
---
# <a name="service-levels-for-azure-netapp-files"></a>Niveles de servicio para Azure NetApp Files
Azure NetApp Files admite dos niveles de servicio: Premium y Standard. 

## <a name="Premium"></a>Almacenamiento Premium

El almacenamiento *Premium* proporciona hasta 64 MiB/s por TiB de rendimiento. El rendimiento se indexa con respecto a la cuota de volumen. Por ejemplo, un volumen del almacenamiento Premium con 2 TiB de cuota aprovisionada (con independencia del consumo real) tiene un rendimiento de 128 MiB/s.

## <a name="Standard"></a>Almacenamiento Standard

El almacenamiento *Standard* proporciona hasta 16 MiB/s por TiB de rendimiento. El rendimiento se indexa con respecto a la cuota de volumen. Por ejemplo, un volumen del almacenamiento Standard con 2 TiB de cuota aprovisionada (con independencia del consumo real) tiene un rendimiento de 32 MiB/s.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [página de precios de Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/) para conocer el precio de los diferentes niveles de servicio.
- [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
