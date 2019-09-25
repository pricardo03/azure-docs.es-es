---
title: Seguridad del contenedor
titleSuffix: Azure Cognitive Services
description: Aprenda a proteger su contenedor
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: dapine
ms.openlocfilehash: d8d069dddbce6ab6ddb541db460634ad3f6fa067
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70994945"
---
## <a name="azure-cognitive-services-container-security"></a>Protección de los contenedores en Azure Cognitive Services

La seguridad debe ser parte central al desarrollar aplicaciones. La importancia de la seguridad es una métrica del éxito. Al diseñar una solución de software que incluye contenedores de Cognitive Services, es fundamental comprender las limitaciones y las funcionalidades disponibles. Para más información, consulte el artículo sobre la [seguridad en Azure][az-security].

> [!IMPORTANT]
> De forma predeterminada, *no hay seguridad* en la API de contenedor de Cognitive Services. Esto es así porque la mayoría de las veces el contenedor se ejecuta como parte de un pod que está protegido desde fuera por un puente de red. Sin embargo, es posible habilitar la autenticación, que funciona de forma idéntica a la autenticación que se usa al acceder a [Cognitive Services en la nube][request-authentication].

En el diagrama siguiente se muestra el enfoque predeterminado **sin seguridad**:

![Seguridad del contenedor](../media/container-security.svg)

Como método alternativo y *seguro*, los consumidores de contenedores de Cognitive Services podrían aumentar un contenedor con un componente frontal y mantener el punto de conexión del contenedor privado. Veamos un escenario en el que usamos [Istio][istio] como puerta de enlace de entrada. Istio admite HTTPS/SSL y la autenticación con certificado de cliente. En este escenario, el front-end de Istio expone el acceso al contenedor, al presentar de antemano el certificado de cliente que se encuentra en la lista de permitidos con Istio.

[Nginx][nginx] es otra opción popular de la misma categoría. Tanto Istio como Nginx actúan como malla de servicio y ofrecen características adicionales, como equilibrio de carga, enrutamiento y control de velocidad.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../../security/fundamentals/overview.md
