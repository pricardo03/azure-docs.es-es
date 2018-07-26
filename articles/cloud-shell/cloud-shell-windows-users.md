---
title: Azure Cloud Shell para usuarios de Windows | Microsoft Docs
description: Guía para los usuarios que no están familiarizados con los sistemas Linux
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 5e318a0f64033aa0c4b306e547c11e1994afa229
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861470"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell en Azure Cloud Shell para usuarios de Windows

En mayo de 2018, se [anunciaron](https://azure.microsoft.com/blog/pscloudshellrefresh/) cambios en PowerShell en Azure Cloud Shell.  La experiencia de PowerShell en Azure Cloud Shell se llama ahora PowerShell Core 6 en Linux.
Con este cambio, hay algunos aspectos de PowerShell en Cloud Shell que pueden ser diferentes a lo que se espera en Windows PowerShell 5.1.

## <a name="case-sensitivity"></a>Distinción entre mayúsculas y minúsculas

En Windows, el sistema de archivos no distingue mayúsculas de minúsculas.  En Linux, el sistema de archivos distingue mayúsculas de minúsculas.
Esto significa que anteriormente `file.txt` y `FILE.txt` se consideraban un mismo archivo pero ahora se consideran distintos archivos.
Se deben usar las mayúsculas y minúsculas correctas al finalizar con `tab` en el sistema de archivos.  Las experiencias de PowerShell específicas, como los cmdlets de `tab`, no distinguen mayúsculas de minúsculas. 

## <a name="windows-powershell-alias-vs-linux-utilities"></a>Alias de Windows PowerShell en comparación con las utilidades de Linux

Los comandos existentes de Linux, como `ls`, `sort` y `sleep` prevalecen sobre sus alias de PowerShell.  A continuación se muestran los alias habituales eliminados, así como los comandos equivalentes:  

|Alias eliminado   |Comando equivalente   |
|---|---|
|`ls`     | `dir` <br> `Get-ChildItem` |
|`sort`   | `Sort-Object` |
|`sleep`  | `Start-Sleep` |

## <a name="persisting-home-vs-homeclouddrive"></a>Conservación de $home vs $home\clouddrive

Para los usuarios que conservaron los scripts y otros archivos en su unidad en la nube, el directorio $HOME ahora se conserva entre sesiones.

## <a name="powershell-profile"></a>Perfil de PowerShell

De forma predeterminada, no se crea ningún perfil de PowerShell.  Para crear el perfil, cree un directorio `PowerShell` en `$HOME/.config`.  En `$HOME/.config/PowerShell`, puede crear el perfil con el nombre `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>Novedades de PowerShell Core 6

Para más información acerca de cuáles son las novedades de PowerShell Core 6, consulte los [documentos de PowerShell](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) y la entrada de blog de [introducción a PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/)
