---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 86d768db7a31c634bdaca6c93f633c7bbaf10a65
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773663"
---
El cmdlet de PowerShell **Get-AzPeeringLocation** devuelve una lista de ubicaciones de emparejamiento con el parámetro obligatorio `Kind`, que usará en pasos posteriores:

```powershell
Get-AzPeeringLocation -Kind Direct
```

Las ubicaciones de emparejamiento directo contienen los siguientes campos:
* PeeringLocation 
* Country
* PeeringDBFacilityId
* PeeringDBFacilityLink
* BandwidthOffers

Para comprobar que está presente en la instalación de emparejamiento deseada consulte [PeeringDB](https://w www.peeringdb.com).

A continuación se incluye un ejemplo en el que se muestra cómo usar Seattle como ubicación de emparejamiento para crear un emparejamiento directo:

```powershell
$peeringLocations = Get-AzPeeringLocation -Kind Direct
$peeringLocation = $peeringLocations | where {$_.PeeringLocation -contains "Seattle"}
$peeringLocation

PeeringLocation       : Seattle
Address               : 2001 Sixth Avenue
Country               : US
PeeringDBFacilityId   : 71
PeeringDBFacilityLink : https://www.peeringdb.com/fac/71
BandwidthOffers       : {10Gbps, 100Gbps}
```