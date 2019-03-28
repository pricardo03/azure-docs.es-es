---
title: 'Inicio rápido: bloquear el acceso cuando se detecta riesgo en una sesión con Azure Active Directory Identity Protection | Microsoft Docs'
description: En esta guía de inicio rápido aprenderá a configurar una directiva de acceso condicional por riesgo de inicio de sesión de Azure Active Directory (Azure AD) Identity Protection para bloquear inicios de sesión en función de los riesgos de la sesión.
services: active-directory
keywords: protección de identidad, acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f5127342f97a90103ef56efbd7465832440ec0f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521821"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Inicio rápido: Bloqueo del acceso cuando se detecta riesgo en una sesión con Azure Active Directory Identity Protection  

Para mantener el entorno protegido, puede bloquear el inicio de sesión de los usuarios sospechosos. Azure Active Directory (Azure AD) Identity Protection analiza todos los inicios de sesión y calcula la probabilidad que hay de que el legítimo propietario de una cuenta de usuario no haya sido el que haya realizado un intento de inicio de sesión. Dicha probabilidad (baja, media, alta) se indica en forma de un valor calculado, que se denomina nivel de riesgo de inicio de sesión. Al establecer la condición de riesgo del inicio de sesión, puede configurar una directiva de acceso condicional por riesgo de inicio de sesión que responda a niveles de riesgo de inicio de sesión concretos. 

En esta guía de inicio rápido se muestra cómo configurar una directiva de acceso condicional por riesgo de inicio de sesión que bloquee un inicio de sesión cuando se haya detectado un nivel de riesgo de inicio de sesión medio y superior. 

![Creación de directiva](./media/quickstart-sign-in-risk-policy/1004.png)


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.



## <a name="prerequisites"></a>Requisitos previos 

Para completar el escenario de este tutorial, necesita:

- **Acceso a una edición de Azure AD Premium P2**: Azure AD Identity Protection es una característica de Azure AD Premium P2. 

- **Identity Protection**: el escenario de esta guía de inicio rápido requiere que se habilite Identity Protection. Si no sabe cómo habilitarlo, consulte [Habilitación de Azure Active Directory Identity Protection](../identity-protection/enable.md).

- **Tor Browser**: [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) está diseñado para ayudarle a mantener su privacidad en Internet. Identity Protection detecta un inicio de sesión desde Tor Browser como **inicios de sesión desde direcciones IP anónimas**, lo que tiene un nivel de riesgo medio. Para más información, consulte [Eventos de riesgo de Azure Active Directory](../reports-monitoring/concept-risk-events.md).  

- **Una cuenta de prueba denominada Alain Charon**: si no sabe cómo crear una cuenta de prueba, consulte [Adición de un nuevo usuario](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).


## <a name="test-your-sign-in"></a>Prueba del inicio de sesión 

El objetivo de este paso es asegurarse de que la cuenta de prueba puede acceder a su inquilino mediante Tor Browser.

**Para probar el inicio de sesión:**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como **Alain Charon**.

2. Cierre la sesión. 


## <a name="create-your-conditional-access-policy"></a>Creación de la directiva de acceso condicional 

El escenario de esta guía de inicio rápido usa un inicio de sesión desde Tor Browser para generar la detección de un evento de riesgo **Inicios de sesión desde direcciones IP anónimas**. El nivel de riesgo de este evento es medio. Para responder a este evento de riesgo, la condición de riesgo de inicio de sesión se establece en medio. 

En esta sección se muestra cómo crear la directiva de acceso condicional por riesgo de inicio de sesión que se requiere. En la directiva, establezca:

|Configuración |Valor|
|---     | --- |
| Usuarios  | Alain Charon  |
| Condiciones | Riesgo de inicio de sesión, medio y superior |
| Controles | Bloquear acceso |
 

![Creación de directiva](./media/quickstart-sign-in-risk-policy/201.png)

 


**Para configurar la directiva de acceso condicional:**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global.

2. Vaya a la [página de Azure AD Identity Protection](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/Overview).
 
3. En la página **Azure AD Identity Protection**, en la sección **Configurar**, haga clic en **Directiva de riesgo de inicio de sesión**.
 
4. En la página de la directiva, en la sección **Asignaciones**, haga clic en **Usuarios**.

5. En la página **Usuarios**, haga clic en **Seleccionar usuarios**.

6. En la página **Seleccionar usuarios**, seleccione **Alain Charon** y haga clic en **Seleccionar**.

7. En la página **Usuarios**, haga clic en **Listo**. 

8. En la página de la directiva, en la sección **Asignaciones**, haga clic en **Condiciones**.

9. En la página **Condiciones**, haga clic en **Riesgo de inicio de sesión**.

10. En la página **Riesgo de inicio de sesión**, seleccione **Medio y superior** y, a continuación, haga clic en **Seleccionar**. 

11. En la página **Condiciones**, haga clic en **Listo**.

12. En la página de la directiva, en la sección **Controles**, haga clic en **Acceso**.

13. En la página **Acceso**, haga clic en **Permitir el acceso**, seleccione **Requerir autenticación multifactor** y, a continuación, haga clic en **Seleccionar**.

14. En la página de la directiva, haga clic en **Guardar**.  


## <a name="test-your-conditional-access-policy"></a>Prueba de la directiva de acceso condicional

Para probar la directiva, pruebe a iniciar sesión en [Azure Portal](https://portal.azure.com) como **Alan Charon** mediante Tor Browser. La directiva de acceso condicional debe bloquear el intento de inicio de sesión.

![Multi-Factor Authentication](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no sea necesario, elimine el usuario de prueba y Tor Browser y deshabilite la directiva de acceso condicional por riesgo de inicio de sesión:

- Si no sabe cómo eliminar un usuario de Azure AD, consulte [cómo agregar o eliminar usuarios](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Para obtener instrucciones para quitar Tor Browser, consulte [Uninstalling](https://tb-manual.torproject.org/uninstalling/) (Desinstalación).


