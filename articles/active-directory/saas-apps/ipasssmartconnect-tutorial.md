---
title: 'Tutorial: integración de Azure Active Directory con iPass SmartConnect | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e iPass SmartConnect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: ecfdd3fae1d394e3b57fcd325f44cad0d1a98534
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444901"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Tutorial: integración de Azure Active Directory con iPass SmartConnect

En este tutorial, obtendrá información sobre cómo integrar iPass SmartConnect con Azure Active Directory (Azure AD).

La integración de iPass SmartConnect con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a iPass SmartConnect.
- Puede permitir que los usuarios inicien sesión automáticamente en iPass SmartConnect (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con iPass SmartConnect, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción que permita el inicio de sesión único en iPass SmartConnect

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar iPass SmartConnect desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Agregar iPass SmartConnect desde la galería
Para configurar la integración de iPass SmartConnect en Azure AD, será preciso que agregue iPass SmartConnect desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar iPass SmartConnect desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **iPass SmartConnect**, seleccione **iPass SmartConnect** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![iPass SmartConnect en la lista de resultados](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con iPass SmartConnect con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de iPass SmartConnect para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de iPass SmartConnect.

Para configurar y probar el inicio de sesión único de Azure AD con iPass SmartConnect, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de iPass SmartConnect](#create-an-ipass-smartconnect-test-user)**: para tener un homólogo de Britta Simon en iPass SmartConnect que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación iPass SmartConnect.

**Para configurar el inicio de sesión único de Azure AD con iPass SmartConnect, realice los pasos siguientes:**

1. En la página de integración de la aplicación **iPass SmartConnect** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

1. En la sección **Dominio y direcciones URL de iPass SmartConnect**, si quiere configurar la aplicación en modo iniciado por **IDP**, no es necesario realizar ningún paso.

    ![Información de dominio y direcciones URL de inicio de sesión único de iPass SmartConnect](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

1. Active Mostrar configuración avanzada de URL y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de iPass SmartConnect](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    En el cuadro de texto URL de inicio de sesión, escriba una dirección URL: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

1. La aplicación iPass SmartConnect espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección "**Atributos de usuario**" de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.

    ![Configurar inicio de sesión único](./media/ipasssmartconnect-tutorial/attribute.png)

1. En la sección **Atributos de usuario**, active la casilla **Ver y editar todos los demás atributos de usuario** para expandir los atributos. Realice los pasos siguientes en cada uno de los atributos mostrados:

    | Nombre del atributo | Valor de atributo | Valor de espacio de nombres|
    | ---------------| --------------- |----------------|
    | firstName | user.givenname |   |
    | lastName | user.surname | |
    | email | user.userprincipalname | |
    | nombre de usuario | user.userprincipalname | |

    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    d. Mantenga el valor del espacio de nombres en blanco para esa fila.

    e. Haga clic en **Aceptar**.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en el lado de **iPass SmartConnect**, debe enviar el archivo **XML de metadatos** descargado y el **nombre de dominio** al [equipo de soporte técnico de iPass SmartConnect](mailto:help@ipass.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-an-ipass-smartconnect-test-user"></a>Creación de un usuario de prueba iPass SmartConnect

En esta sección, creará la usuaria llamada Britta Simon en iPass SmartConnect. Colabore con el [equipo de soporte técnico de iPass SmartConnect](mailto:help@ipass.com) para agregar los usuarios o el dominio que deben incluirse en la lista blanca en la plataforma de iPass SmartConnect. Si el dominio lo agrega el equipo, los usuarios se aprovisionarán automáticamente en la plataforma de iPass SmartConnect. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a iPass SmartConnect para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a iPass SmartConnect, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

1. En la lista de aplicaciones, seleccione **iPass SmartConnect**.

    ![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

**Para probar la aplicación en el flujo iniciado por SP, realice los pasos siguientes:**

a. Descargue el cliente iPass SmartConnect para Windows [aquí](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing3.png)

b. Instale el cliente e inícielo.

c. Para continuar, haga clic en **Introducción**.

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Escriba el nombre de usuario de Azure con el dominio. Haga clic en **Continuar**. Se le redirigirá a la página de inicio de sesión de Azure.

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Tras la autenticación correcta, se iniciará la activación del cliente. Se activará el cliente.

**Para probar la aplicación en el flujo iniciado por IdP, realice los pasos siguientes:**

a. Inicie sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com).

b. Haga clic en la aplicación iPass SmartConnect.

c. Inicia la página de SSA, haga clic en **Download App for Windows** (Descargar la aplicación para Windows) para instalar el cliente de iPass SmartConnect.

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing4.png)

d. Después de la instalación, el cliente en el primer inicio iniciará la activación automáticamente después de aceptar los términos y condiciones.

e. Si no se inicia la activación, haga clic en el botón Activar en la página de SSA para iniciar la activación.

f. Se activará el cliente.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ipasssmartconnect-tutorial/tutorial_general_01.png
[2]: ./media/ipasssmartconnect-tutorial/tutorial_general_02.png
[3]: ./media/ipasssmartconnect-tutorial/tutorial_general_03.png
[4]: ./media/ipasssmartconnect-tutorial/tutorial_general_04.png

[100]: ./media/ipasssmartconnect-tutorial/tutorial_general_100.png

[200]: ./media/ipasssmartconnect-tutorial/tutorial_general_200.png
[201]: ./media/ipasssmartconnect-tutorial/tutorial_general_201.png
[202]: ./media/ipasssmartconnect-tutorial/tutorial_general_202.png
[203]: ./media/ipasssmartconnect-tutorial/tutorial_general_203.png

