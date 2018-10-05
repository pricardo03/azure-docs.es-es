---
title: 'Tutorial: Integración de Azure Active Directory con dmarcian | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y dmarcian.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: jeedes
ms.openlocfilehash: 0f8878505280371bf6046c1d1f0d7fc1275dd496
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039894"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Tutorial: Integración de Azure Active Directory con dmarcian

En este tutorial, obtendrá información sobre cómo integrar dmarcian con Azure Active Directory (Azure AD).

La integración de dmarcian con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a dmarcian.
- Puede permitir que los usuarios inicien sesión automáticamente en dmarcian (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con dmarcian, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en dmarcian

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de dmarcian desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-dmarcian-from-the-gallery"></a>Adición de dmarcian desde la galería
Para configurar la integración de dmarcian en Azure AD, es preciso agregar dmarcian desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar dmarcian desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **dmarcian**, seleccione **dmarcian** en el panel de resultados y, luego, haga clic en el botón **Agregar** para añadir la aplicación.

    ![dmarcian en la lista de resultados](./media/dmarcian-tutorial/tutorial_dmarcian_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con dmarcian con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de dmarcian para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de dmarcian.

Para configurar y probar el inicio de sesión único de Azure AD con dmarcian, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de dmarcian](#create-a-dmarcian-test-user)**: para tener un homólogo de Britta Simon en dmarcian que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación dmarcian.

**Para configurar el inicio de sesión único de Azure AD con dmarcian, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **dmarcian**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/dmarcian-tutorial/tutorial_dmarcian_samlbase.png)

3. En la sección **Dominio y direcciones URL de dmarcian**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de dmarcian](./media/dmarcian-tutorial/tutorial_dmarcian_url.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de dmarcian](./media/dmarcian-tutorial/tutorial_dmarcian_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Estos valores no son reales. Los valores se actualizan con el identificador, la dirección URL de inicio de sesión y la dirección URL de respuesta reales, lo que se explica más adelante en el tutorial. 

5. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/dmarcian-tutorial/tutorial_dmarcian_certificate.png) 

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/dmarcian-tutorial/tutorial_general_400.png)
    
7. En otra ventana del explorador web, inicie sesión en dmarcian como administrador de seguridad.

8. Haga clic en **Profile** (Perfil) en la esquina superior derecha y vaya a **Preferences** (Preferencias).

    ![Preferencias ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

9. Desplácese hacia abajo y haga clic en la sección **Single Sign-On** (Inicio de sesión único) y, después, haga clic en **Configure** (Configurar).

    ![El inicio de sesión único ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

10. En la página **SAML Single Sign-On** (Inicio de sesión único de SAML), establezca **Status** (Estado) en **Enabled** (Habilitado) y realice los pasos siguientes:

    ![Autenticación ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * En la sección **Add dmarcian to your Identity Provider** (Agregar dmarcian al proveedor de identidades), haga clic en **COPY** para copiar el valor de **Assertion Consumer Service URL** (Dirección URL del servicio de consumidor de aserciones) de la instancia y péguelo en el cuadro de texto **Dirección URL de respuesta** de la sección **Dominio y direcciones URL de dmarcian** en Azure Portal.

    * En la sección **Add dmarcian to your Identity Provider** (Agregar dmarcian al proveedor de identidades), haga clic en **COPY** para copiar el valor de **Entity ID** (Identificador de entidad) de la instancia y péguelo en el cuadro de texto **Identificador** de la sección **Dominio y direcciones URL de dmarcian** en Azure Portal.

    * En la sección **Set up Authentication** (Configurar la autenticación), en el cuadro de texto **Identity Provider Metadata** (Metadatos del proveedor de identidades), pegue el valor de **Dirección URL de metadatos de federación de aplicación** que copió de Azure Portal.

    * En la sección **Set up Authentication** (Configurar la autenticación), en el cuadro de texto **Attribute Statements** (Instrucciones de atributo), pegue la dirección URL `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    * En la sección **Set up Login URL** (Configurar dirección URL de inicio de sesión), copie el valor de **Login URL** (Dirección URL de inicio de sesión) de la instancia y péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **Dominio y direcciones URL de dmarcian** en Azure Portal.

        > [!Note]
        > Puede modificar la **dirección URL de inicio de sesión** según su organización.

    * Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/dmarcian-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/dmarcian-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/dmarcian-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/dmarcian-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-dmarcian-test-user"></a>Creación de un usuario de prueba de dmarcian

Para permitir que los usuarios de Azure AD inicien sesión en dmarcian, deben aprovisionarse en dmarcian. En dmarcian, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en dmarcian como administrador de seguridad.

2. Haga clic en **Profile** (Perfil) en la esquina superior derecha y vaya a **Manage Users** (Administrar usuarios).

    ![Usuario ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. En el lado derecho de la sección **SSO Users** (Usuarios de SSO), haga clic en **Add New User** (Agregar nuevo usuario).

    ![Agregar usuario ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. En la ventana emergente **Add New user** (Agregar nuevo usuario), realice los siguientes pasos:

    ![Nuevo usuario ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. En el cuadro de texto **New User Email** (Correo electrónico del nuevo usuario), escriba el correo electrónico del usuario con el siguiente formato **brittasimon@contoso.com**.

    b. Si desea conceder derechos de administrador al usuario, seleccione **Make User an Admin** (Hacer que el usuario sea un administrador).

    c. Haga clic en **Agregar usuario**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a dmarcian.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a dmarcian, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **dmarcian**.

    ![Vínculo a dmarcian en la lista de aplicaciones](./media/dmarcian-tutorial/tutorial_dmarcian_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de dmarcian en el panel de acceso, debería iniciar sesión automáticamente en la aplicación dmarcian.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dmarcian-tutorial/tutorial_general_01.png
[2]: ./media/dmarcian-tutorial/tutorial_general_02.png
[3]: ./media/dmarcian-tutorial/tutorial_general_03.png
[4]: ./media/dmarcian-tutorial/tutorial_general_04.png

[100]: ./media/dmarcian-tutorial/tutorial_general_100.png

[200]: ./media/dmarcian-tutorial/tutorial_general_200.png
[201]: ./media/dmarcian-tutorial/tutorial_general_201.png
[202]: ./media/dmarcian-tutorial/tutorial_general_202.png
[203]: ./media/dmarcian-tutorial/tutorial_general_203.png

