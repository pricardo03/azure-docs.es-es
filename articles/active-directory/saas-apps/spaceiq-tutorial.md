---
title: 'Tutorial: Integración de Azure Active Directory con SpaceIQ | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SpaceIQ.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 5b55ae29-491f-401f-9299-d3a6b64a1b99
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6695fe4854c6d91d3d2ba671104d1b481721356c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162278"
---
# <a name="tutorial-azure-active-directory-integration-with-spaceiq"></a>Tutorial: Integración de Azure Active Directory con SpaceIQ

En este tutorial, aprenderá a integrar SpaceIQ con Azure Active Directory (Azure AD).

La integración de SpaceIQ con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a SpaceIQ.
- Puede permitir que los usuarios inicien sesión automáticamente en SpaceIQ (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SpaceIQ, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en SpaceIQ

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de SpaceIQ desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-spaceiq-from-the-gallery"></a>Incorporación de SpaceIQ desde la galería
Para configurar la integración de SpaceIQ en Azure AD, tendrá que agregar SpaceIQ desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SpaceIQ desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **SpaceIQ**, seleccione **SpaceIQ** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![SpaceIQ en la lista de resultados](./media/spaceiq-tutorial/tutorial_spaceiq_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SpaceIQ con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de SpaceIQ para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SpaceIQ.

Para establecer la relación de vínculo, en SpaceIQ asigne el valor de **nombre de usuario** de Azure AD como valor del **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con SpaceIQ, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de SpaceIQ](#create-a-spaceiq-test-user)**: para tener un homólogo de Britta Simon en SpaceIQ que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en su aplicación SpaceIQ.

**Para configurar el inicio de sesión único de Azure AD con SpaceIQ, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **SpaceIQ**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/spaceiq-tutorial/tutorial_spaceiq_samlbase.png)

1. En la sección **Dominio y direcciones URL de SpaceIQ**, lleve a cabo los pasos siguientes:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de SpaceIQ](./media/spaceiq-tutorial/tutorial_spaceiq_url.png)

     a. En el cuadro de texto **Identificador**, escriba la dirección URL: `https://api.spaceiq.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://api.spaceiq.com/saml/<instanceid>/callback`.

    > [!NOTE] 
    > Actualice estos valores con la dirección URL de respuesta y el identificador reales, que se explican más adelante en el tutorial.
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/spaceiq-tutorial/tutorial_spaceiq_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/spaceiq-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de SpaceIQ**, haga clic en **Configurar SpaceIQ** para abrir la ventana **Configurar inicio de sesión**. Copie el valor de **Identificador de entidad de SAML** de la **sección Referencia rápida**

    ![Configuración de SpaceIQ](./media/spaceiq-tutorial/tutorial_spaceiq_configure.png) 

1.  Abra una nueva ventana del explorador e inicie sesión en el entorno de SpaceIQ como administrador.

1. Una vez que haya iniciado sesión, haga clic en la pieza de puzzle en la esquina superior derecha, y luego haga clic en **"Integraciones"**

    ![Configuración de cuenta](./media/spaceiq-tutorial/setting1.png) 

1. En **All PROVISIONING & SSO** (Todos los aprovisionamientos y SSO), haga clic en el icono de **Azure** para agregar una instancia de Azure como IDP.

    ![Icono de SAML](./media/spaceiq-tutorial/setting2.png)

1. En el cuadro de diálogo **SSO**, realice los pasos siguientes:

    ![Configuración de la autenticación SAML](./media/spaceiq-tutorial/setting3.png)

     a. En el cuadro **SAML Issuer URL** (Dirección URL del emisor de SAML), pegue el valor de **identificador de entidad de SAML** que copió de la ventana de configuración de la aplicación Azure AD.
    
    b. Copie el valor de **dirección URL de punto de conexión de devolución de llamada SAML (solo lectura)** y péguelo en el cuadro **URL de respuesta** en Azure Portal, en la sección **Dominio y direcciones URL** de SpaceIQ.
    
    c. Copie el valor de **URI de audiencia de SAML (solo lectura)** y péguelo en el cuadro **Identificador** en Azure Portal, en la sección **Dominio y direcciones URL** de SpaceIQ.

    d. Abra el certificado descargado en el Bloc de notas, copie el contenido y péguelo en el cuadro **Certificado X.509**.
    
    e. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/spaceiq-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/spaceiq-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/spaceiq-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/spaceiq-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
  
### <a name="create-a-spaceiq-test-user"></a>Creación de un usuario de prueba SpaceIQ

En esta sección, creará un usuario llamado Britta Simon en SpaceIQ. Trabaje con el [equipo de soporte técnico de SpaceIQ](mailto:eng@spaceiq.com) para agregar usuarios en la plataforma de SpaceIQ. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a SpaceIQ.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a SpaceIQ, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **SpaceIQ**.

    ![Vínculo a SpaceIQ en la lista de aplicaciones](./media/spaceiq-tutorial/tutorial_spaceiq_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SpaceIQ del panel de acceso, debería iniciar sesión automáticamente en su aplicación SpaceIQ.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/spaceiq-tutorial/tutorial_general_01.png
[2]: ./media/spaceiq-tutorial/tutorial_general_02.png
[3]: ./media/spaceiq-tutorial/tutorial_general_03.png
[4]: ./media/spaceiq-tutorial/tutorial_general_04.png

[100]: ./media/spaceiq-tutorial/tutorial_general_100.png

[200]: ./media/spaceiq-tutorial/tutorial_general_200.png
[201]: ./media/spaceiq-tutorial/tutorial_general_201.png
[202]: ./media/spaceiq-tutorial/tutorial_general_202.png
[203]: ./media/spaceiq-tutorial/tutorial_general_203.png

