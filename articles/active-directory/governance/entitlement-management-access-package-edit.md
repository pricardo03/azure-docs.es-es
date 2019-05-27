---
title: 'Editar y administrar un paquete existente de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo editar y administrar un paquete existente de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/16/2019
ms.author: rolyon
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce51f4df50de50cef06bba699ca7c6f76bc5d166
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833315"
---
# <a name="edit-and-manage-an-existing-access-package-in-azure-ad-entitlement-management-preview"></a>Editar y administrar un paquete existente de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Un paquete de acceso le permite realizar una instalación única de los recursos y las directivas que administra automáticamente acceso durante la vida del paquete de acceso. Como un administrador de paquetes de acceso, puede cambiar los recursos en un paquete de acceso en cualquier momento sin preocuparse sobre el aprovisionamiento de acceso del usuario a los nuevos recursos o quitar el acceso de los recursos anteriores. Las directivas también se pueden actualizar en cualquier momento, sin embargo, los cambios de directiva solo afectan a los accesos de nuevo.

Este artículo describe cómo editar y administrar paquetes de acceso existentes.

## <a name="add-resource-roles"></a>Agregar roles de recursos

Un rol de recursos es una colección de permisos asociados a un recurso. La manera en que hacer que los recursos disponibles para los usuarios soliciten es mediante la adición de roles de recursos para el paquete de acceso. Puede agregar roles de recursos para los grupos, aplicaciones y sitios de SharePoint.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. En el menú izquierdo, haga clic en **roles de recursos**.

1. Haga clic en **agregar roles de recursos** para abrir el agregar roles de recursos para obtener acceso a la página del paquete.

    ![Acceder al paquete: adición de roles de recursos](./media/entitlement-management-access-package-edit/resource-roles-add.png)

1. Dependiendo de si desea agregar un grupo, aplicación o sitio de SharePoint, realice los pasos de una de las siguientes secciones de roles de recursos.

### <a name="add-a-group-resource-role"></a>Agregar un rol de grupo de recursos

Agregar automáticamente usuarios a un grupo cuando se asignan un paquete de acceso puede tener administración de derechos. 

- Cuando un grupo forma parte de un paquete de acceso y un usuario se asigna a ese paquete de acceso, el usuario se agrega a ese grupo, si todavía no están presentes.
- Cuando expira la asignación de paquetes de acceso de un usuario, se quitan del grupo, a menos que tengan actualmente una asignación a otro paquete de acceso que incluye ese mismo grupo.

Puede seleccionar cualquier grupo de Office 365 o el grupo de seguridad de Azure AD.  Los administradores pueden agregar cualquier grupo a un catálogo; los propietarios de catálogo pueden agregar cualquier grupo en el catálogo si es propietario del grupo. Tenga las siguientes restricciones de Azure AD en cuenta al seleccionar un grupo:

- Cuando un usuario, incluidos a un invitado, se agrega como miembro a un grupo, pueden ver a todos los miembros de ese grupo.
- Azure AD no puede cambiar la pertenencia de un grupo que se ha sincronizado desde Windows Server Active Directory con Azure AD Connect.  
- No se puede actualizar la pertenencia a grupos dinámicos mediante la adición o eliminación de un miembro, por lo que las pertenencias a grupos dinámicos no son adecuadas para su uso con la administración de derechos.

1. En el **agregar roles de recursos para acceder al paquete** página, haga clic en **grupos** para abrir el panel de grupos seleccionados.

1. Seleccione los grupos que van a incluir en el paquete de acceso.

    ![Acceder al paquete: adición de roles de recursos: seleccionar grupos](./media/entitlement-management-access-package-edit/group-select.png)

1. Haga clic en **Seleccionar**.

1. En el **rol** lista, seleccione **propietario** o **miembro**.

    Normalmente, se selecciona la función miembro. Si selecciona el rol de propietario, que permitirá a los usuarios agregar o quitar otros miembros o los propietarios.

    ![Acceder al paquete: adición de roles de recursos para un grupo](./media/entitlement-management-access-package-edit/group-role.png)

1. Haga clic en **Agregar**.

    Los usuarios con las asignaciones existentes para el paquete de acceso se convertirá automáticamente en miembros de este grupo cuando se agrega.

### <a name="add-an-application-resource-role"></a>Agregar un rol de recursos de aplicación

