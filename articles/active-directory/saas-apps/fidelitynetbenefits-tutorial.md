---
title: 'Tutorial: Integración de Azure Active Directory con Fidelity NetBenefits | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Fidelity NetBenefits.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf7de6d0416c60f33f57fd83768fd23b0a09e0f1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184880"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Tutorial: Integración de Azure Active Directory con Fidelity NetBenefits

En este tutorial, obtendrá información sobre cómo integrar Fidelity NetBenefits con Azure Active Directory (Azure AD).

La integración de Fidelity NetBenefits con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a Fidelity NetBenefits.
- Puede permitir que los usuarios inicien sesión automáticamente en Fidelity NetBenefits (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Fidelity NetBenefits se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Fidelity NetBenefits

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Fidelity NetBenefits desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Adición de Fidelity NetBenefits desde la galería

Para configurar la integración de Fidelity NetBenefits en Azure AD, será preciso que agregue Fidelity NetBenefits desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Fidelity NetBenefits desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Fidelity NetBenefits**, seleccione **Fidelity NetBenefits** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Fidelity NetBenefits en la lista de resultados](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Fidelity NetBenefits con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Fidelity NetBenefits para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Fidelity NetBenefits.

En Fidelity NetBenefits, la asignación de **usuario** debe realizarse con el **usuario de Azure AD** para establecer la relación de vínculo.

Para configurar y probar el inicio de sesión único de Azure AD con Fidelity NetBenefits, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Crear un usuario de prueba de Fidelity NetBenefits](#create-a-fidelity-netbenefits-test-user)**: para tener un homólogo de Britta Simon en Fidelity NetBenefits que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Fidelity NetBenefits.

**Para configurar el inicio de sesión único de Azure AD con Fidelity NetBenefits, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Fidelity NetBenefits** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_samlbase.png)

3. En la sección **Dominio y direcciones URL de Fidelity NetBenefits**, lleve a cabo los pasos siguientes:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de Fidelity NetBenefits](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL:

    Para un entorno de pruebas: `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Para un entorno de producción: `urn:sp:fidelity:geninbndnbparts20`

    b. En el cuadro de texto **URL de respuesta**, escriba la dirección URL que va a proporcionar Fidelity en el momento de la implementación, o bien póngase en contacto con un responsable del servicio de atención al cliente de Fidelity.

4. La aplicación Fidelity NetBenefits espera las aserciones de SAML en un formato específico. Hemos asignado a **Identificador de usuario** la propiedad **user.userprincipalname**. Puede asignarle la propiedad **employeeid** o cualquier otra notificación que sea aplicable a su organización como **Identificador de usuario**. La siguiente captura de pantalla le muestra un ejemplo.

    ![Atributo Fidelity NetBenefits](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_attribute.png)

    >[!Note]
    >Fidelity NetBenefits admite la federación estática y dinámica. La opción estática significa que no usará el aprovisionamiento de usuarios Just-In-Time basado en SAML y la opción dinámica significa que admite el aprovisionamiento de usuarios Just-In-Time. Para utilizar el aprovisionamiento basado en JIT, los clientes tienen que agregar algunas notificaciones más en Azure AD, como la fecha de nacimiento del usuario, etc. Estos detalles los proporciona el **responsable del servicio de atención al cliente de Fidelity** asignado, quien debe habilitar esta federación dinámica para su instancia.

5. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_certificate.png)

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/fidelitynetbenefits-tutorial/tutorial_general_400.png)

7. En la sección **Configuración de Fidelity NetBenefits**, haga clic en **Configurar Fidelity NetBenefits** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configuración de Fidelity NetBenefits](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_configure.png)

8. Para configurar el inicio de sesión único en **Fidelity NetBenefits**, es preciso enviar los valores descargados de **XML de metadatos**, **SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML) y **SAML Entity ID** (Id. de entidad SAML) al **responsable del servicio de atención al cliente de Fidelity que tenga asignado**. Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/fidelitynetbenefits-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/fidelitynetbenefits-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/fidelitynetbenefits-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/fidelitynetbenefits-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
  
### <a name="create-a-fidelity-netbenefits-test-user"></a>Crear un usuario de prueba de Fidelity NetBenefits

En esta sección, creará un usuario llamado Britta Simon en Fidelity NetBenefits. Si va a crear la federación estática, trabaje con el **responsable del servicio de atención al cliente de Fidelity** para crear usuarios en la plataforma Fidelity NetBenefits. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

Para la federación dinámica, los usuarios se crean mediante el aprovisionamiento de usuarios Just-In-Time. Para utilizar el aprovisionamiento basado en JIT, los clientes tienen que agregar algunas notificaciones más en Azure AD, como la fecha de nacimiento del usuario, etc. Estos detalles los proporciona el **responsable del servicio de atención al cliente de Fidelity** asignado, quien debe habilitar esta federación dinámica para su instancia.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Fidelity NetBenefits para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200]

**Para asignar Britta Simon a Fidelity NetBenefits, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Fidelity NetBenefits**.

    ![Vínculo de Fidelity NetBenefits en la lista de aplicaciones](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Fidelity NetBenefits en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Fidelity NetBenefits.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fidelitynetbenefits-tutorial/tutorial_general_01.png
[2]: ./media/fidelitynetbenefits-tutorial/tutorial_general_02.png
[3]: ./media/fidelitynetbenefits-tutorial/tutorial_general_03.png
[4]: ./media/fidelitynetbenefits-tutorial/tutorial_general_04.png

[100]: ./media/fidelitynetbenefits-tutorial/tutorial_general_100.png

[200]: ./media/fidelitynetbenefits-tutorial/tutorial_general_200.png
[201]: ./media/fidelitynetbenefits-tutorial/tutorial_general_201.png
[202]: ./media/fidelitynetbenefits-tutorial/tutorial_general_202.png
[203]: ./media/fidelitynetbenefits-tutorial/tutorial_general_203.png
