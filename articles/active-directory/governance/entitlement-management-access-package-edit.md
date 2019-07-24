---
title: Editar y administrar un paquete de acceso en la administración de derechos de Azure AD (versión preliminar) - Azure Active Directory
description: Obtenga información sobre cómo editar y administrar un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.openlocfilehash: 73c1717249abac30847f96073ee6daf7d98112d7
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190347"
---
# <a name="edit-and-manage-an-existing-access-package-in-azure-ad-entitlement-management-preview"></a>Editar y administrar un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Un paquete de acceso le permite realizar una instalación única de los recursos y las directivas que administran automáticamente el acceso durante toda la vida del paquete de acceso. Como administrador de paquetes de acceso, puede cambiar los recursos de un paquete de acceso en cualquier momento sin preocuparse de aprovisionar el acceso del usuario a los nuevos recursos o quitárselo de los recursos anteriores. Las directivas también se pueden actualizar en cualquier momento, pero los cambios de la directiva solo afectan a los nuevos accesos.

En este artículo se describe cómo editar y administrar los paquetes de acceso existentes.

## <a name="add-resource-roles"></a>Agregar roles de recursos

Un rol de recursos es una colección de permisos asociados a un recurso. Para hacer que los usuarios puedan solicitar los recursos es necesario agregar roles de recursos al paquete de acceso. Puede agregar roles de recursos para grupos, aplicaciones y sitios de SharePoint.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En el menú de la izquierda, haga clic en **Roles de recursos**.

1. Haga clic en **Agregar roles de recursos** para abrir la página Agregar roles de recursos al paquete de acceso.

    ![Paquete de acceso - Agregar roles de recursos](./media/entitlement-management-access-package-edit/resource-roles-add.png)

1. En función de si quiere agregar un grupo, una aplicación o un sitio de SharePoint, realice los pasos de una de las siguientes secciones de roles de recursos.

### <a name="add-a-group-resource-role"></a>Agregar un rol de recursos del grupo

Puede hacer que la administración de derechos agregue usuarios automáticamente a un grupo cuando se les asigna un paquete de acceso. 

- Cuando un grupo forma parte de un paquete de acceso y se asigna a un usuario a ese paquete de acceso, se agrega al usuario a ese grupo, si todavía no está presente.
- Cuando la asignación de paquetes de acceso de un usuario expira, se le quita del grupo, a menos que tenga actualmente una asignación a otro paquete de acceso que incluya ese mismo grupo.

Puede seleccionar cualquier grupo de seguridad de Office 365 o de Azure AD.  Los administradores pueden agregar cualquier grupo a un catálogo; los propietarios de catálogos pueden agregar cualquier grupo al catálogo si son los propietarios del grupo. Tenga en cuenta las siguientes restricciones de Azure AD al seleccionar un grupo:

- Cuando se agrega como miembro a un usuario, incluido un invitado, a un grupo, puede ver a los demás miembros de ese grupo.
- Azure AD no puede cambiar la pertenencia de un grupo que se ha sincronizado desde Windows Server Active Directory con Azure AD Connect.  
- No se puede actualizar la pertenencia a grupos dinámicos agregando o eliminando a un miembro, por lo que las pertenencias a grupos dinámicos no son adecuadas para su uso con la administración de derechos.

1. En la página **Agregar roles de recursos al paquete de acceso**, haga clic en **Grupos** para abrir el panel Seleccionar grupos.

1. Seleccione los grupos que quiere incluir en el paquete de acceso.

    ![Paquete de acceso - Agregar roles de recursos - Seleccionar grupos](./media/entitlement-management-access-package-edit/group-select.png)

1. Haga clic en **Seleccionar**.

1. En la lista **Rol**, seleccione **Propietario** o **Miembro**.

    Normalmente se selecciona el rol Miembro. Si selecciona el rol Propietario, permitirá que los usuarios agreguen o quiten a otros miembros o propietarios.

    ![Paquete de acceso - Agregar rol de recursos para un grupo](./media/entitlement-management-access-package-edit/group-role.png)

1. Haga clic en **Agregar**.

    Cuando se agregue, los usuarios con asignaciones existentes para el paquete de acceso se convertirán automáticamente en miembros de este grupo.

### <a name="add-an-application-resource-role"></a>Agregar un rol de recursos de la aplicación

