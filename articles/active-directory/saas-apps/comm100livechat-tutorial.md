---
title: 'Tutorial: Integración de Azure Active Directory con Comm100 Live Chat | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Comm100 Live Chat.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0340d7f3-ab54-49ef-b77c-62a0efd5d49c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: b85162c8392e8ecdb08a3ed04ff5b9de835808a1
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42143041"
---
# <a name="tutorial-azure-active-directory-integration-with-comm100-live-chat"></a>Tutorial: Integración de Azure Active Directory con Comm100 Live Chat

En este tutorial, aprenderá a integrar Comm100 Live Chat con Azure Active Directory (Azure AD).

Integrar Comm100 Live Chat con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Comm100 Live Chat.
- Puede permitir que los usuarios inicien sesión automáticamente en Comm100 Live Chat (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Comm100 Live Chat, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Comm100 Live Chat

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Comm100 Live Chat en directo desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-comm100-live-chat-from-the-gallery"></a>Adición de Comm100 Live Chat en directo desde la Galería
Para configurar la integración de Comm100 Live Chat en Azure AD, será preciso que agregue Comm100 Live Chat Comm100 Live Chat desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Comm100 Live Chat desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Comm100 Live Chat**, seleccione **Comm100 Live Chat** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Comm100 Live Chat en directo en la lista de resultados](./media/comm100livechat-tutorial/tutorial_comm100livechat_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Comm100 Live Chat con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Comm100 Live Chat para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Comm100 Live Chat.

Para configurar y probar el inicio de sesión único de Azure AD con Comm100 Live Chat, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Comm100 Live Chat](#create-a-comm100-live-chat-test-user)**: para tener un homólogo de Britta Simon en Comm100 Live Chat que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Comm100 Live Chat.

**Para configurar el inicio de sesión único de Azure AD con Comm100 Live Chat, siga este procedimiento:**

1. En la página de integración de aplicaciones **Comm100 Live Chat** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/comm100livechat-tutorial/tutorial_comm100livechat_samlbase.png)

3. En la sección **Dominio y direcciones URL de Comm100 Live Chat**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.comm100.com/AdminManage/LoginSSO.aspx?siteId=<SITEID>`.
    
    > [!NOTE] 
    > El valor de la dirección URL de inicio de sesión no es real. El valor de la dirección URL de inicio de sesión se actualizará con el real, lo que se explica más adelante en el tutorial.

4. La aplicación Comm100 Live Chat espera que las aserciones SAML contengan atributos específicos. Configure los siguientes atributos para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.

    ![Configurar inicio de sesión único](./media/comm100livechat-tutorial/tutorial_attribute_03.png)
    
5. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    |  Nombre del atributo  | Valor de atributo |
    | --------------- | -------------------- |    
    |   email    | user.mail |

    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/comm100livechat-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/comm100livechat-tutorial/tutorial_attribute_05.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    d. Deje **Espacio de nombres** en blanco.
    
    e. Haga clic en **Aceptar**.

6. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/comm100livechat-tutorial/tutorial_comm100livechat_certificate.png) 

7. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/comm100livechat-tutorial/tutorial_general_400.png)

8. En la sección **Configuración de Comm100 Live Chat**, haga clic en **Configurar Comm100 Live Chat** para abrir la ventana **Configurar inicio de sesión**. Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_configure.png)

9. En otra ventana del explorador web, inicie sesión en Comm100 Live Chat como administrador de seguridad.

10. En la parte superior derecha de la página, haga clic en **My Account** (Mi cuenta).

    ![Mi cuenta de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

11. En el lado izquierdo del menú, haga clic en **Seguridad** y, a continuación, haga clic en **Agent Single Sign-On** (Agente Inicio de sesión único).

    ![Seguridad de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_security.png)

12. En la página **Agent Single Sign-On** (Agente Inicio de sesión único) realice los pasos siguientes:

    ![Seguridad de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_singlesignon.png)

    a. Copie el primer vínculo resaltado y péguelo en el cuadro de texto **URL de inicio de sesión** en la sección **Comm100 Live Chat Domain and URLs** (Dominio y direcciones URL de Comm100 Live Chat) de Azure Portal.

    b. En el cuadro de texto **SAML SSO URL** (Dirección URL de inicio de sesión único de SAML), pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Remote Logout URL** (Dirección URL de cierre de sesión remoto), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

    d. Haga clic en **Choose a File** (Elegir un archivo) para cargar el certificado codificado en base 64 que descargó de Azure Portal en **Certificado**.

    e. Haga clic en **Guardar cambios**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/comm100livechat-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/comm100livechat-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/comm100livechat-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/comm100livechat-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-comm100-live-chat-test-user"></a>Creación de un usuario de prueba Comm100 Live Chat

Para permitir que los usuarios de Azure AD inicien sesión en Comm100 Live Chat, es necesario que se aprovisionen en Comm100 Live Chat. En Comm100 Live Chat, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Comm100 Live Chat como administrador de seguridad.

2. En la parte superior derecha de la página, haga clic en **My Account** (Mi cuenta).

    ![Mi cuenta de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

3. En el lado izquierdo del menú, haga clic en **Agents** (Agentes) y, a continuación, haga clic en **New Agent** (Nuevo agente).

    ![Agente de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_agent.png)

4. En la página **New Agent** (Nuevo agente), realice los pasos siguientes:

    ![Nuevo agente de Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_newagent.png)

    a. a. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon@contoso.com**.

    b. En el cuadro de texto **First Name** (Nombre), escriba el nombre de usuario, en este caso **Britta**.

    c. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso **Simon**.

    d. En el cuadro de texto **Display Name** (Nombre para mostrar), escriba el nombre para mostrar del usuario, en este caso, **Britta Simon**.

    e. En el cuadro de texto **Password** (Contraseña), escriba su contraseña.

    f. Haga clic en **Save**(Guardar).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Comm100 Live Chat para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Comm100 Live Chat, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Comm100 Live Chat**.

    ![Vínculo a Comm100 Live Chat en la lista de aplicaciones](./media/comm100livechat-tutorial/tutorial_comm100livechat_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Comm100 Live Chat en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación Comm100 Live Chat.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/comm100livechat-tutorial/tutorial_general_01.png
[2]: ./media/comm100livechat-tutorial/tutorial_general_02.png
[3]: ./media/comm100livechat-tutorial/tutorial_general_03.png
[4]: ./media/comm100livechat-tutorial/tutorial_general_04.png

[100]: ./media/comm100livechat-tutorial/tutorial_general_100.png

[200]: ./media/comm100livechat-tutorial/tutorial_general_200.png
[201]: ./media/comm100livechat-tutorial/tutorial_general_201.png
[202]: ./media/comm100livechat-tutorial/tutorial_general_202.png
[203]: ./media/comm100livechat-tutorial/tutorial_general_203.png

