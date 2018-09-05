---
title: 'Tutorial: Integración de Azure Active Directory con Workteam | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Workteam.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 41df17a1-ba69-414f-8ec3-11079b030df6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.openlocfilehash: 8d6ca6395e4f5e1aca361c56e21afc4e6bd1fc0c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43132447"
---
# <a name="tutorial-azure-active-directory-integration-with-workteam"></a>Tutorial: Integración de Azure Active Directory con Workteam

En este tutorial, aprenderá a integrar Workteam con Azure Active Directory (Azure AD).

La integración de Workteam con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Workteam.
- Puede permitir que los usuarios inicien sesión automáticamente en Workteam (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Workteam, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción a Workteam habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Workteam desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-workteam-from-the-gallery"></a>Incorporación de Workteam desde la Galería
Para configurar la integración de Workteam en Azure AD, es preciso agregar Workteam desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Workteam desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Workteam**, seleccione **Workteam** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Workteam en la lista de resultados](./media/workteam-tutorial/tutorial_workteam_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Workteam con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Workteam para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Workteam.

Para configurar y probar el inicio de sesión único de Azure AD con Workteam, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Workteam](#create-a-workteam-test-user)**: para tener un homólogo de Britta Simon en Workteam que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Workteam.

**Para configurar el inicio de sesión único de Azure AD con Workteam, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Workteam**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/workteam-tutorial/tutorial_workteam_samlbase.png)

3. En la sección **Dominio y direcciones URL de Workteam**, el usuario no tiene que realizar ningún paso ya que la aplicación se ha integrado previamente con Azure.

    ![Información de dominio y direcciones URL de inicio de sesión único de Workteam](./media/workteam-tutorial/tutorial_workteam_url.png)

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Workteam](./media/workteam-tutorial/tutorial_workteam_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://app.workte.am`
     
5. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/workteam-tutorial/tutorial_workteam_certificate.png) 

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/workteam-tutorial/tutorial_general_400.png)
    
7. En la sección **Configuración de Workteam**, haga clic en **Configurar Workteam** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configuración de Workteam](./media/workteam-tutorial/tutorial_workteam_configure.png) 

8. En otra ventana del explorador web, inicie sesión en Workteam como administrador de seguridad.

9. En la esquina superior derecha, haga clic en el **logotipo del perfil** y, a continuación, haga clic en **Configuración de la organización**. 

    ![Configuración de Workteam](./media/workteam-tutorial/tutorial_workteam_settings.png)

10. En la sección **AUTENTICACIÓN**, haga clic en el **icono de configuración**.

     ![Workteam azure](./media/workteam-tutorial/tutorial_workteam_azure.png)

11. En la página **SAML Settings** (Configuración de SAML), realice los pasos siguientes:

     ![Workteam saml](./media/workteam-tutorial/tutorial_workteam_saml.png)

    a. En **SAML IdP** (Proveedor de identidades de SAML), seleccione **AD Azure**.

    b. En el cuadro de texto **SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML), pegue el valor **correspondiente**, que ha copiado de Azure Portal.

    c. En el cuadro de texto **SAML Entity ID** (Identificador de entidad de SAML), pegue el valor **correspondiente** que ha copiado de Azure Portal.

    d. En el Bloc de notas, abra el **certificado codificado en Base 64** que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro **Certificado de firma SAML (Base64)**.

    e. Haga clic en **OK**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/workteam-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/workteam-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/workteam-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/workteam-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-workteam-test-user"></a>Creación de un usuario de prueba de Workteam

Para permitir que los usuarios de Azure AD inicien sesión en Workteam, tienen que aprovisionarse en Workteam. En Workteam, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Workteam como administrador de seguridad.

2. En la parte superior central de la página **Configuración de la organización** página, haga clic en **USUARIOS** y, a continuación, haga clic en **NUEVO USUARIO**.

    ![Usuario de Workteam](./media/workteam-tutorial/tutorial_workteam_user.png)

3. En la página **Nuevo empleado**, realice los pasos siguientes:

    ![Nuevo usuario de Workteam](./media/workteam-tutorial/tutorial_workteam_newuser.png)

    a. En el cuadro de texto **Name** (Nombre), escriba el nombre del usuario de la siguiente manera: **Brittasimon**.

    b. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon@contoso.com**.

    c. Haga clic en **OK**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Workteam.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Workteam, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Workteam**.

    ![Vínculo a Workteam en la lista de aplicaciones](./media/workteam-tutorial/tutorial_workteam_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Workteam en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Workteam.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/workteam-tutorial/tutorial_general_01.png
[2]: ./media/workteam-tutorial/tutorial_general_02.png
[3]: ./media/workteam-tutorial/tutorial_general_03.png
[4]: ./media/workteam-tutorial/tutorial_general_04.png

[100]: ./media/workteam-tutorial/tutorial_general_100.png

[200]: ./media/workteam-tutorial/tutorial_general_200.png
[201]: ./media/workteam-tutorial/tutorial_general_201.png
[202]: ./media/workteam-tutorial/tutorial_general_202.png
[203]: ./media/workteam-tutorial/tutorial_general_203.png