Puede hacer que Azure AD asigne automáticamente a los usuarios acceso a una aplicación empresarial de Azure AD, incluidas tanto las aplicaciones SaaS como las aplicaciones de su organización federadas con Azure AD, cuando se asigne a un usuario un paquete de acceso. Para las aplicaciones que se integran con Azure AD a través de un inicio de sesión único federado, Azure AD emitirá los tokens de federación para los usuarios asignados a la aplicación.

Las aplicaciones pueden tener varios roles. Al agregar una aplicación a un paquete de acceso, si esa aplicación tiene más de un rol, deberá especificar el rol adecuado para esos usuarios.  Si está desarrollando aplicaciones, puede leer más sobre cómo se proporcionan estos roles a las aplicaciones en el artículo sobre los procedimientos para la [configuración de la notificación de rol emitida en el token SAML para aplicaciones empresariales](../develop/active-directory-enterprise-app-role-management.md).

Una vez que un rol de aplicación es parte de un paquete de acceso:

- Cuando se asigna un usuario a ese paquete de acceso, el usuario se agrega a ese rol de aplicación, si todavía no está presente.
- Cuando la asignación de paquetes de acceso de un usuario expira, se le quitará el acceso a la aplicación, a menos que tenga una asignación a otro paquete de acceso que incluya ese mismo rol de aplicación.

Estas son algunas opciones a tener en cuenta cuando seleccione una aplicación:

- Las aplicaciones también pueden tener, además, grupos asignados a sus roles.  Puede elegir agregar un grupo en lugar de un rol de aplicación en un paquete de acceso, pero entonces la aplicación no será visible para el usuario como parte del paquete de acceso en el portal Mi acceso.

1. En la página **Agregar roles de recursos al paquete de acceso**, haga clic en **Aplicaciones** para abrir el panel Seleccionar aplicaciones.

1. Seleccione las aplicaciones que quiere incluir en el paquete de acceso.

    ![Paquete de acceso - Agregar roles de recursos - Seleccionar aplicaciones](./media/entitlement-management-access-package-edit/application-select.png)

1. Haga clic en **Seleccionar**.

1. En la lista **Rol**, seleccione un rol de aplicación.

    ![Paquete de acceso - Agregar rol de recursos para una aplicación](./media/entitlement-management-access-package-edit/application-role.png)

1. Haga clic en **Agregar**.

    Cuando se agregue, se otorgará acceso a esta aplicación a los usuarios con asignaciones existentes para el paquete de acceso.

### <a name="add-a-sharepoint-site-resource-role"></a>Agregar un rol de recursos de sitio de SharePoint

Azure AD puede asignar automáticamente a los usuarios acceso a un sitio de SharePoint Online o una colección de sitios de SharePoint Online cuando se asignan a un paquete de acceso.

1. En la página **Agregar roles de recursos a un paquete de acceso**, haga clic en **Sitios de SharePoint** para abrir el panel Select SharePoint Online sites (Seleccionar sitios de SharePoint Online).

1. Seleccione los sitios de SharePoint Online que quiere incluir en el paquete de acceso.

    ![Paquete de acceso - Agregar roles de recursos - Select SharePoint Online sites (Seleccionar sitios de SharePoint Online)](./media/entitlement-management-access-package-edit/sharepoint-site-select.png)

1. Haga clic en **Seleccionar**.

1. En la lista **Rol**, seleccione un rol de sitio de SharePoint Online.

    ![Paquete de acceso - Agregar un rol de recursos para un sitio de SharePoint Online](./media/entitlement-management-access-package-edit/sharepoint-site-role.png)

1. Haga clic en **Agregar**.

    Cuando se agregue, se otorgará acceso a este sitio de SharePoint Online a los usuarios con asignaciones existentes para el paquete de acceso.

## <a name="remove-resource-roles"></a>Eliminar roles de recursos

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En el menú de la izquierda, haga clic en **Roles de recursos**.

1. En la lista de roles de recursos, busque el rol de recurso que quiere quitar.

1. Haga clic en el botón de puntos suspensivos **...** y luego en **Eliminar rol de recurso**.

    Cuando se elimine, se revocará automáticamente el acceso a este rol de recurso de los usuarios con asignaciones existentes para el paquete de acceso.

## <a name="add-a-new-policy"></a>Agregar una nueva directiva

