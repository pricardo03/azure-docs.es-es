---
title: 'Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS) | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e377cf749119c23d37bb4db8ab78abb1ce8c82ae
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199755"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS)

En este tutorial, obtendrá información sobre cómo integrar Amazon Web Services (AWS) con Azure Active Directory (Azure AD).
La integración de Amazon Web Services (AWS) con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Amazon Web Services (AWS).
* Puede permitir que los usuarios inicien sesión automáticamente en Amazon Web Services (AWS) (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Puede configurar varios identificadores para varias instancias de la manera siguiente.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Con estos valores, Azure AD quitará el valor de **#** y enviará el valor correcto `https://signin.aws.amazon.com/saml` como dirección URL del público en el token de SAML.

**Se recomienda usar este enfoque por las siguientes razones:**

 a. Cada aplicación le proporcionará el certificado X509 único, luego cada instancia puede tener una fecha de expiración de certificado diferente, que puede administrar según cada cuenta de AWS. En este caso la sustitución general del certificado será fácil.

b. Puede habilitar el aprovisionamiento de usuarios con la aplicación AWS en Azure AD y, luego, nuestro servicio capturará todos los roles de esa cuenta de AWS. No tiene que agregar ni actualizar manualmente los roles de AWS en la aplicación.

c. Puede asignar individualmente el propietario de la aplicación, quien puede administrar la aplicación directamente en Azure AD.

> [!Note]
> Asegurarse de usar solo aplicaciones de la galería

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Amazon Web Services (AWS), necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en Amazon Web Services (AWS)

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

> [!Note]
> Si desea integrar varias cuentas de AWS en una cuenta de Azure para el inicio de sesión único, consulte [este](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) artículo.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Amazon Web Services (AWS) admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adición de Amazon Web Services (AWS) desde la galería

Para configurar la integración de Amazon Web Services (AWS) en Azure AD, es preciso agregar Amazon Web Services (AWS) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Amazon Web Services (AWS) desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Amazon Web Services (AWS)**, seleccione **Amazon Web Services (AWS)** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Amazon Web Services (AWS) en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS) con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Amazon Web Services (AWS).

