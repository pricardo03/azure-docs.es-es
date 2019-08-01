---
title: Preguntas frecuentes de Azure App Configuration | Microsoft Docs
description: Preguntas frecuentes sobre Azure App Configuration
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393345"
---
# <a name="azure-app-configuration-faq"></a>Preguntas frecuentes de Azure App Configuration

En este artículo se abordan las preguntas más frecuentes sobre Azure App Configuration.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>¿En qué se diferencia App Configuration de Azure Key Vault?

App Configuration está diseñado para un conjunto distinto de casos de uso: ayuda a los desarrolladores a administrar la configuración de las aplicaciones y a controlar la disponibilidad de características. Pretende simplificar muchas de las tareas del trabajo con datos de configuración complejos.

App Configuration admite:

- Espacios de nombres jerárquicos
- Etiquetado
- Consultas amplias
- Recuperación por lotes
- Operaciones de administración especializadas
- Una interfaz de usuario de administración de características

App Configuration es complementario a Key Vault y los dos deben usarse en paralelo en la mayoría de las implementaciones de aplicaciones.

## <a name="should-i-store-secrets-in-app-configuration"></a>¿Debo almacenar secretos en App Configuration?

Aunque App Configuration ofrece mayor seguridad, Key Vault sigue siendo el mejor lugar para almacenar los secretos de la aplicación. Key Vault proporciona cifrado de nivel de hardware, directivas de acceso granulares y operaciones de administración, como la rotación de certificados.

## <a name="does-app-configuration-encrypt-my-data"></a>¿Cifra App Configuration mis datos?

Sí. App Configuration cifra todos los valores de clave que contiene, así como la comunicación de red. Los nombres de clave se usan como índices para recuperar los datos de configuración y no se cifran.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>¿Cómo debo almacenar las configuraciones para varios entornos (pruebas, ensayo, producción etc.)?

Actualmente puede controlar quién tiene acceso a App Configuration en cada almacén. Use un almacén independiente para cada entorno que requiera permisos distintos. Este enfoque le ofrece el mejor aislamiento de seguridad.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>¿Cuáles son los métodos recomendados para usar App Configuration?

Consulte los [procedimientos recomendados](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>¿Cuánto cuesta App Configuration?

El uso del servicio es gratuito durante la versión preliminar pública.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>¿Cómo puedo notificar un problema o proporcionar una sugerencia?

Puede comunicarse con nosotros directamente en [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Pasos siguientes

* [Acerca de Azure App Configuration](./overview.md)
