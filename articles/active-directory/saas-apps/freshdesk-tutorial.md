---
title: 'Tutorial: Integración de Azure Active Directory con Freshdesk | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: jeedes
ms.openlocfilehash: b5968b83fc9beb481e2ad2c0cd44d2c284747fa1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010820"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Integración de Azure Active Directory con Freshdesk

En este tutorial, obtendrá información sobre cómo integrar FreshDesk con Azure Active Directory (Azure AD).

La integración de FreshDesk con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a FreshDesk.
- Puede permitir que los usuarios inicien sesión automáticamente en FreshDesk (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con FreshDesk, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en FreshDesk

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar FreshDesk desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-freshdesk-from-the-gallery"></a>Agregar FreshDesk desde la galería

Para configurar la integración de FreshDesk en Azure AD, deberá agregar FreshDesk desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar FreshDesk desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **FreshDesk**, seleccione **FreshDesk** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![FreshDesk en la lista de resultados](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con FreshDesk con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de FreshDesk para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de FreshDesk.

Para configurar y probar el inicio de sesión único de Azure AD con FreshDesk, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de FreshDesk](#creating-a-freshdesk-test-user)**: para tener un homólogo de Britta Simon en FreshDesk que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación FreshDesk.

**Para configurar el inicio de sesión único de Azure AD con FreshDesk, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **FreshDesk**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de FreshDesk](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.freshdesk.com` u otro valor que Freshdesk haya sugerido.

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.freshdesk.com` u otro valor que Freshdesk haya sugerido.

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg).

5. La aplicación FreshDesk espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla le muestra un ejemplo de esto. El valor predeterminado de **Identificador de usuario** es **user.userprincipalname**, pero **FreshDesk** espera que este valor se asigne a la dirección de correo electrónico del usuario. Para ello, puede usar el atributo **user.mail** de la lista o usar el valor de atributo correspondiente en función de la configuración de su organización.

    ![imagen](./media/freshdesk-tutorial/i4-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:
    
    a. Haga clic en el icono **Edit** (Editar) para abrir el cuadro de diálogo **Manage user claims** (Administrar notificaciones de usuario).

    ![imagen](./media/freshdesk-tutorial/i2-attribute.png)

    ![imagen](./media/freshdesk-tutorial/i3-attribute.png)

    b. En la lista **Source attribute** (Atributo de origen), seleccione **user.mail**.

    c. Haga clic en **Save**(Guardar).

7. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

8. Abra un **símbolo del sistema** y ejecute los comandos siguientes:

    a. Escriba el valor `certutil.exe -dump FreshDesk.cer` en el símbolo del sistema.

    > [!NOTE]
    > Aquí, **FreshDesk.cer** es el certificado que ha descargado de Azure Portal.

    b. Copie el valor **Cert Hash(sha256)** (Hash de certificado[sha256]) y péguelo en el Bloc de notas. 

9. En la sección **Set up FreshDesk** (Configurar FreshDesk), copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de FreshDesk](common/configuresection.png)

10. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Freshdesk como administrador.

11. Seleccione el **icono Configuración** y, en la sección **Seguridad**, siga estos pasos:

    ![Inicio de sesión único](./media/freshdesk-tutorial/IC776770.png "Inicio de sesión único")
  
    a. En **Inicio de sesión único (SSO)**, seleccione **Activado**.

    b. Seleccione **Inicio de sesión único de SAML**.

    c. En el cuadro de texto **SAML Login URL** (URL de inicio de sesión SAML), pegue el valor de **Login URL** (URL de inicio de sesión) que ha copiado de Azure Portal.

    d. En el cuadro de texto **Sign Out URL** (URL de cierre de sesión), pegue el valor de **Logout URL** (Dirección URL de cierre de sesión) que ha copiado de Azure Portal.

    e. En el cuadro de texto **Security Certificate Fingerprint** (Huella digital de certificado de seguridad), pegue el valor **Cert Hash(sha256)** (Hash de certificado[sha256]) que ha obtenido anteriormente.
  
    f. Haga clic en **Save**(Guardar).

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-freshdesk-test-user"></a>Creación de un usuario de prueba de FreshDesk

Para permitir que los usuarios de Azure AD inicien sesión en FreshDesk, deben aprovisionarse en FreshDesk.  
En el caso de FreshDesk, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **Freshdesk** .

2. En el menú de la parte superior, haga clic en **Administrador**.

    ![Administración](./media/freshdesk-tutorial/IC776772.png "Administración")

3. En la pestaña **Configuración general**, haga clic en **Agentes**.
  
    ![Agentes](./media/freshdesk-tutorial/IC776773.png "Agentes")

4. Haga clic en **Nuevo agente**.

    ![Nuevo agente](./media/freshdesk-tutorial/IC776774.png "Nuevo agente")

5. En el cuadro de diálogo Agent Information (Información de agente), realice los pasos siguientes:

    ![Información sobre agentes](./media/freshdesk-tutorial/IC776775.png "Información sobre agentes")

    a. En el cuadro de texto **Email** (Correo electrónico), escriba la dirección de correo electrónico de la cuenta de Azure AD que quiera aprovisionar.

    b. En el cuadro de texto **Nombre completo** , escriba el nombre de la cuenta de Azure AD que quiera aprovisionar.

    c. En el cuadro de texto **Título** , escriba el título de la cuenta de Azure AD que quiera aprovisionar.

    d. Haga clic en **Save**(Guardar).

    >[!NOTE]
    >El titular de la cuenta de Azure AD recibirá un mensaje de correo electrónico que incluye un vínculo para confirmar la cuenta antes de que se active.
    >
    >[!NOTE]
    >Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Freshdesk que proporcione Freshdesk para aprovisionar cuentas de usuario de AAD a FreshDesk.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure, para lo cual le concederá acceso a FreshDesk.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **FreshDesk**.

    ![Configurar inicio de sesión único](./media/freshdesk-tutorial/tutorial_freshdesk_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de FreshDesk en el Panel de acceso, iniciará sesión automáticamente en su aplicación FreshDesk.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
