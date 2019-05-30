---
title: Preguntas más frecuentes sobre configuración de aplicación de Azure | Microsoft Docs
description: Preguntas más frecuentes sobre la configuración de la aplicación de Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: e321c0b473b110597b5b87a6e67666737116daa2
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393345"
---
# <a name="azure-app-configuration-faq"></a>Preguntas más frecuentes sobre configuración de aplicación de Azure

Este artículo tratan las preguntas más frecuentes sobre la configuración de la aplicación de Azure.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>¿En qué se diferencia App Configuration de Azure Key Vault?

Configuración de la aplicación está diseñada para un conjunto distinto de los casos de uso: lo ayuda a los desarrolladores administrar la configuración de la aplicación y controlar la disponibilidad de características. Pretende simplificar muchas de las tareas del trabajo con datos de configuración compleja.

Configuración de la aplicación es compatible con:

- Espacios de nombres jerárquicos
- Etiquetado
- Consultas amplias
- Recuperación por lotes
- Operaciones de administración especializadas
- Una interfaz de usuario de administración de características

Configuración de la aplicación es complementario a Key Vault y los dos deben usarse en paralelo en la mayoría de las implementaciones de aplicaciones.

## <a name="should-i-store-secrets-in-app-configuration"></a>¿Debo almacenar secretos en la configuración de la aplicación?

Aunque la configuración de la aplicación ofrece mayor seguridad, el almacén de claves sigue siendo el mejor lugar para almacenar secretos de aplicación. Key Vault proporciona cifrado de nivel de hardware, las directivas de acceso granulares y las operaciones de administración, como la rotación de certificados.

## <a name="does-app-configuration-encrypt-my-data"></a>¿Configuración de la aplicación cifra Mis datos?

Sí. Configuración de la aplicación cifra todos los valores de clave que contiene, y cifra la comunicación de red. Los nombres de clave se usan como los índices para recuperar datos de configuración y no se cifran.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>¿Cómo debo almacenar configuraciones para varios entornos (pruebas, ensayo, producción etc.)?

Actualmente puede controlar quién tiene acceso a la configuración de la aplicación en un nivel por almacén. Use un almacén independiente para cada entorno que requiere permisos distintos. Este enfoque le ofrece el mejor aislamiento de seguridad.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>¿Cuáles son los métodos recomendados para usar la configuración de la aplicación?

Consulte [procedimientos recomendados](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>¿Cuánto cuesta la configuración de la aplicación?

El servicio de uso es gratuito durante la versión preliminar pública.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>¿Cómo puedo notificar un problema o proporcionar una sugerencia?

Puede comunicarse con nosotros directamente en [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Pasos siguientes

* [Acerca de la configuración de aplicación de Azure](./overview.md)
