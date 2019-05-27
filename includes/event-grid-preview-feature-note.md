---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 85a1579e32b4c216f234f77c76316bedeaea77b0
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119556"
---
Esta característica se encuentra en su versión preliminar. Para usarlo, debe instalar una extensión o módulo de la versión preliminar.

### <a name="install-extension-for-azure-cli"></a>Instalación de la extensión para la CLI de Azure

Para la CLI de Azure, necesita la [extensión de Event Grid](/cli/azure/azure-cli-extensions-list).

En [CloudShell](/azure/cloud-shell/quickstart):

* Si ha instalado la extensión anteriormente, actualícela `az extension update -n eventgrid`
* Si no ha instalado la extensión anteriormente, instálela `az extension add -n eventgrid`

Para una instalación local:

1. Desinstale la CLI de Azure localmente.
1. Instale la [versión más reciente](/cli/azure/install-azure-cli) de la CLI de Azure.
1. Inicie la ventana de comando.
1. Desinstale las versiones anteriores de la extensión `az extension remove -n eventgrid`.
1. Instale la extensión `az extension add -n eventgrid`.

### <a name="install-module-for-powershell"></a>Instalación del módulo para PowerShell

Para PowerShell, necesita el [módulo AzureRM.EventGrid](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

En [CloudShell](/azure/cloud-shell/quickstart-powershell):

* Instale el módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Para una instalación local:

1. Abra una consola de PowerShell como administrador
1. Instalación del módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Si el parámetro `-AllowPrerelease` no está disponible, siga estos pasos:

1. Ejecute `Install-Module PowerShellGet -Force`
1. Ejecute `Update-Module PowerShellGet`
1. Cierre la consola de PowerShell.
1. Reinicie PowerShell como administrador.
1. Instale el módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.
