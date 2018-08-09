---
title: 'Tutorial: integrar Azure Active Directory con Jamf Pro | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2018
ms.author: jeedes
ms.openlocfilehash: 94b8b935728110cd5dd07b2066e8320274e3b082
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428424"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: integrar Azure Active Directory con Jamf Pro

En este tutorial, aprenderá a integrar Jamf Pro con Azure Active Directory (Azure AD).

La integración de Jamf Pro con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Jamf Pro.
- Puede permitir que los usuarios inicien sesión automáticamente en Jamf Pro (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Jamf Pro, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Jamf Pro.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporar Jamf Pro desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-jamf-pro-from-the-gallery"></a>Incorporar Jamf Pro desde la galería
Para configurar la integración de Jamf Pro en Azure AD, deberá agregar Jamf Pro desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Jamf Pro desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Jamf Pro**, seleccione **Jamf Pro** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Jamf Pro en la lista de resultados](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Jamf Pro con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Jamf Pro para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Jamf Pro.

Para configurar y probar el inicio de sesión único de Azure AD con Jamf Pro, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Jamf Pro](#create-a-jamf-pro-test-user)**: para tener un homólogo de Britta Simon en Jamf Pro que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Jamf Pro.

**Para configurar el inicio de sesión único de Azure AD con Jamf Pro, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Jamf Pro**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

1. En la sección **Dominio y direcciones URL de Jamf Pro**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com/saml/SSO`.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com`.
     
    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Obtendrá el valor del identificador real de la sección **Inicio de sesión único** en el portal de Jamf Pro, que se explica más adelante en el tutorial. Puede extraer el valor real del **subdominio** del valor del identificador y usar esa información del **subdominio** en las direcciones URL de inicio de sesión y de respuesta.

1. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa Jamf Pro como administrador.

1. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Haga clic en **Inicio de sesión único de SAML**.

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

1. En la página **Single Sign-On** (Inicio de sesión único) realice los pasos siguientes:

    ![Inicio de sesión único de Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Seleccione **Jamf Pro Server** para habilitar el acceso mediante el inicio de sesión único.

    b. Si selecciona **Allow bypass for all users** (Permitir omisión para todos los usuarios), no se remitirá a los usuarios a la página de inicio de sesión del proveedor de identidades para fines de autenticación, pero podrán iniciar sesión directamente en Jamf Pro. Cuando un usuario intenta tener acceso a Jamf Pro a través del proveedor de identidades, se produce la autorización y autenticación mediante SSO iniciadas por el proveedor de identidades.

    c. Seleccione la opción **NameID** para **USER MAPPING: SAML** (ASIGNACIÓN DE USUARIO: SAML). De forma predeterminada, este valor se establece en **NameID**, pero se puede definir un atributo personalizado.

    d. Seleccione **Email** (Correo electrónico) para **USER MAPPING: JAMF PRO** (ASIGNACIÓN DE USUARIO: JAMF PRO). Jamf Pro asigna los atributos SAML enviados por el proveedor de identidades de las siguientes maneras: por usuarios y por grupos. Cuando un usuario intenta acceder a Jamf Pro, de forma predeterminada, Jamf Pro obtiene información sobre el usuario del proveedor de identidades y la compara con las cuentas de usuario de Jamf Pro. Si la cuenta de usuario entrante no existe en Jamf Pro, se produce una asociación de nombres por grupo.

    e. Pegue el valor `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` en el cuadro de texto **GROUP ATTRIBUTE NAME** (NOMBRE DE ATRIBUTO DE GRUPO).
 
1. En la misma página, desplácese a la sección **PROVEEDOR DE IDENTIDADES** en la sección **Inicio de sesión único** y realice los pasos siguientes:

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. En el menú desplegable **PROVEEDOR DE IDENTIDADES**, seleccione **Otro**.

    b. En el cuadro de texto **OTRO PROVEEDOR**, escriba **Azure AD**.

    c. Seleccione la **dirección URL de metadatos** como una opción del menú desplegable **ORIGEN DE METADATOS DEL PROVEEDOR DE IDENTIDADES** y, en el siguiente cuadro de texto, pegue el valor de la **dirección URL de metadatos de la federación de aplicaciones** que copió en Azure Portal.

    d. Copie el valor del **id. de entidad** y péguelo en el cuadro de texto **Identificador (id. de entidad)**, que se encuentra en la sección **Dominio y direcciones URL de Jamf Pro** de Azure Portal.

    >[!NOTE]
    > Aquí el valor borroso es la parte del subdominio. Use este valor para completar las direcciones URL de inicio de sesión y respuesta en la sección **Dominio y direcciones URL de Jamf Pro** en Azure Portal.

    e. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-jamf-pro-test-user"></a>Crear un usuario de prueba de Jamf Pro

Para permitir que los usuarios de Azure AD inicien sesión en Jamf Pro, tienen que aprovisionarse en Jamf Pro. En el caso de Jamf Pro, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su sitio de la empresa de Jamf Pro como administrador.

1. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Haga clic en **Grupos y cuentas de usuario de Jamf Pro**.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user1.png)

1. Haga clic en **Nuevo**.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user2.png)

1. Seleccione **Crear cuenta estándar**.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user3.png)

1. En el cuadro de diálogo **Nueva cuenta**, realice los pasos siguientes:

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user4.png)

    a. En el cuadro de texto **NOMBRE DE USUARIO**, escriba el nombre completo de Britta Simon.

    b. Seleccione las opciones adecuadas según su organización para **NIVEL DE ACCESO**, **CONJUNTO DE PRIVILEGIOS** y **ESTADO DE ACCESO**.
    
    c. En el cuadro de texto **NOMBRE COMPLETO**, escriba el nombre completo de Britta Simon.

    d. En el cuadro de texto **DIRECCIÓN DE CORREO ELECTRÓNICO**, escriba la dirección de correo electrónico de la cuenta de Britta Simon.

    e. En el cuadro de texto **CONTRASEÑA**, escriba la contraseña del usuario.

    f. En el cuadro de texto **CONFIRMAR CONTRASEÑA**, escriba la contraseña del usuario.

    g. Haga clic en **Save**(Guardar).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Jamf Pro.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Jamf Pro, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Jamf Pro**.

    ![Vínculo a Jamf Pro en la lista de aplicaciones](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Jamf Pro en el Panel de acceso, debe iniciar sesión automáticamente en la aplicación Jamf Pro.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/jamfprosamlconnector-tutorial/tutorial_general_203.png

