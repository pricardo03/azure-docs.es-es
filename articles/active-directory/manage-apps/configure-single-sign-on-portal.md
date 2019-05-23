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
ms.openlocfilehash: fa18bc637ec31a1f83b5cab090e008715c5e0c2a
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825011"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>Tutorial: Configuración del inicio de sesión único basado en SAML para una aplicación con Azure Active Directory

Este tutorial usa [Azure Portal](https://portal.azure.com) para configurar el inicio de sesión único basado en SAML para una aplicación con Azure Active Directory (Azure AD). Use este tutorial cuando un [tutorial específico de la aplicación](../saas-apps/tutorial-list.md) no está disponible. 

Este tutorial usa Azure Portal para:

> [!div class="checklist"]
> * Seleccionar el modo de inicio de sesión único basado en SAML
> * Configurar el dominio y las direcciones URL específicos de la aplicación
> * Configurar atributos de usuario
> * Crear un certificado de firma de SAML
> * Asignación de usuarios a la aplicación
> * Configurar la aplicación para el inicio de sesión único basado en SAML
> * Probar la configuración de SAML

## <a name="before-you-begin"></a>Antes de empezar

1. Si la aplicación no se ha agregado a su inquilino de Azure AD, consulte [Inicio rápido: Incorporación de una aplicación a un inquilino de Azure AD](add-application-portal.md).

2. Pregunte al proveedor de la aplicación por la información descrita en [Configuración básica de SAML](#configure-basic-saml-options).

3. Para probar los pasos de este tutorial, use un entorno que no sea de producción. Si no dispone de un entorno de Azure AD que no sea de producción, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

4. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de aplicaciones en la nube o administrador de aplicaciones para el inquilino de Azure AD.

## <a name="select-a-single-sign-on-mode"></a>Selección de un modo de inicio de sesión único

Una vez que haya agregado una aplicación al inquilino de Azure AD, estará listo para configurar el inicio de sesión único para la aplicación.

Para abrir la configuración del inicio de sesión único:

1. En [Azure Portal](https://portal.azure.com), en el panel de navegación izquierdo, seleccione **Azure Active Directory**. 

2. En el panel de navegación de **Azure Active Directory** que aparece, en **Administrar**, seleccione **Aplicaciones empresariales**. Se mostrará una muestra aleatoria de las aplicaciones en su inquilino de Azure AD. 

3. En el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y, después, **Aplicar**.

4. Escriba el nombre de la aplicación para la que desea configurar el inicio de sesión único. Por ejemplo, puede entrar en **GitHub-test** para configurar la aplicación que agregó en el inicio rápido [Agregar aplicación](add-application-portal.md).  

     ![Captura de pantalla que muestra la barra de búsqueda de la aplicación.](media/configure-single-sign-on-portal/azure-portal-application-search.png)

5. Elija la aplicación para la que quiere configurar el inicio de sesión único.

6. En la sección **Administrar**, seleccione **Inicio de sesión único**. 

7. Seleccione **SAML** para configurar el inicio de sesión único. Se muestra la página **Configurar el inicio de sesión único con SAML (versión preliminar)**.

## <a name="configure-basic-saml-options"></a>Configuración básica de SAML

Para configurar el dominio y las direcciones URL:

1. Póngase en contacto con el proveedor de la aplicación para obtener la información correcta sobre las siguientes opciones:

    | Opciones de configuración | Iniciado por el proveedor de servicios | Iniciado por IdP | DESCRIPCIÓN |
    |:--|:--|:--|:--|
    | Identificador (identificador de entidad) | Obligatorio para algunas aplicaciones | Obligatorio para algunas aplicaciones | Identifica de forma exclusiva la aplicación para la que se configura el inicio de sesión único. Azure AD envía el identificador a la aplicación como el parámetro Audiencia del token SAML. Se espera que la aplicación lo valide. Este valor también aparece como el id. de entidad en los metadatos SAML proporcionados por la aplicación.|
    | URL de respuesta | Opcional | Obligatorio | Especifica el lugar donde la aplicación espera recibir el token SAML. La dirección URL de respuesta también se conoce como dirección URL del Servicio de consumidor de aserciones (ACS). |
    | URL de inicio de sesión | Obligatorio | No especificar | Cuando un usuario abre esta dirección URL, el proveedor de servicios lo redirige a Azure AD para autenticar el usuario e iniciar sesión. Azure AD usa la dirección URL para iniciar la aplicación desde el Panel de acceso de Azure AD u Office 365. Cuando está en blanco, Azure AD se basa en el proveedor de identidades para comenzar el inicio de sesión único cuando un usuario inicia la aplicación.|
    | Estado de la retransmisión | Opcional | Opcional | Especifica a la aplicación a dónde debe redirigir al usuario una vez completada la autenticación. Normalmente, el valor es una dirección URL válida para la aplicación. Sin embargo, algunas aplicaciones usan este campo de forma diferente. Para más información, pregunte al proveedor de la aplicación.
    | URL de cierre de sesión | Opcional | Opcional | Se usa para devolver las respuestas de cierre de sesión SAML a la aplicación.


2. Para editar las opciones de configuración básicas de SAML, seleccione el icono **Editar** (un lápiz) en la esquina superior derecha de la sección **Configuración básica de SAML**.

     ![Configuración de certificados](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

3. En los campos correspondientes en la página, escriba la información proporcionada por el proveedor de la aplicación en el paso 1.

4. En la parte superior de la página, seleccione **Guardar**.

## <a name="configure-user-attributes-and-claims"></a>Configuración de atributos y notificaciones de usuario 

Puede controlar qué información envía Azure AD a la aplicación en el token SAML cada vez que un usuario inicia sesión. Para controlar esta información, configure atributos de usuario. Por ejemplo, puede configurar Azure AD para enviar el id. de empleado, el correo electrónico y el nombre del usuario a la aplicación cuando un usuario inicia sesión. 

Estos atributos pueden ser obligatorios u opcionales para hacer que el inicio de sesión único funcione correctamente. Para más información, consulte el [tutorial específico de la aplicación](../saas-apps/tutorial-list.md) o pregunte al proveedor de la aplicación.

1. Para editar los atributos y las notificaciones del usuario, seleccione el icono **Editar** (un lápiz) en la esquina superior derecha de la sección **Atributos y notificaciones de usuario**.

   El **valor de identificador de nombre** se establece con el valor predeterminado de *user.principalname*. El identificador de usuario permite identificar de manera exclusiva a cada usuario dentro de la aplicación. Por ejemplo, si la dirección de correo electrónico es a la vez el nombre de usuario y el identificador único, establezca el valor en *user.mail*.

2. Para modificar el **valor de identificador de nombre**, seleccione el icono **Editar** (un lápiz) del campo **Valor de identificador de nombre**. Realice los cambios que considere necesarios en el formato y el origen del identificador. Al acabar, guarde los cambios. Para más información acerca de cómo personalizar notificaciones, consulte el artículo de procedimientos [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](../develop/active-directory-saml-claims-customization.md).

3. Para agregar una notificación, seleccione **Agregar nueva notificación** en la parte superior de la página. Escriba el **nombre** y seleccione el origen adecuado. Si selecciona el origen **Atributo**, deberá elegir el **Atributo de origen** que quiere usar. Si selecciona el origen **Traducción**, deberá elegir los valores de **Transformación** y **Parámetro 1** que quiere usar.

4. Seleccione **Guardar**. La nueva notificación aparece en la tabla.
 
## <a name="generate-a-saml-signing-certificate"></a>Generación de un certificado de firma de SAML

Azure AD usa un certificado para firmar los tokens SAML que envía a la aplicación. 

1. Para generar un nuevo certificado, seleccione el icono **Editar** (un lápiz) en la esquina superior derecha de la sección **Certificado de firma de SAML**.

2. En la sección **Certificado de firma de SAML**, seleccione **Nuevo certificado**.

3. En la nueva fila de certificado que aparece, establezca la **Fecha de expiración**. Para obtener más información acerca de las opciones de configuración disponibles, consulte el artículo [Opciones avanzadas de firma de certificados](certificate-signing-options.md).

4. En la parte superior de la sección **Certificado de firma de SAML**, seleccione **Guardar**. 

## <a name="assign-users-to-the-application"></a>Asignación de usuarios a la aplicación

Se recomienda probar el inicio de sesión único con varios usuarios o grupos antes de implementar la aplicación en su organización.

> [!NOTE]
>
> Estos pasos le llevan a la sección de configuración **Usuarios y grupos** del portal. Cuando termine, deberá volver a la sección **Inicio de sesión único** para completar el tutorial.

Para asignar un usuario o grupo a la aplicación:

1. Abra la aplicación en el portal, si aún no está abierta.
2. En el panel de navegación izquierdo de la aplicación, seleccione **Usuarios y grupos**.
3. Seleccione **Agregar usuario**.
4. Después, en la sección **Agregar asignación**, seleccione **Usuarios y grupos**.
5. Para buscar un usuario específico, escriba el nombre del usuario en el cuadro **Seleccione un miembro o invite a un usuario externo**. A continuación, seleccione el logotipo o la foto de perfil del usuario y, después, elija **Seleccionar**. 
6. En la sección **Agregar asignación**, seleccione **Asignar**. Cuando haya terminado, los usuarios seleccionados aparecerán en la lista **Usuarios y grupos**.

## <a name="set-up-the-application-to-use-azure-ad"></a>Configuración de la aplicación para que use Azure AD

Ya casi ha terminado.  Como paso final, deberá configurar la aplicación para que use Azure AD como proveedor de identidades SAML. 

1. Desplácese hacia abajo hasta la sección **Configurar<applicationName>**. Para este tutorial, esta sección se denomina **Configurar GitHub-test**. 
2. Copie el valor de cada fila en esta sección. A continuación, pegue cada valor en la fila correspondiente de la sección **Configuración básica de SAML**. Por ejemplo, copie el valor **Dirección URL de inicio de sesión** de la sección **Configurar GitHub-test** y péguelo en el campo **Dirección URL de inicio de sesión** de la sección **Configuración básica de SAML** y así sucesivamente.
3. Cuando haya pegado todos los valores en los campos correspondientes, seleccione **Guardar**.

## <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Ya está todo listo para probar la configuración.  

1. Abra la configuración de inicio de sesión único para la aplicación. 
2. Desplácese hasta la sección **Validate single sign-on with <applicationName>** (Validar el inicio de sesión único con <applicationName>). Para este tutorial, esta sección se denomina **Configurar GitHub-test**.
3. Seleccione **Probar**. Aparecen las opciones de pruebas.
4. Seleccione **Iniciar sesión en nombre del usuario actual**. Esta prueba le permite ver primero si el inicio de sesión único funciona para usted, el administrador.

Si se produce un error, aparece un mensaje de error. Complete los siguientes pasos:

1. Copie y pegue los detalles en el cuadro **What does the error look like?** (¿Qué aspecto tiene el error?).

    ![Obtención de instrucciones para la resolución](media/configure-single-sign-on-portal/error-guidance.png)

2. Seleccione **Obtenga instrucciones para la resolución**. Se muestran la causa principal e instrucciones para la resolución.  En este ejemplo, el usuario no se asignó a la aplicación.

3. Lea las instrucciones de resolución y, después, si es posible, solucione el problema.

4. Vuelva a ejecutar la prueba hasta que se complete correctamente.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha definido la configuración de inicio de sesión único para una aplicación. Después de finalizar la configuración, ha asignado un usuario a la aplicación y configurado esta para que use el inicio de sesión único basado en SAML. Una vez terminadas todas estas operaciones, ha comprobado que el inicio de sesión de SAML funcionaba correctamente.

Hizo todo esto:
> [!div class="checklist"]
> * Seleccionó SAML como modo de inicio de sesión único
> * Se puso en contacto con el proveedor de la aplicación para configurar el dominio y las direcciones URL
> * Configuró los atributos de usuario
> * Creó un certificado de firma de SAML
> * Asignó manualmente usuarios o grupos a la aplicación
> * Configuró la aplicación para que usara Azure AD como proveedor de identidades SAML
> * Probó el inicio de sesión único basado en SAML

Para implementar la aplicación para más usuarios de la organización, use el aprovisionamiento automático de usuarios.

> [!div class="nextstepaction"]
> [Aprenda a asignar usuarios con el aprovisionamiento automático](configure-automatic-user-provisioning-portal.md)


