---
title: 'Tutorial: Integración de Azure Active Directory con SensoScientific Wireless Temperature Monitoring System | Microsoft Docs'
description: Obtenga información sobre cómo configurar un inicio de sesión único entre Azure Active Directory y SensoScientific Wireless Temperature Monitoring System.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33a625e82f41bee1b8e3980192076d24a7471953
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183418"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Tutorial: Integración de Azure Active Directory con SensoScientific Wireless Temperature Monitoring System

En este tutorial, aprenderá a integrar SensoScientific Wireless Temperature Monitoring System con Azure Active Directory (Azure AD).

La integración de SensoScientific Wireless Temperature Monitoring System con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién accede a SensoScientific Wireless Temperature Monitoring System.
- Puede habilitar que los usuarios inicien sesión automáticamente en SensoScientific Wireless Temperature Monitoring System (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SensoScientific Wireless Temperature Monitoring System, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción que tenga habilitado el inicio de sesión único en SensoScientific Wireless Temperature Monitoring System.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de SensoScientific Wireless Temperature Monitoring System desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Adición de SensoScientific Wireless Temperature Monitoring System desde la galería
Para configurar la integración de SensoScientific Wireless Temperature Monitoring System en Azure AD, debe agregar dicho sistema desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SensoScientific Wireless Temperature Monitoring System desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **SensoScientific Wireless Temperature Monitoring System**.

    ![Creación de un usuario de prueba de Azure AD](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

1. En el panel de resultados, seleccione **SensoScientific Wireless Temperature Monitoring System** y después haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SensoScientific Wireless Temperature Monitoring System mediante un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de SensoScientific Wireless Temperature Monitoring System para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SensoScientific Wireless Temperature Monitoring System.

Esta relación de vínculo se establece mediante la asignación del valor del **nombre de usuario** en Azure AD como el valor del **nombre de usuario** en SensoScientific Wireless Temperature Monitoring System.

Para configurar y probar el inicio de sesión único de Azure AD con SensoScientific Wireless Temperature Monitoring System, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de SensoScientific Wireless Temperature Monitoring System](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**: para encontrar un homólogo de Britta Simon en SensoScientific Wireless Temperature Monitoring System vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación SensoScientific Wireless Temperature Monitoring System.

**Para configurar el inicio de sesión único de Azure AD con SensoScientific Wireless Temperature Monitoring System, realice los pasos siguientes:**

1. En la página de integración de la aplicación **SensoScientific Wireless Temperature Monitoring System** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

1. En la sección **Dominio y direcciones URL de SensoScientific Wireless Temperature Monitoring System**, no necesita seguir ningún paso, ya que la aplicación ya está integrada previamente en Azure:

    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de SensoScientific Wireless Temperature Monitoring System**, haga clic en **Configurar SensoScientific Wireless Temperature Monitoring System** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad en SAML** y la **dirección URL de cierre de sesión** de la **sección Referencia rápida**.

    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

1. Inicie sesión en la aplicación SensoScientific Wireless Temperature Monitoring System como administrador.

1. En el menú de navegación de la parte superior, haga clic en **Configuration** (Configuración) y vaya a **Configure** (Configurar) en **Single Sign On** (Inicio de sesión único) para abrir la configuración del inicio de sesión.

    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

1. En el formulario **Single Sign On Settings** (Configuración de inicio de sesión), realice estos pasos:
 
     a. Seleccione **Issuer Name** (Nombre del emisor) como Azure AD.
    
    b. Pegue el **Identificador de entidad en SAML** que ha copiado de Azure Portal en el cuadro de texto Issuer URL (Dirección URL del emisor).
    
    c. Copie la **Dirección URL de inicio de sesión único de SAML** que ha copiado de Azure Portal en el cuadro de texto Single Sign-On Service URL (Dirección URL del servicio de inicio de sesión único).

    d. Pegue la **Dirección URL de cierre de sesión** que ha copiado de Azure Portal en el cuadro de texto Single Sign-Out Service URL (Dirección URL del servicio de cierre de sesión único).

    e. Examine el certificado que ha descargado de Azure Portal y cárguelo aquí.
    
    f. Haga clic en **Save**(Guardar).
  
> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/sensoscientific-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/sensoscientific-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/sensoscientific-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/sensoscientific-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Creación de un usuario de prueba de SensoScientific Wireless Temperature Monitoring System

Para permitir que los usuarios de Azure AD se registren en SensoScientific Wireless Temperature Monitoring System, es necesario aprovisionarlos en dicha aplicación. Colabore con el  [equipo de soporte técnico de SensoScientific Wireless Temperature Monitoring System](https://www.sensoscientific.com/contact-us/) para agregar usuarios a la plataforma de SensoScientific Wireless Temperature Monitoring System. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a SensoScientific Wireless Temperature Monitoring System.

![Asignar usuario][200] 

**Para asignar a Britta Simon a SensoScientific Wireless Temperature Monitoring System, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **SensoScientific Wireless Temperature Monitoring System**.

    ![Configurar inicio de sesión único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso. Haga clic en el icono de SensoScientific Wireless Temperature Monitoring System en el panel de acceso y después iniciará sesión automáticamente en dicha aplicación. Para más información sobre el Panel de acceso, vea la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/sensoscientific-tutorial/tutorial_general_203.png