Puede tener Azure AD y asignar automáticamente a los usuarios acceso a una aplicación empresarial de Azure AD, incluidos las aplicaciones SaaS y aplicaciones de su organización federadas con Azure AD, cuando un usuario está asignado a un paquete de acceso. Para las aplicaciones que se integran con Azure AD a través de un inicio de sesión único federado, Azure AD emitirá tokens de federación para los usuarios asignados a la aplicación.

Las aplicaciones pueden tener varios roles. Al agregar una aplicación a un paquete de acceso, si esa aplicación tiene más de un rol, deberá especificar el rol adecuado para esos usuarios.  Si está desarrollando aplicaciones, puede leer más sobre estos roles se proporcionan a las aplicaciones en el artículo sobre cómo [configurar la notificación de rol emitida en el token SAML](../develop/active-directory-enterprise-app-role-management.md).

Una vez que un rol de aplicación es parte de un paquete de acceso:

- Cuando un usuario está asignado ese paquete de acceso, el usuario se agrega a ese rol de aplicación, si no está presente.
- Cuando expira la asignación de paquetes de acceso de un usuario, su acceso se quitará de la aplicación, a menos que tengan una asignación a otro paquete de acceso que incluya ese rol de aplicación.

Estas son algunas consideraciones al seleccionar una aplicación:

- Las aplicaciones también pueden tener grupos asignados a sus roles también.  Puede elegir agregar un grupo en lugar de un rol de aplicación en un paquete de acceso, sin embargo, a continuación, la aplicación no estará visible para el usuario como parte del paquete en el portal de acceso a mi acceso.

1. En el **agregar roles de recursos para acceder al paquete** página, haga clic en **aplicaciones** para abrir el panel de selección de aplicaciones.

1. Seleccione las aplicaciones que van a incluir en el paquete de acceso.

    ![Acceder al paquete: adición de roles de recursos: seleccione las aplicaciones](./media/entitlement-management-access-package-edit/application-select.png)

1. Haga clic en **Seleccionar**.

1. En el **rol** lista, seleccione un rol de aplicación.

    ![Acceder al paquete: adición de roles de recursos para una aplicación](./media/entitlement-management-access-package-edit/application-role.png)

1. Haga clic en **Agregar**.

    Los usuarios con las asignaciones existentes al paquete acceso automáticamente se otorgará acceso a esta aplicación cuando se agrega.

### <a name="add-a-sharepoint-site-resource-role"></a>Agregar un rol de recursos de sitio de SharePoint

Azure AD puede asignar automáticamente a los usuarios acceso a un sitio de SharePoint Online o una colección de sitios de SharePoint Online cuando se asignan un paquete de acceso.

1. En el **agregar roles de recursos para acceder al paquete** página, haga clic en **sitios de SharePoint** para abrir el panel de sitios Seleccione SharePoint Online.

1. Seleccione los sitios de SharePoint Online que van a incluir en el paquete de acceso.

    ![Acceder al paquete: adición de roles de recursos - sitios Seleccione SharePoint Online](./media/entitlement-management-access-package-edit/sharepoint-site-select.png)

1. Haga clic en **Seleccionar**.

1. En el **rol** lista, seleccione un rol de sitio de SharePoint Online.

    ![Acceder al paquete: adición de roles de recursos para un sitio de SharePoint Online](./media/entitlement-management-access-package-edit/sharepoint-site-role.png)

1. Haga clic en **Agregar**.

    Los usuarios con las asignaciones existentes al paquete acceso automáticamente se otorgará acceso a este sitio de SharePoint Online cuando se agrega.

## <a name="remove-resource-roles"></a>Quitar roles de recursos

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. En el menú izquierdo, haga clic en **roles de recursos**.

1. En la lista de roles de recursos, busque el rol de recurso que desea quitar.

1. Haga clic en el botón de puntos suspensivos (**...** ) y, a continuación, haga clic en **quitar roles de recursos**.

    Los usuarios con las asignaciones existentes para el paquete de acceso tendrán automáticamente su acceso denegado a este rol de recursos cuando se elimina.

## <a name="add-a-new-policy"></a>Agregar una directiva

