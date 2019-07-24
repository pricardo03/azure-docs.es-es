---
title: 'Creación y administración de un catálogo en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo crear un contenedor de recursos y paquetes de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 05/29/2019
ms.author: rolyon
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5988f4723f1ef73cf0767ef8ac1b9adf3c1435d
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190241"
---
# <a name="create-and-manage-a-catalog-in-azure-ad-entitlement-management-preview"></a>Creación y administración de un catálogo en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-catalog"></a>Creación de un catálogo

Un catálogo es un contenedor de recursos y paquetes de acceso. Creará un catálogo cuando quiera agrupar recursos relacionados y paquetes de acceso. Quien cree el catálogo se convertirá en su primer propietario. El propietario de un catálogo puede agregar otros propietarios.

**Rol necesario:** administrador de usuarios o creador de catálogos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Haga clic en **Azure Active Directory** y, luego, haga clic en **Gobierno de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos**.

    ![Catálogos de la administración de derechos en Azure Portal](./media/entitlement-management-catalog-create/catalogs.png)

1. Haga clic en **Nuevo catálogo**.

1. Escriba un nombre único para el catálogo y proporcione una descripción.

    Los usuarios verán esta información en los detalles de un paquete de acceso.

1. Si quiere que los usuarios puedan solicitar los paquetes de acceso de este catálogo en cuanto se creen, establezca **Habilitado** en **Sí**.

1. Si quiere permitir que los usuarios de directorios externos seleccionados puedan solicitar los paquetes de acceso de este catálogo, establezca **Enabled for external users** (Habilitado para usuarios externos) en **Sí**.

    ![Panel Nuevo catálogo](./media/entitlement-management-catalog-create/new-catalog.png)

1. Haga clic en **Crear** para crear el catálogo.

## <a name="add-resources-to-a-catalog"></a>Adición de recursos a un catálogo

Para incluir recursos en un paquete de acceso, deben estar en un catálogo. Los tipos de recursos que puede agregar son grupos, aplicaciones y sitios de SharePoint Online. Los grupos pueden ser grupos de Office 365 creados en la nube o grupos de seguridad de Azure AD creados en la nube. Las aplicaciones pueden ser aplicaciones empresariales de Azure AD, incluidas las aplicaciones SaaS y sus propias aplicaciones federadas con Azure AD. Los sitios pueden ser sitios de SharePoint Online o colecciones de sitios de SharePoint Online.

**Rol necesario:** vea [Roles necesarios para agregar recursos a un catálogo](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo al que quiere agregar recursos.

1. En el menú izquierdo, haga clic en **Recursos**.

1. Haga clic en **Agregar recursos**.

1. Haga clic en un tipo de recurso: **Grupos**, **Aplicaciones** o **Sitios de SharePoint**.

    Si no ve un recurso que quiere agregar o no puede agregar un recurso, asegúrese de que tiene los roles de administración de derechos y de directorio de Azure AD que se requieren. Es posible que alguien que tenga los roles necesarios tenga que agregar el recurso al catálogo. Para obtener más información, vea [Roles necesarios para agregar recursos a un catálogo](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog).

1. Seleccione uno o varios recursos del tipo que quiera agregar al catálogo.

1. Cuando haya finalizado, haga clic en **Agregar**.

    Ahora, estos recursos se pueden incluir en paquetes de acceso del catálogo.

## <a name="remove-resources-from-a-catalog"></a>Eliminación de recursos de un catálogo

Puede quitar recursos de un catálogo. Solo se puede quitar un recurso de un catálogo si no se está usando en ninguno de los paquetes de acceso del catálogo.

**Rol necesario:** vea [Roles necesarios para agregar recursos a un catálogo](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo del que quiere quitar recursos.

1. En el menú izquierdo, haga clic en **Recursos**.

1. Seleccione los recursos que quiera quitar.

1. Haga clic en **Quitar** (o haga clic en el botón de puntos suspensivos ( **...** ) y, luego, en **Quitar recurso**).

## <a name="edit-a-catalog"></a>Edición de un catálogo

Puede editar el nombre y la descripción de un catálogo. Los usuarios ven esta información en los detalles de un paquete de acceso.

**Rol necesario:** administrador de usuarios o propietario del catálogo

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo que quiere editar.

1. En la página **Introducción** del catálogo, haga clic en **Editar**.

1. Edite el nombre o la descripción del catálogo.

1. Haga clic en **Save**(Guardar).

## <a name="delete-a-catalog"></a>Eliminación de un catálogo

Puede eliminar un catálogo, pero solo si no tiene ningún paquete de acceso.

**Rol necesario:** administrador de usuarios o propietario del catálogo

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobierno de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo que quiere eliminar.

1. En la página **Introducción** del catálogo, haga clic en **Eliminar**.

1. En el cuadro de mensaje que aparece, haga clic en **Sí**.

## <a name="next-steps"></a>Pasos siguientes

- [Adición de un creador de catálogos](entitlement-management-delegate.md#add-a-catalog-creator)
- [Creación y administración de un paquete de acceso](entitlement-management-access-package-create.md)
