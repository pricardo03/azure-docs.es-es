---
title: Configuración de una máquina virtual de Azure con la identidad de Managed Service Identity habilitada mediante un SDK de Azure
description: Instrucciones paso a paso para configurar y utilizar una identidad de Managed Service Identity en una máquina virtual de Azure mediante un SDK de Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: daveba
ms.openlocfilehash: 2763c78d309f5a90d68429caa46581e50f8b4303
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257667"
---
# <a name="configure-a-vm-managed-service-identity-using-an-azure-sdk"></a>Configuración de una identidad Managed Service Identity de máquina virtual mediante un SDK de Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (AD). Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a habilitar y eliminar Managed Service Identity en una máquina virtual de Azure mediante un SDK de Azure.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-service-identity-support"></a>SDK de Azure con compatibilidad con Managed Service Identity 

Azure admite varias plataformas de programación a través de una serie de [SDK de Azure](https://azure.microsoft.com/downloads). Varias de ellas se han actualizado para ser compatibles con Managed Service Identity y proporcionan los ejemplos correspondientes para mostrar el uso. Esta lista se actualiza a medida que se agrega compatibilidad adicional:

| SDK | Muestra |
| --- | ------ | 
| .NET   | [Administración de recursos desde una máquina virtual con MSI habilitado](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Administración de almacenamiento desde una máquina virtual con MSI habilitado](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Creación de una máquina virtual con MSI habilitado](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Creación de una máquina virtual con MSI habilitado](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Creación de una máquina virtual de Azure con MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información acerca de cómo puede usar también Azure Portal, PowerShell, la CLI y las plantillas de recursos, consulte los artículos relacionados en "Configuración de Managed Service Identity en una máquina virtual de Azure".

Use la siguiente sección de comentarios para proporcionar sus opiniones y ayudarnos a afinar y remodelar el contenido.
