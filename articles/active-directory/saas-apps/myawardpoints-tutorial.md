---
title: 'Tutorial: integración de Azure Active Directory con My Award Points Top Sub/Top Team | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y My Award Points Top Sub/Top Team.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a7a08eed-7a6b-4a83-8f8e-0add6d2fb8cf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeedes
ms.openlocfilehash: 9e3e3ff05836cb49d46a25ef3fa55d40a0bd134f
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184674"
---
# <a name="tutorial-azure-active-directory-integration-with-my-award-points-top-subtop-team"></a>Tutorial: integración de Azure Active Directory con My Award Points Top Sub/Top Team

En este tutorial, aprenderá a integrar My Award Points Top Sub/Top Team con Azure Active Directory (Azure AD).

La integración de My Award Points Top Sub/Top Team con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a My Award Points Top Sub/Top Team.
- Puede permitir que los usuarios inicien sesión automáticamente en My Award Points Top Sub/Top Team (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con My Award Points Top Sub/Top Team, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único de My Award Points Top Sub/Top Team

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de My Award Points Top Sub/Top Team desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-my-award-points-top-subtop-team-from-the-gallery"></a>Incorporación de My Award Points Top Sub/Top Team desde la galería

Para configurar la integración de My Award Points Top Sub/Top Team en Azure AD, deberá agregar My Award Points Top Sub/Top Team desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar My Award Points Top Sub/Top Team desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **My Award Points Top Sub/Top Team**, seleccione **My Award Points Top Sub/Top Team** del panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![My Award Points Top Sub/Top Team en la lista de resultados](./media/myawardpoints-tutorial/tutorial_myawardpoints_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con My Award Points Top Sub/Top Team con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD necesita saber cuál es el usuario homólogo de My Award Points Top Sub/Top Team para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de My Award Points Top Sub/Top Team.

Para configurar y probar el inicio de sesión único de Azure AD con My Award Points Top Sub/Top Team, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de My Award Points Top Sub/Top Team](#create-a-my-award-points-top-subtop-team-test-user)**: para tener un homólogo de Britta Simon en My Award Points Top Sub/Top Team que esté vinculado a la representación del usuario de Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación My Award Points Top Sub/Top Team.

**Para configurar el inicio de sesión único de Azure AD con My Award Points Top Sub/Top Team, realice los siguientes pasos:**

1. En Azure Portal, en la pagina de integración de aplicaciones **My Award Points Top Sub/Top Team**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/myawardpoints-tutorial/tutorial_myawardpoints_samlbase.png)

3. En la sección **Dominio y direcciones URL de My Award Points Top Sub/Top Team**, realice los siguientes pasos:

    ![Información de inicio de sesión único de Dominio y direcciones URL de My Award Points Top Sub/Top Team](./media/myawardpoints-tutorial/tutorial_myawardpoints_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://microsoftrr.performnet.com/biwv1auth/Shibboleth.sso/Login?providerId=<SAMLENTITYID>`.

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de My Award Points Top Sub/Top Team](mailto:myawardpoints@biworldwide.com) para obtener este valor.

4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/myawardpoints-tutorial/tutorial_myawardpoints_certificate.png)

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/myawardpoints-tutorial/tutorial_general_400.png)

6. Para configurar el inicio de sesión único en **My Award Points Top Sub/Top Team**, es preciso enviar el **XML de metadatos** descargado al [equipo de soporte técnico de My Award Points Top Sub/Top Team](mailto:myawardpoints@biworldwide.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/myawardpoints-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/myawardpoints-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/myawardpoints-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/myawardpoints-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-my-award-points-top-subtop-team-test-user"></a>Creación de un usuario de prueba de My Award Points Top Sub/Top Team

En esta sección, creará el usuario Britta Simon en My Award Points Top Sub/Top Team. Trabaje con el [equipo de soporte técnico de My Award Points Top Sub/Top Team](mailto:myawardpoints@biworldwide.com) para agregar los usuarios en la plataforma de My Award Points Top Sub/Top Team. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a My Award Points Top Sub/Top Team para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a My Award Points Top Sub/Top Team, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **My Award Points Top Sub/Top Team**.

    ![El vínculo de My Award Points Top Sub/Top Team en la lista de aplicaciones](./media/myawardpoints-tutorial/tutorial_myawardpoints_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de My Award Points Top Sub/Top Team en el panel de acceso, debería iniciar sesión automáticamente en esta aplicación.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/myawardpoints-tutorial/tutorial_general_01.png
[2]: ./media/myawardpoints-tutorial/tutorial_general_02.png
[3]: ./media/myawardpoints-tutorial/tutorial_general_03.png
[4]: ./media/myawardpoints-tutorial/tutorial_general_04.png

[100]: ./media/myawardpoints-tutorial/tutorial_general_100.png

[200]: ./media/myawardpoints-tutorial/tutorial_general_200.png
[201]: ./media/myawardpoints-tutorial/tutorial_general_201.png
[202]: ./media/myawardpoints-tutorial/tutorial_general_202.png
[203]: ./media/myawardpoints-tutorial/tutorial_general_203.png