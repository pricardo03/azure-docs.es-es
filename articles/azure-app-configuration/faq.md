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
ms.openlocfilehash: 8d286cbab33a1fb6a2d2a2cb70caed11b21af735
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904092"
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

Puede crear valores de App Configuration que hagan referencia a secretos almacenados en Key Vault. Para obtener más información, consulte [Uso de referencias de Key Vault en una aplicación de ASP.NET Core](./use-key-vault-references-dotnet-core.md).

## <a name="does-app-configuration-encrypt-my-data"></a>¿Cifra App Configuration mis datos?

Sí. App Configuration cifra todos los valores de clave que contiene, así como la comunicación de red. Los nombres de clave se usan como índices para recuperar los datos de configuración y no se cifran.

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>¿En qué se diferencia App Configuration de la configuración de Azure App Service?

Azure App Service permite definir la configuración de la aplicación para cada instancia de App Service. Estas configuraciones se pasan como variables de entorno al código de la aplicación. Si quiere, puede asociar un valor a una ranura de implementación específica. Para obtener más información, consulte [Configuración de aplicaciones](/azure/app-service/configure-common#configure-app-settings).

En cambio, Azure App Configuration permite definir la configuración que se puede compartir entre varias aplicaciones, incluidas las que se ejecutan en App Service. Se puede acceder a esta configuración en el código de aplicación a través de los proveedores de configuración para .NET y Java, a través del SDK de Azure o directamente a través de las API REST.

También puede importar y exportar configuraciones entre App Service y App Configuration. Esto le permite configurar rápidamente un nuevo almacén de App Configuration en función de la configuración existente de App Service o compartir fácilmente la configuración con una aplicación existente que se basa en la configuración de App Service.

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
