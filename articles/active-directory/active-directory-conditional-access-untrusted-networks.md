---
title: Configuración de directivas de acceso condicional de Azure Active Directory para los intentos de acceso por parte de redes que no son de confianza | Microsoft Docs
description: Obtenga información acerca de cómo configurar una directiva de acceso condicional en Azure Active Directory (Azure AD) para intentos de acceso de redes que no son de confianza.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: protection
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b37c9017d6c9a8b1d5f53141e28c170307206f3b
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225832"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Configuración de directivas de acceso condicional para intentos de acceso de redes que no son de confianza   

En un mundo donde la nube y la movilidad son prioritarias, Azure Active Directory (Azure AD) permite el inicio de sesión único en dispositivos, aplicaciones y servicios desde cualquier parte. Como resultado, los usuarios pueden tener acceso a las aplicaciones de nube no solo desde la red de la organización, sino también desde cualquier ubicación de Internet que no sea de confianza. Con el [acceso condicional de Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), puede controlar el modo en que los usuarios autorizados pueden acceder a las aplicaciones en la nube. Un requisito común en este contexto es controlar los intentos de acceso que se inicien desde las redes de confianza. En este artículo se proporciona la información necesaria para configurar una directiva de acceso condicional que controle este requisito. 

## <a name="prerequisites"></a>Requisitos previos

En este artículo se asume que está familiarizado con: 

- Los conceptos básicos de acceso condicional de Azure AD 
- La configuración de directivas de acceso condicional en Azure Portal

Consulte:

- Para ver una introducción al acceso condicional, consulte [Acceso condicional en Azure Active Directory](active-directory-conditional-access-azure-portal.md). 

- Si quiere obtener algo más de experiencia con la configuración de directivas de acceso condicional, consulte [Inicio rápido: requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](active-directory-conditional-access-app-based-mfa.md). 


## <a name="scenario-description"></a>Descripción del escenario

Para dominar el equilibrio entre seguridad y productividad, podría ser suficiente requerir al usuario que se autentique solo mediante contraseña. Sin embargo, cuando se realiza un intento de acceso desde una ubicación de red que no es de confianza, hay un mayor riesgo de que los inicios de sesión no procedan de usuarios legítimos. Para solucionar este problema, puede bloquear los intentos de acceso desde redes que no son de confianza. También puede requerir la autenticación multifactor (MFA) para obtener más garantías de que el intento ha sido realizado por el propietario legítimo de la cuenta. 

Con el acceso condicional de Azure AD, puede abordar este requisito con una única directiva que concede acceso: 

- A determinadas aplicaciones en la nube

- A los usuarios y grupos seleccionados  

- Con autenticación multifactor 

- Cuando se realiza un intento de acceso desde: 

    - Una ubicación que no es de confianza


## <a name="considerations"></a>Consideraciones

El desafío de este escenario consiste en traducir *cuando se realiza un intento de acceso desde una ubicación que no es de confianza* en una condición de acceso condicional. En una directiva de acceso condicional, puede configurar la [condición de ubicación](active-directory-conditional-access-locations.md) para abordar los escenarios relacionados con las ubicaciones de red. La condición de ubicación permite seleccionar [ubicaciones con nombre](active-directory-conditional-access-locations.md#named-locations), que representan agrupaciones lógicas de intervalos de direcciones IP, países y regiones.  

Normalmente, su organización posee uno o más intervalos de direcciones, por ejemplo, 199.30.16.0 - 199.30.16.24.
Para configurar una ubicación con nombre, puede:

- Especificar este intervalo (199.30.16.0/24) 

- Asignar un nombre descriptivo como **Red corporativa** 


En lugar de intentar definir cuáles son todas las ubicaciones que no son de confianza, hacer lo siguiente:

- Include 

    ![Acceso condicional](./media/active-directory-conditional-access-untrusted-networks/02.png)

- Excluir todas las ubicaciones de confianza 

    ![Acceso condicional](./media/active-directory-conditional-access-untrusted-networks/01.png)



## <a name="implementation"></a>Implementación

Con el enfoque descrito en este artículo, ahora puede configurar una directiva de acceso condicional para las ubicaciones que no son de confianza. Siempre debe probar la directiva antes de implementarla en producción para asegurarse de que funciona según lo previsto. Lo ideal es realizar las pruebas iniciales en un inquilino de prueba, si es posible. Para más información, consulte [Cómo debe implementar una directiva nueva](active-directory-conditional-access-best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el acceso condicional, consulte [Acceso condicional de Azure Active Directory](active-directory-conditional-access-azure-portal.md).