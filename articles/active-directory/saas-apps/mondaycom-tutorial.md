---
title: 'Tutorial: Integración de Azure Active Directory con monday.com | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y monday.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9e8ad807-0664-4e31-91de-731097c768e2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc142bf02a44ea85861f4cc648fd7ee8602c7520
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65896884"
---
# <a name="tutorial-azure-active-directory-integration-with-mondaycom"></a>Tutorial: Integración de Azure Active Directory con monday.com

En este tutorial, aprenderá a integrar monday.com con Azure Active Directory (Azure AD).

La integración de monday.com con Azure AD le proporciona las siguientes ventajas:

* Puede usar Azure AD para controlar quién tiene acceso a monday.com.
* Puede permitir que los usuarios inicien sesión automáticamente en monday.com (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información acerca de la integración de aplicaciones SaaS (software como servicio) con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con monday.com, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción de Azure AD, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una suscripción de monday.com con el inicio de sesión único habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba e integrar monday.com con Azure AD.

monday.com admite las siguientes características:

* **Inicio de sesión único iniciado por SP**
* **Inicio de sesión único iniciado por IDP**
* **Aprovisionamiento de usuarios Just-In-Time**

## <a name="add-mondaycom-in-the-azure-portal"></a>Agregar monday.com en Azure Portal

Para integrar monday.com con Azure AD, tiene que agregar monday.com a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Seleccione **Azure Active Directory** en el menú izquierdo.

    ![Opción de Azure Active Directory](common/select-azuread.png)

1. Seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Panel Aplicaciones empresariales](common/enterprise-applications.png)

1. Para agregar una aplicación, seleccione **Nueva aplicación**.

    ![Opción Nueva aplicación](common/add-new-app.png)

