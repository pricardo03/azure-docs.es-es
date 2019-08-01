---
title: 'JavaScript y versiones de contrato de página: Azure Active Directory B2C | Microsoft Docs'
description: Aprenda a habilitar JavaScript y a usar versiones de contrato de página en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ef474bec71a9015209b5748b6947816002bd4a5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66511973"
---
# <a name="javascript-and-page-contract-versions-in-azure-active-directory-b2c"></a>JavaScript y versiones de contrato de página en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C proporciona un conjunto de contenido empaquetado que contiene HTML, CSS y JavaScript para los elementos de la interfaz de usuario de los flujos de usuarios y las directivas personalizadas. Para habilitar JavaScript para sus aplicaciones, debe agregar un elemento a su [directiva personalizada](active-directory-b2c-overview-custom.md) o habilitarlo en el portal para flujos de usuario, seleccionar un contrato de página y usar [b2clogin.com](b2clogin.md) en las solicitudes.

Si desea habilitar el código en el lado cliente de [JavaScript](javascript-samples.md), es conveniente asegurarse de que los elementos en los que se basa el código JavaScript son inmutables. En caso contrario, cualquier cambio podría provocar un comportamiento inesperado en las páginas de usuario. Para evitar estos problemas, puede requerir el uso de un contrato de página y especificar una versión del contrato de página. Si lo hace así, se asegurará de que todas las definiciones de contenido en las que ha basado el código JavaScript son inmutables. Incluso si no piensa habilitar JavaScript, puede especificar una versión del contrato de página para las páginas.

## <a name="user-flows"></a>Flujos de usuario

En las propiedades del flujo de usuarios, puede habilitar JavaScript, el cual requiere también el uso de un contrato de página. A continuación, puede establecer la versión del contrato de página como se indica en la sección siguiente.

![Habilitar configuración de JavaScript](media/user-flow-javascript-overview/javascript-settings.png)

Tanto si habilita JavaScript como si no lo hace en las propiedades de un flujo de usuarios, puede especificar una versión del contrato de página para las páginas del flujo de usuarios. Abra el flujo de usuarios y seleccione **Diseños de página**. En **Nombre del diseño**, seleccione una página del flujo de usuarios y elija **Versión del contrato de la página**.

![Habilitar configuración de JavaScript](media/user-flow-javascript-overview/page-contract-version.png)

## <a name="custom-policies"></a>Directivas personalizadas

Para habilitar JavaScript en las directivas personalizadas, agregue el elemento **ScriptExecution** al elemento **RelyingParty** en el archivo de directiva personalizada. Para más información, consulte [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).

Tanto si habilita JavaScript en las directivas personalizadas como si no lo hace, puede especificar una versión del contrato de página para las páginas. Para más información acerca de cómo especificar un contrato de página, consulte [Selección de un contrato de página en Azure Active Directory B2C con directivas personalizadas](page-contract.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).
