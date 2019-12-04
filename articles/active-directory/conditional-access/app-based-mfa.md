---
title: 'Solicitud de Azure MFA con acceso condicional: Azure Active Directory'
description: En este inicio rápido, conocerá cómo puede enlazar sus requisitos de autenticación al tipo de aplicación en la nube a la que se accede mediante el acceso condicional de Azure Active Directory (Azure AD).
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55c9188a1320b92aafa5fc67a253b42b6b107711
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381092"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Inicio rápido: Exigir MFA para aplicaciones específicas con acceso condicional de Azure Active Directory

Para simplificar la experiencia de inicio de sesión de los usuarios, puede permitirles iniciar sesión en sus aplicaciones en la nube mediante un nombre de usuario y una contraseña. Sin embargo, muchos entornos tienen al menos algunas aplicaciones para las que es preferible requerir una forma más segura para verificar la cuenta, como la autenticación multifactor (MFA). Esta directiva podría aplicarse para el acceso a las aplicaciones de recursos humanos o al sistema de correo electrónico de su organización. En Azure Active Directory (Azure AD), puede lograr este objetivo con una directiva de acceso condicional.

En este inicio rápido se muestra cómo configurar una [directiva de acceso condicional de Azure AD](../active-directory-conditional-access-azure-portal.md) que exija la autenticación multifactor para una aplicación en la nube específica de su entorno.

![Ejemplo de directiva de acceso condicional en Azure Portal](./media/app-based-mfa/32.png)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar el escenario en este inicio rápido, necesita:

