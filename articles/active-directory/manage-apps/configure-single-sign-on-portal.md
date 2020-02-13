---
title: 'Configuración del inicio de sesión único: Azure Active Directory | Microsoft Docs'
description: Este tutorial usa Azure Portal para configurar el inicio de sesión único basado en SAML para una aplicación con Azure Active Directory (Azure AD).
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 04/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: a415ac749d0d322bc2f71f64d4bec6e32ad1f12e
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77063499"
---
# <a name="how-to-configure-saml-based-single-sign-on"></a>Configuración del inicio de sesión único basado en SAML

Después de agregar una aplicación a las aplicaciones empresariales de Azure AD, configure las opciones de inicio de sesión único. En este artículo se describe cómo configurar el inicio de sesión único basado en SAML para una aplicación que no es de la galería. 

> [!NOTE]
> ¿Desea agregar una aplicación de galería? Consulte las instrucciones de configuración paso a paso en la [lista de tutoriales de aplicaciones SaaS](../saas-apps/tutorial-list.md)

Para configurar un inicio de sesión único para una aplicación que no esté en la galería no *sin escribir código*, debe tener una suscripción o Azure AD Premium, y la aplicación debe ser compatible con SAML 2.0. Para más información acerca de las versiones de Azure AD, visite [Precios de Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="before-you-begin"></a>Antes de empezar

- Si la aplicación no se ha agregado a su inquilino de Azure AD, consulte [Incorporación de una aplicación de la galería](add-gallery-app.md) o [Incorporación de una aplicación que no es de la galería](add-non-gallery-app.md).
- Póngase en contacto con el proveedor de la aplicación para obtener la información correcta sobre las siguientes opciones:

    | Opción de configuración básica de SAML | Iniciado por el proveedor de servicios | Iniciado por IdP | Descripción |
    |:--|:--|:--|:--|
    | Identificador (identificador de entidad) | Obligatorio para algunas aplicaciones | Obligatorio para algunas aplicaciones | Identifica de forma exclusiva la aplicación para la que se configura el inicio de sesión único. Azure AD envía el identificador a la aplicación como el parámetro Audiencia del token SAML. Se espera que la aplicación lo valide. Este valor también aparece como el id. de entidad en los metadatos SAML proporcionados por la aplicación. *Puede encontrar este valor como el elemento **Issuer** en el elemento **AuthRequest** (solicitud SAML) enviado por la aplicación.* |
    | URL de respuesta | Opcional | Obligatorio | Especifica el lugar donde la aplicación espera recibir el token SAML. La dirección URL de respuesta también se conoce como dirección URL del Servicio de consumidor de aserciones (ACS). |
    | URL de inicio de sesión | Obligatorio | No especificar | Cuando un usuario abre esta dirección URL, el proveedor de servicios lo redirige a Azure AD para autenticar el usuario e iniciar sesión. Azure AD usa la dirección URL para iniciar la aplicación desde el Panel de acceso de Azure AD u Office 365. Cuando está en blanco, Azure AD se basa en el proveedor de identidades para comenzar el inicio de sesión único cuando un usuario inicia la aplicación.|
    | Estado de la retransmisión | Opcional | Opcional | Especifica a la aplicación a dónde debe redirigir al usuario una vez completada la autenticación. Normalmente, el valor es una dirección URL válida para la aplicación. Sin embargo, algunas aplicaciones usan este campo de forma diferente. Para más información, pregunte al proveedor de la aplicación.
    | URL de cierre de sesión | Opcional | Opcional | Se usa para devolver las respuestas de cierre de sesión SAML a la aplicación.

## <a name="step-1-edit-the-basic-saml-configuration"></a>Paso 1. Edición de la configuración básica de SAML

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de aplicaciones en la nube o administrador de aplicaciones para el inquilino de Azure AD.

1. Vaya a **Azure Active Directory** > **Aplicaciones empresariales** y seleccione la aplicación en la lista. 
   
   - Para buscar la aplicación, en el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y, después, **Aplicar**. Escriba el nombre de la aplicación en el cuadro de búsqueda y seleccione la aplicación en los resultados.

1. En la sección **Administrar**, seleccione **Inicio de sesión único**. 

1. Seleccione **SAML**. Se muestra la página **Configurar el inicio de sesión único con SAML (versión preliminar)** .

1. Para editar las opciones de configuración básicas de SAML, seleccione el icono **Editar** (un lápiz) en la esquina superior derecha de la sección **Configuración básica de SAML**.

     ![Configuración de certificados](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

1. En los campos correspondientes, escriba la información que se describe en la sección [Antes de empezar](#before-you-begin).

1. En la parte superior de la página, seleccione **Guardar**.

## <a name="step-2-configure-user-attributes-and-claims"></a>Paso 2. Configuración de atributos y notificaciones de usuario 

Una aplicación podría requerir atributos o notificaciones de usuario específicos en el token SAML que recibe de Azure AD cuando un usuario inicia sesión. Por ejemplo, es posible que se requieran valores de URI de notificaciones o valores de notificaciones específicos, o puede que el **Nombre** tenga que ser distinto del nombre de usuario almacenado en la plataforma de identidad de Microsoft. Los requisitos para las aplicaciones de la galería se describen en los [tutoriales específicos de cada aplicación](../saas-apps/tutorial-list.md), o puede solicitar la información al proveedor de la aplicación. A continuación se describen los pasos generales para configurar los atributos y las notificaciones de usuario.

1. En la esquina superior derecha de la sección **Atributos y notificaciones de usuario**, seleccione el icono **Editar** (un lápiz).

1. Compruebe el **Valor de identificador de nombre**. El valor predeterminado es *user.principalname*. El identificador de usuario permite identificar de manera exclusiva a cada usuario dentro de la aplicación. Por ejemplo, si la dirección de correo electrónico es a la vez el nombre de usuario y el identificador único, establezca el valor en *user.mail*.

1. Para modificar el **valor de identificador de nombre**, seleccione el icono **Editar** (un lápiz) del campo **Valor de identificador de nombre**. Realice los cambios que considere necesarios en el formato y el origen del identificador. Para más información, consulte [Edición de NameId](https://docs.microsoft.com/azure/active-directory//develop/active-directory-saml-claims-customization#editing-nameid). Al acabar, guarde los cambios. 
 
1. Para configurar notificaciones de grupo, seleccione el icono **Editar** para el campo **Grupos devueltos en la notificación**. Para más información, consulte [Configuración de notificaciones de grupos](../hybrid/how-to-connect-fed-group-claims.md).

3. Para agregar una notificación, seleccione **Agregar nueva notificación** en la parte superior de la página. Escriba el **nombre** y seleccione el origen adecuado. Si selecciona el origen **Atributo**, deberá elegir el **Atributo de origen** que quiere usar. Si selecciona el origen **Traducción**, deberá elegir los valores de **Transformación** y **Parámetro 1** que quiere usar. Para más información consulte [Incorporación de notificaciones específicas de la aplicación](https://docs.microsoft.com/azure/active-directory//develop/active-directory-saml-claims-customization#adding-application-specific-claims). Al acabar, guarde los cambios. 

4. Seleccione **Guardar**. La nueva notificación aparece en la tabla.

   > [!NOTE]
   > Para más formas de personalizar el token SAML desde Azure AD a la aplicación, consulte los siguientes recursos.
   >- Para crear roles personalizados a través de Azure Portal, consulte [Configuración de notificaciones de rol](../develop/active-directory-enterprise-app-role-management.md).
   >- Para personalizar las notificaciones a través de PowerShell, consulte [Personalización de notificaciones: PowerShell](../develop/active-directory-claims-mapping.md).
   >- Para modificar el manifiesto de aplicación para configurar notificaciones opcionales, consulte [Configuración de notificaciones opcionales](../develop/active-directory-optional-claims.md).
   >- Para establecer directivas de vigencia de los tokens de actualización, tokens de acceso, tokens de sesión y tokens de identificador consulte [Configuración de la vigencia de los tokens](../develop/active-directory-configurable-token-lifetimes.md). O bien, para restringir las sesiones de autenticación a través del acceso condicional de Azure AD, consulte las [funcionalidades de administración de sesiones de autenticación](https://go.microsoft.com/fwlink/?linkid=2083106).

## <a name="step-3-manage-the-saml-signing-certificate"></a>Paso 3. Administración del certificado de firma SAML

Azure AD usa un certificado para firmar los tokens SAML que envía a la aplicación. En la página **Configurar inicio de sesión único con SAML**, puede ver o descargar el certificado activo. También puede actualizar, crear o importar un certificado. En el caso de las aplicaciones de la galería, los detalles sobre el formato del certificado están disponibles en la documentación de SAML de la aplicación (consulte los [tutoriales específicos de la aplicación](../saas-apps/tutorial-list.md)). 

1. Vaya a la sección **Certificado de firma de SAML**. En función del tipo de aplicación, verá opciones para descargar el certificado en formato Base64, formato sin procesar o XML de metadatos de Federación. Azure AD también proporciona la **dirección URL de metadatos de federación de la aplicación**, en donde puede acceder a los `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`metadatos específicos de la aplicación en el formato.

1. Para administrar, crear o importar un certificado, seleccione el icono **Editar** (un lápiz) en la esquina superior derecha de la sección **Certificado de firma de SAML**, y después realice una de las siguientes acciones:

   - Para crear un certificado nuevo, seleccione **Nuevo certificado**, seleccione la **Fecha de expiración** y, a continuación, seleccione **Guardar**. Para activar el certificado, seleccione el menú contextual ( **...** ) y seleccione **Activar el certificado**.
   - Para cargar un certificado con una clave privada y credenciales pfx, seleccione **Importar certificado** y búsquelo. Escriba la **Contraseña PFX**, y seleccione **Agregar**.  
   - Para configurar las opciones avanzadas de firma de certificados, utilice las opciones siguientes. Para ver las descripciones de estas opciones consulte el artículo [Opciones avanzadas de firma de certificado](certificate-signing-options.md).
      - En la lista desplegable **Opción de firma**, seleccione **Firmar respuesta SAML**, **Firmar aserción SAML** o **Firmar respuesta y aserción SAML**.
      - En la lista desplegable **Algoritmo de firma**, elija **SHA-1** o **SHA-256**.
   - Para notificar a otras personas cuando el certificado activo esté cerca de su fecha de expiración, escriba las direcciones de correo electrónico en los campos **Direcciones de correo electrónico de notificación**.

1. En la parte superior de la sección **Certificado de firma de SAML**, seleccione **Guardar**. 

## <a name="step-4-set-up-the-application-to-use-azure-ad"></a>Paso 4. Configuración de la aplicación para que use Azure AD

En la sección **Configurar \<applicationName>** se enumeran los valores que tienen que configurarse en la aplicación para que use Azure AD como proveedor de identidades de SAML. Los valores necesarios varían dependiendo de la aplicación. Para más información, consulte la documentación de SAML de la aplicación.

1. Desplácese hacia abajo hasta la sección **Configurar \<applicationName>** . 
2. Copie el valor de cada fila en esta sección según sea necesario, y siga las instrucciones específicas de la aplicación para agregar el valor a la aplicación. En el caso de las aplicaciones de la galería, puede ver la documentación seleccionando **Ver instrucciones detalladas**. 
   - Los valores **Dirección URL de inicio de sesión** y **URL de cierre de sesión** se resuelven en el mismo punto de conexión, que es el punto de conexión de control de solicitudes SAML de su instancia de Azure AD. 
   - **Identificador de Azure AD** es el valor del **Emisor** en el token SAML emitido a la aplicación.
1. Cuando haya pegado todos los valores en los campos correspondientes, seleccione **Guardar**.

## <a name="step-5-validate-single-sign-on"></a>Paso 5. Validación del inicio de sesión único

Ya está listo para probar la configuración y ver si el inicio de sesión único funciona para usted, el administrador.  

1. Abra la configuración de inicio de sesión único para la aplicación. 
2. Desplácese hasta la sección **Validate single sign-on with <applicationName>** (Validar el inicio de sesión único con <applicationName>). Para este tutorial, esta sección se denomina **Configurar GitHub-test**.
3. Seleccione **Probar**. Aparecen las opciones de pruebas.
4. Seleccione **Iniciar sesión en nombre del usuario actual**. 

Si el inicio de sesión se realiza correctamente, está listo para asignar usuarios y grupos a la aplicación SAML.
Si aparece un mensaje de error, realice los pasos siguientes:

1. Copie y pegue los detalles en el cuadro **What does the error look like?** (¿Qué aspecto tiene el error?).

    ![Utilice el cuadro "What does the error look like?" (¿Qué aspecto tiene el error?) para obtener una guía para la resolución.](media/configure-single-sign-on-portal/error-guidance.png)

1. Seleccione **Obtenga instrucciones para la resolución**. Se muestran la causa principal e instrucciones para la resolución.  En este ejemplo, el usuario no se asignó a la aplicación.
1. Lea las instrucciones de resolución y, después, si es posible, solucione el problema.
1. Vuelva a ejecutar la prueba hasta que se complete correctamente.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de usuarios y grupos a la aplicación](methods-for-assigning-users-and-groups.md)
- [Configuración del aprovisionamiento automático de cuentas de usuario](../app-provisioning/configure-automatic-user-provisioning-portal.md)
