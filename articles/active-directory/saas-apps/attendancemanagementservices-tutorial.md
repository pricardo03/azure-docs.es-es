---
title: 'Tutorial: Integración de Azure Active Directory con Attendance Management Services | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Attendance Management Services.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jeedes
ms.openlocfilehash: 331a8d07004a8915d27b28ca0fba940b1aa3ab72
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164816"
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Tutorial: Integración de Azure Active Directory con Attendance Management Services

En este tutorial, obtendrá información sobre cómo integrar Attendance Management Services con Azure Active Directory (Azure AD).

La integración de Attendance Management Services con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Attendance Management Services.
- Puede permitir que los usuarios inicien sesión automáticamente en Attendance Management Services (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Attendance Management Services, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción a Attendance Management Services con el inicio de sesión único habilitado

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Attendance Management Services desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-attendance-management-services-from-the-gallery"></a>Adición de Attendance Management Services desde la galería
Para configurar la integración de Attendance Management Services en Azure AD, es preciso agregar Attendance Management Services desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Attendance Management Services desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Attendance Management Services**, seleccione **Attendance Management Services** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Attendance Management Services en la lista de resultados](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Attendance Management Services con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Attendance Management Services para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Attendance Management Services.

Para configurar y probar el inicio de sesión único de Azure AD con Attendance Management Services, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Attendance Management Services](#create-an-attendance-management-service-test-user)**: para tener un homólogo de Britta Simon en Attendance Management Services que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Attendance Management Services.

**Para configurar el inicio de sesión único de Azure AD con Attendance Management Services, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Attendance Management Services**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_samlbase.png)

1. En la sección **Dominio y direcciones URL de Attendance Management Services**, siga estos pasos:

    ![Información de inicio de sesión único del dominio y las direcciones URL de Attendance Management Services](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://id.obc.jp/<tenant information >/`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Attendance Management Services](https://www.obcnet.jp/) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/attendancemanagementservices-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Attendance Management Services**, haga clic en **Configurar Attendance Management Services** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configuración de Attendance Management Services](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_configure.png) 

1. En otra ventana del explorador, inicie sesión en su sitio de la empresa de Attendance Management Services como administrador.

1. Haga clic en **SAML authentication** (Autenticación SAML) en la sección **Administración de seguridad**.

    ![Configuración de Attendance Management Services](./media/attendancemanagementservices-tutorial/user1.png)

1. Lleve a cabo los siguiente pasos:

    ![Configuración de Attendance Management Services](./media/attendancemanagementservices-tutorial/user2.png)

     a. Seleccione **Use SAML authentication** (Usar autenticación SAML).

    b. En el cuadro de texto **Identifier** (Identificador), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal. 

    c. En el cuadro de texto **Authentication endpoint URL** (Dirección URL del punto de conexión de autenticación), pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que ha copiado de Azure Portal.

    d. Haga clic en **Seleccionar un archivo** para cargar el certificado que ha descargado de Azure AD.

    e. Seleccione **Disable password authentication** (Deshabilitar autenticación de contraseña).

    f. Haga clic en **Registro**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación. Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/attendancemanagementservices-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/attendancemanagementservices-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/attendancemanagementservices-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/attendancemanagementservices-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-an-attendance-management-services-test-user"></a>Creación de un usuario de prueba de Attendance Management Services

Para permitir que los usuarios de Azure AD inicien sesión en Attendance Management Services, deben aprovisionarse en Attendance Management Services. En el caso de Attendance Management Services, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de su empresa de Attendance Management Services como administrador.

1. Haga clic en **Administración de usuarios** en la sección **Administración de seguridad**.

    ![Agregar empleado](./media/attendancemanagementservices-tutorial/user5.png)

1. Haga clic en **New rules login** (Nuevas reglas de inicio de sesión).

    ![Agregar empleado](./media/attendancemanagementservices-tutorial/user3.png)

1. En la sección **OBCiD information** (Información de OBCiD), lleve a cabo estos pasos:

    ![Agregar empleado](./media/attendancemanagementservices-tutorial/user4.png)

     a. En el cuadro de texto **OBCiD**, escriba el correo electrónico del usuario, por ejemplo, **BrittaSimon@contoso.com**.

    b. En el cuadro de texto **Contraseña**, escriba la contraseña del usuario.

    c. Haga clic en **Registro**.


### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure para concederle acceso a Attendance Management Services.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Attendance Management Services, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Attendance Management Services**.

    ![Vínculo de Attendance Management Services en la lista de aplicaciones](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Attendance Management Services en el panel de acceso, automáticamente iniciará sesión en su aplicación Attendance Management Services.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/attendancemanagementservices-tutorial/tutorial_general_01.png
[2]: ./media/attendancemanagementservices-tutorial/tutorial_general_02.png
[3]: ./media/attendancemanagementservices-tutorial/tutorial_general_03.png
[4]: ./media/attendancemanagementservices-tutorial/tutorial_general_04.png

[100]: ./media/attendancemanagementservices-tutorial/tutorial_general_100.png

[200]: ./media/attendancemanagementservices-tutorial/tutorial_general_200.png
[201]: ./media/attendancemanagementservices-tutorial/tutorial_general_201.png
[202]: ./media/attendancemanagementservices-tutorial/tutorial_general_202.png
[203]: ./media/attendancemanagementservices-tutorial/tutorial_general_203.png

