---
title: JavaScript y versiones de diseño de página
titleSuffix: Azure AD B2C
description: Aprenda a habilitar JavaScript y a usar versiones de diseño de página en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 3f6d4849b02f320c7479469b4ee56be50e4f8dee
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840101"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>JavaScript y versiones de diseño de página en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C proporciona un conjunto de contenido empaquetado que contiene HTML, CSS y JavaScript para los elementos de la interfaz de usuario de los flujos de usuarios y las directivas personalizadas. Para habilitar JavaScript para sus aplicaciones, debe agregar un elemento a su [directiva personalizada](custom-policy-overview.md) o habilitarlo en el portal para flujos de usuario, seleccionar un diseño de página y usar [b2clogin.com](b2clogin.md) en las solicitudes.

Si desea habilitar el código en el lado cliente de [JavaScript](javascript-samples.md), es conveniente asegurarse de que los elementos en los que se basa el código JavaScript son inmutables. En caso contrario, cualquier cambio podría provocar un comportamiento inesperado en las páginas de usuario. Para evitar estos problemas, puede requerir el uso de un diseño de página y especificar una versión del diseño de página. Si lo hace así, se asegurará de que todas las definiciones de contenido en las que ha basado el código JavaScript son inmutables. Incluso si no piensa habilitar JavaScript, puede especificar una versión del diseño de página para las páginas.

## <a name="user-flows"></a>Flujos de usuario

En la sección **Propiedades** del flujo de usuario, puede habilitar JavaScript, el cual también requiere el uso de un diseño de página. Después, puede establecer la versión del diseño de página para el flujo de usuario tal y como se describe en la sección siguiente.

![Página de propiedades del flujo de usuario con la opción Habilitar JavaScript resaltada](media/user-flow-javascript-overview/javascript-settings.png)

### <a name="select-a-page-layout-version"></a>Selección de una versión de diseño de página

Tanto si habilita JavaScript como si no lo hace en las propiedades de un flujo de usuario, puede especificar una versión del diseño de página para las páginas del flujo de usuario. Abra el flujo de usuario y seleccione **Diseños de página**. En **NOMBRE DEL DISEÑO**, seleccione una página del flujo de usuario y elija la **versión del diseño de página**.

Para obtener información sobre las distintas versiones de diseño de página, consulte el [Registro de cambios de versión](page-layout.md#version-change-log).

![Configuración del diseño de página del portal que muestra el menú desplegable de la versión del diseño de página](media/user-flow-javascript-overview/page-layout-version.png)

## <a name="custom-policies"></a>Directivas personalizadas

Para habilitar JavaScript en las directivas personalizadas, agregue el elemento **ScriptExecution** al elemento **RelyingParty** en el archivo de directiva personalizada. Para más información, consulte [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).

Tanto si habilita JavaScript en las directivas personalizadas como si no lo hace, puede especificar una versión del diseño de página para las páginas. Para obtener más información acerca de cómo especificar un diseño de página, consulte [Selección de un diseño de página en Azure Active Directory B2C mediante directivas personalizadas](page-layout.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre las distintas versiones del diseño de página, consulte la sección **Registro de cambios de versión** del artículo [Selección de un diseño de página en Azure Active Directory B2C mediante directivas personalizadas](page-layout.md#version-change-log).

Puede encontrar ejemplos de uso de JavaScript en [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).
