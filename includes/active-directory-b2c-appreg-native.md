---
author: mmacy
ms.service: active-directory-b2c
ms.topic: include
ms.date: 09/27/2019
ms.author: marsma
ms.openlocfilehash: b83a6b9185eb4ef127da1dd3b55aba4e8b2945f9
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326305"
---
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *nativeapp1*.
1. En **Cliente nativo**, seleccione **Sí**.
1. Escriba un **URI de redirección personalizado** con un esquema único. Por ejemplo, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:
    1. **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`, `com.onmicrosoft.contosob2c.exampleapp` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario tiene opción de elegir una aplicación. Si el usuario elige incorrectamente, se produce un error en el inicio de sesión.
    1. **Completo**: el URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//oauth/` sirve y `//oauth` produce un error. No incluya caracteres especiales en el URI, por ejemplo, caracteres de subrayado.
1. Seleccione **Crear**.