Es la manera de especificar quién puede solicitar un paquete de acceso crear una directiva. Puede crear varias directivas para un paquete único acceso si desea permitir que los distintos conjuntos de usuarios que se deben conceder las asignaciones con aprobación diferentes y valores de expiración. No se puede usar una sola directiva para asignar a usuarios internos y externos para el mismo paquete de acceso. Sin embargo, puede crear dos directivas en el mismo paquete de acceso: uno para los usuarios internos y otro para usuarios externos. Si hay varias directivas que se aplican a un usuario, se pedirá en el momento de su solicitud para seleccionar la directiva que les gustaría que se asignará a.

El siguiente diagrama muestra el proceso de alto nivel para crear una directiva para un paquete de acceso existente.

![Crear un proceso de directiva](./media/entitlement-management-access-package-edit/policy-process.png)

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. Haga clic en **directivas** y, a continuación, **Agregar directiva**.

1. Escriba un nombre y una descripción para la directiva.

    ![Crear directiva con nombre y descripción](./media/entitlement-management-access-package-edit/policy-name-description.png)

1. Según su selección para **a los usuarios pueden solicitar acceso**, realice los pasos de una de las siguientes secciones de la directiva.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="edit-an-existing-policy"></a>Editar una directiva existente

Puede editar una directiva en cualquier momento. Si cambia la fecha de expiración para una directiva, la fecha de expiración para las solicitudes que ya están en una aprobación pendiente o aprobado estado no cambiará.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. Haga clic en **directivas** y, a continuación, haga clic en la directiva que desea editar.

    El **detalles de la directiva** panel se abre en la parte inferior de la página.

    ![Paquete de Access - panel de detalles de directiva](./media/entitlement-management-access-package-edit/policy-details.png)

1. Haga clic en **editar** para editar la directiva.

    ![Paquete de acceso - directiva de edición](./media/entitlement-management-access-package-edit/policy-edit.png)

1. Cuando termine, haga clic en **actualización**.

## <a name="directly-assign-a-user"></a>Asignar directamente un usuario

En algunos casos, puede asignar directamente a usuarios específicos a un paquete de acceso para que los usuarios no tienen que pasar por el proceso de solicitar el paquete de acceso. Para asignar directamente a los usuarios, el paquete de acceso debe tener una directiva que permita las asignaciones directas de administrador.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. En el menú izquierdo, haga clic en **asignaciones**.

1. Haga clic en **nueva asignación** para abrir Agregar usuario al paquete de acceso.

    ![Agregar asignaciones: usuario al paquete de acceso](./media/entitlement-management-access-package-edit/assignments-add-user.png)

1. Haga clic en **agregar usuarios** para seleccionar los usuarios que desea asignar el paquete de acceso.

