---
title: 'Tutorial: Integración de Azure Active Directory con ArcGIS Enterprise | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y ArcGIS Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24809e9d-a4aa-4504-95a9-e4fcf484f431
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: ca5bf7ae49cf120c0566419ccadeff92433c6467
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42819437"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Tutorial: Integración de Azure Active Directory con ArcGIS Enterprise

En este tutorial, aprenderá a integrar ArcGIS Enterprise con Azure Active Directory (Azure AD).

La integración de ArcGIS Enterprise con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a ArcGIS Enterprise.
- Puede permitir que los usuarios inicien sesión automáticamente en ArcGIS Enterprise (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ArcGIS Enterprise, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en ArcGIS Enterprise

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de ArcGIS Enterprise desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-arcgis-enterprise-from-the-gallery"></a>Adición de ArcGIS Enterprise desde la galería

Para configurar la integración de ArcGIS Enterprise en Azure AD, es preciso agregar dicha solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ArcGIS Enterprise desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **ArcGIS Enterprise**, seleccione **ArcGIS Enterprise** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![ArcGIS Enterprise en la lista de resultados](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con ArcGIS Enterprise mediante un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ArcGIS Enterprise para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ArcGIS Enterprise.

Para configurar y probar el inicio de sesión único de Azure AD con ArcGIS Enterprise, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de ArcGIS Enterprise](#create-an-arcgis-enterprise-test-user)**: el objetivo es tener un homólogo de Britta Simon en ArcGIS Enterprise que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación ArcGIS Enterprise.

**Para configurar el inicio de sesión único de Azure AD con ArcGIS Enterprise, realice los pasos siguientes:**

1. En la página de integración de la aplicación **ArcGIS Enterprise** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_samlbase.png)

3. En la sección **Dominio y direcciones URL de ArcGIS Enterprise**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url1.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `<EXTERNAL_DNS_NAME>.portal`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`.

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url2.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de ArcGIS Enterprise](mailto:nshampur@esri.com) para obtener estos valores. Obtendrá el valor del identificador en la sección **Set Identity Provider** (Definición del proveedor de identidades), que se explica más adelante en este tutorial.

5. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_certificate.png)

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/arcgisenterprise-tutorial/tutorial_general_400.png)

7. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de ArcGIS Enterprise.

8. Seleccione **Organization >EDIT SETTINGS** (Organización > Editar configuración).

    ![Configuración de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure1.png)

9. Seleccione la pestaña **Seguridad**.

    ![Configuración de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure2.png)

10. Desplácese hacia abajo hasta la sección **Enterprise Logins via SAML** (Inicios de sesión empresariales mediante SAML) y seleccione **SET ENTERPRISE LOGIN** (Establecer inicio de sesión empresarial).

    ![Configuración de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure3.png)

11. En la sección **Set Identity Provider** (Definición del proveedor de identidades), realice los siguientes pasos:

    ![Configuración de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure4.png)

    a. Proporcione un nombre como **Azure Active Directory Test** (Prueba de Azure Active Directory) en el cuadro de texto **Name** (Nombre).

    b. En el cuadro de texto **URL**, pegue el valor de **Dirección URL de metadatos de federación de aplicación** que copió de Azure Portal.

    c. Haga clic en **Show advanced settings** (Mostrar configuración avanzada) y copie el valor de **Entity ID** (Id. de entidad) y péguelo en el cuadro de texto **Identificador** de la sección **Dominio y direcciones URL de ArcGIS Enterprise** de Azure Portal.
    
    ![Configuración de ArcGIS Enterprise](./media/arcgisenterprise-tutorial/configure5.png)

    d. Haga clic en **UPDATE IDENTITY PROVIDER** (Actualizar proveedor de identidades).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/arcgisenterprise-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/arcgisenterprise-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/arcgisenterprise-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/arcgisenterprise-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-an-arcgis-enterprise-test-user"></a>Creación de un usuario de prueba de ArcGIS Enterprise

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en ArcGIS Enterprise. ArcGIS Enterprise admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Durante un intento de acceder a ArcGIS Enterprise se crea un nuevo usuario, en caso de que no exista.

> [!Note]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de ArcGIS Enterprise](mailto:nshampur@esri.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ArcGIS Enterprise.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a ArcGIS Enterprise, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **ArcGIS Enterprise**.

    ![Vínculo a ArcGIS Enterprise en la lista de aplicaciones](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ArcGIS Enterprise del panel de acceso, debería iniciar sesión automáticamente en la aplicación ArcGIS Enterprise.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/arcgisenterprise-tutorial/tutorial_general_01.png
[2]: ./media/arcgisenterprise-tutorial/tutorial_general_02.png
[3]: ./media/arcgisenterprise-tutorial/tutorial_general_03.png
[4]: ./media/arcgisenterprise-tutorial/tutorial_general_04.png

[100]: ./media/arcgisenterprise-tutorial/tutorial_general_100.png

[200]: ./media/arcgisenterprise-tutorial/tutorial_general_200.png
[201]: ./media/arcgisenterprise-tutorial/tutorial_general_201.png
[202]: ./media/arcgisenterprise-tutorial/tutorial_general_202.png
[203]: ./media/arcgisenterprise-tutorial/tutorial_general_203.png