Para especificar quién puede solicitar un paquete de acceso debe crear una directiva. Puede crear varias directivas para un único paquete de acceso si quiere permitir que se concedan asignaciones a los distintos conjuntos de usuarios con valores de aprobación y expiración diferentes. No se puede usar una sola directiva para asignar usuarios internos y externos al mismo paquete de acceso. Pero puede crear dos directivas en el mismo paquete de acceso: una para los usuarios internos y otra para los usuarios externos. Si hay varias directivas que se aplican a un usuario, se le pedirá en el momento de su solicitud que seleccione la directiva que le gustaría que se le asignara.

En el siguiente diagrama se muestra el proceso general para crear una directiva para un paquete de acceso existente.

![Proceso de creación de una directiva](./media/entitlement-management-access-package-edit/policy-process.png)

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Directivas**  y luego en **Agregar directiva**.

1. Escriba un nombre y una descripción para la directiva.

    ![Crear directiva con nombre y descripción](./media/entitlement-management-access-package-edit/policy-name-description.png)

1. En función de su selección para **Users who can request access** (Usuarios que pueden solicitar acceso), realice los pasos de una de las siguientes secciones de la directiva.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="edit-an-existing-policy"></a>Editar una directiva existente

Puede editar una directiva en cualquier momento. Si cambia la fecha de expiración para una directiva, la fecha de expiración para las solicitudes que ya están aprobadas o en un estado de aprobación pendiente no cambiará.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Directivas** y luego en la directiva que quiere editar.

    Se abre el panel **Detalles de directiva** en la parte inferior de la página.

    ![Paquete de acceso - Panel Detalles de directiva](./media/entitlement-management-access-package-edit/policy-details.png)

1. Haga clic en **Editar** para editar la directiva.

    ![Paquete de acceso - Editar directiva](./media/entitlement-management-access-package-edit/policy-edit.png)

1. Cuando termine, haga clic en **Actualizar**.

## <a name="directly-assign-a-user"></a>Asignar directamente un usuario

En algunos casos, es posible que quiera asignar directamente usuarios específicos a un paquete de acceso para que los usuarios no tengan que pasar por el proceso de solicitar el paquete de acceso. Para asignar directamente usuarios, el paquete de acceso debe tener una directiva que permita las asignaciones directas de administrador.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En el menú de la izquierda, haga clic en **Asignaciones**.

1. Haga clic en **Nueva asignación** para abrir Add user to access package (Agregar usuario al paquete de acceso).

    ![Asignaciones - Add user to access package (Agregar usuario al paquete de acceso)](./media/entitlement-management-access-package-edit/assignments-add-user.png)

1. Haga clic en **Agregar usuarios** para seleccionar los usuarios a los que quiere asignar el paquete de acceso.

