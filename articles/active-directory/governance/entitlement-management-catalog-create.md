---
title: 'Crear y administrar un catálogo en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo crear un nuevo contenedor de recursos y acceso a paquetes de administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6d9220cd2162b4c8cb77c1e7abd0372052f5454
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541621"
---
# <a name="create-and-manage-a-catalog-in-azure-ad-entitlement-management-preview"></a>Crear y administrar un catálogo en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-catalog"></a>Crear un catálogo

Un catálogo es un contenedor de recursos y los paquetes de acceso. Crear un catálogo cuando desea agrupar recursos relacionados y tener acceso a los paquetes. Persona que crea el catálogo se convierte en el primer propietario del catálogo. Un propietario del catálogo puede agregar los propietarios de catálogo adicionales.

**Función de requisitos previos:** Usuario administrador o creador del catálogo

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **catálogos**.

    ![Catálogos de administración de derechos en el portal de Azure](./media/entitlement-management-catalog-create/catalogs.png)

1. Haga clic en **nuevo catálogo**.

1. Escriba un nombre único para el catálogo y proporcione una descripción.

    Los usuarios verán esta información en los detalles de un paquete de acceso.

1. Si desea que los paquetes de acceso en este catálogo esté disponible para los usuarios soliciten tan pronto como se crean, establezca **habilitado** a **Sí**.

1. Si desea permitir que los usuarios en directorios externos seleccionados para que pueda solicitar acceso a los paquetes en este catálogo, establezca **habilitado para los usuarios externos** a **Sí**.

    ![Nuevo panel de catálogo](./media/entitlement-management-catalog-create/new-catalog.png)

1. Haga clic en **crear** para crear el catálogo.

## <a name="add-resources-to-a-catalog"></a>Agregar recursos a un catálogo

Para incluir recursos en un paquete de acceso, los recursos deben existir en un catálogo. Los tipos de recursos que puede agregar son grupos, aplicaciones y sitios de SharePoint Online.

**Función de requisitos previos:** Usuario administrador o propietario del catálogo

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **catálogos** y, a continuación, abra el catálogo que desea agregar a los recursos.

1. En el menú izquierdo, haga clic en **recursos**.

1. Haga clic en **agregar recursos**.

1. Haga clic en un tipo de recurso: **Grupos**, **aplicaciones**, o **sitios de SharePoint**.

    Si es el creador del catálogo, puede agregar cualquier grupo de Office 365 o el grupo de seguridad de Azure AD que posee a su catálogo. Si hay un grupo que desea asignar a los usuarios, pero no es propietario del grupo, deberá tener un usuario administrador agregar ese grupo en el catálogo.

    Si es el creador del catálogo, puede agregar cualquier aplicación empresarial de Azure AD dispone, incluidas las aplicaciones SaaS y sus propias aplicaciones federado con Azure AD, en el catálogo. Si hay una aplicación que desea asignar a los usuarios, pero no le pertenece, deberá tener un usuario administrador agregar la aplicación en el catálogo. Una vez que la aplicación forma parte del catálogo, puede seleccionar cualquiera de las funciones de la aplicación en un paquete de acceso.

1. Seleccione uno o más recursos del tipo que desea agregar al catálogo.

1. Cuando termine, haga clic en **agregar**.

    Estos recursos se pueden incluir ahora en paquetes de acceso en el catálogo.

## <a name="remove-resources-from-a-catalog"></a>Quitar recursos de un catálogo

Puede quitar los recursos de un catálogo. Solo se puede quitar un recurso de un catálogo si no se emplean en cualquiera de los paquetes de acceso del catálogo.

**Función de requisitos previos:** Usuario administrador o propietario del catálogo

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **catálogos** y, a continuación, abra el catálogo que desea quitar recursos de.

1. En el menú izquierdo, haga clic en **recursos**.

1. Seleccione los recursos que desea quitar.

1. Haga clic en **quitar** (o haga clic en el botón de puntos suspensivos (**...** ) y, a continuación, haga clic en **Quitar recurso**).

## <a name="add-catalog-owners-or-access-package-managers"></a>Agregar propietarios de catálogo o tener acceso a los administradores de paquetes

Si desea delegar la administración de paquetes de acceso en el catálogo o el catálogo, agrega los propietarios de catálogo o tener acceso a los administradores de paquetes. Persona que crea un catálogo se convierte en el primer propietario del catálogo.

**Función de requisitos previos:** Usuario administrador o propietario del catálogo

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **catálogos** y, a continuación, abra el catálogo que desea agregar a los administradores.

1. En el menú izquierdo, haga clic en **Roles y administradores**.

1. Haga clic en **agregar propietarios** o **agregar administradores de paquetes de acceso** para seleccionar los miembros de estos roles.

1. Haga clic en **seleccione** para agregar estos miembros.

## <a name="edit-a-catalog"></a>Editar un catálogo

Puede editar el nombre y una descripción para un catálogo. Los usuarios ven esta información en los detalles de un paquete de acceso.

**Función de requisitos previos:** Usuario administrador o propietario del catálogo

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **catálogos** y, a continuación, abra el catálogo que desea editar.

1. En el catálogo **Introducción** página, haga clic en **editar**.

1. Editar nombre o la descripción del catálogo.

1. Haga clic en **Save**(Guardar).

## <a name="delete-a-catalog"></a>Eliminar un catálogo

Puede eliminar un catálogo, pero solo si no tiene los paquetes de acceso.

**Función de requisitos previos:** Usuario administrador o propietario del catálogo

1. En el portal de Azure, haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **catálogos** y, a continuación, abra el catálogo que desea eliminar.

1. En el catálogo **Introducción**, haga clic en **eliminar**.

1. En el cuadro de mensaje que aparece, haga clic en **Sí**.

## <a name="next-steps"></a>Pasos siguientes

- [Crear y administrar un paquete de acceso](entitlement-management-access-package-create.md)
