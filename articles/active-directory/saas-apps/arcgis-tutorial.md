---
title: 'Tutorial: Integración de Azure Active Directory con ArcGIS Online | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y ArcGIS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 3284202ffaa6767a8dd4a6a5050dbdc928075237
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846128"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Tutorial: Integración de Azure Active Directory con ArcGIS Online

En este tutorial, obtendrá información sobre cómo integrar ArcGIS Online con Azure Active Directory (Azure AD).

La integración de ArcGIS Online con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a ArcGIS Online.
- Puede permitir que los usuarios inicien sesión automáticamente en ArcGIS Online (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ArcGIS Online, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en ArcGIS Online

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de ArcGIS Online desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-arcgis-online-from-the-gallery"></a>Incorporación de ArcGIS Online desde la galería

Para configurar la integración de ArcGIS Online en Azure AD, deberá agregar ArcGIS Online desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ArcGIS Online desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/arcgis-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/arcgis-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/arcgis-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **ArcGIS Online**, seleccione **ArcGIS Online** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/arcgis-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con ArcGIS Online con un usuario de prueba denominado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ArcGIS Online para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ArcGIS Online.

Para establecer la relación de vínculo, en ArcGIS Online, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con ArcGIS Online, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de ArcGIS Online](#create-an-arcgis-online-test-user)**: para tener un homólogo de Britta Simon en ArcGIS Online que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación ArcGIS Online.

**Para configurar el inicio de sesión único de Azure AD con ArcGIS Online, siga estos pasos:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **ArcGIS Online**, haga clic en **Inicio de sesión único**.

    ![imagen](./media/arcgis-tutorial/b1_b2_select_sso.png)

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

      ![imagen](./media/arcgis-tutorial/b1_b2_saml_ssso.png)

3. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/arcgis-tutorial/b1_b2_saml_sso.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/arcgis-tutorial/b1-domains_and_urlsedit.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.maps.arcgis.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `<companyname>.maps.arcgis.com`.

    ![imagen](./media/arcgis-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico para clientes de ArcGIS Online](https://support.esri.com/en/) para obtener estos valores.

6. En la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar **XML de metadatos de federación** y, luego, guarde el archivo XML en el equipo.

    ![imagen](./media/arcgis-tutorial/federationxml.png)

7. Para automatizar la configuración en **ArcGIS Online**, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![imagen](./media/arcgis-tutorial/install_extension.png)

8. Después de agregar la extensión al explorador, haga clic en **Setup ArcGIS Online** (Configurar ArcGIS Online) para ir a la aplicación ArcGIS Online. Desde allí, proporcione las credenciales de administrador para iniciar sesión en ArcGIS Online. La extensión de explorador configurará automáticamente la aplicación y automatizará el paso 9-13.

9. Si quiere configurar ArcGIS Online manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de ArcGIS Online como administrador y realice los pasos siguientes:

10. Haga clic en **EDITAR CONFIGURACIÓN**.

    ![Editar configuración](./media/arcgis-tutorial/ic784742.png "Editar configuración")

11. Haga clic en **Seguridad**.

    ![Seguridad](./media/arcgis-tutorial/ic784743.png "Seguridad")

12. En **Inicios de sesión de la empresa**, haga clic en **ESTABLECER PROVEEDOR DE IDENTIDADES**.

    ![Enterprise Logins (Inicios de sesión de la empresa)](./media/arcgis-tutorial/ic784744.png "Enterprise Logins (Inicios de sesión de la empresa)")

13. En la sección **Configurar proveedor de identidades** , realice los pasos siguientes:

    ![Set Identity Provider (Establecer proveedor de identidades)](./media/arcgis-tutorial/ic784745.png "Set Identity Provider (Establecer proveedor de identidades)")

     a. En el cuadro de texto **Nombre**, escriba el nombre de la organización.

    b. En **Metadata for the Enterprise Identity Provider will be supplied using** (Los metadatos para el proveedor de identidades de la empresa se proporcionarán con), seleccione **A File** (Un archivo).

    c. Haga clic en **Choose file**(Elegir archivo) para cargar el archivo de metadatos descargado.

    d. Haga clic en **ESTABLECER PROVEEDOR DE IDENTIDADES**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/arcgis-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/arcgis-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/arcgis-tutorial/d_userproperties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="create-an-arcgis-online-test-user"></a>Creación de un usuario de prueba de ArcGIS Online

Para permitir que los usuarios de Azure AD inicien sesión en ArcGIS Online, tienen que aprovisionarse en ArcGIS Online.  
En el caso de ArcGIS Online, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **ArcGIS** .

2. Haga clic en **INVITAR A MIEMBROS**.
   
    ![Invitar a miembros](./media/arcgis-tutorial/ic784747.png "Invitar a miembros")

3. Seleccione **Agregar miembros automáticamente sin enviar un correo electrónico** y luego haga clic en **SIGUIENTE**.
   
    ![Agregar miembros automáticamente](./media/arcgis-tutorial/ic784748.png "Agregar miembros automáticamente")

4. En la página de diálogo **Miembros** , realice los pasos siguientes:
   
     ![Agregar y revisar](./media/arcgis-tutorial/ic784749.png "Agregar y revisar")
    
      a. Escriba los valores de **correo electrónico**, **nombre** y **apellido** de una cuenta de AAD válida que desea aprovisionar.
  
     b. Haga clic en **AGREGAR Y REVISAR**.
5. Revise los datos que ha escrito y luego haga clic en **AGREGAR MIEMBROS**.
   
    ![Agregar miembros](./media/arcgis-tutorial/ic784750.png "Agregar miembros")
        
    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ArcGIS Online.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/arcgis-tutorial/d_all_applications.png)

2. En la lista de aplicaciones. seleccione **ArcGIS Online**.

    ![imagen](./media/arcgis-tutorial/d_all_application.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/arcgis-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/arcgis-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ArcGIS Online en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación ArcGIS Online.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



