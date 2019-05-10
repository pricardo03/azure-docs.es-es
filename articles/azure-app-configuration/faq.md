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
ms.openlocfilehash: 3181609bf34a04de4e31b73429f9bc5fa3fe3408
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411902"
---
# <a name="azure-app-configuration-faq"></a>Preguntas más frecuentes sobre configuración de aplicación de Azure

Este artículo se abordan las preguntas sobre la configuración de la aplicación de Azure con frecuencia.

## <a name="how-is-app-configuration-different-from-key-vault"></a>¿Cómo difiere la configuración de la aplicación de Key Vault?

Configuración de la aplicación está diseñada para un conjunto distinto de los casos de uso: lo ayuda a los desarrolladores administrar la configuración de la aplicación y controlar la disponibilidad de características. Pretende simplificar muchas de las tareas del trabajo con datos de configuración compleja. Admite el espacio de nombres jerárquico, etiquetado, consultas amplias, la recuperación por lotes y las operaciones de administración especializados y interfaces de usuario. Configuración de la aplicación es complementaria a Key Vault y los dos deben usarse en paralelo en la mayoría de las implementaciones de aplicaciones.

## <a name="should-i-store-secrets-in-app-configuration"></a>¿Debo almacenar secretos en la configuración de la aplicación?

Mientras la configuración de la aplicación ofrece mayor seguridad, el almacén de claves sigue siendo el mejor lugar para almacenar secretos de aplicación. Proporciona cifrado de nivel de hardware, las directivas de acceso granular y operaciones de administración como la rotación de certificados.

## <a name="does-app-configuration-encrypt-my-data"></a>¿Configuración de la aplicación cifra Mis datos?

Sí. Configuración de la aplicación cifra todos los valores de clave contiene y comunicación de red. Los nombres de clave se usan como los índices para recuperar datos de configuración y no están cifrados.

## <a name="does-app-configuration-support-azure-virtual-network-vnet"></a>¿Admite la configuración de aplicaciones de Azure Virtual Network (VNET)?

Todavía no. Compatibilidad con redes virtuales está prevista para la disponibilidad general.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>¿Cómo debo almacenar configuraciones para varios entornos (pruebas, ensayo, producción etc.)?

Actualmente puede controlar quién tiene acceso a la configuración de la aplicación en un nivel por almacén. Debe usar un almacén independiente para cada entorno que requiere permisos distintos. Este enfoque le ofrece el mejor aislamiento de seguridad.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>¿Cuáles son los métodos recomendados para usar la configuración de la aplicación?

Consulte [procedimientos recomendados](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>¿Cuánto cuesta la configuración de la aplicación?

El servicio de uso es gratuito durante la versión preliminar pública.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>¿Cómo puedo notificar un problema o proporcionar una sugerencia?

Puede comunicarse con nosotros directamente en [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Pasos siguientes

* [Acerca de la configuración de aplicación de Azure](./overview.md)
