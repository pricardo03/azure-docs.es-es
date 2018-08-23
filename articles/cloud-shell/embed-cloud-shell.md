---
title: Insertar Azure Cloud Shell | Microsoft Docs
description: Aprenda a insertar Azure Cloud Shell.
services: cloud-shell
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 0bd5382e5ea37f7c3c52d119e9d39fe7e0bfdc7c
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42146649"
---
# <a name="embed-azure-cloud-shell"></a>Insertar Azure Cloud Shell

Al insertar Cloud Shell, los desarrolladores y los escritores de contenido pueden abrir directamente Cloud Shell desde una dirección URL dedicada, [shell.azure.com](https://shell.azure.com). Esto proporciona inmediatamente a los usuarios todas las funcionalidades de autenticación de Cloud Shell, y las herramientas actualizadas de la CLI de Azure y de Azure PowerShell.

Botón de tamaño normal

[![](https://shell.azure.com/images/launchcloudshell.png "Inicio de Azure Cloud Shell")](https://shell.azure.com)

Botón de tamaño grande

[![](https://shell.azure.com/images/launchcloudshell@2x.png "Inicio de Azure Cloud Shell")](https://shell.azure.com)

## <a name="how-to"></a>Procedimiento

Para integrar el botón de inicio de Cloud Shell en archivos Markdown, copie lo siguiente:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Encontrará el código HTML para insertar una instancia de Cloud Shell emergente a continuación:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><img alt="Launch Azure Cloud Shell" src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Personalizar la experiencia

Establezca una experiencia de shell específica aumentando la dirección URL.
|Experiencia   |URL   |
|---|---|
|Shell usado más recientemente   |[shell.azure.com](https://shell.azure.com)           |
|Bash                       |[shell.azure.com/bash](https://shell.azure.com/bash)       |
|PowerShell                 |[shell.azure.com/powershell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Pasos siguientes
[Guía de inicio rápido de Bash en Cloud Shell](quickstart.md)<br>
[Guía de inicio rápido de PowerShell en Cloud Shell](quickstart-powershell.md)
