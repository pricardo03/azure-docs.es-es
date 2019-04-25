---
title: Administración de administradores de servidor en Azure Analysis Services | Microsoft Docs
description: Aprenda a administrar administradores de servidor de Analysis Services en Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 25eb111871bbe3b18f59b0d6c123c72f3e55c859
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60498719"
---
# <a name="manage-server-administrators"></a>Administración de administradores de servidor

Los administradores del servidor deben ser un usuario o grupo de seguridad válido en Azure Active Directory (Azure AD) para el inquilino en el que reside el servidor. Puede usar **Administradores de Analysis Services** en el servidor de Azure Portal, Propiedades del servidor en SSMS, PowerShell o la API de REST para administrar los administradores del servidor. 

> [!NOTE]
> Los grupos de seguridad deben tener la propiedad `MailEnabled` establecida en `True`.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Incorporación de administradores de servidor mediante Azure Portal

1. En el portal, en su servidor, haga clic en **Administradores de Analysis Services**.
2. En **\<nombre de servidor> - Administradores de Analysis Services**, haga clic en **Agregar**.
3. En **Agregar administradores de servidor**, seleccione las cuentas de usuario de Azure AD o invite usuarios externos por dirección de correo electrónico.

    ![Administradores del servidor en Azure Portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Incorporación de administradores de servidor mediante SSMS

1. Haga clic con el botón derecho en el servidor > **Propiedades**.
2. En **Propiedades de Analysis Server**, haga clic en **Seguridad**.
3. Haga clic en **Agregar** y escriba la dirección de correo electrónico de un usuario o grupo de Azure AD.
   
    ![Incorporación de administradores de servidor en SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Use [New AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet para especificar el parámetro de administrador al crear un nuevo servidor. <br>
Use [conjunto AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet para modificar el parámetro de administrador para un servidor existente.

## <a name="rest-api"></a>API DE REST

Use [Crear](https://docs.microsoft.com/rest/api/analysisservices/servers/create) para especificar la propiedad asAdministrator al crear un servidor. <br>
Use [Actualizar](https://docs.microsoft.com/rest/api/analysisservices/servers/update) para especificar la propiedad asAdministrator al modificar un servidor existente. <br>



## <a name="next-steps"></a>Pasos siguientes 

[Permisos de usuario y autenticación](analysis-services-manage-users.md)  
[Manage database roles and users](analysis-services-database-users.md) (Administración de roles y usuarios de una base de datos)  
[Control de acceso basado en roles](../role-based-access-control/overview.md)  

