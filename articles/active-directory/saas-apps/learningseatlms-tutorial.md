---
title: 'Tutorial: Integración de Azure Active Directory con Learning Seat LMS | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Learning Seat LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bb056fcf-4135-478e-85b1-5015d1f07b85
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: jeedes
ms.openlocfilehash: edc3bedaa005bb52ddbf46d9f70ddc16bf8790e2
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55815084"
---
# <a name="tutorial-azure-active-directory-integration-with-learning-seat-lms"></a>Tutorial: Integración de Azure Active Directory con Learning Seat LMS

En este tutorial, obtendrá información sobre cómo integrar Learning Seat LMS con Azure Active Directory (Azure AD).

La integración de Learning Seat LMS con Azure AD ofrece las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Learning Seat LMS
- Puede permitir que los usuarios inicien sesión automáticamente en Learning Seat LMS (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si quiere conocer más detalles sobre la integración de aplicaciones SaaS con Azure AD, vea: [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Learning Seat LMS, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Learning Seat LMS

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Learning Seat LMS desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-learning-seat-lms-from-the-gallery"></a>Incorporación de Learning Seat LMS desde la galería
Para configurar la integración de Learning Seat LMS en Azure AD, deberá agregar Learning Seat LMS desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Learning Seat LMS desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Learning Seat LMS**.

    ![Creación de un usuario de prueba de Azure AD](./media/learningseatlms-tutorial/tutorial_learnconnect_search.png)

1. En el panel de resultados, seleccione **Learning Seat LMS** y luego haga clic en el botón **Agregar** para agregar la aplicación.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Learning Seat LMS con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Learning Seat LMS para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Learning Seat LMS.

Esta relación de vínculo se establece mediante la asignación del valor del **nombre de usuario** en Azure AD como el valor de **Nombre de usuario** en Learning Seat LMS.

Para configurar y probar el inicio de sesión único de Azure AD con Learning Seat LMS, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **Creación de un usuario de prueba de Learning Seat LMS**: para tener un homólogo de Britta Simon en Learning Seat LMS que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Learning Seat LMS.

**Para configurar el inicio de sesión único de Azure AD con Learning Seat LMS, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Learning Seat LMS** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/learningseatlms-tutorial/tutorial_learnconnect_samlbase.png)

1. En la sección **Dominio y direcciones URL de Learning Seat LMS**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Configurar inicio de sesión único](./media/learningseatlms-tutorial/tutorial_learnconnect_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.learningseatlms.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`.

1. Active **Mostrar configuración avanzada de URL**, si desea volver a configurar la aplicación en modo iniciado por **SP**:

    ![Configurar inicio de sesión único](./media/learningseatlms-tutorial/tutorial_learnconnect_url2.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.learningseatlms.com`.
     
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Learning Seat LMS](http://help.learningseatlms.com/help) para obtener estos valores. 

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/learningseatlms-tutorial/tutorial_learnconnect_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/learningseatlms-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en **Learning Seat LMS**, es preciso enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de Learning Seat LMS](http://help.learningseatlms.com/help).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/learningseatlms-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/learningseatlms-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/learningseatlms-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/learningseatlms-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-learning-seat-lms-test-user"></a>Creación de un usuario de prueba de Learning Seat LMS

En esta sección, creará un usuario llamado Britta Simon en Learning Seat LMS. Póngase en contacto con el [equipo de soporte técnico de Learning Seat LMS](http://help.learningseatlms.com/help) con toda la información de usuario para agregar los usuarios en la aplicación Learning Seat LMS.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Learning Seat LMS para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Learning Seat LMS, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Learning Seat LMS**.

    ![Configurar inicio de sesión único](./media/learningseatlms-tutorial/tutorial_learnconnect_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso. 

Al hacer clic en el icono de Learning Seat LMS en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Learning Seat LMS. Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/learnconnect-tutorial/tutorial_general_01.png
[2]: ./media/learnconnect-tutorial/tutorial_general_02.png
[3]: ./media/learnconnect-tutorial/tutorial_general_03.png
[4]: ./media/learnconnect-tutorial/tutorial_general_04.png

[100]: ./media/learnconnect-tutorial/tutorial_general_100.png

[200]: ./media/learnconnect-tutorial/tutorial_general_200.png
[201]: ./media/learnconnect-tutorial/tutorial_general_201.png
[202]: ./media/learnconnect-tutorial/tutorial_general_202.png
[203]: ./media/learnconnect-tutorial/tutorial_general_203.png

