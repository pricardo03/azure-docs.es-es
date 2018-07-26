---
title: 'Inicio rápido: solicitar la aceptación de las condiciones de uso antes de acceder a aplicaciones en la nube protegidas mediante el acceso condicional de Azure Active Directory | Microsoft Docs'
description: En esta guía de inicio rápido, aprenderá a solicitar que se acepten las condiciones de uso para poder que el acceso condicional de Azure Active Directory conceda acceso acceder a determinadas aplicaciones en la nube.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional, condiciones de uso
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d5fad320c49691626c820429b07b6864137760b9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449221"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Inicio rápido: solicitar la aceptación de las condiciones de uso antes de acceder a aplicaciones en la nube 

Antes de acceder a determinadas aplicaciones en la nube de su entorno, puede obtener el consentimiento de los usuarios en forma de aceptación de las condiciones de uso (CDU). El acceso condicional de Azure Active Directory (Azure AD) proporciona: 

- Un método sencillo para configurar las condiciones de uso
- La opción para solicitar la aceptación de las condiciones de uso a través de una directiva de acceso condicional  

En esta guía de inicio rápido se muestra cómo configurar una [directiva de acceso condicional de Azure AD](active-directory-conditional-access-azure-portal.md) que requiera la aceptación de unas condiciones de uso para una aplicación en la nube seleccionada en su entorno.

![Creación de directiva](./media/active-directory-conditional-access-tou/5555.png)


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.



## <a name="prerequisites"></a>requisitos previos 

Para completar el escenario en este inicio rápido, necesita:

- **Acceso a una edición de Azure AD Premium**: el acceso condicional a Azure AD es una funcionalidad de Azure AD Premium. 

- **Una cuenta de prueba denominada Isabella Simonsen**: si no sabe cómo crear una cuenta de prueba, consulte [Agregar usuarios basados en la nube](fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


## <a name="test-your-sign-in"></a>Prueba del inicio de sesión

El objetivo de este paso es obtener una impresión del inicio de sesión sin una directiva de acceso condicional.

**Para probar el inicio de sesión:**

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como Isabella Simonsen.

2. Cierre la sesión.




## <a name="create-your-terms-of-use"></a>Creación de condiciones de uso

En esta sección se explican los pasos necesarios para crear unas condiciones de uso de ejemplo. Cuando cree unas condiciones de uso, seleccione el valor de **Enforce with conditional access policy templates** (Exigir con plantillas de directivas de acceso condicional). Al seleccionar **Custom policy** (Directiva personalizada) se abre el cuadro de diálogo para crear una nueva directiva de acceso condicional en cuanto se hayan creado las condiciones de uso.


**Para crear las condiciones de uso:**

1. En Microsoft Word, cree un nuevo documento.

2. Escriba **Mis condiciones de uso**y guárdelo en el equipo como **mytou.pdf**.

3. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.

4. En Azure Portal, en la barra de navegación izquierda, haga clic en **Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-tou/02.png)

5. En la página **Azure Active Directory**, en la sección **Administrar**, haga clic en **Acceso condicional**.

    ![Acceso condicional](./media/active-directory-conditional-access-tou/03.png) 

6. En la sección **Administrar**, haga clic en **Condiciones de uso**.

    ![Términos de uso](./media/active-directory-conditional-access-tou/04.png) 

7. En el menú de la parte superior, haga clic en **Nuevos términos**.

    ![Términos de uso](./media/active-directory-conditional-access-tou/05.png) 

8. En la página **Nuevos términos de uso**:

    ![Términos de uso](./media/active-directory-conditional-access-tou/112.png) 

    a. En el cuadro de texto **Nombre**, escriba un **Mis CDU**.

    b. En el cuadro de texto **Nombre para mostrar**, escriba **Mis CDU**.

    c. Cargue el archivo PDF de las condiciones de uso.

    d. En **Idioma**, seleccione **Inglés**.

    e. En **Requerir a los usuarios que expandan las condiciones de uso**, seleccione **Activado**.

    f. En **Exigir con plantillas de directiva de acceso condicional**, seleccione **Directiva personalizada**.

    g. Haga clic en **Create**(Crear).
 


## <a name="create-your-conditional-access-policy"></a>Creación de la directiva de acceso condicional 

En esta sección se muestra cómo crear la directiva de acceso condicional que se requiere. En el escenario de esta guía de inicio rápido se usa:

- Azure Portal como marcador de posición para las aplicaciones en la nube que requieran que se acepte el CDU. 
- El usuario de ejemplo para probar la directiva de acceso condicional.  

En la directiva, establezca:

|Configuración |Valor|
|---     | --- |
|Usuarios y grupos | Isabella Simonsen |
|Aplicaciones de nube | Microsoft Azure Management (Administración de Microsoft Azure) |
|Conceder acceso | Mi CDU |
 

![Creación de directiva](./media/active-directory-conditional-access-tou/1234.png)

 


**Para configurar la directiva de acceso condicional:**

1. En la página **Nuevo**, en el cuadro de texto **Nombre**, escriba **Requerir CDU para Isabella**.

    ![NOMBRE](./media/active-directory-conditional-access-tou/71.png)

