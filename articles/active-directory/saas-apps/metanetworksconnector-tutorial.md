---
title: 'Tutorial: integración de Azure Active Directory con Meta Networks Connector | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Meta Networks Connector.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c4d702a33742da5501be208154cbdd593fa0524
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199721"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Tutorial: integración de Azure Active Directory con Meta Networks Connector

En este tutorial, aprenderá a integrar Meta Networks Connector con Azure Active Directory (Azure AD).

La integración de Meta Networks Connector con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Meta Networks Connector.
- Puede habilitar que los usuarios inicien sesión automáticamente en Meta Networks Connector (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Meta Networks Connector se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Meta Networks Connector

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Meta Networks Connector desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Incorporación de Meta Networks Connector desde la galería
Para configurar la integración de Meta Networks Connector en Azure AD, deberá agregar Meta Networks Connector desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Meta Networks Connector desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 
    
    ![Botón Azure Active Directory][1]
    
1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.
    
    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.
    
    ![Botón Nueva aplicación][3]
    
1. En el cuadro de búsqueda, escriba **Meta Networks Connector**, seleccione **Meta Networks Connector** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.
    
    ![Meta Networks Connector en la lista de resultados](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Meta Networks Connector utilizando un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, es preciso que Azure AD sepa cuál es el usuario homólogo de Meta Networks Connector para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Meta Networks Connector.

Para configurar y probar el inicio de sesión único de Azure AD con Meta Networks Connector, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Meta Networks Connector](#create-a-meta-networks-connector-test-user)**: para tener un homólogo de Britta Simon en Meta Networks Connector que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Meta Networks Connector.

**Para configurar el inicio de sesión único de Azure AD con Meta Networks Connector, realice los pasos siguientes:**

1. En la página de integración de aplicaciones **Meta Networks Connector** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_samlbase.png)

1. En la sección **Dominio y direcciones URL de Meta Networks Connector**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Meta Networks Connector](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url.png)

    1. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`
    
    1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`.
    
1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Meta Networks Connector](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url1.png)

    1. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`.
    
    1. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL que siga este patrón: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`
    
    > [!NOTE]
    > Estos valores no son reales. La actualización de estos valores por el identificador, dirección URL de respuesta y dirección URL de inicio de sesión reales se explica más adelante en el tutorial.
    
1. La aplicación Meta Networks Connector espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.
    
    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_attribute.png)
    
1. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | Nombre del atributo | Valor de atributo | Espacio de nombres|
    | ---------------| --------------- | -------- |
    | firstname | user.givenname | |
    | lastname | user.surname | |
    | emailaddress| user.mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | Nombre | user.userprincipalname| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | phone | user.telephonenumber | |

    1. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

        ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/tutorial_attribute_04.png)
    
        ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/tutorial_attribute_05.png)   
    
    1. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    1. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.
    
    1. En el cuadro de texto **Espacio de nombres**, escriba el valor del espacio de nombres que se muestra para esa fila.
    
    1. Haga clic en **Aceptar**.
    
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.
    
    ![Vínculo de descarga del certificado](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_certificate.png)
    
1. En la sección **Configuración de Meta Networks Connector**, haga clic en **Configurar Meta Networks Connector** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.
    
    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_configure.png)
    
1. Haga clic en el botón **Guardar** .
    
    ![Botón Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/tutorial_general_400.png)
    
1. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Meta Networks Connector.
    
    > [!NOTE]
    > Meta Networks Connector es un sistema seguro. Por lo que antes de acceder a su portal deberá hacer que su dirección IP pública se incluya en lista de permitidos en su lado. Para obtener la dirección IP pública, siga el vínculo especificado [aquí](https://whatismyipaddress.com/). Envíe su dirección IP al [equipo de soporte técnico de Meta Networks Connector](mailto:support@metanetworks.com) para que su dirección IP se incluya en lista de permitidos.
    
1. Vaya a **Administrador** y seleccione **Configuración**.
    
    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/configure3.png)
    
