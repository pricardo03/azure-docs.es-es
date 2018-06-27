---
title: 'Tutorial: Integración de Azure Active Directory con Autotask Endpoint Backup | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Autotask Endpoint Backup.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9f55319e-895b-4130-8460-71713f25ed04
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeedes
ms.openlocfilehash: 0cbdb297f7f92c247295f11df459fe682ccebf47
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220574"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-endpoint-backup"></a>Tutorial: Integración de Azure Active Directory con Autotask Endpoint Backup

En este tutorial, obtendrá información sobre cómo integrar Autotask Endpoint Backup con Azure Active Directory (Azure AD).

La integración de Autotask Endpoint Backup con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Autotask Endpoint Backup.
- Puede permitir que los usuarios inicien sesión automáticamente en Autotask Endpoint Backup (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>requisitos previos

Para configurar la integración de Azure AD con Autotask Endpoint Backup, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Autotask Endpoint Backup

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Autotask Endpoint Backup desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-autotask-endpoint-backup-from-the-gallery"></a>Adición de Autotask Endpoint Backup desde la galería
Para configurar la integración de Autotask Workplace en Azure AD, deberá agregar Autotask Endpoint Backup desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Autotask Endpoint Backup desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Autotask Endpoint Backup**, seleccione **Autotask Endpoint Backup** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Autotask Endpoint Backup en la lista de resultados](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Autotask Endpoint Backup con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Autotask Endpoint Backup para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Autotask Endpoint Backup.

Para configurar y probar el inicio de sesión único de Azure AD con Autotask Endpoint Backup, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Autotask Endpoint Backup](#create-a-autotask-endpoint-backup-test-user)**: para tener un homólogo de Britta Simon en Autotask Endpoint Backup que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Autotask Endpoint Backup.

**Para configurar el inicio de sesión único de Azure AD con Autotask Endpoint Backup, siga estos pasos:**

1. En la página de integración de la aplicación **Autotask Endpoint Backup** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_samlbase.png)

3. En la sección **Dominio y direcciones URL de Autotask Endpoint Backup**, realice los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Autotask Endpoint Backup](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_url.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.backup.autotask.net/singlesignon/saml/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.backup.autotask.net/singlesignon/saml/SSO`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte de cliente de Autotask Endpoint Backup](https://backup.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obtener estos valores.

4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/autotaskendpointbackup-tutorial/tutorial_general_400.png)

6. Para configurar el inicio de sesión único en **Autotask Endpoint Backup**, necesita enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de Autotask Endpoint Backup](https://backup.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/autotaskendpointbackup-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/autotaskendpointbackup-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/autotaskendpointbackup-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/autotaskendpointbackup-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
  
### <a name="create-a-autotask-endpoint-backup-test-user"></a>Creación de un usuario de prueba de Autotask Endpoint Backup

En esta sección, creará un usuario llamado Britta Simon en Autotask Endpoint Backup. Colabore con el [equipo de soporte técnico de Autotask Endpoint Backup](https://backup.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para agregar los usuarios en la plataforma Autotask Endpoint Backup. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Autotask Endpoint Backup.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Autotask Endpoint Backup, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Autotask Endpoint Backup**.

    ![Vínculo de Autotask Endpoint Backup en la lista de aplicaciones](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Autotask Endpoint Backup en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación de Autotask Endpoint Backup.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/autotaskendpointbackup-tutorial/tutorial_general_01.png
[2]: ./media/autotaskendpointbackup-tutorial/tutorial_general_02.png
[3]: ./media/autotaskendpointbackup-tutorial/tutorial_general_03.png
[4]: ./media/autotaskendpointbackup-tutorial/tutorial_general_04.png

[100]: ./media/autotaskendpointbackup-tutorial/tutorial_general_100.png

[200]: ./media/autotaskendpointbackup-tutorial/tutorial_general_200.png
[201]: ./media/autotaskendpointbackup-tutorial/tutorial_general_201.png
[202]: ./media/autotaskendpointbackup-tutorial/tutorial_general_202.png
[203]: ./media/autotaskendpointbackup-tutorial/tutorial_general_203.png

