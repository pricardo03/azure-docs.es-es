---
title: JavaScript y versiones de contrato de página para flujos de usuarios de Azure Active Directory B2C | Microsoft Docs
description: Aprenda a habilitar JavaScript y a usar versiones de contratos de página para personalizar un flujo de usuarios en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5102755c9e830f43fa92e8546e5125960e0a2f9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401559"
---
# <a name="about-using-javascript-and-page-contract-versions-in-a-user-flow"></a>Acerca del uso de JavaScript y las versiones de contratos de páginas en un flujo de usuarios

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C proporciona un conjunto de contenido empaquetado que contiene HTML, CSS y JavaScript para los elementos de la interfaz de usuario de los flujos de usuarios. Si desea habilitar el código en el lado cliente de [JavaScript](javascript-samples.md) en sus flujos de usuarios, es conveniente asegurarse de que los elementos en los que se basa el código JavaScript son inmutables. En caso contrario, cualquier cambio podría provocar un comportamiento inesperado en las páginas del flujo de usuarios. Para evitar estos problemas, puede requerir el uso de un contrato de página para un flujo de usuarios y especificar una versión del contrato de página. Si lo hace así, se asegurará de que todas las definiciones de contenido en las que ha basado el código JavaScript son inmutables. Incluso si no piensa habilitar JavaScript para un flujo de usuarios, puede especificar una versión del contrato de página para las páginas del flujo de usuarios.

> [!NOTE]
> En este artículo analiza JavaScript para flujos de usuarios, pero también puede usar JavaScript y seleccionar versiones de contratos de página cuando se usan [directivas personalizadas](page-contract.md).

## <a name="enable-javascript"></a>Habilitar JavaScript

En las propiedades del flujo de usuarios, puede habilitar JavaScript, el cual requiere también el uso de un contrato de página. A continuación, puede establecer la versión del contrato de página como se indica en la sección siguiente.

![Habilitar configuración de JavaScript](media/user-flow-javascript-overview/javascript-settings.PNG)

## <a name="specify-a-page-contract-version"></a>Especificación de una versión de contrato de página

Tanto si habilita JavaScript como si no lo hace en las propiedades de un flujo de usuarios, puede especificar una versión del contrato de página para las páginas del flujo de usuarios. Abra el flujo de usuarios y seleccione **Diseños de página**. En **Nombre del diseño**, seleccione una página del flujo de usuarios y elija **Versión del contrato de la página**.

![Habilitar configuración de JavaScript](media/user-flow-javascript-overview/page-contract-version.PNG)

## <a name="next-steps"></a>Pasos siguientes
Consulte [Ejemplos de JavaScript para usar en Azure Active Directory B2C](javascript-samples.md).
