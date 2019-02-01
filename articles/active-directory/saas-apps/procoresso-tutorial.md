---
title: 'Tutorial: Integración de Azure Active Directory con Procore SSO | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Procore SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: 1cd82188ef9f5a4e0078a8ea21882f99bb8b8f44
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171293"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Tutorial: Integración de Azure Active Directory con Procore SSO

En este tutorial aprenderá a integrar Procore SSO con Azure Active Directory (Azure AD).

La integración de Procore SSO con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Procore SSO.
- Puede permitir que los usuarios inicien sesión automáticamente en Procore SSO (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Procore SSO, necesita lo siguiente:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Procore SSO

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Procore SSO desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-procore-sso-from-the-gallery"></a>Agregar Procore SSO desde la galería

Para configurar la integración de Procore SSO en Azure AD, tiene que agregar Procore SSO desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Procore SSO desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Procore SSO**, seleccione **Procore SSO** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Procore SSO en la lista de resultados](./media/procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Procore SSO con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Procore SSO para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Procore SSO.

Para configurar y probar el inicio de sesión único de Azure AD con Procore SSO, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Procore SSO](#creating-a-procore-sso-test-user)**: para tener un homólogo de Britta Simon en Procore SSO que esté vinculado a la representación de usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Procore SSO.

**Para configurar el inicio de sesión único de Azure AD con Procore SSO, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Procore SSO** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

    ![Información de dominio y direcciones URL de inicio de sesión único de Procore SSO](./media/procoresso-tutorial/tutorial_procoresso_url.png)

5. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **archivo XML de metadatos de federación** y, luego, guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/procoresso-tutorial/tutorial_procoresso_certificate.png) 

6. En la sección **Set up Procore SSO** (Configurar Procore SSO), copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de Procore SSO](common/configuresection.png)

7. Para configurar el inicio de sesión único en **Procore SSO**, inicie sesión como administrador en su sitio de la compañía de procore.

8. En la lista desplegable del cuadro de herramientas, haga clic en **Administrador** para abrir la página de configuración del SSO.

    ![Configurar inicio de sesión único](./media/procoresso-tutorial/procore_tool_admin.png)

9. Pegue los valores en los cuadros tal y como se describe a continuación:

    ![Configurar inicio de sesión único](./media/procoresso-tutorial/procore_setting_admin.png)  

     a. En el cuadro de texto **Dirección URL del emisor de inicio de sesión único**, pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Dirección URL de destino de inicio de sesión único de SAML**, pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    c. Abra ahora el **XML de metadatos de la federación** que descargó antes de Azure Portal y copie el certificado en el etiqueta denominada **X509Certificate**. Pegue el valor copiado en el cuadro **Single Sign On x509 Certificate** (Certificado x509 de inicio de sesión único).

10. Haga clic en **Guardar cambios**.

11. Al finalizar esta configuración, tiene que enviar el **nombre de dominio** (por ejemplo, **contoso.com**) con el que inicia sesión en Procore al [equipo de soporte técnico de Procore](https://support.procore.com/) y ellos activarán el SSO federado para ese dominio.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->

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

### <a name="creating-a-procore-sso-test-user"></a>Creación de un usuario de prueba de Procore SSO

Siga estos pasos para crear un usuario de prueba en Procore del lado de Procore SSO.

1. Inicie sesión como administrador en su sitio de la compañía de procore.  

2. En la lista desplegable del cuadro de herramientas, haga clic en **Directorio** para abrir la página de directorio de la compañía.

    ![Configurar inicio de sesión único](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Haga clic en la opción **Add a Person** (Agregar una persona) para abrir el formulario y especificar la ejecución de las opciones siguientes:

    ![Configurar inicio de sesión único](./media/procoresso-tutorial/Procore_user_add.png)

     a. En el cuadro de texto **Nombre**, escriba el nombre del usuario, por ejemplo, **Britta**.

    b. En el cuadro de texto **Apellido**, escriba el apellido del usuario, por ejemplo, **Simon**.

    c. En el cuadro de texto **Dirección de correo electrónico**, escriba la dirección de correo del usuario**BrittaSimon@contoso.com**.

    d. Seleccione **Permission Template** (Plantilla de permisos) como **Apply Permission Template Later** (Aplicar plantilla de permisos más tarde).

    e. Haga clic en **Create**(Crear).

4. Compruebe los datos del contacto recién agregado y actualícelos.

    ![Configurar inicio de sesión único](./media/procoresso-tutorial/Procore_user_check.png)

5. Haga clic en **Save and Send Invitiation** (Guardar y enviar invitación), si se requiere una invitación por correo electrónico, o en **Guardar** (guardar directamente) para completar el registro del usuario.
    
    ![Configurar inicio de sesión único](./media/procoresso-tutorial/Procore_user_save.png)

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a Procore SSO.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Procore SSO**.

    ![Configurar inicio de sesión único](./media/procoresso-tutorial/tutorial_procoresso_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Procore SSO del panel de acceso, debería iniciar sesión automáticamente en su aplicación Procore SSO.
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
