---
title: Cambio a un inquilino B2C en Azure Active Directory B2C | Microsoft Docs
description: Cómo cambiar al contexto del inquilino de Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/13/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9315b3a5e1641098daf2c7dadf9fa4f09d0cb2a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60317621"
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Cambio a inquilino de Azure AD B2C

Para configurar Azure AD B2C, debe estar con el inquilino de Azure AD B2C.

## <a name="log-into-azure-ad-b2c-tenant"></a>Inicio de sesión en el inquilino de Azure AD B2C

Para ir hasta el inquilino de Azure AD B2C, debe haber iniciado sesión en Azure Portal como administrador global del inquilino de Azure AD B2C.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Para cambiar el inquilino, haga clic en la dirección de correo electrónico o la imagen de la esquina superior derecha.
1. En la lista `Directory` que aparece, seleccione el inquilino de Azure AD B2C que desea administrar.

Se actualiza Azure Portal.  Ahora estará conectado en Azure Portal al inquilino de Azure AD B2C.

## <a name="navigate-to-the-b2c-features-pane"></a>Desplazamiento al panel de características de B2C

1. Haga clic en **Examinar** en el panel de navegación de la izquierda.
1. Haga clic en **Todos los servicios** y busque `Azure AD B2C` en el panel de navegación izquierdo.  (Para anclar al panel de inicio izquierdo, haga clic en la estrella a la izquierda de Azure AD B2C)
1. Haga clic en **Azure AD B2C** para tener acceso al panel de características de B2C.
   
    ![Captura de pantalla de ir a Panel de características B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Necesita ser administrador global del inquilino B2C para poder acceder al panel de características de B2C. Un administrador global de cualquier otro inquilino o un usuario de cualquier inquilino no puede acceder a dicha hoja.  Puede cambiar a su inquilino B2C mediante el selector de inquilinos de la esquina superior derecha de Azure Portal.