1. Asegúrese de que **Registro del tráfico de Internet** y **Forzar MFA de VPN** se establecen en Desactivado.
    
    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/configure1.png)
    
1. Vaya a **Administrador** y seleccione **SAML**.
    
    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/configure4.png)
    
1. En la página **Detalles**, lleve a cabo los pasos siguientes:
    
    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/configure2.png)
    
    1. Copie el valor de **Dirección URL de inicio de sesión único** y péguelo en el cuadro de texto **Dirección URL de inicio de sesión** en la sección **Dominio y direcciones URL de Meta Networks Connector**.
    
    1. Copie el valor de **Dirección URL del destinatario** y péguelo en el cuadro de texto **Dirección URL de respuesta** en la sección **Dominio y direcciones URL de Meta Networks Connector**.
    
    1. Copie el valor de **Identificador URI de audiencia** y péguelo en el cuadro de texto **Identificador (Identificador de entidad)** en la sección **Dominio y direcciones URL de Meta Networks Connector**.
    
    1. Habilitación de SAML
    
1. En la pestaña **General**, realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/metanetworksconnector-tutorial/configure5.png)

    1. En **Dirección URL de inicio de sesión único del proveedor de identidades**, pegue el valor de **Dirección URL del servicio de inicio de sesión único de SAML** que ha copiado desde Azure Portal.

    1. En el cuadro de texto **Emisor de proveedor de identidades**, pegue el valor de **Identificador de entidad de SAML** que ha copiado de Azure Portal.

    1. Abra el certificado descargado desde Azure Portal en el Bloc de notas y péguelo en el cuadro de texto **Certificado X.509**.

    1. Habilite el **Aprovisionamiento Just-in-Time**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".
    
![Creación de un usuario de prueba de Azure AD][100]
    
**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.
    
    ![Botón Azure Active Directory](./media/metanetworksconnector-tutorial/create_aaduser_01.png)
    
1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.
    
    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/metanetworksconnector-tutorial/create_aaduser_02.png)
    
1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.
    
    ![Botón Agregar](./media/metanetworksconnector-tutorial/create_aaduser_03.png)
    
1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/metanetworksconnector-tutorial/create_aaduser_04.png)
    
    1. En el cuadro **Nombre**, escriba **BrittaSimon**.

    1. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.
    
    1. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.
    
    1. Haga clic en **Create**(Crear).
    
### <a name="create-a-meta-networks-connector-test-user"></a>Creación de un usuario de prueba de Meta Networks Connector

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Meta Networks Connector. Meta Networks Connector admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Durante un intento de acceder a Meta Networks Connector se creará un nuevo usuario, en caso de que no exista.

>[!Note]
>Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el  [equipo de soporte técnico de Meta Networks Connector](mailto:support@metanetworks.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para usar el inicio de sesión único de Azure al concederle acceso a Meta Networks Connector.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Meta Networks Connector, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.
    
    ![Asignar usuario][201]
    
1. En la lista de aplicaciones, seleccione **Meta Networks Connector**.
    
    ![Vínculo de Meta Networks Connector en la lista de aplicaciones](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_app.png)  
    
1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.
    
    ![Vínculo "Usuarios y grupos"][202]
    
1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.
    
    ![Panel Agregar asignación][203]
    
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.
    
1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.
    
1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Meta Networks Connector en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación Meta Networks Connector.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/metanetworksconnector-tutorial/tutorial_general_01.png
[2]: ./media/metanetworksconnector-tutorial/tutorial_general_02.png
[3]: ./media/metanetworksconnector-tutorial/tutorial_general_03.png
[4]: ./media/metanetworksconnector-tutorial/tutorial_general_04.png

[100]: ./media/metanetworksconnector-tutorial/tutorial_general_100.png

[200]: ./media/metanetworksconnector-tutorial/tutorial_general_200.png
[201]: ./media/metanetworksconnector-tutorial/tutorial_general_201.png
[202]: ./media/metanetworksconnector-tutorial/tutorial_general_202.png
[203]: ./media/metanetworksconnector-tutorial/tutorial_general_203.png