Para configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Amazon Web Services (AWS)](#configure-amazon-web-services-aws-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Amazon Web Services (AWS)](#create-amazon-web-services-aws-test-user)**: para tener un homólogo de Britta Simon en Amazon Web Services (AWS) que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Amazon Web Services (AWS)**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Cuando se configure más de una instancia, proporcione el valor del identificador. Desde la segunda instancia en adelante, proporcione el valor de identificador en el siguiente formato: Use un signo **#** para especificar un valor único de SPN.

    `https://signin.aws.amazon.com/saml#2`

    ![Información de dominio y direcciones URL de inicio de sesión único de Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_identifier.png)

6. La aplicación Amazon Web Services (AWS) espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE  | Atributo de origen  | Espacio de nombres |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "proporcione un valor comprendido entre 900 segundos (15 minutos) y 43200 segundos (12 horas)" |  https://aws.amazon.com/SAML/Attributes |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En el cuadro de texto **Espacio de nombres**, escriba el valor del espacio de nombres que se muestra para esa fila.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

9. En la sección **Set up Amazon Web Services (AWS)** (Configurar Amazon Web Services [AWS]), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>Configuración del inicio de sesión único en Amazon Web Services (AWS)

1. En otra ventana del explorador, inicie sesión en su sitio de la compañía de Amazon Web Services (AWS) como administrador.

2. Haga clic en **AWS Home** (Página principal de AWS).

    ![Configurar página principal de inicio de sesión único][11]

3. Haga clic en **Administración de identidades y acceso**.

    ![Configurar identidad de inicio de sesión único][12]

4. Haga clic en **Proveedores de identidades** y, después, en **Create Provider** (Crear proveedor).

    ![Configurar proveedor de inicio de sesión único][13]

5. En la página de diálogo **Configure Provider** (Configurar proveedor), realice los pasos siguientes:

    ![Configurar cuadro de diálogo de inicio de sesión único][14]

     a. En **Tipo de proveedor**, seleccione **SAML**.

    b. En el cuadro de texto **Provider Name** (Nombre de proveedor), escriba un nombre de proveedor (por ejemplo, *WAAD*).

    c. Para cargar el **archivo de metadatos** descargado de Azure Portal, haga clic en **Choose file** (Elegir archivo).

    d. Haga clic en **Siguiente paso**.

6. En la página de diálogo **Verify Provider Information** (Comprobar la información del proveedor), haga clic en **Crear**.

    ![Configurar verificación de inicio de sesión único][15]

7. Haga clic en **Roles** y, después, en **Crear rol**.

    ![Configurar roles de inicio de sesión único][16]

8. En la página **Crear rol**, realice los pasos siguientes:  

    ![Configurar confianza de inicio de sesión único][19]

     a. Seleccione **SAML 2.0 federation** (Federación de SAML 2.0) en **Select type of trusted entity** (Seleccionar tipo de entidad de confianza).

    b. En la sección **Choose a SAML 2.0 Provider** (Elegir un proveedor de SAML 2.0), seleccione **SAML provider** (Proveedor de SAML) que ha creado anteriormente (por ejemplo, *WAAD*)

    c. Seleccione **Allow programmatic and AWS Management Console access** (Permitir acceso mediante programación y a consola de AWS Management Console).
  
    d. Haga clic en **Siguiente: Permisos**.

9. En el cuadro de diálogo **Attach Permissions Policies** (Adjuntar directivas de permisos), adjunte la directiva adecuada según su organización. Haga clic en **Siguiente: Review** (Siguiente: revisar).  

    ![Configurar directiva de inicio de sesión único][33]

10. En el cuadro de diálogo **Revisar** , realice los pasos siguientes:

    ![Configurar revisión de inicio de sesión único][34]

     a. En el cuadro de texto **Role name** (Nombre de rol), escriba su nombre de rol.

    b. En el cuadro de texto **Role description**, escriba la descripción.

    c. Haga clic en **Crear rol**.

    d. Cree tantos roles como sea necesario y asígnelos al proveedor de identidades.

11. Use credenciales de cuenta de servicio de AWS para obtener los roles de cuenta de AWS en aprovisionamiento de usuarios de Azure AD. Para ello, abra la página principal de la consola de AWS.

12. Haga clic en **Servicios** -> **Seguridad, identidad y cumplimiento** -> **IAM**.

    ![obtención de roles de la cuenta de AWS](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Seleccione la pestaña **Tabs** (Directivas) en la sección IAM.

    ![obtención de roles de la cuenta de AWS](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Para crear una nueva directiva, haga clic en **Create policy** (Crear directiva) para recuperar los roles de la cuenta de AWS de aprovisionamiento de usuarios de Azure AD.

    ![Creación de una nueva directiva](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Cree su propia directiva para obtener todos los roles de las cuentas de AWS con estos pasos:

    ![Creación de una nueva directiva](./media/amazon-web-service-tutorial/policy1.png)

     a. En la sección **"Crear directiva"**, haga clic en la pestaña **"JSON"**.

    b. En el documento de la directiva, agregue el siguiente JSON.

    ```

    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",

    "Action": [

    "iam:ListRoles"

    ],

    "Resource": "*"

    }

    ]

    }

    ```

    c. Haga clic en el botón **Revisar directiva** para validar la directiva.

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy5.png)

16. Defina la **directiva nueva** con estos pasos:

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy2.png)

     a. En **Policy Name** (Nombre de la directiva), especifique **AzureAD_SSOUserRole_Policy**.

    b. En **Description** (Descripción), puede describir la directiva como **Esta directiva permitirá obtener los roles de cuentas de AWS**.

    c. Haga clic en el botón **"Crear directiva"**.

17. Cree una nueva cuenta de usuario en el servicio IAM de AWS mediante los pasos siguientes:

     a. Haga clic en el panel de navegación **Users** (Usuarios) en la consola de IAM de AWS.

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy3.png)

    b. Haga clic en el botón **Add user** (Agregar usuario) para crear un nuevo usuario.

    ![Agregar usuario](./media/amazon-web-service-tutorial/policy4.png)

    c. En la sección **Add user** (Agregar usuario), lleve a cabo estos pasos:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser1.png)

    * Escriba el nombre de usuario como **AzureADRoleManager**.

    * En el tipo de acceso, seleccione la opción **Programmatic access** (Acceso mediante programación). De este modo, el usuario puede llamar a las API y obtener los roles de la cuenta de AWS.

    * Haga clic en el botón **Next Permissions** (Permisos siguientes) en la esquina inferior derecha.

