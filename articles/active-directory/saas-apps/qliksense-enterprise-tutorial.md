---
title: 'Tutorial: Integración de Azure Active Directory con Qlik Sense Enterprise | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Qlik Sense Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e72ec4f9c512f6525f790d555794c1a120ac07c9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67093433"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Tutorial: Integración de Azure Active Directory con Qlik Sense Enterprise

En este tutorial, aprenderá a integrar Qlik Sense Enterprise con Azure Active Directory (Azure AD).
La integración de Qlik Sense Enterprise con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Qlik Sense Enterprise.
* Puede permitir que los usuarios inicien sesión automáticamente en Qlik Sense Enterprise (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Qlik Sense Enterprise, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en Qlik Sense Enterprise

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Qlik Sense Enterprise admite el inicio de sesión único iniciado por **SP**

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Adición de Qlik Sense Enterprise desde la galería

Para configurar la integración de Qlik Sense Enterprise en Azure AD, deberá agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Qlik Sense Enterprise desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Qlik Sense Enterprise**, seleccione **Qlik Sense Enterprise** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Qlik Sense Enterprise en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Qlik Sense Enterprise utilizando un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Qlik Sense Enterprise.

Para configurar y probar el inicio de sesión único de Azure AD con Qlik Sense Enterprise, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Qlik Sense Enterprise](#configure-qlik-sense-enterprise-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Qlik Sense Enterprise](#create-qlik-sense-enterprise-test-user)** : para tener un homólogo de Britta Simon en Qlik Sense Enterprise que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Qlik Sense Enterprise, realice los pasos siguientes:

1. En la página de integración de la aplicación **Qlik Sense Enterprise** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Qlik Sense Enterprise](common/sp-identifier-reply.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<Qlik Sense Fully Qualifed Hostname>:4443/azure/hub`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente modelo:

    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|
    | |

    c. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:

    `https://<Qlik Sense Fully Qualifed Hostname>:4443/samlauthn/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de respuesta, el identificador y la dirección URL de inicio de sesión reales, que se explican más adelante en el tutorial, o póngase en contacto con el [equipo de soporte técnico de Qlik Sense Enterprise](https://www.qlik.com/us/services/support) para obtener estos valores.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-qlik-sense-enterprise-single-sign-on"></a>Configuración del inicio de sesión único de Qlik Sense Enterprise

1. Prepare el archivo XML de metadatos de federación para que pueda cargarse en el servidor de Qlik Sense.

    > [!NOTE]
    > Antes de cargar los metadatos de IdP en el servidor de Qlik Sense, hay que editar el archivo para quitar información y, de este modo, garantizar que la conexión entre Azure AD y el servidor de Qlik se realice sin problemas.

    ![QlikSense][qs24]

    a. Abra el archivo FederationMetaData.xml que ha descargado de Azure Portal en un editor de texto.

    b. Busque el valor **RoleDescriptor**.  Hay cuatro entradas (dos pares de etiquetas de elementos de apertura y cierre).

    c. Elimine del archivo las etiquetas de RoleDescriptor y toda la información intermedia.

    d. Guarde el archivo y téngalo a mano, ya que vamos a utilizarlo más adelante en este documento.

2. Acceda a Qlik Sense Qlik Management Console (QMC) como un usuario que pueda crear configuraciones de proxy virtual.

3. En la consola QMC, haga clic en el elemento de menú **Virtual Proxies** (servidores proxy virtuales).

    ![QlikSense][qs6]

4. Haga clic en el botón **Create new** (Crear nuevo) situado en la parte inferior de la pantalla.

    ![QlikSense][qs7]

5. Aparece la pantalla de edición Virtual Proxy (Proxy virtual).  En la parte derecha de la pantalla hay un menú con el que puede hacer que se muestren las opciones de configuración.

    ![QlikSense][qs9]

6. Active la opción de menú Identificación y escriba la información de identificación de la configuración de proxy virtual de Azure.

    ![QlikSense][qs8]  

    a. El campo **Description** (Descripción) es un nombre descriptivo de la configuración del proxy virtual.  Escriba un valor para este campo.

    b. El campo **Prefix** (Prefijo) identifica el punto de conexión del proxy virtual para conectarse a Qlik Sense con el inicio de sesión único de Azure AD.  Escriba un nombre de prefijo único para este proxy virtual.

    c. **Session inactivity timeout (minutes)** (Tiempo de espera de inactividad de la sesión [minutos]) es el tiempo de espera de las conexiones que se establecen a través de este proxy virtual.

    d. **Session cookie header name** (Nombre de encabezado de cookie de sesión) es el nombre de la cookie que almacena el identificador de la sesión de Qlik Sense que recibe un usuario tras autenticarse correctamente.  Este nombre debe ser único.

7. Haga clic en la opción de menú Autenticación para que se muestre.  Aparecerá la pantalla Autenticación.

    ![QlikSense][qs10]

    a. El menú desplegable **Anonymous access mode** (Modo de acceso anónimo) determina si los usuarios anónimos pueden acceder a Qlik Sense a través del proxy virtual.  La opción predeterminada es No anonymous user (Ningún usuario anónimo).

    b. El menú desplegable **Authentication method** (Método de autenticación) determina el esquema de autenticación que utilizará el proxy virtual.  Seleccione SAML en la lista desplegable.  Como resultado, aparecerán más opciones.

    c. En el campo **SAML host URI**(identificador URI de host SAML), especifique el nombre de host que tendrán que escribir los usuarios para acceder a Qlik Sense a través de este proxy virtual de SAML.  El nombre de host es el URI del servidor de Qlik Sense.

    d. En el campo **SAML entity ID** (Id. de entidad SAML), escriba el mismo valor que especificó en el campo de URI de host SAML.

    e. **SAML IdP metadata** (Metadatos de IdP de SAML) es el archivo que se editó anteriormente en la sección de **modificación de los metadatos de federación desde la configuración de Azure AD**.  **Antes de cargar los metadatos de IdP, hay que editar el archivo** para quitar información y, de este modo, garantizar que la conexión entre Azure AD y el servidor de Qlik se realice sin problemas.  **Consulte las instrucciones anteriores si aún no ha modificado el archivo.**  Si ya lo ha hecho, haga clic en el botón Examinar y seleccione el archivo de metadatos editado para cargarlo en la configuración de proxy virtual.

    f. Escriba el nombre de atributo o la referencia de esquema del atributo SAML que representa el **UserID** (identificador de usuario) que Azure AD enviará al servidor de Qlik Sense.  La información de referencia del esquema está disponible en las pantallas de la aplicación de Azure que aparecen tras el proceso de configuración.  Para usar el atributo name, escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    g. Escriba el valor del **directorio de usuarios** que se asociará a los usuarios cuando se autentiquen en el servidor de Qlik Sense a través de Azure AD.  Los valores codificados deben estar entre **corchetes**.  Para utilizar un atributo enviado en la aserción SAML de Azure AD, escriba el nombre del atributo en este cuadro de texto **sin** corchetes.

    h. El **algoritmo de firma SAML** establece la firma de certificados del proveedor de servicios (en este caso, el servidor de Qlik Sense) en la configuración de proxy virtual.  Si el servidor de Qlik Sense utiliza un certificado de confianza generado mediante el proveedor de servicios criptográficos AES y RSA mejorado de Microsoft, cambie el algoritmo de firma SAML a **SHA-256**.

    i. La sección de asignación de atributos SAML permite enviar otros atributos, como grupos, a Qlik Sense para utilizarse en reglas de seguridad.

8. Haga clic en la opción de menú **LOAD BALANCING** (Equilibrio de carga) para que se muestre.  Aparecerá la pantalla Equilibrio de carga.

    ![QlikSense][qs11]

9. Haga clic en el botón **Add new server node** (Agregar nuevo nodo de servidor), seleccione los nodos de motor a los que Qlik Sense enviará sesiones para equilibrar la carga y, luego, haga clic en el botón **Add** (Agregar).

    ![QlikSense][qs12]

10. Haga clic en la opción de menú Opciones avanzadas para que se muestre. Aparece la pantalla Opciones avanzadas.

    ![QlikSense][qs13]

    La lista blanca de Host identifica los nombres de host que se aceptan al conectarse al servidor de Qlik Sense.  **Escriba el nombre de host que especificarán los usuarios al conectarse al servidor de Qlik sentido.** El nombre de host es el mismo valor que el URI de host SAML, pero sin "https://".

11. Haga clic en el botón **Apply** (Aplicar).

    ![QlikSense][qs14]

12. Haga clic en Aceptar para aceptar el mensaje de advertencia que indica que se reiniciará el proxy vinculado al proxy virtual.

    ![QlikSense][qs15]

13. En la parte derecha de la pantalla, aparece el menú de elementos Asociado.  Haga clic en la opción de menú **Proxies** (Servidores proxy).

    ![QlikSense][qs16]

14. Aparecerá la pantalla Servidores proxy.  Haga clic en el botón **Link** (Vincular) de la parte inferior para vincular un proxy al proxy virtual.

    ![QlikSense][qs17]

15. Seleccione el nodo de proxy que admitirá esta conexión de proxy virtual y haga clic en el botón **Link** (Vincular).  Cuando termine el proceso de vinculación, el proxy aparecerá debajo de los servidores proxy asociados.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

16. Cuando pasen entre unos cinco y diez segundos, se mostrará el mensaje Actualizar QMC.  Haga clic en el botón **Refresh QMC** (Actualizar QMC).

    ![QlikSense][qs20]

17. Cuando se actualice la consola QMC, haga clic en el elemento de menú **Virtual proxies** (Servidores proxy virtuales). La nueva entrada SAML virtual proxy (Proxy virtual de SAML) se muestra en la tabla que aparece en la pantalla.  Haga clic en la entrada de proxy virtual.

    ![QlikSense][qs51]

18. En la parte inferior de la pantalla, se activará el botón Download SP metadata (Descargar metadatos de SP).  Haga clic en el botón **Download SP metadata** (Descargar metadatos de SP) para guardar los metadatos en un archivo.

    ![QlikSense][qs52]

19. Abra el archivo de metadatos de SP.  Observe las entradas **entityID** y **AssertionConsumerService**.  Estos valores son equivalentes a los de **Identificador**, **URL de inicio de sesión** y **URL de respuesta** de la configuración de la aplicación de Azure AD. Pegue estos valores en la sección **Dominio y direcciones URL de Qlik Sense Enterprise** en la configuración de la aplicación de Azure AD; si no coinciden, se deben reemplazar en el Asistente para configuración de aplicaciones de Azure AD.

    ![QlikSense][qs53]

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Qlik Sense Enterprise.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Qlik Sense Enterprise**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **Qlik Sense Enterprise**.

    ![Vínculo a Qlik Sense Enterprise en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-qlik-sense-enterprise-test-user"></a>Creación de un usuario de prueba de Qlik Sense Enterprise

En esta sección, creará un usuario llamado "Britta Simon" en Qlik Sense Enterprise. Colabore con el  [equipo de soporte técnico de Qlik Sense Enterprise](https://www.qlik.com/us/services/support) para agregar los usuarios en esta plataforma. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Qlik Sense Enterprise del panel de acceso, debería iniciar sesión automáticamente en la aplicación Qlik Sense Enterprise para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

