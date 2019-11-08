---
title: Preguntas frecuentes de Azure App Configuration | Microsoft Docs
description: Preguntas frecuentes sobre Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: dbfb6a1c4c53b1bd255560e688d3dc0cf3835a3a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73469637"
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

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>¿Hay alguna limitación de tamaño en las claves y los valores almacenados en App Configuration?

Hay un límite de 10 kB para un único elemento de clave-valor.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>¿Cómo debo almacenar las configuraciones para varios entornos (pruebas, ensayo, producción etc.)?

Actualmente puede controlar quién tiene acceso a App Configuration en cada almacén. Use un almacén independiente para cada entorno que requiera permisos distintos. Este enfoque le ofrece el mejor aislamiento de seguridad.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>¿Cuáles son los métodos recomendados para usar App Configuration?

Consulte los [procedimientos recomendados](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>¿Cuánto cuesta App Configuration?

El uso del servicio es gratuito durante la versión preliminar pública.

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>¿Cómo puedo recibir anuncios sobre nuevas versiones y otra información relacionada con App Configuration?

Suscríbase a nuestro [repositorio de anuncios de GitHub](https://github.com/Azure/AppConfiguration-Announcements).

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>¿Cómo puedo notificar un problema o proporcionar una sugerencia?

Puede comunicarse con nosotros directamente en [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Pasos siguientes

* [Acerca de Azure App Configuration](./overview.md)
