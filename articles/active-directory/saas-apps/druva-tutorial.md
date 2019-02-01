---
title: 'Tutorial: Integración de Azure Active Directory con Druva | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Druva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2017
ms.author: jeedes
ms.openlocfilehash: 0429a7d0845b8590570dfb8ffc513ca3bb0ce998
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195960"
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Tutorial: Integración de Azure Active Directory con Druva

En este tutorial, obtendrá información sobre cómo integrar Druva con Azure Active Directory (Azure AD).

La integración de Druva con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Druva.
- Puede permitir que los usuarios inicien sesión automáticamente en Druva (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Druva, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Druva

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Druva desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-druva-from-the-gallery"></a>Incorporación de Druva desde la galería
Para configurar la integración de Druva en Azure AD, tendrá que agregar Druva desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Druva desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Druva**, seleccione **Druva** en el panel de resultados y, luego, haga clic en el botón **Agregar** para añadir la aplicación.

    ![Druva en la lista de resultados](./media/druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Druva con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD necesita saber cuál es el usuario homólogo de Druva para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Druva.

Para establecer la relación de vínculo, en Druva, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Druva, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Druva](#create-a-druva-test-user)**: para tener un homólogo de Britta Simon en Druva que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en su aplicación Druva.

**Para configurar el inicio de sesión único de Azure AD con Druva, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Druva**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/druva-tutorial/tutorial_druva_samlbase.png)

1. Vaya a la sección **Dominio y direcciones URL de Druva**, si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_druva_url.png)

    En el cuadro de texto **Identificador**, escriba el valor de cadena: `druva-cloud`
    
1. Active **Mostrar configuración avanzada de URL**. Si quiere volver a configurar la aplicación en modo iniciado por **SP**:

    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_druva_url1.png)
    
    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://cloud.druva.com/home`

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/druva-tutorial/tutorial_druva_certificate.png) 

1. La aplicación Druva espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los **atributos del token SAML**. 

    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_druva_attribute.png)

1. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo del token de SAML como muestra la imagen anterior y realice los siguientes pasos:

    | Nombre del atributo      | Valor de atributo      |
    | ------------------- | -------------------- |
    | insync\_auth\_token |Escriba el valor del token generado |
    
     a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.
    
    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_attribute_04.png)
    
    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_attribute_05.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila. El valor del token generado se explica más adelante en el tutorial.
    
    d. Haga clic en **Aceptar**.    

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Druva**, haga clic en **Configurar Druva** para abrir la ventana **Configurar inicio de sesión**. Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/druva-tutorial/tutorial_druva_configure.png) 

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía en Druva.

1. Vaya a **Administrar \> Configuración**.

    ![Configuración](./media/druva-tutorial/ic795091.png "Configuración")

1. En el cuadro de diálogo Single Sign-On Settings (Configuración de inicio de sesión único), siga estos pasos:

    ![Configuración de inicio de sesión único](./media/druva-tutorial/ic795092.png "Configuración de inicio de sesión único")
    
     a. En el cuadro de texto **ID Provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Dirección URL del servicio de inicio de sesión único** que copió de Azure Portal.
        
    b. En el cuadro de texto **ID Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.
        
    c. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de Id.** .
     
    d. Para abrir la página **Configuración**, haga clic en **Guardar**.

1. En la página **Configuración**, haga clic en**Generar token de SSO**.

    ![Configuración](./media/druva-tutorial/ic795093.png "Configuración")

1. En el cuadro de diálogo **Token de autenticación de inicio de sesión único** , siga estos pasos:

    ![Token de SSO](./media/druva-tutorial/ic795094.png "Token de SSO")
    
     a. Haga clic en **Copiar**, pegue el valor copiado en el cuadro de texto **Valor** en la sección **Agregar atributo** de Azure Portal.
    
    b. Haga clic en **Cerrar**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/druva-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/druva-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/druva-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/druva-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-druva-test-user"></a>Crear un usuario de prueba de Druva

Para permitir que los usuarios de Azure AD inicien sesión en Druva, tiene que aprovisionarse en Druva. En el caso de Druva, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **Druva** como administrador.

1. Vaya a **Administrar \> usuarios**.
   
   ![Administración de usuarios](./media/druva-tutorial/ic795097.png "Administración de usuarios")

1. Haga clic en **Create New**.
   
   ![Administración de usuarios](./media/druva-tutorial/ic795098.png "Administración de usuarios")

1. En el cuadro de diálogo Create New User (Crear nuevo usuario), realice los pasos siguientes:
   
   ![Creación de un nuevo usuario](./media/druva-tutorial/ic795099.png "Creación de un nuevo usuario")
   
    a. En el cuadro de texto **Email address** (Dirección de correo electrónico), escriba el correo electrónico del usuario, en este caso, **brittasimon@contoso.com**.
   
   b. En el cuadro de texto **Name** (Nombre), escriba el nombre de usuario, por ejemplo, **BrittaSimon**.
   
   c. Haga clic en **Create User**(Crear usuario).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Druva que proporcione Druva para aprovisionar cuentas de usuario de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Druva.

![Asignación de rol de usuario][200] 

**Para asignar el usuario Britta Simon a Druva, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Druva**.

    ![Vínculo a Druva en la lista de aplicaciones](./media/druva-tutorial/tutorial_druva_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Druva en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Druva.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/druva-tutorial/tutorial_general_01.png
[2]: ./media/druva-tutorial/tutorial_general_02.png
[3]: ./media/druva-tutorial/tutorial_general_03.png
[4]: ./media/druva-tutorial/tutorial_general_04.png

[100]: ./media/druva-tutorial/tutorial_general_100.png

[200]: ./media/druva-tutorial/tutorial_general_200.png
[201]: ./media/druva-tutorial/tutorial_general_201.png
[202]: ./media/druva-tutorial/tutorial_general_202.png
[203]: ./media/druva-tutorial/tutorial_general_203.png

