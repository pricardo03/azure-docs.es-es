---
title: 'JavaScript y la página del contrato de versiones: Azure Active Directory B2C | Microsoft Docs'
description: Aprenda a habilitar JavaScript y usar versiones de contratos de página en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 91b4b621fc3dcedb52f88372fbfac222a744dbd1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64570628"
---
# <a name="javascript-and-page-contract-versions-in-azure-active-directory-b2c"></a>Versiones de contratos de JavaScript y página en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

B2C de Azure AD proporciona un conjunto de contenido empaquetado que contiene HTML, CSS y JavaScript para los elementos de interfaz de usuario en los flujos de usuario y las directivas personalizadas. Para habilitar JavaScript para sus aplicaciones, debe agregar un elemento a su [directiva personalizada](active-directory-b2c-overview-custom.md) o habilitarlo en el portal para flujos de usuario, seleccione un contrato de página y use [b2clogin.com](b2clogin.md) en las solicitudes.

Si piensa habilitar [JavaScript](javascript-samples.md) código del lado cliente, es conveniente para asegurarse de que los elementos que se basa en el código JavaScript son inmutables. De lo contrario, cualquier cambio podrían provocar un comportamiento inesperado en las páginas de usuario. Para evitar estos problemas, puede exigir el uso de un contrato de página y especificar una versión de contrato de la página. Esto garantiza que todas las definiciones de contenido que ha basado en el código JavaScript son inmutables. Incluso si no piensa habilitar JavaScript, puede especificar una versión de contrato de página para las páginas.

## <a name="user-flows"></a>Flujos de usuario

En las propiedades del flujo de usuarios, puede habilitar JavaScript, el cual requiere también el uso de un contrato de página. A continuación, puede establecer la versión del contrato de página como se indica en la sección siguiente.

![Habilitar configuración de JavaScript](media/user-flow-javascript-overview/javascript-settings.png)

Tanto si habilita JavaScript como si no lo hace en las propiedades de un flujo de usuarios, puede especificar una versión del contrato de página para las páginas del flujo de usuarios. Abra el flujo de usuarios y seleccione **Diseños de página**. En **Nombre del diseño**, seleccione una página del flujo de usuarios y elija **Versión del contrato de la página**.

![Habilitar configuración de JavaScript](media/user-flow-javascript-overview/page-contract-version.png)

## <a name="custom-policies"></a>Directivas personalizadas

Para habilitar JavaScript en las directivas personalizadas, se agrega el **ScriptExecution** elemento a la **RelyingParty** element en el archivo de directiva personalizada. Para obtener más información, consulte [muestras de JavaScript para su uso en Azure Active Directory B2C](javascript-samples.md).

Si Habilitar JavaScript en las directivas personalizadas, puede especificar una versión de contrato de página para las páginas. Para obtener más información acerca de cómo especificar un contrato de página, vea [seleccionar un contrato de página en Azure Active Directory B2C mediante directivas personalizadas](page-contract.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).