18. Ahora cree una nueva directiva para este usuario mediante los pasos siguientes:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser2.png)

     a. Haga clic en el botón **Attach existing policies directly** (Asociar directivas existentes directamente).

    b. Busque la directiva recién creada en la sección de filtro **AzureAD_SSOUserRole_Policy**.

    c. Seleccione la **directiva** y, a continuación, haga clic en el botón **Next: Review** (Siguiente: Revisión).

19. Revise la directiva del usuario asociado mediante los pasos siguientes:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser3.png)

     a. Revise el nombre de usuario, el tipo de acceso y la directiva asociada al usuario.

    b. Haga clic en el botón **Create user** (Crear usuario) situado en la esquina inferior derecha para crear el usuario.

20. Descargue las credenciales de usuario de un usuario mediante estos pasos:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser4.png)

     a. Copie el **identificador de la clave de acceso** y la **clave de acceso secreta** del usuario.

    b. Escriba estas credenciales en la sección de aprovisionamiento de usuarios de Azure AD para obtener los roles de la consola de AWS.

    c. Haga clic en el botón **Close** (Cerrar) en la parte inferior.

21. Vaya a la sección **Aprovisionamiento de usuarios** de la aplicación Amazon Web Services en el Portal de administración de Azure AD.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning.png)

22. Escriba la **clave de acceso** y la **clave secreta** en los campos **Secreto de cliente** y **Token secreto** respectivamente.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning1.png)

     a. Escriba la clave de acceso de usuario de AWS en el campo **Secreto de cliente**.

    b. Escriba el secreto de usuario de AWS en el campo **Token secreto**.

    c. Haga clic en el botón **Probar conexión**. Al hacerlo, debería poder probar correctamente esta conexión.

    d. Haga clic en el botón **Guardar** situado en la parte superior para guardar la configuración.

23. Ahora, asegúrese de establecer el estado de aprovisionamiento en **Activado** en la sección de configuración, para lo que debe activar el conmutador y, a continuación, hacer clic en el botón **Guardar** situado en la parte superior.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Amazon Web Services (AWS).

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales**, **Todas las aplicaciones** y **Amazon Web Services (AWS)**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **Amazon Web Services (AWS)**.

    ![En la lista de aplicaciones, seleccione el vínculo de Amazon Web Services (AWS).](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-amazon-web-services-aws-test-user"></a>Creación de un usuario de prueba de Amazon Web Services (AWS)

El objetivo de esta sección es crear un usuario llamado Britta Simon en Amazon Web Services (AWS). Amazon Web Services (AWS) no necesita que se cree un usuario en su sistema para el inicio de sesión único, por lo que no es necesario realizar ninguna acción aquí.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Amazon Web Services (AWS) en el Panel de acceso, automáticamente iniciará sesión en Amazon Web Services (AWS) para el que se configura el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Problemas conocidos

 * En la sección **Aprovisionamiento**, en la subsección **Asignaciones** aparece un mensaje del tipo "Cargando..." y no se muestran las asignaciones de atributos. El único flujo de trabajo de aprovisionamiento que se admite actualmente es la importación de roles desde AWS a Azure AD para su selección durante la asignación de usuarios y grupos. Las asignaciones de atributos para esto vienen predeterminadas y no se pueden configurar.
 
 * La sección **Aprovisionamiento** solo admite escribir un conjunto de credenciales para un inquilino de AWS cada vez. Todos los roles importados se escriben en la propiedad appRoles del [objeto servicePrincipal](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) de Azure AD para el inquilino de AWS. Se pueden agregar varios inquilinos de AWS (representados por objetos servicePrincipal) a Azure AD desde la galería para el aprovisionamiento, pero hay un problema conocido que impide escribir automáticamente todos los roles importados desde los múltiples objetos servicePrincipal de AWS que se usaron para el aprovisionamiento en el objeto servicePrincipal único empleado para el inicio de sesión único. Una posible solución alternativa consiste en usar [Microsoft Graph API](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) para extraer todos los objetos appRoles importados en cada objeto servicePrincipal de AWS en los que esté configurado el aprovisionamiento. Estas cadenas de roles se pueden agregar posteriormente al objeto servicePrincipal de AWS donde esté configurado el inicio de sesión único.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