- **Acceso a una edición de Azure AD Premium**: el acceso condicional de Azure AD es una funcionalidad de Azure AD Premium.
- **Una cuenta de prueba denominada Isabella Simonsen**: si no sabe cómo crear una cuenta de prueba, consulte [Agregar usuarios basados en la nube](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

El escenario descrito en este inicio rápido requiere que la MFA de cada usuario no esté habilitada para la cuenta de prueba. Para obtener más información, consulte [Exigencia de verificación en dos pasos para un usuario](../authentication/howto-mfa-userstates.md).

## <a name="test-your-experience"></a>Pruebe su experiencia

El objetivo de este paso es obtener una impresión de la experiencia sin una directiva de acceso condicional.

**Para inicializar el entorno:**

1. Inicie sesión en Azure Portal como Isabella Simonsen.
1. Cierre la sesión.

## <a name="create-your-conditional-access-policy"></a>Creación de la directiva de acceso condicional

En esta sección se muestra cómo crear la directiva de acceso condicional necesaria. En el escenario de esta guía de inicio rápido se usa:

- Azure Portal como marcador de posición para una aplicación en la nube que requiere MFA. 
- El usuario de ejemplo para probar la directiva de acceso condicional.  

En la directiva, establezca:

| Configuración | Valor |
| --- | --- |
| Usuarios y grupos | Isabella Simonsen |
| Aplicaciones de nube | Microsoft Azure Management (Administración de Microsoft Azure) |
| Conceder acceso | Requerir autenticación multifactor |

![Directiva de acceso condicional expandida](./media/app-based-mfa/31.png)

**Para configurar la directiva de acceso condicional:**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. En Azure Portal, busque y seleccione **Azure Active Directory**.

   ![Azure Active Directory](./media/app-based-mfa/02.png)

1. En la página **Azure Active Directory**, en la sección **Seguridad**, haga clic en **Acceso condicional**.

   ![Acceso condicional](./media/app-based-mfa/03.png)

1. En la página **Acceso condicional**, en la barra de herramientas de la parte superior, haga clic en **Nueva directiva**.

   ![Sumar](./media/app-based-mfa/04.png)

1. En la página **Nuevo**, en el cuadro de texto **Nombre**, escriba **Requerir MFA para obtener acceso a Azure Portal**.

   ![NOMBRE](./media/app-based-mfa/05.png)

1. En la sección **Asignación**, haga clic en **Usuarios y grupos**.

   ![Usuarios y grupos](./media/app-based-mfa/06.png)

1. En la página **Usuarios y grupos**, siga estos pasos:

   ![Usuarios y grupos](./media/app-based-mfa/24.png)

   1. Haga clic en **Seleccionar usuarios y grupos** y, luego, seleccione **Usuarios y grupos**.
   1. Haga clic en **Seleccionar**.
   1. En la página **Seleccionar**, seleccione **Isabella Simonsen** y, luego, haga clic en **Seleccionar**.
   1. En la página **Usuarios y grupos**, haga clic en **Listo**.

1. Haga clic en **Aplicaciones en la nube**.

   ![Aplicaciones de nube](./media/app-based-mfa/08.png)

1. En la página **Aplicaciones en la nube**, siga estos pasos:

   ![Seleccionar aplicaciones en la nube](./media/app-based-mfa/26.png)

   1. Haga clic en **Seleccionar aplicaciones**.
   1. Haga clic en **Seleccionar**.
   1. En la página **Seleccionar**, seleccione **Microsoft Azure Management** (Administración de Microsoft Azure) y, luego, haga clic en **Seleccionar**.
   1. En la página **Aplicaciones en la nube**, haga clic en **Listo**.

1. En la sección **Controles de acceso**, haga clic en **Conceder**.

   ![Controles de acceso](./media/app-based-mfa/10.png)

1. En la página **Conceder**, siga estos pasos:

   ![Conceder](./media/app-based-mfa/11.png)

   1. Seleccione **Conceder acceso**.
   1. Seleccione **Requerir autenticación multifactor**.
   1. Haga clic en **Seleccionar**.

1. En la sección **Habilitar directiva**, haga clic en **Activar**.

   ![Habilitar directiva](./media/app-based-mfa/18.png)

1. Haga clic en **Create**(Crear).

## <a name="evaluate-a-simulated-sign-in"></a>Evaluación de un inicio de sesión simulado

Ahora que ha configurado la directiva de acceso condicional, probablemente quiera saber si funciona según lo previsto. Como primer paso, use la herramienta de directivas What If de acceso condicional para simular un inicio de sesión del usuario de prueba. La simulación calcula el impacto que tiene este inicio de sesión en las directivas y genera un informe de simulación.  

Para inicializar la herramienta de evaluación de directivas **What If**, establezca:

- **Isabella Simonsen** como usuario
- **Microsoft Azure Management** (Administración de Microsoft Azure) como aplicación en la nube

Al hacer clic en **What If**, se crea un informe de simulación que muestra:

- **Requerir MFA para obtener acceso a Azure Portal** en **Directivas que se aplicarán**
- **Requerir autenticación multifactor** para **Conceder controles**.

![Herramienta de directivas What If](./media/app-based-mfa/23.png)

**Para evaluar la directiva de acceso condicional:**

1. En la página [Acceso condicional - Directivas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies), en el menú de la parte superior, haga clic en **What If**.  

   ![What If](./media/app-based-mfa/14.png)

1. Haga clic en **Usuarios**, seleccione **Isabella Simonsen** y, luego, haga clic en **Seleccionar**.

   ![Usuario](./media/app-based-mfa/15.png)

1. Para seleccionar una aplicación en la nube, siga los pasos siguientes:

   ![Aplicaciones de nube](./media/app-based-mfa/16.png)

   1. Haga clic en **Aplicaciones en la nube**.
   1. En la **página Aplicaciones en la nube**, haga clic en **Seleccionar aplicaciones**.
   1. Haga clic en **Seleccionar**.
   1. En la página **Seleccionar**, seleccione **Microsoft Azure Management** (Administración de Microsoft Azure) y, luego, haga clic en **Seleccionar**.
   1. En la página Aplicaciones en la nube, haga clic en **Listo**.

1. Haga clic en **What If**.

## <a name="test-your-conditional-access-policy"></a>Prueba de la directiva de acceso condicional

En la sección anterior, ha aprendido a evaluar un inicio de sesión simulado. Además de una simulación, también debe probar la directiva de acceso condicional para asegurarse de que funciona según lo previsto.

Para probar la directiva, intente iniciar sesión en [Azure Portal](https://portal.azure.com) con su cuenta de prueba **Isabella Simonsen**. Verá un cuadro de diálogo que le pide que configure su cuenta para la comprobación adicional de la seguridad.

![Multi-Factor Authentication](./media/app-based-mfa/22.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sean necesarios, elimine el usuario de prueba y la directiva de acceso condicional:

- Si no sabe cómo eliminar un usuario de Azure AD, consulte [Eliminación de usuarios desde Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Para eliminar la directiva, selecciónela y, a continuación, haga clic en **Eliminar** en la barra de herramientas de acceso rápido.

    ![Multi-Factor Authentication](./media/app-based-mfa/33.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solicitar la aceptación de las condiciones de uso](require-tou.md)
> [Bloquear el acceso si se detecta un riesgo para la sesión](app-sign-in-risk.md)
