---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: daveba
ms.service: active-directory
ms.component: msi
ms.topic: include
ms.date: 05/31/2018
ms.author: daveba
ms.custom: include file
ms.openlocfilehash: 55814f5515649e0fc6d646d3580f281fd9ec37a5
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47424050"
---
| Categoría | Límite |
| --- | --- |
| Identidades administradas asignadas por el usuario | <ul><li>Al crear identidades administradas asignadas por el usuario, solo se admiten caracteres alfanuméricos (0-9, a-z, A-Z) y el guion (-). Además, el nombre debe limitarse a una longitud de 24 caracteres para que la asignación a VM/VMSS funcione correctamente.</li><li>Si se utiliza la extensión de máquina virtual de identidades administradas, el límite admitido es de 32 identidades administradas asignadas por el usuario.  Sin la extensión de máquina virtual de identidades administradas, el límite admitido es de 512 identidades administradas asignadas por el usuario.</li>|

