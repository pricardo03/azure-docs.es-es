---
title: Administración del acceso a la administración de Azure con acceso condicional en Azure Active Directory
description: Aprenda a usar el acceso condicional en Azure AD para administrar el acceso a la administración de Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2019
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: b824d122a5d26c17c41a0e2ea1c595c9e2dd7206
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354355"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Administración el acceso a la administración de Azure con acceso condicional

El acceso condicional en Azure Active Directory (Azure AD) controla el acceso a las aplicaciones en la nube en función de las condiciones concretas que especifique. Para permitir el acceso, cree directivas de acceso condicional que permitan o bloqueen el acceso en función de si se cumplen los requisitos de la directiva. 

Normalmente, se utiliza el acceso condicional para controlar el acceso a las aplicaciones en la nube. También puede configurar directivas para controlar el acceso a la administración de Azure en función de ciertas condiciones (como el riesgo de inicio de sesión, la ubicación o el dispositivo) y para exigir requisitos como la autenticación multifactor.

Para crear una directiva de administración de Azure, seleccione **Microsoft Azure Management** en **Aplicaciones en la nube** al elegir la aplicación a la que se va a aplicar la directiva.

![Acceso condicional para la administración de Azure](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

La directiva que se crea se aplica a todos los puntos de conexión de la administración de Azure, incluidos Azure Portal, el proveedor de Azure Resource Manager, las instancias clásicas de Service Management API, Azure PowerShell y el portal de administración de suscripciones de Visual Studio. Tenga en cuenta que la directiva se aplica a Azure PowerShell, que llama a la API de Azure Resource Manager. No es aplicable a [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), que llama a Microsoft Graph.

> [!CAUTION]
> Asegúrese de comprender cómo funciona el acceso condicional antes de configurar una directiva para administrar el acceso a la administración de Azure. Asegúrese de no crear condiciones que pudieran bloquear su propio acceso al portal.

Para obtener más información sobre cómo configurar y usar el acceso condicional, consulte [Acceso condicional de Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).