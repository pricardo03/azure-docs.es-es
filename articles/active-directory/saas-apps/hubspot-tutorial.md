---
title: 'Tutorial: Integración de Azure Active Directory con HubSpot | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y HubSpot.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26fd70b80dd512b530d367d57cd9fc04ec28f27b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211706"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Tutorial: Integración de Azure Active Directory con HubSpot

En este tutorial, aprenderá a integrar HubSpot con Azure Active Directory (Azure AD).

La integración de HubSpot con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a HubSpot.
- Puede permitir que los usuarios inicien sesión automáticamente en HubSpot (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con HubSpot, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en HubSpot.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de HubSpot desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-hubspot-from-the-gallery"></a>Incorporación de HubSpot desde la galería

Para configurar la integración de HubSpot en Azure AD, será preciso que agregue HubSpot desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar HubSpot desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. En el cuadro de búsqueda, escriba **HubSpot**. En el panel de resultados, seleccione **HubSpot** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/hubspot-tutorial/tutorial_hubspot_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con HubSpot con un usuario de prueba llamado "Britta Simon"

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de HubSpot para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de HubSpot.

Para configurar y probar el inicio de sesión único de Azure AD con HubSpot, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **Creación de un usuario de prueba de HubSpot**, para tener un homólogo de Britta Simon en HubSpot que esté vinculado a la representación de Azure AD de usuario.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación HubSpot.

**Para configurar el inicio de sesión único de Azure AD con HubSpot, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **HubSpot**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/tutorial_general_301.png)

3. Si necesita cambiar al modo **SAML** desde cualquier otro, haga clic en la opción **Cambiar el modo de inicio de sesión único** de la parte superior de la pantalla.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/tutorial_general_300.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/tutorial_general_302.png)

5. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de HubSpot](./media/hubspot-tutorial/tutorial_hubspot_url.png)

     a. En el cuadro de texto **Identificador**, escriba la dirección URL con el siguiente patrón: `https://api.hubspot.com/login-api/v1/saml/login?portalId=<CUSTOMER ID>`.

    b. En el cuadro de texto **URL de respuesta**, escriba la dirección URL con el siguiente patrón: `https://api.hubspot.com/login-api/v1/saml/acs?portalId=<CUSTOMER ID>`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de respuesta y el identificador reales, que se explican más adelante en el tutorial. Póngase en contacto con el [equipo de soporte de cliente de HubSpot](https://help.hubspot.com/) para obtener estos valores.

    c. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de HubSpot](./media/hubspot-tutorial/tutorial_hubspot_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://app.hubspot.com/login`

6. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/tutorial_hubspot_certificate.png)

7. En la sección **Configurar HubSpot**, haga clic en el botón Copiar para copiar los valores de **Dirección URL de inicio de sesión** e **Identificador de Azure AD**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/tutorial_hubspot_configure.png)

8. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de HubSpot.

9. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config1.png)

10. Haga clic en **Account Defaults** (Valores predeterminados de cuenta).

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config2.png)

11. Desplácese hacia abajo hasta la sección **Security** (Seguridad) y haga clic en **Set up** (Configurar).

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config3.png)

12. En la sección **Set Up Single Sign-On** (Configurar inicio de sesión único), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config4.png)

     a. Haga clic en el botón **copy** (copiar) para copiar el valor de **Audience URl(Service Provider Entity ID)** (URI de audiencia [Identificador de entidad de proveedor de servicios]) y pegarlo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** de Azure Portal.

    b. Haga clic en el botón **copy** (copiar) para copiar el valor de **Sign on URl,ACS,Recipient, or Redirect** (Redireccionamiento, destinatario, ACS o URI de inicio de sesión) y pegarlo en el cuadro de texto **URL de respuesta** de la sección **Configuración básica de SAML** de Azure Portal.

    c. En el cuadro de texto **Identity Provider Identifier or Issuer URL** (URL del emisor o identificador de proveedor de identidades), pegue el valor de **Azure AD Identifier** (Identificador de Azure AD) que ha copiado de Azure Portal.

    d. En el cuadro de texto **Dirección URL del inicio de sesión único del proveedor de identidades**, pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    e. Abra el archivo **Certificate(Base64)** descargado en el Bloc de notas. Copie el contenido del mismo en el Portapapeles y, después, péguelo en el cuadro de texto  **Certificado X.509**.

    f. Haga clic en **Comprobar**.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](./media/hubspot-tutorial/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](./media/hubspot-tutorial/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-hubspot-test-user"></a>Creación de un usuario de prueba de HubSpot

Para permitir que los usuarios de Azure AD inicien sesión en HubSpot, deben aprovisionarse en HubSpot.
En el caso de HubSpot, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de compañía de **HubSpot** como administrador.

2. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config1.png)

3. Haga clic en **Users & Teams** (Usuarios y equipos).

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user1.png)

4. Haga clic en **Crear usuario**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user2.png)

5. Escriba la dirección de correo electrónico del usuario como **brittasimon@contoso.com** en el cuadro de texto **Add email addess(es)** (Agregar direcciones de correo electrónico) y haga clic en **Siguiente**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user3.png)

6. En la sección **Crear usuarios**, desplácese por cada pestaña y seleccione las opciones y los permisos adecuados para el usuario y haga clic en **Siguiente**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user4.png)

7. Haga clic en **Enviar** para enviar la invitación al usuario.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > El usuario se activará después de aceptar la invitación.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a HubSpot para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **HubSpot**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/tutorial_hubspot_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de HubSpot en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de HubSpot.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/hubspot-tutorial/tutorial_general_01.png
[2]: ./media/hubspot-tutorial/tutorial_general_02.png
[3]: ./media/hubspot-tutorial/tutorial_general_03.png
[4]: ./media/hubspot-tutorial/tutorial_general_04.png
[100]: ./media/hubspot-tutorial/tutorial_general_100.png
[200]: ./media/hubspot-tutorial/tutorial_general_200.png
[201]: ./media/hubspot-tutorial/tutorial_general_201.png
[202]: ./media/hubspot-tutorial/tutorial_general_202.png
[203]: ./media/hubspot-tutorial/tutorial_general_203.png
