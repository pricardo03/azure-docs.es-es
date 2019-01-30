---
title: 'Tutorial: Integración de Azure Active Directory con BitaBIZ | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y BitaBIZ.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: ed4bada75bad19522e74b9064601c6882331e66e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815895"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Tutorial: Integración de Azure Active Directory con BitaBIZ

En este tutorial, aprenderá a integrar BitaBIZ con Azure Active Directory (Azure AD).

La integración de BitaBIZ con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a BitaBIZ.
- Puede permitir que los usuarios inicien sesión automáticamente en BitaBIZ (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con BitaBIZ, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en BitaBIZ

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de BitaBIZ desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-bitabiz-from-the-gallery"></a>Adición de BitaBIZ desde la galería
Para configurar la integración de BitaBIZ en Azure AD, deberá agregar BitaBIZ desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar BitaBIZ desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **BitaBIZ**, seleccione **BitaBIZ** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![BitaBIZ en la lista de resultados](./media/bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con BitaBIZ con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de BitaBIZ para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de BitaBIZ.

Para establecer la relación de vínculo, en BitaBIZ, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con BitaBIZ, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de BitaBIZ](#create-a-bitabiz-test-user)**: para tener un homólogo de Britta Simon en BitaBIZ que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación BitaBIZ.

**Para configurar el inicio de sesión único de Azure AD con BitaBIZ, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **BitaBIZ**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

1. En la sección **Dominio y direcciones URL de BitaBIZ**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por IDP:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de BitaBIZ](./media/bitabiz-tutorial/tutorial_bitabiz_url.png)

    En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > El valor de la dirección URL anterior es exclusivamente para la demostración. Actualice el valor con el identificador real, lo que se explica más adelante en el tutorial.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de BitaBIZ](./media/bitabiz-tutorial/tutorial_bitabiz_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://www.bitabiz.com/dashboard`

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/bitabiz-tutorial/tutorial_general_400.png)
    
1. En la sección **Configuración de BitaBIZ**, haga clic en **Configurar BitaBIZ** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configuración de BitaBIZ](./media/bitabiz-tutorial/tutorial_bitabiz_configure.png) 

1. En otra ventana del explorador web, inicie sesión en el inquilino de BitaBIZ como administrador.

1. Haga clic en **SETUP ADMIN**.

    ![Configuración de BitaBIZ](./media/bitabiz-tutorial/settings1.png)

1. Haga clic en **Microsoft integrations** (Integraciones de Microsoft) en la sección **Agregar valor**.

    ![Configuración de BitaBIZ](./media/bitabiz-tutorial/settings2.png)

1. Desplácese hacia abajo hasta la sección **Microsoft Azure AD (Habilitar inicio de sesión único)** y realice los siguientes pasos:

    ![Configuración de BitaBIZ](./media/bitabiz-tutorial/settings3.png)

     a. Copie el valor del cuadro de texto **Id. de entidad ("identificador" en Azure AD)** y péguelo en el cuadro de texto **Identificador** de la sección **Dominio y direcciones URL de BitaBIZ** en Azure Portal. 
    
    b. En el cuadro de texto **Dirección URL del servicio de inicio de sesión único de Azure AD**, pegue la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.
    
    c. En el cuadro de texto **Identificador de entidad de SAML de Azure AD**, pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que copió de Azure Portal.

    d. Abra en el Bloc de notas el archivo **Certificado (Base64)** descargado, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Azure AD Signing Certificate (Base64 encoded)** [Certificado de firma de Azure AD (codificado en Base64)].

    e. Agregue el nombre de dominio del correo electrónico empresarial, es decir, mycompany.com, en el cuadro de texto **Nombre de dominio** para asignar SSO a los usuarios de su empresa con este dominio de correo electrónico (no es obligatorio).
    
    f. Marque la cuenta de BitaBIZ como **SSO enabled** (habilitada para SSO).
    
    g. Haga clic en **Save Azure AD configuration** (Guardar configuración de Azure AD) para guardar y activar la configuración de SSO.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/bitabiz-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/bitabiz-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/bitabiz-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/bitabiz-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-bitabiz-test-user"></a>Creación de un usuario de prueba de BitaBIZ

Para permitir que los usuarios de Azure AD inicien sesión en BitaBIZ, deben aprovisionarse en BitaBIZ.  
En el caso de BitaBIZ, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en su sitio de la compañía de BitaBIZ.

1. Haga clic en **SETUP ADMIN**.

    ![Agregar usuario de BitaBIZ](./media/bitabiz-tutorial/settings1.png)

1. Haga clic en **Agregar usuarios** en la sección **Organización**.

    ![Agregar usuario de BitaBIZ](./media/bitabiz-tutorial/user1.png)

1. Haga clic en **Add new employee** (Agregar nuevo empleado).

    ![Agregar usuario de BitaBIZ](./media/bitabiz-tutorial/user2.png)

1. En la página del cuadro de diálogo **"Add new Employee"** (Agregar nuevo empleado), realice los siguientes pasos:

    ![Agregar usuario de BitaBIZ](./media/bitabiz-tutorial/user3.png)

     a. En el cuadro de texto **Nombre**, escriba el nombre del usuario, en este caso, Britta.

    b. En el cuadro de texto **Apellidos**, escriba los apellidos del usuario, en este caso, Simon.

    c. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    d. Seleccione una fecha en **Date of employment** (Fecha del contrato).

    e. Hay otros atributos de usuario no obligatorios que se pueden configurar para el usuario. Consulte la [documentación de configuración de los empleados](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) para obtener más detalles.    
    
    f. Haga clic en **Save employee** (Guardar empleado).
    
    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.
    
### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a BitaBIZ.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a BitaBIZ, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **BitaBIZ**.

    ![Vínculo a BitaBIZ en la lista de aplicaciones](./media/bitabiz-tutorial/tutorial_bitabiz_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de BitaBIZ en el panel de acceso, debería iniciar sesión automáticamente en su aplicación BitaBIZ.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/bitabiz-tutorial/tutorial_general_203.png

