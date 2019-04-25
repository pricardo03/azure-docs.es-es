---
title: Cómo usar las identidades administradas de recursos de Azure en una VM de Azure con SDK de Azure
description: Ejemplos de código para usar SDK de Azure con una máquina virtual de Azure con identidades administradas de recursos de Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00c86562e0fdb4e6d62d44088b7aba08e45e22a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60293241"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Cómo usar las identidades administradas de recursos de Azure en una VM de Azure con SDK de Azure 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
En este artículo se proporciona una lista de ejemplos de SDK que demuestran el uso de sus SDK de Azure respectivos con identidades administradas de recursos de Azure.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - En todo el script o código de ejemplo de este artículo se da por supuesto que el cliente se ejecuta en una VM con identidades administradas de recursos de Azure habilitadas. Use la característica "Conectar" de la máquina virtual en Azure Portal para conectarse a la máquina virtual de forma remota. Para más información sobre la habilitación de identidades administradas de recursos de Azure en una VM, consulte [Configure managed identities for Azure resources on a VM using the Azure portal](qs-configure-portal-windows-vm.md) (Configuración de identidades administradas de recursos de Azure en una VM mediante Azure Portal) o uno de los artículos derivados (mediante PowerShell, la CLI, una plantilla o un SDK de Azure). 

## <a name="sdk-code-samples"></a>Ejemplos de código de SDK

| SDK             | Código de ejemplo |
| --------------- | ----------- |
| .NET            | [Deploy an Azure Resource Manager template from a Windows VM using managed identities for Azure resources](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) (Implementación de una plantilla de Azure Resource Manager desde una máquina virtual Windows mediante identidades administradas de recursos de Azure) |
| .NET Core       | [Call Azure services from a Linux VM using managed identities for Azure resources](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) (Llamada a servicios de Azure desde una máquina virtual Linux mediante identidades administradas de recursos de Azure) |
| Node.js         | [Administración de recursos mediante identidades administradas de recursos de Azure](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Uso de identidades administradas de recursos de Azure para la autenticación simple desde una máquina virtual](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Administración de recursos de una máquina virtual con identidades administradas de recursos de Azure habilitadas](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Pasos siguientes

- Consulte los [SDK de Azure](https://azure.microsoft.com/downloads/) para ver la lista completa de recursos de SDK de Azure, como las descargas de bibliotecas, la documentación y mucho más.
- Para habilitar las identidades administradas de recursos de Azure en una máquina virtual de Azure, consulte [Configure managed identities for Azure resources on a VM using the Azure portal](qs-configure-portal-windows-vm.md) (Configuración de identidades administradas de recursos de Azure en una VM mediante Azure Portal).








