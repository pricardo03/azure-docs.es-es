---
title: Deshabilitación de los inicios de sesión de usuario de una aplicación empresarial en Azure Active Directory | Microsoft Docs
description: Cómo deshabilitar una aplicación empresarial para que ningún usuario pueda iniciar sesión en ella en Azure Active Directory
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7256791c0b6bfbc72a26f6093cdd3c39410f702f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807605"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Deshabilitación de los inicios de sesión de usuario de una aplicación empresarial en Azure Active Directory

Deshabilitar una aplicación empresarial para que ningún usuario pueda iniciar sesión en ella en Azure Active Directory (Azure AD) es fácil. Tiene que tener los permisos adecuados para administrar la aplicación empresarial. Y tiene que ser el administrador global del directorio.

## <a name="how-do-i-disable-user-sign-ins"></a>¿Cómo puedo deshabilitar los inicios de sesión de usuario?

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
1. Seleccione **Todos los servicios**, escriba **Azure Active Directory** en el cuadro de texto y, después, seleccione **Entrar**.
1. En el panel **Azure Active Directory** -  ***nombreDelDirectorio*** (es decir, el panel de Azure AD del directorio que está administrando), seleccione **Aplicaciones empresariales**.
1. En el panel **Aplicaciones empresariales - Todas las aplicaciones**, verá una lista de las aplicaciones que puede administrar. Seleccione una aplicación.
1. En el panel ***nombreDeLaAplicación*** (es decir, el panel con el nombre de la aplicación seleccionada en el título), seleccione **Propiedades**.
1. En el panel ***nombreDeLaAplicación*** - **Propiedades**, en **¿Habilitado para que los usuarios inicien sesión?** , seleccione **No**.
1. Haga clic en el comando **Guardar** .

## <a name="next-steps"></a>Pasos siguientes

* [Ver todos mis grupos](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Asignar un usuario o grupo a una aplicación empresarial](assign-user-or-group-access-portal.md)
* [Quitar una asignación de usuario o grupo de una aplicación empresarial](remove-user-or-group-access-portal.md)
* [Cambio del nombre o el logotipo de una aplicación empresarial](change-name-or-logo-portal.md)
