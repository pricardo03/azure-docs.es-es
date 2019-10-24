---
title: 'Gobernanza del acceso de los usuarios externos en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información acerca de la configuración que puede especificar para controlar el acceso de los usuarios externos en la administración de derechos de Azure Active Directory.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcf4a0272e21a1fba3cf9adbd9158492e4318578
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72452910"
---
# <a name="govern-access-for-external-users-in-azure-ad-entitlement-management-preview"></a>Gobernanza del acceso de los usuarios externos en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La administración de derechos de Azure AD emplea [Azure AD de negocio a negocio (B2B)](../b2b/what-is-b2b.md) para colaborar con personas ajenas a su organización en otro directorio. Con Azure AD B2B, los usuarios externos se autentican en su directorio particular, pero están representados en el directorio. La representación en el directorio permite asignar al usuario acceso a los recursos.

En este artículo se describen los valores que puede especificar para controlar el acceso de los usuarios externos.

## <a name="how-entitlement-management-can-help"></a>Cómo puede ayudar la administración de derechos

Cuando utilice la experiencia de invitación de [Azure AD B2B](../b2b/what-is-b2b.md), ya debe conocer las direcciones de correo electrónico de los usuarios invitados externos que desea incorporar al directorio de recursos y con los que desea trabajar. Esto funciona muy bien cuando está trabajando en un proyecto más pequeño o a corto plazo y ya conoce a todos los participantes, pero es más difícil de administrar si tiene muchos usuarios con los que quiere trabajar o si los participantes cambian con el tiempo.  Por ejemplo, puede estar trabajando con otra organización y tener un punto de contacto con ella, pero con el tiempo otros usuarios de esa organización también necesitarán acceso.

Con la administración de derechos, puede definir una directiva que permita a los usuarios de las organizaciones que especifique poder solicitar un paquete de acceso. Puede especificar si se requiere aprobación y una fecha de expiración para el acceso. Si se requiere aprobación, también puede invitar a uno o varios usuarios de la organización externa a su directorio y designarlos como aprobadores, ya que es probable que sepan qué usuarios externos de su organización necesitan tener acceso. Una vez configurado el paquete de acceso, puede enviar el vínculo a su persona de contacto (patrocinador) en la organización externa. Ese contacto puede compartirlo con otros usuarios de la organización externa y pueden usar este vínculo para solicitar el paquete de acceso. Los usuarios de esa organización que ya han recibido la invitación a su directorio también pueden usar ese vínculo.

Cuando se aprueba una solicitud, la administración de derechos proporciona al usuario el acceso necesario, que puede incluir invitar al usuario si aún no está en el directorio. Azure AD creará automáticamente una cuenta de invitado B2B para ellos. Tenga en cuenta que un administrador puede haber limitado previamente qué organizaciones están permitidas para la colaboración, mediante el establecimiento de una [lista de permitidos y denegados de B2B](../b2b/allow-deny-list.md) para permitir o bloquear las invitaciones a otras organizaciones.  Si el usuario no está autorizado por la lista de permitidos o bloqueados, no se le invitará.

Puesto que no desea que el acceso del usuario externo dure para siempre, especifique una fecha de expiración en la directiva, por ejemplo 180 días. Después de 180 días, si no se amplía el acceso, la administración de derechos eliminará todo el acceso asociado con ese paquete de acceso. De forma predeterminada, si el usuario invitado a través de la administración de derechos no tiene ninguna otra asignación de paquetes de acceso, cuando pierda la última asignación, la cuenta de invitado se bloqueará durante 30 días y posteriormente se eliminará. Esto evita la proliferación de cuentas innecesarias. Tal y como se describe en las secciones siguientes, estos valores se pueden configurar.

## <a name="how-access-works-for-external-users"></a>Cómo funciona el acceso para los usuarios externos

En el diagrama y los pasos siguientes se proporciona información general sobre cómo conceder acceso a los usuarios externos a un paquete de acceso.

![Diagrama que muestra el ciclo de vida de los usuarios externos](./media/entitlement-management-external-users/external-users-lifecycle.png)

