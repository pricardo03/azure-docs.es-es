---
title: 'Tutorial: Integración de Azure Active Directory con iWellnessNow | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e iWellnessNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24ffc841-7a77-481c-9cc4-6f8bda58fe66
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: b02a5ec5a68743e148bcfef3c631d7777fd0a067
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871703"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Tutorial: Integración de Azure Active Directory con iWellnessNow

En este tutorial se aprende a integrar iWellnessNow con Azure Active Directory (Azure AD).

La integración de iWellnessNow con Azure AD proporciona las siguientes ventajas:

- En Azure AD, puede controlar quién tiene acceso a iWellnessNow.
- Puede permitir que los usuarios inicien sesión automáticamente en iWellnessNow (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con iWellnessNow, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en iWellnessNow

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de iWellnessNow desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-iwellnessnow-from-the-gallery"></a>Incorporación de iWellnessNow desde la galería
Para configurar la integración de iWellnessNow en Azure AD, es preciso agregar iWellnessNow desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar iWellnessNow desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **iWellnessNow**, seleccione **iWellnessNow** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![iWellnessNow en la lista de resultados](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección se configura y se prueba el inicio de sesión único de Azure AD con iWellnessNow con un usuario de prueba de nombre "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de iWellnessNow para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de iWellnessNow.

Para configurar y probar el inicio de sesión único de Azure AD con iWellnessNow, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de iWellnessNow](#create-an-iwellnessnow-test-user)**, para tener un homólogo de Britta Simon en iWellnessNow que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección se habilita el inicio de sesión único de Azure AD en Azure Portal y se configura en la aplicación iWellnessNow.

**Para configurar el inicio de sesión único de Azure AD con iWellnessNow, realice los pasos siguientes:**

1. En la página de integración de la aplicación **iWellnessNow** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

1. En la sección **Dominio y direcciones URL de iWellnessNow**, si tiene **archivo de metadatos del proveedor de servicios** y quiere configurar la aplicación en el modo iniciado por **IDP**, realice los siguientes pasos:

    ![Carga de inicio de sesión único de Dominio y direcciones URL de iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Configuración de carga de inicio de sesión único de Dominio y direcciones URL de iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.
    
    c. Cuando el **archivo de metadatos del proveedor de servicios** se haya cargado correctamente, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección **Dominio y direcciones URL de iWellnessNow**, como se muestra a continuación:

    ![Información de inicio de sesión único de Dominio y direcciones URL de iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

1. Si no tiene el **archivo de metadatos del proveedor de servicios** y quiere configurar la aplicación en el modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información de inicio de sesión único de Dominio y direcciones URL de iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `http://<CustomerName>.iwellnessnow.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<CustomerName>.iwellnessnow.com/ssologin`.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de inicio de sesión único de Dominio y direcciones URL de iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<CustomerName>.iwellnessnow.com/`.
     
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de atención al cliente de iWellnessNow](mailto:info@iwellnessnow.com) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/iwellnessnow-tutorial/tutorial_general_400.png)
    
1. Para configurar el inicio de sesión único en **iWellnessNow**, necesita enviar el **XML de metadatos** descargado al [equipo de atención al cliente de iWellnessNow](mailto:info@iwellnessnow.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/iwellnessnow-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/iwellnessnow-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/iwellnessnow-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/iwellnessnow-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-an-iwellnessnow-test-user"></a>Crear un usuario de prueba de iWellnessNow

En esta sección se crea un usuario de nombre Britta Simon en iWellnessNow. Trabaje con el  [equipo de atención al cliente de iWellnessNow](mailto:info@iwellnessnow.com) para agregar los usuarios a la plataforma iWellnessNow. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección se habilita a Britta Simon para que use el inicio de sesión único de Azure al concederle acceso a iWellnessNow.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a iWellnessNow, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **iWellnessNow**.

    ![Vínculo de iWellnessNow en la lista de aplicaciones](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de iWellnessNow en el panel de acceso, debería iniciar sesión automáticamente en la aplicación iWellnessNow.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/iwellnessnow-tutorial/tutorial_general_203.png