1. En el cuadro de búsqueda, escriba **monday.com**. En los resultados de la búsqueda, seleccione **monday.com** y, luego, **Agregar**.

    ![monday.com en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con monday.com con un usuario de prueba llamado **Britta Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de monday.com.

Para configurar y probar el inicio de sesión único de Azure AD con monday.com, debe completar los siguientes bloques de creación:

| Tarea | DESCRIPCIÓN |
| --- | --- |
| **[Configuración del inicio de sesión único en Azure AD](#configure-azure-ad-single-sign-on)** | Permite que los usuarios usen esta característica. |
| **[Configuración del inicio de sesión único de monday.com](#configure-mondaycom-single-sign-on)** | Configura los valores de inicio de sesión único en la aplicación. |
| **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** | Prueba el inicio de sesión único de Azure AD con el usuario Britta Simon. |
| **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** | Permite que Britta Simon use el inicio de sesión único de Azure AD. |
| **[Creación de un usuario de prueba de monday.com](#create-a-mondaycom-test-user)** | Crea un homólogo de Britta Simon en monday.com que está vinculado a la representación del usuario en Azure AD. |
| **[Prueba de inicio de sesión único](#test-single-sign-on)** | Comprueba que la configuración funciona. |

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, configurará el inicio de sesión único de Azure AD con monday.com en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en el panel de integración de la aplicación **monday.com**, seleccione **Inicio de sesión único**.

    ![Configuración de la opción de inicio de sesión único](common/select-sso.png)

1. En el panel **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** o **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

1. En el panel **Configurar el inicio de sesión único con SAML**, seleccione **Editar** (icono de lápiz) para abrir el panel **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En el panel **Configuración básica de SAML**, si tiene el archivo de metadatos del proveedor de servicios y desea configurar el *modo iniciado por IDP*, realice los pasos siguientes:

    1. Seleccione **Cargar el archivo de metadatos**.

       ![La opción Cargar el archivo de metadatos](common/upload-metadata.png)

    1. Para seleccionar el archivo de metadatos, seleccione el icono de carpeta y, a continuación, seleccione **Cargar**.

       ![Selección del archivo de metadatos y después del botón Cargar](common/browse-upload-metadata.png)

    1. Una vez que se haya cargado correctamente el archivo de metadatos, los valores de **Identificador** y **URL de respuesta** se rellenan automáticamente en el panel **Configuración básica de SAML**:

       ![Valores de IDP en el panel Configuración básica de SAML](common/idp-intiated.png)

       > [!Note]
       > Si los valores **Identificador** y **Dirección URL de respuesta** no se rellenan automáticamente, escríbalos manualmente.

1. Para configurar la aplicación en modo *iniciado por SP*:

    1. Seleccione **Establecer direcciones URL adicionales**.
    
    1. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: https:\//\<su-dominio>.monday.com. Póngase en contacto con el [equipo de soporte al cliente de monday.com](mailto:support@monday.com) para obtener la dirección URL de inicio de sesión.

        ![Opción Establecer direcciones URL adicionales](common/metadata-upload-additional-signon.png)

1. La aplicación monday.com espera que las aserciones de SAML tengan un formato específico. Configure las siguientes notificaciones para esta aplicación. Para administrar estos valores de atributo, en el panel **Configurar el inicio de sesión único con SAML**, seleccione **Editar** para abrir el panel **Atributos de usuario**.

    ![El panel Atributos de usuario](common/edit-attribute.png)

1. En **Notificaciones del usuario**, seleccione **Editar** para editar las notificaciones. Para agregar una notificación, seleccione **Agregar nueva notificación** y, a continuación, configure el atributo del token SAML, tal como se muestra en la imagen anterior. Después, complete los siguientes pasos: 

    1. Seleccione **Agregar nueva notificación**.

        ![La opción Agregar nueva notificación e el panel Notificaciones del usuario](common/new-save-attribute.png)

    1. En el panel **Administrar las notificaciones del usuario**, defina los siguientes valores:
        
       1. En el cuadro **Nombre**, escriba el nombre de atributo que se muestra para la fila de la notificación del usuario.

       1. Deje **Espacio de nombres** en blanco.

       1. En **Origen**, seleccione **Atributo**.

       1. En la lista **Atributo de origen**, seleccione el valor del atributo que se muestra para la fila de la notificación del usuario.

       1. Seleccione **Aceptar** y después **Guardar**.

       ![Administrar las notificaciones del usuario](common/new-attribute-details.png)

1. En el panel **Configurar el inicio de sesión único con SAML**, en **Certificado de firma de SAML**, seleccione **Descargar** junto a **Certificado (base 64)** . Seleccione una opción de descarga según sus requisitos. Guarde el certificado en el equipo.

    ![Opción de descarga del certificado (Base64)](common/certificatebase64.png)

1. En la sección **Configurar monday.com**, copie las direcciones URL siguientes según sus necesidades:

    * URL de inicio de sesión
    * Identificador de Azure AD
    * URL de cierre de sesión

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="configure-mondaycom-single-sign-on"></a>Configuración del inicio de sesión único de monday.com

Para configurar el inicio de sesión único en monday.com, envíe el archivo del certificado (Base64) descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de monday.com](mailto:support@monday.com). El equipo de soporte técnico de monday.com usa la información que les envíe para asegurarse de que la conexión de inicio de sesión único de SAML está configurada correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.

    ![Opciones Usuarios y Todos los usuarios](common/users.png)

1. Seleccione **Nuevo usuario**.

    ![Opción Nuevo usuario](common/new-user.png)

1. En el panel **Usuario**, siga estos pasos:

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.
  
    1. En el cuadro de texto **Nombre de usuario**, escriba **brittasimon\@\<dominiodeempresa>.\<extensión>** . Por ejemplo, **brittasimon\@contoso.com**.

    1. Active la casilla de verificación **Mostrar contraseña**. Anote el valor que se muestra en el cuadro **Contraseña**.

    1. Seleccione **Crear**.

    ![Panel Usuario](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá a Britta Simon acceso a monday.com para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones** > **monday.com**.

    ![Panel Aplicaciones empresariales](common/enterprise-applications.png)

1. En la lista de aplicaciones, seleccione **monday.com**.

    ![monday.com en la lista de aplicaciones](common/all-applications.png)

1. En el menú, seleccione **Usuarios y grupos**.

    ![Opción Usuarios y grupos](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. Después, en el panel **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![El panel Agregar asignación](common/add-assign-user.png)

1. En el panel **Usuarios y grupos**, en la lista de usuarios, seleccione **Britta Simon**. Elija **Seleccionar**.

1. Si espera algún valor de rol en la aserción de SAML, en el panel **Seleccionar rol**, seleccione el rol adecuado para el usuario de la lista. Elija **Seleccionar**.

1. En el panel **Agregar asignación**, seleccione **Asignar**.

### <a name="create-a-mondaycom-test-user"></a>Creación de un usuario de prueba de monday.com

En esta sección, se crea un usuario llamado Britta Simon en la aplicación monday.com. monday.com admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en monday.com, se crea otro después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el portal Aplicaciones.

Después de configurar el inicio de sesión único, cuando se selecciona **monday.com** en el portal Aplicaciones, se inicia sesión automáticamente en monday.com. Para más información acerca del portal Aplicaciones, consulte [Acceso y uso de aplicaciones en el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte estos artículos:

- [Lista de tutoriales para integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Inicio de sesión único en aplicaciones en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