1. En el **seleccione Directiva** lista, seleccione una directiva que tiene el [ninguno (sólo en la asignaciones directas de administrador)](#policy-none-administrator-direct-assignments-only) configuración.

    Si este paquete de acceso no tiene este tipo de directiva, haga clic en **crear nueva directiva** para agregar uno.

1. Establecer la fecha y hora que desea que la asignación de los usuarios seleccionados que comience y finalice. Si no se proporciona una fecha de finalización, se utilizará la configuración de expiración de la directiva.

1. Opcionalmente, proporcione una justificación para la asignación directa para mantener un registro.

1. Haga clic en **agregar** para asignar directamente los usuarios seleccionados al paquete acceso.

    Transcurridos unos instantes, haga clic en **actualizar** para ver los usuarios en la lista de asignaciones.

## <a name="view-who-has-an-assignment"></a>Ver quién tiene una asignación

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. Haga clic en **asignaciones** para ver una lista de las asignaciones activas.

1. Haga clic en una asignación específica para ver detalles adicionales.

1. Para ver una lista de asignaciones que no tenía aprovisionados correctamente todos los roles de recursos, haga clic en el estado del filtro y seleccione **entregando**.

    Puede ver detalles adicionales sobre los errores de entrega localizando la solicitud del usuario correspondiente en el **solicitudes** página.

1. Para ver las asignaciones expiradas, haga clic en el estado del filtro y seleccione **expirado**.

1. Para descargar un archivo CSV de la lista filtrada, haga clic en **descargar**.

## <a name="view-requests"></a>Ver solicitudes

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. Haga clic en **solicitudes**.

1. Haga clic en una solicitud específica para ver detalles adicionales.

## <a name="view-a-requests-delivery-errors"></a>Ver errores de entrega de una solicitud

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. Haga clic en **solicitudes**.

1. Seleccione la solicitud que desea ver.

    Si la solicitud tiene errores de entrega, el estado de la solicitud será **sin entregar** y será el subestado **entregados parcialmente**.

    Si hay errores de entrega, en el panel de detalles de la solicitud, habrá un recuento de errores de entrega.

1. Haga clic en el recuento para ver todos los errores de entrega de la solicitud.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Sólo se puede cancelar una solicitud pendiente que aún no se ha entregado.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. Haga clic en **solicitudes**.

1. Haga clic en la solicitud que desea cancelar

1. En el panel de detalles de la solicitud, haga clic en **solicitud de cancelación**.

## <a name="copy-my-access-portal-link"></a>Copie el vínculo del portal Mi acceso

La mayoría de los usuarios en el directorio pueden iniciar sesión en el portal de mi acceso y para ver una lista de paquetes de acceso pueden solicitar automáticamente. Sin embargo, para usuarios de asociados empresariales externos que aún no están en el directorio, deberá enviarles un vínculo que pueden usar para solicitar un paquete de acceso. Siempre que el paquete de acceso está habilitado para los usuarios externos y tener una directiva para el directorio del usuario externo, el usuario externo puede utilizar el vínculo del portal Mi acceso para solicitar el paquete de acceso.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. En la página información general, copie el **vínculo del portal Mi acceso**.

    ![Introducción al paquete acceso - vínculo del portal Mi acceso](./media/entitlement-management-shared/my-access-portal-link.png)

1. Enviar por correo electrónico o enviar un vínculo a su socio comercial externo. Pueden compartir el vínculo con sus usuarios para solicitar el paquete de acceso.

## <a name="change-the-hidden-setting"></a>Cambiar el valor oculto

Los paquetes de acceso son reconocibles de forma predeterminada. Esto significa que si una directiva permite que un usuario que solicite el paquete de acceso, aparecerá automáticamente el paquete de acceso aparece en el portal de mi acceso.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. En la página de introducción, haga clic en **editar**.

1. Establecer el **Hidden** configuración.

    Si establece en **No**, el paquete de acceso se mostrará en el portal del usuario Mi acceso.

    Si establece en **Sí**, el paquete de acceso no se mostrarán en el portal del usuario Mi acceso. La única manera que un usuario puede ver el paquete de acceso es si tienen la directa **vínculo del portal Mi acceso** al paquete acceso.

## <a name="delete"></a>Eliminar

Solo se puede eliminar un paquete de acceso si no tiene ninguna asignación de usuarios activos.

**Función de requisitos previos:** Usuario administrador, el propietario del catálogo o administrador de paquetes de acceso

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes** y, a continuación, abra el paquete de acceso.

1. En el menú izquierdo, haga clic en **asignaciones** y quitar el acceso para todos los usuarios.

1. En el menú izquierdo, haga clic en **Introducción** y, a continuación, haga clic en **eliminar**.

1. En el mensaje de eliminación que aparece, haga clic en **Sí**.

## <a name="when-are-changes-applied"></a>Cuando se aplican los cambios

En administración de derechos, Azure AD a procesar los cambios de forma masiva para la asignación y los recursos en los paquetes de acceso varias veces al día. Por lo tanto, si se realizan una asignación o cambiar los roles de recursos del paquete de acceso, puede tardar hasta 24 horas para que dicho cambio se realiza en Azure AD, además de la cantidad de tiempo se tarda en propagar los cambios a otros servicios en línea de Microsoft o aplicación de SaaS conectada s. Si el cambio afecta a unos pocos objetos, el cambio probablemente tardará sólo unos minutos en aplicarse en Azure AD, tras el cual otros componentes de Azure AD, a continuación, detecta que cambiar y actualizar las aplicaciones SaaS. Si el cambio afecta a miles de objetos, el cambio tardará más tiempo. Por ejemplo, si tiene un paquete de acceso con 2 aplicaciones y 100 asignaciones de usuario y decide agregar un rol de sitio de SharePoint para el paquete de acceso, puede haber un retraso hasta que todos los usuarios forman parte de ese rol de sitio de SharePoint. Puede supervisar el progreso a través de los registros de auditoría del sitio de SharePoint, el registro de aprovisionamiento de Azure AD y el registro de auditoría de Azure AD.

## <a name="next-steps"></a>Pasos siguientes

- [Solicitar notificaciones de correo electrónico y de proceso](entitlement-management-process.md)
