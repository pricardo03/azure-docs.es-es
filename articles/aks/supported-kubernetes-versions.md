---
title: Versiones de Kubernetes compatibles en Azure Kubernetes Service
description: Obtener información sobre la directiva de soporte técnico de la versión de Kubernetes y el ciclo de vida de los clústeres en Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 09/21/2018
ms.author: saudas
ms.openlocfilehash: 3e8342a719bf9ae7174195f88b97972d7f13193c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465792"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)

La Comunidad de Kubernetes libera versiones secundarias aproximadamente cada tres meses. Estas versiones incluyen nuevas características y mejoras. Versiones de revisión son más frecuentes (a veces semanales) y solo están previstas para correcciones de errores críticos en una versión secundaria. Estas versiones de revisión incluyen correcciones de vulnerabilidades de seguridad o errores importantes que afectan a un gran número de clientes y productos que se ejecutan en producción basada en Kubernetes.

Un nueva versión secundaria de Kubernetes está disponible en [aks-engine][aks-engine] desde el primer día. El objetivo de nivel de servicio (SLO) de AKS tiene como objetivo publicar la versión secundaria para los clústeres AKS en 30 días, dependiendo de la estabilidad de la versión.

## <a name="kubernetes-version-support-policy"></a>Directiva de soporte técnico de versión de Kubernetes

AKS es compatible con cuatro versiones secundarias de Kubernetes:

- La versión secundaria actual que está publicado en el canal de subida (n)
- Tres versiones secundarias anteriores. Cada versión secundaria compatible también admite dos revisiones estables.

Por ejemplo, si AKS presenta *1.11.x* hoy, también se proporciona compatibilidad para *1.10.a* + *1.10.b*, *1.9.c* + *1.9d*, *1.8.e* + *1.8F* (donde las versiones de revisión indicadas con letras son las compilaciones estables más recientes).

Cuando se introduce una nueva versión secundaria, se retirarán la versión secundaria y la versión de revisión compatibles más antiguas. 15 días antes de la publicación de la nueva versión secundaria y la próxima retirada de la versión, se realiza un anuncio a través de los [canales de actualización de Azure][azure-update-channel]. En el ejemplo anterior en el que se publicó *1.11.x*, las versiones retiradas son *1.7.g* + *1.7.h*.

Al implementar un clúster de AKS en Azure Portal o con la CLI de Azure, el clúster siempre se establece en la versión secundaria n-1 y la revisión más reciente. Por ejemplo, si AKS es compatible con *1.11.x*, *1.10.a* + *1.10.b*, *1.9.c* + *1.9d*, *1.8.e* + *1.8F*, la versión predeterminada para nuevos clústeres es *1.10.b*.

## <a name="list-currently-supported-versions"></a>Lista de versiones admitidas actualmente

Para averiguar qué versiones están disponibles actualmente para su suscripción y región, utilice el comando [az aks get-versions][az-aks-get-versions]. En el ejemplo siguiente se enumeran las versiones de Kubernetes disponibles para la región *EastUS*:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

La salida es similar al ejemplo siguiente, que muestra que la versión de Kubernetes *1.12.4* es la más reciente disponible:

```
KubernetesVersion    Upgrades
-------------------  -----------------------
1.12.4               None available
1.11.6               1.12.4
1.11.5               1.11.6, 1.12.4
1.10.12              1.11.5, 1.11.6
1.10.9               1.10.12, 1.11.5, 1.11.6
1.9.11               1.10.9, 1.10.12
1.9.10               1.9.11, 1.10.9, 1.10.12
1.8.15               1.9.10, 1.9.11
1.8.14               1.8.15, 1.9.10, 1.9.11
```

## <a name="faq"></a>Preguntas más frecuentes

**¿Qué ocurre cuando un cliente actualiza un clúster de Kubernetes con una versión secundaria que no es compatible?**

Si se encuentra en la versión *n-4*, está fuera del SLO. Si se realiza correctamente la actualización de la versión n-4 a n-3, volverá a estar en el SLO. Por ejemplo: 

- Si las versiones compatibles de AKS son *1.10.a* + *1.10.b*, *1.9.c* + *1.9d*, *1.8.e* + *1.8F* y se encuentra en *1.7.g* o *1.7.h*, está fuera del SLO.
- Si la actualización de *1.7.g* o *1.7.h* a *1.8.e* o *1.8.f* se realiza correctamente, volverá a estar en el SLO.

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
