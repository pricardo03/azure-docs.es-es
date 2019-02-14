---
title: 'Tutorial: Integración de Azure Active Directory con Lecorpio | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Lecorpio.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e09693d17f5eca1b5e4597c522903c3f9148bc74
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56178896"
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a>Tutorial: Integración de Azure Active Directory con Lecorpio

En este tutorial, aprenderá a integrar Lecorpio con Azure Active Directory (Azure AD).

La integración de Lecorpio con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Lecorpio.
- Puede permitir que los usuarios inicien sesión automáticamente en Lecorpio (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Lecorpio, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Lecorpio.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Lecorpio desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-lecorpio-from-the-gallery"></a>Adición de Lecorpio desde la galería
Para configurar la integración de Lecorpio en Azure AD, deberá agregar Lecorpio desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Lecorpio desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Lecorpio**.

    ![Creación de un usuario de prueba de Azure AD](./media/lecorpio-tutorial/tutorial_lecorpio_search.png)

1. En el panel de resultados, seleccione **Lecorpio** y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Lecorpio con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Lecorpio para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Lecorpio.

Esta relación de vínculo se establece mediante la asignación del valor de **nombre de usuario** en Azure AD como el valor de **nombre de usuario** en Lecorpio.

Para configurar y probar el inicio de sesión único de Azure AD con Lecorpio, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Lecorpio](#creating-a-lecorpio-test-user)**: para tener un homólogo de Britta Simon en Lecorpio que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en el portal de Azure y configurará el inicio de sesión único en la aplicación Lecorpio.

**Para configurar el inicio de sesión único de Azure AD con Lecorpio, realice los pasos siguientes:**

1. En el portal de Azure, en la página de integración de la aplicación **Lecorpio**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

1. En la sección **Dominio y direcciones URL de Lecorpio**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/lecorpio-tutorial/tutorial_lecorpio_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba el valor con el siguiente patrón: `https://<instance name>.lecorpio.com/<customer name>`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<instance name>.lecorpio.com/<customer name>`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Aquí le recomendamos que utilice el valor de cadena único en el identificador. Póngase en contacto con el [equipo de soporte de cliente de Lecorpio](mailto:info@lecorpio.com) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/lecorpio-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en el lado de **Lecorpio**, debe enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de Lecorpio](mailto:info@lecorpio.com).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/lecorpio-tutorial/create_aaduser_01.png) 

1. Vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios** para mostrar la lista de usuarios.
    
    ![Creación de un usuario de prueba de Azure AD](./media/lecorpio-tutorial/create_aaduser_02.png) 

1. En la parte superior del diálogo, haga clic en **Agregar** para abrir el diálogo **Usuario**.
 
    ![Creación de un usuario de prueba de Azure AD](./media/lecorpio-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/lecorpio-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-lecorpio-test-user"></a>Creación de un usuario de prueba de Lecorpio

En esta sección, creará un usuario llamado Britta Simon en Lecorpio. 

Póngase en contacto con el [equipo de soporte técnico del cliente de Lecorpio](mailto:info@lecorpio.com) para agregar los usuarios en la aplicación Lecorpio.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Lecorpio.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Lecorpio, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Lecorpio**.

    ![Configurar inicio de sesión único](./media/lecorpio-tutorial/tutorial_lecorpio_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Lecorpio en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Lecorpio.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/lecorpio-tutorial/tutorial_general_203.png

