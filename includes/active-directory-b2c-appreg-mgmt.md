---
author: mmacy
ms.service: active-directory-b2c
ms.topic: include
ms.date: 10/01/2019
ms.author: marsma
ms.openlocfilehash: 67a0832f868fecd47983aa8cddff9cdf139b3f2a
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71702186"
---
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
