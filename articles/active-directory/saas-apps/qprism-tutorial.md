---
title: 'Tutorial: Integración de Azure Active Directory con QPrism | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y QPrism.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 72ab75ba-132b-4f83-a34b-d28b81b6d7bc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 286af8b927f488acaf2877e753f6e4689c4b000f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199466"
---
# <a name="tutorial-azure-active-directory-integration-with-qprism"></a>Tutorial: Integración de Azure Active Directory con QPrism

En este tutorial, aprenderá a integrar QPrism con Azure Active Directory (Azure AD).

La integración de QPrism con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a QPrism.
- Puede permitir que los usuarios inicien sesión automáticamente en QPrism (inicio de sesión único) con su cuenta de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para obtener más detalles sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con QPrism, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en QPrism

Para probar los pasos de este tutorial, siga estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de QPrism desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="add-qprism-from-the-gallery"></a>Incorporación de QPrism desde la galería
Para configurar la integración de QPrism en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar QPrism desde la galería:**

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **QPrism**y seleccione **QPrism** desde el panel de resultados. Haga clic en el botón **Agregar** para agregar la aplicación.

    ![QPrism en la lista de resultados](./media/qprism-tutorial/tutorial_qprism_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, puede configurar y probar el inicio de sesión único de Azure AD con QPrism con un usuario de prueba llamado Britta Simon.

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de QPrism para el usuario de Azure AD. Es decir, debe haber una relación de vínculo entre un usuario de Azure AD y el relacionado de QPrism.

Para establecer la relación, en QPrism, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con QPrism, complete los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on), para permitir que los usuarios utilicen esta característica.
1. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
1. [Creación de un usuario de prueba de QPrism](#create-a-qprism-test-user), para tener un homólogo de Britta Simon en QPrism vinculado a la representación del usuario en Azure AD.
1. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. [Prueba del inicio de sesión único](#test-single-sign-on), para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación QPrism.

1. En Azure Portal, en la página de integración de la aplicación **QPrism**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/qprism-tutorial/tutorial_qprism_samlbase.png)

1. En la sección **Dominio y direcciones URL de QPrism**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de QPrism](./media/qprism-tutorial/tutorial_qprism_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<customer domain>.qmyzone.com/login`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<customer domain>.qmyzone.com/metadata.php`.
         
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con el identificador y la dirección URL de inicio de sesión reales. Póngase en contacto con el [equipo de atención al cliente de QPrism](mailto:qsupport-ce@quatrro.com) para obtener estos valores. 

1. En la sección  **Certificado de firma de SAML** , haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

     ![Vínculo de descarga del certificado](./media/qprism-tutorial/tutorial_qprism_certificate.png)

1. Seleccione **Guardar**.

    ![Botón Guardar de Configuración de inicio de sesión único](./media/qprism-tutorial/tutorial_general_400.png)
    
1. Para configurar el inicio de sesión único en **QPrism**, debe enviar la **dirección URL de metadatos de federación de la aplicación** al [equipo de soporte técnico de QPrism](mailto:qsupport-ce@quatrro.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/qprism-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/qprism-tutorial/create_aaduser_02.png)

1. Para abrir el cuadro de diálogo **Usuario**, en la parte superior del cuadro de diálogo **Todos los usuarios**, seleccione **Agregar**.

    ![Botón Agregar](./media/qprism-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario**, haga lo siguiente:

    ![Cuadro de diálogo Usuario](./media/qprism-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Seleccione **Crear**.
 
### <a name="create-a-qprism-test-user"></a>Creación de un usuario de prueba de QPrism

En esta sección, creará un usuario llamado Britta Simon en QPrism. Trabaje con el [equipo de atención al cliente de QPrism](mailto:qsupport-ce@quatrro.com) para agregar usuarios a la plataforma de QPrism. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a QPrism.

![Asignación de rol de usuario][200] 

**Para asignar a Simon Britta a QPrism:**

1. En Azure Portal, abra la vista de aplicaciones y, a continuación, vaya a la vista de directorio. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **QPrism**.

    ![Vínculo a QPrism en la lista de aplicaciones](./media/qprism-tutorial/tutorial_qprism_app.png)  

1. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Seleccione **Agregar**. Después, en **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista **Usuarios**.

1. En el cuadro de diálogo **Usuarios y grupos**, elija **Seleccionar**.

1. En **Agregar asignación**, seleccione **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

En el panel de acceso, al seleccionar el icono de QPrism, debería iniciar sesión automáticamente en la aplicación QPrism.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/qprism-tutorial/tutorial_general_01.png
[2]: ./media/qprism-tutorial/tutorial_general_02.png
[3]: ./media/qprism-tutorial/tutorial_general_03.png
[4]: ./media/qprism-tutorial/tutorial_general_04.png

[100]: ./media/qprism-tutorial/tutorial_general_100.png

[200]: ./media/qprism-tutorial/tutorial_general_200.png
[201]: ./media/qprism-tutorial/tutorial_general_201.png
[202]: ./media/qprism-tutorial/tutorial_general_202.png
[203]: ./media/qprism-tutorial/tutorial_general_203.png

