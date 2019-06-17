---
title: 'Tutorial: integración de Azure Active Directory con SecureW2 JoinNow Connector | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SecureW2 JoinNow Connector.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2445b3af-f827-40de-9097-6f5c933d0f53
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b367befb90ec28ece963d67b479749e1c8ad363
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60340002"
---
# <a name="tutorial-azure-active-directory-integration-with-securew2-joinnow-connector"></a>Tutorial: integración de Azure Active Directory con SecureW2 JoinNow Connector

En este tutorial, obtendrá información sobre cómo integrar SecureW2 JoinNow Connector con Azure Active Directory (Azure AD).

La integración de SecureW2 JoinNow Connector con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a SecureW2 JoinNow Connector.
- Puede habilitar que los usuarios inicien sesión automáticamente en SecureW2 JoinNow Connector (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SecureW2 JoinNow Connector se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en SecureW2 JoinNow Connector

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de SecureW2 JoinNow Connector desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-securew2-joinnow-connector-from-the-gallery"></a>Incorporación de SecureW2 JoinNow Connector desde la galería
Para configurar la integración de SecureW2 JoinNow Connector en Azure AD, deberá agregar SecureW2 JoinNow Connector desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SecureW2 JoinNow Connector desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **SecureW2 JoinNow Connector**, seleccione **SecureW2 JoinNow Connector** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![SecureW2 JoinNow conector en la lista de resultados](./media/securejoinnow-tutorial/tutorial_securejoinnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SecureW2 JoinNow Connector utilizando un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, es preciso que Azure AD sepa cuál es el usuario homólogo de SecureW2 JoinNow Connector para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SecureW2 JoinNow Connector.

Para configurar y probar el inicio de sesión único de Azure AD con SecureW2 JoinNow Connector, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de SecureW2 JoinNow Connector](#create-a-securew2-joinnow-connector-test-user)** : para tener un homólogo de Britta Simon en SecureW2 JoinNow Connector que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación SecureW2 JoinNow Connector.

**Para configurar el inicio de sesión único de Azure AD con SecureW2 JoinNow Connector, siga este procedimiento:**

1. En la página de integración de aplicaciones **SecureW2 JoinNow Connector** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/securejoinnow-tutorial/tutorial_securejoinnow_samlbase.png)

3. En la sección **Dominio y direcciones URL de SecureW2 JoinNow Connector**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de SecureW2 JoinNow Connector](./media/securejoinnow-tutorial/tutorial_securejoinnow_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<organization-identifier>-auth.securew2.com/auth/saml/SSO`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<organization-identifier>-auth.securew2.com/auth/saml`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de atención al cliente de SecureW2 JoinNow Connector](mailto:support@securew2.com) para obtener estos valores. 

4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/securejoinnow-tutorial/tutorial_securejoinnow_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/securejoinnow-tutorial/tutorial_general_400.png)

6. Para configurar el inicio de sesión único en **SecureW2 JoinNow Connector**, es preciso enviar los **XML de metadatos** descargados a un [equipo de soporte técnico de SecureW2 JoinNow Connector](mailto:support@securew2.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/securejoinnow-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/securejoinnow-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/securejoinnow-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/securejoinnow-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-securew2-joinnow-connector-test-user"></a>Creación de un usuario de prueba de SecureW2 JoinNow Connector

En esta sección, creará el usuario Britta Simon en SecureW2 JoinNow Connector. Trabaje con el  [equipo de soporte técnico de withSecureW2 JoinNow Connector](mailto:support@securew2.com) para agregar usuarios a la plataforma de SecureW2 JoinNow conector. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para usar el inicio de sesión único de Azure al concederle acceso a SecureW2 JoinNow Connector.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a SecureW2 JoinNow Connector, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **SecureW2 JoinNow Connector**.

    ![Vínculo de SecureW2 JoinNow Connector en la lista de aplicaciones](./media/securejoinnow-tutorial/tutorial_securejoinnow_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

**Para probar la aplicación, realice los pasos siguientes:** 

a. Abra el cliente SecureW2 JoinNow Connector, seleccione el dispositivo adecuado de la lista y haga clic en el botón **Iniciar sesión**.

b. Se abrirá el explorador predeterminado y se le redirigirá a Azure Portal para la autenticación.

c. Tras una correcta autenticación, se debería volver a la página de aterrizaje inicial de SecureW2 JoinNow Connector.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/securejoinnow-tutorial/tutorial_general_01.png
[2]: ./media/securejoinnow-tutorial/tutorial_general_02.png
[3]: ./media/securejoinnow-tutorial/tutorial_general_03.png
[4]: ./media/securejoinnow-tutorial/tutorial_general_04.png

[100]: ./media/securejoinnow-tutorial/tutorial_general_100.png

[200]: ./media/securejoinnow-tutorial/tutorial_general_200.png
[201]: ./media/securejoinnow-tutorial/tutorial_general_201.png
[202]: ./media/securejoinnow-tutorial/tutorial_general_202.png
[203]: ./media/securejoinnow-tutorial/tutorial_general_203.png