1. Se crea un paquete de acceso en el directorio que incluye una directiva [Para usuarios que no están en el directorio](entitlement-management-access-package-create.md#for-users-not-in-your-directory).

1. Envía un [vínculo al portal Mi acceso](entitlement-management-access-package-settings.md) a su contacto en la organización externa que pueden compartir con sus usuarios para solicitar el paquete de acceso.

1. Un usuario externo (**Solicitante A** en este ejemplo) usa el vínculo al portal Mi acceso para [solicitar acceso](entitlement-management-request-access.md) al paquete de acceso.

1. Un aprobador [aprueba la solicitud](entitlement-management-request-approve.md) (o se aprueba automáticamente).

1. La solicitud entra en el estado [en entrega](entitlement-management-process.md).

1. El proceso de invitación B2B crea una cuenta de usuario invitado en el directorio, **Solicitante A (invitado)** en este ejemplo. Si se define una [lista de permitidos o denegados](../b2b/allow-deny-list.md), se aplicará la configuración de la lista.

1. Al usuario invitado se le asigna acceso a todos los recursos del paquete de acceso. Los cambios pueden tardar algún tiempo en realizarse en Azure AD y en otros servicios en línea de Microsoft o en aplicaciones de SaaS conectadas. Para más información, consulte [Cuándo se aplican los cambios](entitlement-management-access-package-resources.md#when-changes-are-applied).

1. El usuario externo recibe un correo electrónico que indica que el acceso se ha [entregado](entitlement-management-process.md).

1. Para tener acceso a los recursos, el usuario externo puede hacer clic en el vínculo del correo electrónico o intentar acceder a cualquiera de los recursos del directorio directamente para completar el proceso de invitación.

1. En función de la configuración de la directiva, la asignación de paquetes de acceso para el usuario externo expira con el tiempo y se quita el acceso del usuario externo.

1. En función del ciclo de vida de la configuración de usuarios externos, cuando el usuario externo ya no tenga ninguna asignación de paquete de acceso, no podrá iniciar sesión y la cuenta de usuario invitado se quitará de su directorio.

## <a name="manage-the-lifecycle-of-external-users"></a>Administración del ciclo de vida de los usuarios externos

Puede seleccionar lo que ocurre cuando un usuario externo, invitado a su directorio mediante una solicitud de paquete de acceso aprobada, ya no tiene ningún paquete de acceso asignado. Esto puede ocurrir si el usuario renuncia a todas sus asignaciones de paquetes de acceso o la asignación del último paquete de acceso expira. De forma predeterminada, cuando un usuario externo ya no tiene un paquete de acceso asignado, se le impide iniciar sesión en el directorio. Después de 30 días, la cuenta de usuario invitado se quitará del directorio.

**Rol necesario:** administrador global o administrador de usuarios.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú izquierdo, en la sección **Administración de derechos**, haga clic en **Configuración**.

1. Haga clic en **Editar**.

    ![Configuración para administrar el ciclo de vida de los usuarios externos](./media/entitlement-management-external-users/settings-external-users.png)

1. En la sección **Administración del ciclo de vida de los usuarios externos**, seleccione las diferentes opciones para los usuarios externos.

1. Cuando un usuario externo pierde la última asignación de un paquete de acceso, si desea bloquearlos para que no inicien sesión en este directorio, establezca **Impedir que los usuarios externos inicien sesión en este directorio** en **Sí**.

1. Cuando un usuario externo pierde la última asignación de un paquete de acceso, si desea quitar su cuenta de usuario invitado del directorio, establezca **Quitar usuario externo** en **Sí**.

    > [!NOTE]
    > La administración de derechos solo quita las cuentas a las que se ha invitado mediante la administración de derechos. Tenga en cuenta también que se impedirá que el usuario inicie sesión y se eliminará del directorio aunque ese usuario se haya agregado a recursos del directorio que no eran asignaciones de paquetes de acceso. Si el invitado estaba presente en el directorio antes de recibir las asignaciones de paquetes de acceso, se mantendrá. Sin embargo, si se le invitó mediante una asignación de paquete de acceso y, después de invitarlo, se ha asignado a un sitio de OneDrive para la Empresa o SharePoint Online, se quitará.

1. Si desea quitar la cuenta de usuario invitado de su directorio, puede establecer el número de días antes de que se quite. Si desea quitar la cuenta de usuario invitado en cuanto pierda la última asignación a cualquier paquete de acceso, establezca **Número de días antes de que se quite el usuario externo de este directorio** en **0**.

1. Haga clic en **Save**(Guardar).

## <a name="enable-a-catalog-for-external-users"></a>Habilitación de un catálogo para usuarios externos

Al crear un [catálogo nuevo](entitlement-management-catalog-create.md), hay una opción que permite a los usuarios de los directorios externos solicitar los paquetes de acceso del catálogo. Si no desea que los usuarios externos tengan permisos para solicitar los paquetes de acceso del catálogo, establezca **Habilitado para usuarios externos** en **No**.

**Rol necesario:** administrador global, administrador de usuarios o propietario del catálogo.

![Panel Nuevo catálogo](./media/entitlement-management-shared/new-catalog.png)

También puede cambiar esta configuración después de crear el catálogo.

![Edición de la configuración del catálogo](./media/entitlement-management-shared/catalog-edit.png)

## <a name="next-steps"></a>Pasos siguientes

- [Para los usuarios que no están en el directorio](entitlement-management-access-package-create.md#for-users-not-in-your-directory)
- [Creación y administración de un catálogo de recursos](entitlement-management-catalog-create.md)
- [Delegación y roles](entitlement-management-delegate.md)
