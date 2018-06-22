---
title: Protección de base de referencia en el acceso condicional de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de cómo la protección de base de referencia garantiza que al menos tiene el nivel de base de referencia de seguridad habilitado en el entorno.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 25ae4db2cd4f2a2cea74c428a272c6868acaa5c5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248969"
---
# <a name="what-is-baseline-protection"></a>¿Qué es la protección de base de referencia?  

En el último año, los ataques de identidad han aumentado un 300 %. Para proteger el entorno contra los crecientes ataques, Azure Active Directory (Azure AD) incorpora una nueva característica llamada protección de base de referencia. La protección de base de referencia es un conjunto de directivas de acceso condicional predefinidas. El objetivo de estas directivas es garantizar que al menos tiene el nivel de base de referencia de seguridad habilitado en el entorno. 

Durante la versión preliminar, debe habilitar las directivas de base de referencia si desea activarlas. Después de la disponibilidad general, estas directivas estarán habilitadas de forma predeterminada. 

La primera directiva de protección de base de referencia requiere MFA para las cuentas con privilegios. Los atacantes que tomen el control de cuentas con privilegios pueden producir enormes daños, por lo que es fundamental proteger estas cuentas en primer lugar. Los siguientes roles con privilegios pertenecen al ámbito de esta directiva: 

- Administrador global  

- Administrador de SharePoint  

- Administrador de Exchange  

- Administrador de acceso condicional  

- Administrador de seguridad  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

## <a name="how-to-get-started"></a>Primeros pasos 

Para habilitar la directiva de base de referencia:  

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.

2. En **Azure Portal**, en la barra de navegación izquierda, haga clic en **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. En la página **Azure Active Directory**, en la sección **Administrar**, haga clic en **Acceso condicional**.

    ![Acceso condicional](./media/active-directory-conditional-access-baseline-protection/03.png)

4. En la lista de directivas, haga clic en **Baseline policy: Require MFA for admins (Preview)** [Directiva de base de referencia: requerir MFA para los administradores (versión preliminar)]. 

5. Para habilitar la directiva, haga clic en **Usar la directiva inmediatamente**.

6. Haga clic en **Save**(Guardar). 
 

La directiva de base de referencia permite excluir usuarios y grupos. Podría desear excluir una *[cuenta administrativa de acceso de emergencia](active-directory-admin-manage-emergency-access-accounts.md)* para asegurarse de que no queda bloqueado fuera del inquilino.
  
 

## <a name="what-you-should-know"></a>Qué debería saber 

Los roles de directorio que se incluyen en la directiva de base de referencia son los roles de Azure AD con más privilegios. En función de los comentarios, otros roles podrían estar incluidos en el futuro. 

Si tiene cuentas con privilegios de administrador en los scripts, debería usar [Managed Service Identity (MSI)](managed-service-identity/overview.md) o [entidades de servicio (con certificados)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) en su lugar. Como solución temporal, puede excluir determinadas cuentas de usuario de la directiva de base de referencia. 

La directiva se aplica a los flujos de autenticación heredados como IMAP, POP o el cliente de escritorio de Office anterior. 

## <a name="next-steps"></a>Pasos siguientes

Si quiere saber cómo configurar una directiva de acceso condicional, consulte [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) (Introducción al acceso condicional en Azure Active Directory).

Si está listo para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](active-directory-conditional-access-best-practices.md). 
