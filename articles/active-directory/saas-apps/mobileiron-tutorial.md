---
title: 'Tutorial: Integración de Azure Active Directory con MobileIron | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y MobileIron.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3e4bbd5b-290e-4951-971b-ec0c1c11aaa2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: jeedes
ms.openlocfilehash: 8bdf49f4cea7c6f0ff30e37bcf1cf2fed3abc2bb
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963817"
---
# <a name="tutorial-azure-active-directory-integration-with-mobileiron"></a>Tutorial: integración de Azure Active Directory con MobileIron

En este tutorial, aprenderá a integrar MobileIron con Azure Active Directory (Azure AD).

La integración de MobileIron con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a MobileIron.
- Puede permitir que los usuarios inicien sesión automáticamente en MobileIron (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con MobileIron, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en MobileIron

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de MobileIron desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-mobileiron-from-the-gallery"></a>Adición de MobileIron desde la galería

Para configurar la integración de MobileIron en Azure AD, es preciso agregar MobileIron desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar MobileIron desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **MobileIron**, seleccione **MobileIron** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![MobileIron en la lista de resultados](./media/mobileiron-tutorial/tutorial_mobileiron_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con MobileIron con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de MobileIron para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de MobileIron.

Para establecer la relación de vínculo, en MobileIron, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con MobileIron, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de MobileIron](#create-a-mobileiron-test-user)**: para tener un homólogo de Britta Simon en MobileIron que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación MobileIron.

**Para configurar el inicio de sesión único de Azure AD con MobileIron, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **MobileIron**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/mobileiron-tutorial/tutorial_mobileiron_samlbase.png)

1. En la sección **Dominio y direcciones URL de MobileIron**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por  **IDP** :

    ![Información sobre dominio y direcciones URL de inicio de sesión único de MobileIron](./media/mobileiron-tutorial/tutorial_mobileiron_url.png)

    1. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://www.mobileiron.com/<key>`

    1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<host>.mobileiron.com/saml/SSO/alias/<key>`.

1. Active  **Mostrar configuración avanzada de URL**  y siga estos pasos si desea configurar la aplicación en el modo iniciado por  **SP** :

    ![Información sobre dominio y direcciones URL de inicio de sesión único de MobileIron](./media/mobileiron-tutorial/tutorial_mobileiron_url1.png)

    En el cuadro de texto  **URL de inicio de sesión** , escriba la dirección URL con el siguiente patrón: `https://<host>.mobileiron.com/user/login.html`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Obtendrá los valores de clave y host desde el portal de administración de MobileIron, como se explica más adelante en el tutorial.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/mobileiron-tutorial/tutorial_mobileiron_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/mobileiron-tutorial/tutorial_general_400.png)

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la empresa de MobileIron.

1. Vaya a **Admin** (Administración) > **Identity** (Identidad).

   - Seleccione la opción **AAD** en el campo **Info on Cloud IDP Setup** (Información de configuración del IDP en la nube).

    ![Botón Configurar inicio de sesión único](./media/mobileiron-tutorial/tutorial_mobileiron_admin.png)

1. Copie los valores de **Key** (Clave) y **Host** y péguelos para completar las direcciones URL en la sección **Dominio y direcciones URL de MobileIron** en Azure Portal.

    ![Botón Configurar inicio de sesión único](./media/mobileiron-tutorial/key.png)

1. En el campo**Export metadata file from AAD and import to MobileIron Cloud** (Exportar archivo de metadatos de AAD e importar en MobileIron Cloud) haga clic en **Choose File** (Elegir archivo) para cargar los metadatos descargados desde Azure Portal. Haga clic en **Done** (Listo) una vez cargado.

    ![Botón de metadatos de administración de inicio de sesión único](./media/mobileiron-tutorial/tutorial_mobileiron_adminmetadata.png)

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/mobileiron-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/mobileiron-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/mobileiron-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/mobileiron-tutorial/create_aaduser_04.png)

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.

    1. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    1. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    1. Haga clic en **Create**(Crear).
  
### <a name="create-a-mobileiron-test-user"></a>Creación de un usuario de prueba de MobileIron

Para permitir que los usuarios de Azure AD inicien sesión en MobileIron, es necesario que se aprovisionen en MobileIron.  
En el caso de MobileIron, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de MobileIron como administrador.

1. Vaya a **Users** (Usuarios) y haga clic en **Add** (Agregar)  > **Single User** (Usuario único).

    ![Botón Configurar usuario de inicio de sesión único](./media/mobileiron-tutorial/tutorial_mobileiron_user.png)

1. En la página de diálogo **“Single User”** (Usuario único), realice los siguientes pasos:

    ![Botón Configurar agregar usuario de inicio de sesión único](./media/mobileiron-tutorial/tutorial_mobileiron_useradd.png)

    1. En el cuadro de texto **E-mail Address** (Dirección de correo electrónico), escriba el correo electrónico del usuario, en este caso, brittasimon@contoso.com.

    1. En el cuadro de texto **First Name** (Nombre), escriba el nombre de usuario, en este caso Britta.

    1. En el cuadro de texto **Last Name** (Apellidos), escriba el nombre de usuario, en este caso Simon.

    1. Haga clic en **Done**(Listo).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a MobileIron.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a MobileIron, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

1. En la lista de aplicaciones, seleccione **MobileIron**.

    ![Vínculo a MobileIron en la lista de aplicaciones](./media/mobileiron-tutorial/tutorial_mobileiron_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de MobileIron en el panel de acceso, debería iniciar sesión automáticamente en la aplicación MobileIron.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mobileiron-tutorial/tutorial_general_01.png
[2]: ./media/mobileiron-tutorial/tutorial_general_02.png
[3]: ./media/mobileiron-tutorial/tutorial_general_03.png
[4]: ./media/mobileiron-tutorial/tutorial_general_04.png

[100]: ./media/mobileiron-tutorial/tutorial_general_100.png

[200]: ./media/mobileiron-tutorial/tutorial_general_200.png
[201]: ./media/mobileiron-tutorial/tutorial_general_201.png
[202]: ./media/mobileiron-tutorial/tutorial_general_202.png
[203]: ./media/mobileiron-tutorial/tutorial_general_203.png