2. En la sección **Asignación**, haga clic en **Usuarios y grupos**.

    ![Usuarios y grupos](./media/active-directory-conditional-access-tou/06.png)

3. En la página **Usuarios y grupos**:

    ![Usuarios y grupos](./media/active-directory-conditional-access-tou/24.png)

    a. Haga clic en **Seleccionar usuarios y grupos** y, luego, seleccione **Usuarios y grupos**.

    b. Haga clic en **Seleccionar**.

    c. En la página **Seleccionar**, seleccione **Isabella Simonsen** y, luego, haga clic en **Seleccionar**.

    d. En la página **Usuarios y grupos**, haga clic en **Listo**.

4. Haga clic en **Aplicaciones en la nube**.

    ![Aplicaciones de nube](./media/active-directory-conditional-access-tou/08.png)

5. En la página **Aplicaciones en la nube**:

    ![Seleccionar aplicaciones en la nube](./media/active-directory-conditional-access-tou/26.png)

    a. Haga clic en **Seleccionar aplicaciones**.

    b. Haga clic en **Seleccionar**.

    c. En la página **Seleccionar**, seleccione **Microsoft Azure Management** (Administración de Microsoft Azure) y, luego, haga clic en **Seleccionar**.

    d. En la página **Aplicaciones en la nube**, haga clic en **Listo**.


6. En la sección **Controles de acceso**, haga clic en **Conceder**.

    ![Controles de acceso](./media/active-directory-conditional-access-tou/10.png)

7. En la página **Conceder**:

    ![Conceder](./media/active-directory-conditional-access-tou/111.png)

    a. Seleccione **Conceder acceso**.

    a. Seleccione **Mis CDU**.

    b. Haga clic en **Seleccionar**.

8. En la sección **Habilitar directiva**, haga clic en **Activar**.

    ![Habilitar directiva](./media/active-directory-conditional-access-tou/18.png)

9. Haga clic en **Create**(Crear).


## <a name="evaluate-a-simulated-sign-in"></a>Evaluación de un inicio de sesión simulado

Ahora que ha configurado la directiva de acceso condicional, probablemente quiere saber si funciona según lo previsto. Como primer paso, use la herramienta de directivas What If del acceso condicional para simular un inicio de sesión del usuario de prueba. La simulación calcula el impacto que este inicio de sesión tiene en las directivas y genera un informe de simulación.  

Para inicializar la herramienta de evaluación de directivas What If, establezca:

- **Isabella Simonsen** como usuario 
- **Microsoft Azure Management** (Administración de Microsoft Azure) como aplicación en la nube


Al hacer clic en **What If**, se crea un informe de simulación que muestra:

- **Requerir CDU para Isabella** en **Directivas que se aplicarán** 
- **Mi CDU** en **Conceder controles**.

![Herramienta de directivas What If](./media/active-directory-conditional-access-tou/79.png)



**Para evaluar la directiva de acceso condicional:**

1. En la página [Acceso condicional - Directivas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies), en la barra de herramientas de la parte superior, haga clic en **What If**.  
 
    ![What If](./media/active-directory-conditional-access-tou/14.png)

2. Haga clic en **Usuarios**, seleccione **Isabella Simonsen** y, luego, haga clic en **Seleccionar**.

    ![Usuario](./media/active-directory-conditional-access-tou/15.png)

2. Para seleccionar una aplicación en la nube:

    ![Aplicaciones de nube](./media/active-directory-conditional-access-tou/16.png)

    a. Haga clic en **Aplicaciones en la nube**.

    b. En la **página Aplicaciones en la nube**, haga clic en **Seleccionar aplicaciones**.

    c. Haga clic en **Seleccionar**.

    d. En la página **Seleccionar**, seleccione **Microsoft Azure Management** (Administración de Microsoft Azure) y, luego, haga clic en **Seleccionar**.

    e. En la página Aplicaciones en la nube, haga clic en **Listo**.

3. Haga clic en **What If**.


## <a name="test-your-conditional-access-policy"></a>Prueba de la directiva de acceso condicional

En la sección anterior, ha aprendido a evaluar un inicio de sesión de simulado. Además de una simulación, también debe probar la directiva de acceso condicional para asegurarse de que funciona según lo previsto. 

Para probar la directiva, intente iniciar sesión en [Azure Portal](https://portal.azure.com) con su cuenta de prueba **Isabella Simonsen**. Debería ver un cuadro de diálogo que requiera que acepte las condiciones de uso.

![Términos de uso](./media/active-directory-conditional-access-tou/57.png)


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, elimine el usuario de prueba y la directiva de acceso condicional:

- Si no sabe cómo eliminar un usuario de Azure AD, consulte [Eliminación de usuarios desde Azure AD](fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Para eliminar la directiva, selecciónela y, a continuación, haga clic en **Eliminar** en la barra de herramientas de acceso rápido.

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-tou/33.png)

- Para eliminar las condiciones de uso, selecciónelo y haga clic en **Eliminar términos** en la barra de herramientas de la parte superior. 

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-tou/29.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Requerir Multi-Factor Authentication para aplicaciones específicas](./active-directory-conditional-access-app-based-mfa.md)
> [Bloquear acceso si se detecta un riesgo para la sesión](./active-directory-conditional-access-app-sign-in-risk.md)