1. En la lista **Seleccionar directiva**, seleccione una directiva que tenga la configuración [Ninguno (solo para las asignaciones directas del administrador)](#policy-none-administrator-direct-assignments-only).

    Si este paquete de acceso no tiene este tipo de directiva, haga clic en **Crear nueva directiva** para agregar una.

1. Establezca la fecha y hora en la que quiere que comience y finalice la asignación de los usuarios seleccionados. Si no se proporciona una fecha de finalización, se utilizará la configuración de expiración de la directiva.

1. Opcionalmente, proporcione una justificación para la asignación directa para mantener un registro.

1. Haga clic en **Agregar** para asignar directamente los usuarios seleccionados al paquete acceso.

    Transcurridos unos instantes, haga clic en **Actualizar** para ver los usuarios en la lista Asignaciones.

## <a name="view-who-has-an-assignment"></a>Ver quién tiene una asignación

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Asignaciones** para ver una lista de las asignaciones activas.

1. Haga clic en una asignación específica para ver detalles adicionales.

1. Para ver una lista de asignaciones que no tenían aprovisionados correctamente todos los roles de recursos, haga clic en el filtro de estado y seleccione **Entrega**.

    Puede ver detalles adicionales sobre los errores de entrega localizando la solicitud del usuario correspondiente en la página **Solicitudes**.

1. Para ver las asignaciones expiradas, haga clic en el filtro de estado y seleccione **Expiradas**.

1. Para descargar un archivo CSV de la lista filtrada, haga clic en **Descargar**.

## <a name="view-requests"></a>Ver solicitudes

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Haga clic en una solicitud específica para ver detalles adicionales.

## <a name="view-a-requests-delivery-errors"></a>Visualización de errores de entrega de una solicitud

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Seleccione la solicitud que quiere ver.

    Si la solicitud tiene errores de entrega, el estado de la solicitud será **Undelivered** (Sin entregar) y el subestado será **Entregado parcialmente**.

    Si hay errores de entrega, en el panel de detalles de la solicitud, habrá un recuento de los errores de entrega.

1. Haga clic en el recuento para ver todos los errores de entrega de la solicitud.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Solo se puede cancelar una solicitud pendiente que aún no se haya entregado.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Solicitudes**.

1. Haga clic en la solicitud que quiere cancelar.

1. En el panel de detalles de la solicitud, haga clic en **Cancelar solicitud**.

## <a name="copy-my-access-portal-link"></a>Copiar el vínculo del portal Mi acceso

La mayoría de los usuarios del directorio pueden iniciar sesión en el portal Mi acceso y ver automáticamente una lista de paquetes de acceso que pueden solicitar. Pero para los usuarios de socios comerciales externos que aún no están en el directorio, deberá enviarles un vínculo que pueden usar para solicitar un paquete de acceso. Siempre que el paquete de acceso esté habilitado para los usuarios externos y tenga una directiva para el directorio del usuario externo, este puede utilizar el vínculo del portal Mi acceso para solicitar el paquete de acceso.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En la página Información general, copie el **vínculo del portal Mi acceso**.

    ![Información general del paquete acceso: vínculo del portal Mi acceso](./media/entitlement-management-shared/my-access-portal-link.png)

1. Envíe por correo electrónico o envíe el vínculo a su socio comercial externo. Este puede compartir el vínculo con sus usuarios para solicitar el paquete de acceso.

## <a name="change-the-hidden-setting"></a>Cambiar el valor Oculto

De forma predeterminada, los paquetes de acceso son detectables. Esto significa que si una directiva permite que un usuario solicite el paquete de acceso, verá automáticamente que el paquete de acceso se muestra en el portal Mi acceso.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En la página Información general, haga clic en **Editar**.

1. Configure el valor **Oculto**.

    Si está establecido en **No**, el paquete de acceso se mostrará en el portal Mi acceso del usuario.

    Si está establecido en **Sí**, el paquete de acceso no se mostrará en el portal Mi acceso del usuario. La única manera que un usuario puede ver el paquete de acceso es si tiene el **vínculo directo del portal Mi acceso** al paquete acceso.

## <a name="delete"></a>Eliminar

Solo se puede eliminar un paquete de acceso si no tiene ninguna asignación de usuarios activa.

**Rol necesario:** Administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. En el menú de la izquierda, haga clic en **Asignaciones** y quite el acceso para todos los usuarios.

1. En el menú de la izquierda, haga clic en **Información general** y luego en **Eliminar**.

1. En el mensaje de eliminación que aparece, haga clic en **Sí**.

## <a name="when-are-changes-applied"></a>Cuándo se aplican los cambios

En administración de derechos, Azure AD procesará los cambios de forma masiva para la asignación y los recursos de los paquetes de acceso varias veces al día. Por lo tanto, si realiza una asignación o cambia los roles de recursos del paquete de acceso, puede tardar hasta 24 horas para que dicho cambio se lleve a cabo en Azure AD, además del tiempo que se tarde en propagar los cambios a otras aplicaciones SaaS conectadas o Microsoft Online Services. Si el cambio solo afecta a unos pocos objetos, probablemente tardará solo unos minutos en aplicarse en Azure AD, tras lo cual otros componentes de Azure AD detectarán dicho cambio y actualizarán las aplicaciones SaaS. Si el cambio afecta a miles de objetos tardará más tiempo. Por ejemplo, si tiene un paquete de acceso con 2 aplicaciones y 100 asignaciones de usuario y decide agregar un rol de sitio de SharePoint al paquete de acceso, podría haber un retraso hasta que todos los usuarios formen parte de ese rol de sitio de SharePoint. Puede supervisar el progreso a través de los registros de auditoría de Azure AD, el registro de aprovisionamiento de Azure AD y los registros de auditoría del sitio de SharePoint.

## <a name="next-steps"></a>Pasos siguientes

- [Adición de un propietario del catálogo o de un administrador de paquetes de acceso](entitlement-management-delegate.md#add-a-catalog-owner-or-an-access-package-manager)
- [Solicitud de notificaciones de proceso y correo electrónico](entitlement-management-process.md)
