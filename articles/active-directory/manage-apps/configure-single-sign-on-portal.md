---
title: 'Configuración del inicio de sesión único: Azure Active Directory | Microsoft Docs'
description: Este tutorial usa Azure Portal para configurar el inicio de sesión único basado en SAML para una aplicación con Azure Active Directory (Azure AD).
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 12/06/2018
ms.author: barbkess
ms.reviewer: arvinh,luleon
ms.openlocfilehash: aceacdea8b3c86a5c4f26a5f082f4c6cf0b3805d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011984"
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

2. Pregunte al proveedor de la aplicación por la información descrita en [Configuración del dominio y las direcciones URL](#configure-domain-and-urls).

3. Para probar los pasos de este tutorial, se recomienda usar un entorno que no sea de producción. Si no dispone de un entorno de Azure AD que no sea de producción, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

4. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de aplicaciones en la nube o administrador de aplicaciones para el inquilino de Azure AD.

## <a name="select-a-single-sign-on-mode"></a>Selección de un modo de inicio de sesión único

Una vez que se ha agregado una aplicación al inquilino de Azure AD, usted está listo para configurar el inicio de sesión único para dicha aplicación.

Para abrir la configuración del inicio de sesión único:

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory**. 

2. En la hoja **Azure Active Directory**, haga clic en **Aplicaciones empresariales**. La hoja **Todas las aplicaciones** se abre para mostrar una muestra aleatoria de las aplicaciones en su inquilino de Azure AD. 

3. En el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y haga clic en **Aplicar**.

4. Escriba el nombre de la aplicación para la que desea configurar el inicio de sesión único. Elija su propia aplicación o entre en **GitHub-test** para configurar la aplicación que agregó en el inicio rápido [Agregar aplicación](add-application-portal.md).

5. Haga clic en **Inicio de sesión único**. En **Modo de inicio de sesión único**, **Inicio de sesión basado en SAML** aparece como la opción predeterminada. 

    ![Opciones de configuración](media/configure-single-sign-on-portal/config-options.png)

6. Haga clic en **Guardar** en la parte superior de la hoja. 

## <a name="configure-domain-and-urls"></a>Configuración del dominio y las direcciones URL

Para configurar el dominio y las direcciones URL:

1. Póngase en contacto con el proveedor de la aplicación para obtener la información correcta sobre las siguientes opciones:

    | Opciones de configuración | Iniciado por el proveedor de servicios | Iniciado por IdP | DESCRIPCIÓN |
    |:--|:--|:--|:--|
    | URL de inicio de sesión | Obligatorio | No especificar | Cuando un usuario abre esta dirección URL, el proveedor de servicios lo redirige a Azure AD para autenticar el usuario e iniciar sesión. Azure AD usa la dirección URL para iniciar la aplicación desde el Panel de acceso de Azure AD u Office 365. Cuando está en blanco, Azure AD se basa en el proveedor de identidades para comenzar el inicio de sesión único cuando un usuario inicia la aplicación.|
    | Identificador (identificador de entidad) | Obligatorio para algunas aplicaciones | Obligatorio para algunas aplicaciones | Identifica de forma exclusiva la aplicación para la que se configura el inicio de sesión único. Azure AD envía el identificador a la aplicación como el parámetro Audiencia del token SAML. Se espera que la aplicación lo valide. Este valor también aparece como el id. de entidad en los metadatos SAML proporcionados por la aplicación.|
    | URL de respuesta | Opcional | Obligatorio | Especifica el lugar donde la aplicación espera recibir el token SAML. La dirección URL de respuesta también se conoce como dirección URL del Servicio de consumidor de aserciones (ACS). |
    | Estado de la retransmisión | Opcional | Opcional | Especifica a la aplicación a dónde debe redirigir al usuario una vez completada la autenticación. Por lo general, el valor suele ser una dirección URL válida de la aplicación; sin embargo, algunas aplicaciones usan este campo de forma diferente. Para más información, pregunte al proveedor de la aplicación.

2. Escriba la información. Para ver toda la configuración, haga clic en **Mostrar configuración avanzada de URL**.

    ![Opciones de configuración](media/configure-single-sign-on-portal/config-urls.png)

3. Haga clic en **Guardar** en la parte superior de la hoja.

4. Hay un botón denominado **Probar la configuración de SAML** en esta sección. Ejecute esta prueba más adelante en el tutorial en la sección [Prueba de inicio de sesión único](#test-single-sign-on).

## <a name="configure-user-attributes"></a>Configurar atributos de usuario

Los atributos de usuario le permiten controlar qué información envía Azure AD a la aplicación en el token SAML cada vez que un usuario inicia sesión. Por ejemplo, Azure AD podría enviar el nombre, el correo electrónico y el identificador de empleado del usuario a la aplicación. 

Estos atributos pueden ser obligatorios u opcionales para hacer que el inicio de sesión único funcione correctamente. Para más información, consulte el [tutorial específico de la aplicación](../saas-apps/tutorial-list.md) o pregunte al proveedor de la aplicación.

1. Para ver todas las opciones, haga clic en **Ver y editar todos los demás atributos de usuario**.

    ![Configurar atributos de usuario](media/configure-single-sign-on-portal/config-user-attributes.png)

2. Escriba el **identificador de usuario**.

    El identificador de usuario permite identificar de manera exclusiva a cada usuario dentro de la aplicación. Por ejemplo, si la dirección de correo electrónico es a la vez el nombre de usuario y el identificador único, establezca el valor en *user.mail*.

3. Para obtener más atributos de token SAML, haga clic en **Ver y editar todos los demás atributos de usuario**.

4. Para agregar un atributo a los **atributos de token SAML**, haga clic en **Agregar atributo**. Escriba el **nombre** y seleccione el **valor** del menú.

5. Haga clic en **Save**(Guardar). En la tabla verá el nuevo atributo.
 
## <a name="create-a-saml-signing-certificate"></a>Crear un certificado de firma de SAML

Azure AD usa un certificado para firmar los tokens SAML que envía a la aplicación. 

1. Para ver todas las opciones, haga clic en **Show advanced certificate signing options** (Mostrar opciones avanzadas de firma de certificados).

    ![Configuración de certificados](media/configure-single-sign-on-portal/config-certificate.png)

2. Para configurar un certificado, haga clic en **Crear nuevo certificado**.

3. En la hoja **Crear nuevo certificado**, establezca la **fecha de expiración** y haga clic en **Guardar**.

4. Haga clic en **Make new certificate active** (Activar nuevo certificado).

5. Para más información, consulte [Opciones avanzadas de firma de certificados](certificate-signing-options.md).

6. Para mantener los cambios realizados hasta ahora, asegúrese de hacer clic en **Guardar** en la parte superior de la hoja **Inicio de sesión único**. 

## <a name="assign-users-to-the-application"></a>Asignación de usuarios a la aplicación

Microsoft recomienda probar el inicio de sesión único con varios usuarios o grupos antes de implementar la aplicación en su organización.

Para asignar un usuario o grupo a la aplicación:

1. Abra la aplicación en el portal, si aún no está abierta.
2. En la hoja de aplicación izquierda, haga clic en **Usuarios y grupos**.
3. Haga clic en **Agregar usuario**.
4. En la hoja **Agregar asignación**, haga clic en **Usuarios y grupos**.
5. Para buscar un usuario específico, escriba el nombre de usuario en el cuadro **Seleccionar**, haga clic en la casilla situada junto a la foto o el logotipo del perfil del usuario y haga clic en **Seleccionar**. 
6. Busque el nombre de usuario actual y selecciónelo. Opcionalmente, puede seleccionar varios usuarios.
7. En la hoja **Agregar asignación**, haga clic en **Asignar**. Cuando haya terminado, los usuarios seleccionados aparecerán en la lista **Usuarios y grupos**.

## <a name="configure-the-application-to-use-azure-ad"></a>Configuración de la aplicación para que use Azure AD

Ya casi ha terminado.  Como paso final, deberá configurar la aplicación para que use Azure AD como proveedor de identidades SAML. 

1. Desplácese hacia abajo hasta el final de la hoja **Inicio de sesión único** de su aplicación. 

    ![Configuración de la aplicación](media/configure-single-sign-on-portal/configure-app.png)

2. Haga clic en **Configurar aplicación** en el portal y siga las instrucciones.
3. Cree manualmente cuentas de usuario en la aplicación para probar el inicio de sesión único. Cree las cuentas de usuario que asignó a la aplicación en la [sección anterior](#assign-users-to-the-application). 

## <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Ahora ya estará preparado para probar la configuración.  

1. Abra la configuración de inicio de sesión único para la aplicación. 
2. Desplácese hasta la sección **Configuración del dominio y las direcciones URL**.
2. Haga clic en **Probar la configuración de SAML**. Aparecen las opciones de pruebas.

    ![Probar opciones de inicio de sesión único](media/configure-single-sign-on-portal/test-single-sign-on.png) 

3. Haga clic en **Iniciar sesión en nombre del usuario actual**. Esta prueba le permite ver primero si el inicio de sesión único funciona para usted, el administrador.
4. Si se produce un error, aparece un mensaje de error. Copie y pegue los detalles en el cuadro **What does the error look like?** (¿Qué aspecto tiene el error?).

    ![Obtención de instrucciones para la resolución](media/configure-single-sign-on-portal/error-guidance.png)

5. Haga clic en **Obtenga instrucciones para la resolución**. Se muestran la causa principal e instrucciones para la resolución.  En este ejemplo, el usuario no se asignó a la aplicación.

    ![Corrección de errores](media/configure-single-sign-on-portal/fix-error.png)

6. Lea las instrucciones de resolución y, a continuación, si es necesario, haga clic en **Reparar**.

7. Vuelva a ejecutar la prueba hasta que se complete correctamente.



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

Para implementar la aplicación para más usuarios de la organización, es recomendable usar el aprovisionamiento automático de usuario.

> [!div class="nextstepaction"]
>[Aprenda a asignar usuarios con el aprovisionamiento automático](configure-automatic-user-provisioning-portal.md)


