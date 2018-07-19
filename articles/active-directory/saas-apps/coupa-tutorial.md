---
title: 'Tutorial: Integración de Azure Active Directory con Coupa | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Coupa.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: jeedes
ms.openlocfilehash: adad60611f1447b78173368ed137205f077cb8b7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047817"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Tutorial: Integración de Azure Active Directory con Coupa

En este tutorial, aprenderá a integrar Coupa con Azure Active Directory (Azure AD).

La integración de Coupa con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Coupa.
- Puede permitir que los usuarios inicien sesión automáticamente en Coupa (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Coupa, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Coupa

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Coupa desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-coupa-from-the-gallery"></a>Adición de Coupa desde la galería
Para configurar la integración de Coupa en Azure AD, deberá agregar Coupa desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Coupa desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Coupa**, seleccione **Coupa** en el panel de resultados y, luego, haga clic en el botón **Agregar** para añadir la aplicación.

    ![Coupa en la lista de resultados](./media/coupa-tutorial/tutorial_coupa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Coupa con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Coupa para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Coupa.

Para establecer la relación de vínculo, en Coupa, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Coupa, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Coupa](#create-a-coupa-test-user)**: para tener un homólogo de Britta Simon en Coupa que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Coupa.

**Para configurar el inicio de sesión único de Azure AD con Coupa, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Coupa**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/coupa-tutorial/tutorial_coupa_samlbase.png)

3. En la sección **Dominio y direcciones URL de Coupa**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Coupa](./media/coupa-tutorial/tutorial_coupa_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.coupahost.com`.

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Coupa](https://success.coupa.com/Support/Contact_Us?) para obtener este valor.

    b. En el cuadro de texto **Identificador**, escriba la dirección URL:

    | Environment  | URL |
    |:-------------|----|
    | Espacio aislado | `devsso35.coupahost.com`|
    | Producción | `prdsso40.coupahost.com`|
    | | |

    c. En el cuadro de texto **URL de respuesta**, escriba la siguiente dirección URL:

    | Environment | URL |
    |------------- |----|
    | Espacio aislado | `https://devsso35.coupahost.com/sp/ACS.saml2`|
    | Producción | `https://prdsso40.coupahost.com/sp/ACS.saml2`|
    | | |

4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/coupa-tutorial/tutorial_coupa_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/coupa-tutorial/tutorial_general_400.png)

6. Inicie sesión en su sitio de la compañía de Coupa como administrador.

7. Vaya a **Configuración \> Control de seguridad**.

   ![Controles de seguridad](./media/coupa-tutorial/ic791900.png "Controles de seguridad")

8. En la sección **Iniciar sesión con credenciales de Coupa** , realice los pasos siguientes:

    ![Metadatos SP de Coupa](./media/coupa-tutorial/ic791901.png "Metadatos SP de Coupa")

    a. Seleccione **Iniciar sesión con SAML**.

    b. Haga clic en **Browse** (Examinar) para cargar los metadatos de Azure Portal.

    c. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/coupa-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/coupa-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/coupa-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/coupa-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-coupa-test-user"></a>Creación de un usuario de prueba de Coupa

Para permitir que los usuarios de Azure AD inicien sesión en Coupa, tienen que aprovisionarse en Coupa.  

* En el caso de Coupa, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión como administrador en el sitio de la compañía de **Coupa** .

2. En el menú en la parte superior, haga clic en **Configurar** y, después, en **Usuarios**.

   ![Usuarios](./media/coupa-tutorial/ic791908.png "Usuarios")

3. Haga clic en **Create**(Crear).

   ![Creación de usuarios](./media/coupa-tutorial/ic791909.png "Creación de usuarios")

4. En la sección **Creación de usuario** , lleve a cabo estos pasos:

   ![Detalles del usuario](./media/coupa-tutorial/ic791910.png "Detalles del usuario")

   a. En los cuadros de texto relacionados, escriba los atributos **Nombre de usuario**, **Nombre**, **Apellidos**, **Id. de inicio de sesión único**, **Correo electrónico** de una cuenta válida de Azure Active Directory que quiera aprovisionar.

   b. Haga clic en **Create**(Crear).

   >[!NOTE]
   >El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active.
   >

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Canvas ofrecida por Coupa para aprovisionar cuentas de usuario de AAD.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Coupa.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Coupa, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Coupa**.

    ![Vínculo a Coupa en la lista de aplicaciones](./media/coupa-tutorial/tutorial_coupa_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Coupa en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación de Coupa.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/coupa-tutorial/tutorial_general_01.png
[2]: ./media/coupa-tutorial/tutorial_general_02.png
[3]: ./media/coupa-tutorial/tutorial_general_03.png
[4]: ./media/coupa-tutorial/tutorial_general_04.png

[100]: ./media/coupa-tutorial/tutorial_general_100.png

[200]: ./media/coupa-tutorial/tutorial_general_200.png
[201]: ./media/coupa-tutorial/tutorial_general_201.png
[202]: ./media/coupa-tutorial/tutorial_general_202.png
[203]: ./media/coupa-tutorial/tutorial_general_203.png
