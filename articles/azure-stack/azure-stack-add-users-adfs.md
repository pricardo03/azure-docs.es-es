---
title: Incorporación de usuarios a los AD FS de Azure Stack | Microsoft Docs
description: Aprenda a agregar usuarios a las implementaciones de Azure Stack
services: azure-stack
documentationcenter: ''
author: patricka
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: f8abacbcb05d1346931b5c2e1097660cfbd8e594
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344173"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Incorporación de usuarios de Azure Stack a AD FS
Puede usar el complemento **Active Directory Users and Computers** (Usuarios y equipos de Active Directory) para incorporar usuarios adicionales a un entorno de Azure Stack, aprovechando a AD FS como proveedor de identidades.

## <a name="add-windows-server-active-directory-users"></a>Incorporación de usuarios de Windows Server Active Directory
> [!TIP]
> En este ejemplo se utiliza el ASDK Active Directory azurestack.local predeterminado. 

1.  Inicie sesión en un equipo con una cuenta que proporcione acceso a las herramientas administrativas de Windows y abra una nueva consola de Microsoft Management Console (MMC).
2.  Haga clic en **File > Add or remove snap-in** (Archivo > Agregar o quitar complemento).
3.  Seleccione **Active Directory Users and Computers** > **AzureStack.local** > **Users** (Usuarios y equipos de Active Directory > AzureStack.local > Usuarios).
4.  Haga clic en **Action** > **New** > **User** (Acción > Nuevo > Usuario).
5.  En la ventana New Object - User (Nuevo objeto: usuario), proporcione una contraseña y confírmela.
6.  Haga clic en **Next** (Siguiente) para finalizar los valores y haga clic en Finish (Finalizar) para crear el usuario.


## <a name="next-steps"></a>Pasos siguientes
[Creación de entidades de servicio](azure-stack-create-service-principals.md)