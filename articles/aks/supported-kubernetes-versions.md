---
title: Versiones de Kubernetes compatibles en Azure Kubernetes Service
description: Obtener información sobre la directiva de soporte técnico de la versión de Kubernetes y el ciclo de vida de los clústeres en Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: a7b3176e39ccaa0f9ddb1ef45c33ec6902e62f1c
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956330"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)

La Comunidad de Kubernetes libera versiones secundarias aproximadamente cada tres meses. Estas versiones incluyen nuevas características y mejoras. Versiones de revisión son más frecuentes (a veces semanales) y solo están previstas para correcciones de errores críticos en una versión secundaria. Estas versiones de revisión incluyen correcciones de vulnerabilidades de seguridad o errores importantes que afectan a un gran número de clientes y productos que se ejecutan en producción basada en Kubernetes.

Un nueva versión secundaria de Kubernetes está disponible en [aks-engine][aks-engine] desde el primer día. El objetivo de nivel de servicio (SLO) de AKS tiene como objetivo publicar la versión secundaria para los clústeres AKS en 30 días, dependiendo de la estabilidad de la versión.

## <a name="kubernetes-version-support-policy"></a>Directiva de soporte técnico de versión de Kubernetes

AKS es compatible con cuatro versiones secundarias de Kubernetes:

- La versión secundaria actual que está publicado en el canal de subida (n)
- Tres versiones secundarias anteriores. Cada versión secundaria compatible también admite dos revisiones estables.

Por ejemplo, si se presentan AKS *1.13.x* hoy en día, también se proporciona compatibilidad para *1.12.a* + *1.12.b*, *1.11.c*  +  *d. 1.11*, *1.10.e* + *1.10F* (donde las versiones de revisión indicados con letras son dos compilaciones estables más recientes).

Cuando se introduce una nueva versión secundaria, se retirarán la versión secundaria y la versión de revisión compatibles más antiguas. un anuncio de 30 días antes del lanzamiento de la nueva versión secundaria y la retirada de la próxima versión, se realiza a través de la [canales de actualización de Azure][azure-update-channel]. En el ejemplo anterior donde *1.13.x* está publicado, las versiones retiradas son *1.9.g* + *1.9.h*.

Al implementar un clúster de AKS en Azure Portal o con la CLI de Azure, el clúster siempre se establece en la versión secundaria n-1 y la revisión más reciente. Por ejemplo, si es compatible con AKS *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +   *d. 1.11*, *1.10.e* + *1.10F*, es la versión predeterminada para nuevos clústeres *1.11.b*.

## <a name="list-currently-supported-versions"></a>Lista de versiones admitidas actualmente

Para averiguar qué versiones están disponibles actualmente para su suscripción y región, utilice el comando [az aks get-versions][az-aks-get-versions]. En el ejemplo siguiente se enumeran las versiones de Kubernetes disponibles para la región *EastUS*:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

El resultado es similar al ejemplo siguiente, que se muestra esa versión de Kubernetes *1.13.5* está disponible la versión más reciente:

```
KubernetesVersion    Upgrades
-------------------  ------------------------
1.13.5               None available
1.12.7               1.13.5
1.12.6               1.12.7, 1.13.5
1.11.9               1.12.6, 1.12.7
1.11.8               1.11.9, 1.12.6, 1.12.7
1.10.13              1.11.8, 1.11.9
1.10.12              1.10.13, 1.11.8, 1.11.9
```

## <a name="faq"></a>Preguntas más frecuentes

**¿Qué ocurre cuando un cliente actualiza un clúster de Kubernetes con una versión secundaria que no es compatible?**

Si se encuentra en la versión *n-4*, está fuera del SLO. Si se realiza correctamente la actualización de la versión n-4 a n-3, volverá a estar en el SLO. Por ejemplo:

- Si las versiones compatibles de AKS son *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +  *d. 1.11*, y *1.10.e* + *1.10F* y se encuentra en *1.9.g* o *1.9.h*, está fuera del SLO.
- Si la actualización de *1.9.g* o *1.9.h* a *1.10.e* o *1.10.f* se realiza correctamente, está en el SLO.

Las actualizaciones a las versiones anteriores a *n-4* no se admiten. En esos casos, se recomienda a los clientes que creen nuevos clústeres de AKS y vuelvan a implementar sus cargas de trabajo.

**¿Qué ocurre cuando un cliente escala un clúster de Kubernetes con una versión secundaria que no es compatible?**

Para las versiones secundarias que no son compatibles con AKS, el escalado sigue funcionando sin ningún problema.

**¿Puede un cliente permanecer en una versión de Kubernetes para siempre?**

Sí. No obstante, si el clúster no está en una de las versiones compatibles con AKS, el clúster está fuera del SLO de AKS. Azure no actualiza el clúster automáticamente ni lo elimina.

**¿Qué versión admite el clúster principal si el clúster de agente no está en una de las versiones admitidas de AKS?**

El clúster maestro se actualiza automáticamente a la última versión compatible.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo actualizar el clúster, vea [Actualización de un clúster de Azure Kubernetes Service (AKS)][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
