---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: baf39c1fe72703d8ebc18b03bae1c963536d7ced
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73475187"
---
Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la experiencia en versión preliminar](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure Active Directory** (*no* Azure AD B2C). O bien seleccione **Todos los servicios** y, luego, busque y seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Registros de aplicaciones (característica heredada)** .
1. Seleccione **Nuevo registro de aplicaciones**.
1. Escriba un nombre para la aplicación. Por ejemplo, *managementapp1*.
1. En **Tipo de aplicación**, seleccione**Aplicación web o API**.
1. En **Dirección URL de inicio de sesión**, escriba una dirección URL válida. Por ejemplo, `https://localhost`. No es necesario que el punto de conexión sea accesible, pero debe ser una dirección URL válida.
1. Seleccione **Crear**.
1. Anote el **identificador de aplicación** que aparece en la página de información general **Aplicación registrada**. Se usará este valor en un paso posterior.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** y luego **Nuevo registro**.
1. Escriba un **nombre** para la aplicación. Por ejemplo, *managementapp1*.
1. Seleccione **Solo las cuentas de este directorio organizativo**.
1. En **Permisos**, desactive la casilla *Conceda permiso del administrador a los permisos openid y offline_access*.
1. Seleccione **Registrar**.
1. Registre el valor **Id. de aplicación (cliente)** que se muestra en la página de información general de la aplicación. Se usará este valor en un paso posterior.