---
title: 'Tutorial: Integración de Azure Active Directory con Projectplace | Microsoft Docs'
description: Con este tutorial aprenderá a configurar el inicio de sesión único entre Azure Active Directory y Projectplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 17fbc6bc4f022a15c34c5ca7b9465be392cdc639
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560625"
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Tutorial: Integración de Azure Active Directory con Projectplace

En este tutorial aprenderá a integrar Projectplace con Azure Active Directory (Azure AD).

Esta integración ofrece estas ventajas:

* Puede usar Azure AD para controlar quién tiene acceso a Projectplace.
* Puede permitir que los usuarios inicien sesión automáticamente en Projectplace (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información acerca de la integración de aplicaciones SaaS con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Projectplace, necesita:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una [suscripción de prueba gratuita durante un mes](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción de Projectplace que tenga el inicio de sesión único habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial configurará y probará el inicio de sesión único de Azure AD en un entorno de prueba.

* Projectplace admite el inicio de sesión único iniciado por SP.

## <a name="add-projectplace-from-the-gallery"></a>Incorporación de Projectplace desde la galería

Para configurar la integración de Projectplace en Azure AD, es preciso agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

1. En [Azure Portal](https://portal.azure.com), en el panel izquierdo, seleccione **Azure Active Directory**:

    ![Seleccione Azure Active Directory.](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**:

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación, seleccione **Nueva aplicación** en la parte superior de la ventana:

    ![Seleccionar Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Projectplace**. Seleccione **Projectplace** en los resultados de búsqueda y, después, **Agregar**.

     ![Search Results](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección podrá configurar y probar el inicio de sesión único de Azure AD con Projectplace con un usuario de prueba llamado Britta Simon.
Para habilitar el inicio de sesión único, tendrá que establecer una relación entre un usuario de Azure AD y el usuario correspondiente de Projectplace.

Para configurar y probar el inicio de sesión único de Azure AD con Projectplace, es preciso completar los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** para permitir que los usuarios utilicen esta característica.
2. **[Configuración del inicio de sesión único en Projectplace](#configure-projectplace-single-sign-on)** en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** para probar el inicio de sesión único de Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** para permitir el inicio de sesión único de Azure AD para el usuario.
5. **[Creación de un usuario de prueba en Projectplace](#create-a-projectplace-test-user)** vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Projectplace, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Projectplace**, seleccione **Inicio de sesión único**:

    ![Seleccionar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único:

    ![Seleccione un método de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**:

    ![Icono Editar](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, en el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente formato:

    `https://<company>.projectplace.com`

   ![Cuadro de diálogo Configuración básica de SAML](common/sp-signonurl.png)
    > [!NOTE]
    > Este valor es un marcador de posición. Debe utilizar la dirección URL real de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Projectplace](https://success.planview.com/Projectplace/Support) para obtener este valor. También puede consultar los patrones que se muestran en el cuadro de diálogo **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el vínculo **Descargar** situado junto a **XML de metadatos de federación**, según sus requisitos, y guarde el certificado en el equipo:

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Projectplace** (Configurar Projectplace), copie las direcciones URL adecuadas en función de sus necesidades.

    ![Copia de las direcciones URL de configuración](common/copy-configuration-urls.png)

    1. **Dirección URL de inicio de sesión**.

    1. **Identificador de Azure AD**.

    1. **Dirección URL de cierre de sesión**.

### <a name="configure-projectplace-single-sign-on"></a>Configuración del inicio de sesión único en Projectplace

Para configurar el inicio de sesión único en **Projectplace**, es preciso enviar el certificado **XML de metadatos de federación** descargado y las direcciones URL que ha copiado de Azure Portal al [ equipo de soporte técnico de Projectplace](https://success.planview.com/Projectplace/Support). El equipo se asegurará de que la conexión de inicio de sesión único de SAML esté establecida correctamente en ambos lados.

>[!NOTE]
>La configuración del inicio de sesión único la debe realizar el [equipo de soporte técnico de Projectplace](https://success.planview.com/Projectplace/Support). Tan pronto como se complete la configuración, recibirá una notificación.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, seleccione **Azure Active Directory** en el panel izquierdo, **Usuarios** y **Todos los usuarios**:

    ![Selección de Todos los usuarios](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla:

    ![Selección de Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.
  
    1. En el cuadro **Nombre de usuario**, escriba **BrittaSimon@\<dominioDeSuEmpresa>.\<extensión>** . (Por ejemplo, BrittaSimon@contoso.com).

    1. Active la casilla **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección se habilitará a Britta Simon para que use el inicio de sesión único de Azure AD concediéndole acceso a Projectplace.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Projectplace**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Projectplace**.

    ![Lista de aplicaciones](common/all-applications.png)

3. En el panel izquierdo, seleccione **Usuarios y grupos**:

    ![Seleccionar Usuarios y grupos](common/users-groups-blade.png)

4. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Selección de Agregar usuario](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario de la lista. Haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

### <a name="create-a-projectplace-test-user"></a>Creación de un usuario de prueba en Projectplace

Para permitir que los usuarios de Azure AD inicien sesión en Projectplace, debe agregarlos a este. Debe hacerlo manualmente.

Para crear una cuenta de usuario, siga estos pasos:

1. Inicie sesión en el sitio de la compañía **Projectplace** como administrador.

2. Vaya a **People** (Personas) y haga clic en **Members** (Miembros):
   
    ![Vaya a People (Personas) y seleccione Members (Miembros)](./media/projectplace-tutorial/ic790228.png "People")

3. Seleccione **Add Member** (Agregar miembro):
   
    ![Select Add Member (Seleccionar agregar miembro)](./media/projectplace-tutorial/ic790232.png "Add Members")

4. En la sección **Add Member** (Agregar miembro), realice los siguientes pasos.
   
    ![Sección Add Member (Agregar miembro)](./media/projectplace-tutorial/ic790233.png "New Members")
   
    1. En el cuadro **New Members** (Nuevos miembros), escriba la dirección de correo electrónico de una cuenta de Azure AD válida que quiera agregar.
   
    1. Seleccione **Enviar**.

   Se enviará un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active al titular de la cuenta de Azure AD.

>[!NOTE]
>Puede usar también cualquier otra API o herramienta de creación de cuentas de usuario proporcionada por Projectplace para configurar cuentas de usuario de Azure AD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Ahora debe probar la configuración de inicio de sesión único de Azure AD mediante el panel de acceso.

Al seleccionar el icono de Projectplace en el panel de acceso, debería iniciar sesión automáticamente en la instancia de Projectplace para la que configuró el inicio de sesión único. Para más información, consulte [Access and use apps on the My Apps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Uso de aplicaciones y acceso a ellas en el portal Aplicaciones).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
