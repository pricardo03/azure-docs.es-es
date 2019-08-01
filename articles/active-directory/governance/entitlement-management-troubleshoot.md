---
title: 'Solución de problemas de administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Aprenda sobre algunos elementos que debe comprobar que le ayudarán a solucionar problemas de administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2526ef10c3080dae1b32881a109a9436a0fd390
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473826"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Solución de problemas de la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se describen algunos elementos que debe comprobar que le ayudarán a solucionar problemas de administración de derechos de Azure Active Directory (Azure AD).

## <a name="checklist-for-entitlement-management-administration"></a>Lista de comprobación para la administración de derechos

* Si recibe un mensaje acceso denegado al configurar la administración de derechos y es administrador global, asegúrese de que el directorio tiene [licencia de Azure AD Premium P2 (o EMS E5)](entitlement-management-overview.md#license-requirements).  
* Si recibe un mensaje de acceso denegado al crear o ver los paquetes de acceso y es miembro de un grupo creador de catálogo, debe crear un catálogo antes de crear el primer paquete de acceso.

## <a name="checklist-for-adding-a-resource"></a>Lista de comprobación para agregar un recurso

* Para que una aplicación sea un recurso en un paquete de acceso, debe tener al menos un rol de recurso que se pueda asignar. Los roles los define la propia aplicación y se administran en Azure AD. Tenga en cuenta que en Azure Portal también se pueden mostrar las entidades de servicio de los servicios que no se pueden seleccionar como aplicaciones.  En concreto, **Exchange Online** y **SharePoint Online** son servicios, no aplicaciones que tengan roles de recursos en el directorio, por lo que no pueden incluirse en un paquete de acceso.  En su lugar, use las licencias de grupo para establecer una licencia adecuada para un usuario que necesite acceder a esos servicios.

* Para que un grupo sea un recurso en un paquete de acceso, debe poder modificarse en Azure AD.  Los grupos que se originan en una instancia local de Active Directory no pueden asignarse como recursos porque su propietario o los atributos de miembro no se pueden cambiar en Azure AD.  

* Las bibliotecas de documentos de SharePoint Online y los documentos individuales no se pueden agregar como recursos.  En su lugar, cree un grupo de seguridad de Azure AD, incluya ese grupo y un rol de sitio en el paquete de acceso y, en SharePoint Online, use ese grupo para controlar el acceso a la biblioteca de documentos o al documento.

* Si hay usuarios que ya se han asignado a un recurso que desee administrar con un paquete de acceso, asegúrese de que los usuarios están asignados al paquete de acceso con una directiva adecuada. Por ejemplo, puede incluir un grupo en un paquete de acceso que ya tenga usuarios en el grupo. Si esos usuarios del grupo requieren acceso continuado, deben tener una directiva adecuada para los paquetes de acceso, de manera que no se queden sin acceso de grupo. Para asignar el paquete de acceso, puede pedir a los usuarios que soliciten el paquete de acceso que contiene el recurso o asignárselo directamente. Para más información, consulte [Edición y administración de un paquete de acceso existente](entitlement-management-access-package-edit.md).

## <a name="checklist-for-providing-external-users-access"></a>Lista de comprobación para proporcionar acceso a usuarios externos

* Si hay una [lista de permitidos](../b2b/allow-deny-list.md) en B2B, los usuarios cuyos directorios no se permitan no podrán solicitar acceso.

* Asegúrese de que no hay [directivas de acceso condicional](../conditional-access/require-managed-devices.md) que pudieran impedir que los usuarios externos soliciten acceso o puedan usar las aplicaciones de los paquetes de acceso.

## <a name="checklist-for-request-issues"></a>Lista de comprobación para problemas de solicitud

* Cuando un usuario desee solicitar acceso a un paquete de acceso, asegúrese de que está utilizando el **vínculo del portal Mi acceso** para el paquete de acceso. Para más información, consulte [Copie el vínculo del portal Mi acceso](entitlement-management-access-package-edit.md#copy-my-access-portal-link).

* Cuando un usuario inicia sesión en el portal Mi acceso para solicitar un paquete de acceso, asegúrese de que se autentique con su cuenta profesional. La cuenta profesional puede ser una cuenta del directorio de recursos o de un directorio incluido en una de las directivas del paquete de acceso. Si la cuenta de usuario no es una cuenta profesional o si el directorio no está incluido en la directiva, el usuario no verá el paquete de acceso. Para más información, consulte [Solicitud de acceso a un paquete acceso](entitlement-management-request-access.md).

* Si un usuario tiene un bloqueo de inicio de sesión al directorio de recursos, no podrá acceder a la solicitud de acceso del portal Mi acceso. Para que pueda solicitar el acceso, debe eliminar el bloqueo de inicio de sesión de su perfil. Para quitar el bloqueo de inicio de sesión en Azure Portal, haga clic en **Azure Active Directory**, en **Usuarios**, en el usuario y, finalmente, en **Perfil**. Edite la sección **Configuración** y cambie **Bloquear inicio de sesión** a **No**. Para más información, consulte [Incorporación o actualización de la información de perfil de un usuario mediante Azure Active Directory](../fundamentals/active-directory-users-profile-azure-portal.md).  También puede comprobar si el usuario se bloqueó debido a una [directiva de protección de identidad](../identity-protection/howto-unblock-user.md).

* En el portal Mi acceso, si un usuario es tanto solicitante como aprobador, no verá su solicitud para un paquete de acceso en la página **Aprobaciones**. Este comportamiento es deliberado: un usuario no puede aprobar sus propias solicitudes. Asegúrese de que el paquete de acceso que solicita tiene otros aprobadores configurados en la directiva. Para más información, vea [Editar una directiva existente](entitlement-management-access-package-edit.md#edit-an-existing-policy).

* Si un nuevo usuario externo que no ha iniciado sesión en el directorio antes recibe un paquete de acceso que incluya un sitio de SharePoint Online, este se mostrará como que no se ha entregado totalmente hasta que la cuenta esté aprovisionada en SharePoint Online.

## <a name="next-steps"></a>Pasos siguientes

- [Visualización de los informes sobre el acceso de los usuarios en la administración de derechos](entitlement-management-reports.md)
