---
title: '¿Qué es la protección de base de referencia en el acceso condicional de Azure Active Directory? : versión preliminar | Microsoft Docs'
description: Obtenga información acerca de cómo la protección de base de referencia garantiza que al menos tiene el nivel de base de referencia de seguridad habilitado en el entorno de Azure Active Directory.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 585a4968672c8bcab0efb988d556e447983dfc5a
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158710"
---
# <a name="what-is-baseline-protection-preview"></a>¿Qué es la protección de base de referencia (versión preliminar)?  

En el último año, los ataques de identidad han aumentado un 300 %. Para proteger el entorno contra los crecientes ataques, Azure Active Directory (Azure AD) incorpora una nueva característica llamada protección de base de referencia. La protección de base de referencia es un conjunto de [directivas de acceso condicional](../active-directory-conditional-access-azure-portal.md) predefinidas. El objetivo de estas directivas es garantizar que al menos tiene el nivel de base de referencia de seguridad habilitado en todas las ediciones de Azure AD. 

En este artículo se proporciona información general sobre la protección de la base de referencia en Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Requerir MFA para administradores

Los usuarios con acceso a cuentas con privilegios tienen acceso sin restricciones a su entorno. Dadas las facultades de estas cuentas, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta cuando se emplean para iniciar sesión. En Azure Active Directory, puede exigir MFA (Multi-Factor Authentication) para conseguir una verificación de cuentas más estricta.  

**Requerir MFA para administradores** es una directiva de base de referencia que exige MFA para los roles de directorio siguientes: 

- Administrador global  

- Administrador de SharePoint  

- Administrador de Exchange  

- Administrador de acceso condicional  

- Administrador de seguridad  


![Azure Active Directory](./media/baseline-protection/01.png)

La directiva de base de referencia permite excluir usuarios. Podría desear excluir una *[cuenta administrativa de acceso de emergencia](../users-groups-roles/directory-emergency-access.md)* para asegurarse de que no queda bloqueado fuera del inquilino.


## <a name="enable-a-baseline-policy"></a>Habilitar una directiva de base de referencia 

Mientras están en versión preliminar, las directivas de base de referencia están inactivas de forma predeterminada. Debe habilitar una directiva manualmente si quiere activarla. Si habilita explícitamente las directivas de línea de base en la etapa de versión preliminar, se mantendrán activas cuando la característica alcance la disponibilidad general. El cambio de comportamiento planeado es el motivo por el que, además de la activación y desactivación, tiene una tercera opción para establecer el estado de una directiva: **Automatically enable policy in the future** (Habilitar la directiva automáticamente en el futuro). Al seleccionar esta opción, puede dejar las directivas deshabilitadas durante la versión preliminar, pero Microsoft las habilitará automáticamente cuando la característica alcance la disponibilidad general. Si no habilita explícitamente las directivas de línea de base ahora y no selecciona la opción **Automatically enable policy in the future** (Habilitar la directiva automáticamente en el futuro), las directivas permanecerán deshabilitadas cuando esta característica alcance la disponibilidad general.


**Para habilitar una directiva de base de referencia:**  

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.

2. En **Azure Portal**, en la barra de navegación izquierda, haga clic en **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. En la página **Azure Active Directory**, en la sección **Seguridad**, haga clic en **Acceso condicional**.

    ![Acceso condicional](./media/baseline-protection/05.png)

4. En la lista de directivas, haga clic en una directiva que empiece por **Baseline policy:** (Directiva de base de referencia:). 

5. Para habilitar la directiva, haga clic en **Usar la directiva inmediatamente**.

6. Haga clic en **Save**(Guardar). 
 
  
 

## <a name="what-you-should-know"></a>Qué debería saber 

Aunque la administración de directivas de acceso condicional personalizadas requiere una licencia de Azure AD Premium, las directivas de base de referencia están disponibles en todas las ediciones de Azure AD.     

Los roles de directorio que se incluyen en la directiva de base de referencia son los roles de Azure AD con más privilegios. 

Si tiene cuentas con privilegios que se usan en sus scripts, debería reemplazarlas por [identidades administradas para los recursos de Azure](../managed-identities-azure-resources/overview.md) o [entidades de servicio con certificados](../develop/howto-authenticate-service-principal-powershell.md). Como solución temporal, puede excluir determinadas cuentas de usuario de la directiva de base de referencia. 

Las directivas de base de referencia se aplican a los flujos de autenticación heredados, como POP, IMAP o el cliente de escritorio de Office anterior. 




## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

- [Cinco pasos para asegurar su infraestructura de identidad](https://docs.microsoft.com/azure/security/azure-ad-secure-steps)

- [¿Qué es el acceso condicional en Azure Active Directory?](overview.md) 

