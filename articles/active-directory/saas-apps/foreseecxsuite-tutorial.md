---
title: 'Tutorial: Integración de Azure Active Directory con ForeSee CX Suite | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ForeSee CX Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5f4b7830-6186-4d17-b77b-504d4192bfde
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 104caa001f773c48a708879f3f9b50b74190fc74
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57449230"
---
# <a name="tutorial-azure-active-directory-integration-with-foresee-cx-suite"></a>Tutorial: Integración de Azure Active Directory con ForeSee CX Suite

En este tutorial, aprenderá a integrar ForeSee CX Suite con Azure Active Directory (Azure AD).

La integración de ForeSee CX Suite con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a ForeSee CX Suite.
- Puede permitir que los usuarios inicien sesión automáticamente en ForeSee CX Suite (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ForeSee CX Suite, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada de ForeSee CX Suite para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de ForeSee CX Suite desde la Galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-foresee-cx-suite-from-the-gallery"></a>Incorporación de ForeSee CX Suite desde la Galería
Para configurar la integración de ForeSee CX Suite en Azure AD, deberá agregarla desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ForeSee CX Suite desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **ForeSee CX Suite**, seleccione **ForeSee CX Suite** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![ForeSee CX Suite en la lista de resultados](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con ForeSee CX Suite con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el homólogo de ForeSee CX Suite del usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre el usuario de Azure AD y el usuario asociado de ForeSee CX Suite.

Para configurar y probar el inicio de sesión único de Azure AD con ForeSee CX Suite, es preciso completar los siguientes bloques de compilación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de ForeSee CX Suite](#create-a-foresee-cx-suite-test-user)**: para tener un homólogo de Britta Simon en ForeSee CX Suite vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación ForeSee CX Suite.

**Para configurar el inicio de sesión único de Azure AD con ForeSee CX Suite, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **ForeSee CX Suite**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_samlbase.png)

1. En la sección **ForeSee CX Suite Domain and URLs** (Dominio y direcciones URL de ForeSee CX Suite), si tiene el **archivo de metadatos del proveedor de servicios**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de ForeSee CX Suite](./media/foreseecxsuite-tutorial/upload.png)

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Información de dominio y direcciones URL de inicio de sesión único de ForeSee CX Suite](./media/foreseecxsuite-tutorial/tutorial_foreseen_uploadconfig.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    c. Cuando el **archivo de metadatos del proveedor de servicios** se haya cargado correctamente, el valor del **identificador** se rellenará automáticamente en el cuadro de texto de la sección **Dominio y direcciones URL de ForeSee CX Suite** tal y como se muestra a continuación:

    ![Información de dominio y direcciones URL de inicio de sesión único de ForeSee CX Suite](./media/foreseecxsuite-tutorial/urlupload.png)

1. Si no tienes **archivo de metadatos del proveedor de servicios**, realice los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de ForeSee CX Suite](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://cxsuite.foresee.com/`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://www.okta.com/saml2/service-provider/<UniqueID>`

    > [!NOTE]
    > El valor del identificador no es real. Actualícelo con el identificador real. Póngase en contacto con el [equipo de soporte técnico del cliente ForeSee CX](mailto:support@foresee.com) para obtener este valor.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/foreseecxsuite-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en **ForeSee CX Suite**, necesita enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de ForeSee CX Suite](mailto:support@foresee.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/foreseecxsuite-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/foreseecxsuite-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/foreseecxsuite-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/foreseecxsuite-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-foresee-cx-suite-test-user"></a>Crear un usuario de prueba de ForeSee CX Suite

En esta sección, creará un usuario llamado Britta Simon en ForeSee CX Suite. Colabore con el [equipo de soporte técnico de ForeSee CX Suite](mailto:support@foresee.com) para agregar los usuarios o el dominio que deben incluirse en la lista blanca en la plataforma de ForeSee CX Suite. Si el dominio lo agrega el equipo, los usuarios se aprovisionarán automáticamente en la plataforma de ForeSee CX Suite. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, podrá permitir que Britta Simon utilice el inicio de sesión único de Azure concediéndole acceso a ForeSee CX Suite.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a ForeSee CX Suite, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

1. En la lista de aplicaciones, seleccione **ForeSee CX Suite**.

    ![Vínculo a ForeSee CX Suite en la lista de aplicaciones](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_app.png)

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ForeSee CX Suite en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación ForeSee CX Suite.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/foreseecxsuite-tutorial/tutorial_general_01.png
[2]: ./media/foreseecxsuite-tutorial/tutorial_general_02.png
[3]: ./media/foreseecxsuite-tutorial/tutorial_general_03.png
[4]: ./media/foreseecxsuite-tutorial/tutorial_general_04.png

[100]: ./media/foreseecxsuite-tutorial/tutorial_general_100.png

[200]: ./media/foreseecxsuite-tutorial/tutorial_general_200.png
[201]: ./media/foreseecxsuite-tutorial/tutorial_general_201.png
[202]: ./media/foreseecxsuite-tutorial/tutorial_general_202.png
[203]: ./media/foreseecxsuite-tutorial/tutorial_general_203.png

