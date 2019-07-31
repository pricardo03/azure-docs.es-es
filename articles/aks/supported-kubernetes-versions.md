---
title: Versiones de Kubernetes compatibles en Azure Kubernetes Service
description: Obtener información sobre la directiva de soporte técnico de la versión de Kubernetes y el ciclo de vida de los clústeres en Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: a4726444e2103228cd26be3d3a6d516b31e315ee
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304328"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)

La Comunidad de Kubernetes libera versiones secundarias aproximadamente cada tres meses. Estas versiones incluyen nuevas características y mejoras. Versiones de revisión son más frecuentes (a veces semanales) y solo están previstas para correcciones de errores críticos en una versión secundaria. Estas versiones de revisión incluyen correcciones de vulnerabilidades de seguridad o errores importantes que afectan a un gran número de clientes y productos que se ejecutan en producción basada en Kubernetes.

AKS tiene como objetivo certificar y publicar nuevas versiones de Kubernetes dentro de los 30 días posteriores a la versión anterior, con sujeción a la estabilidad de la versión.

## <a name="kubernetes-versions"></a>Versiones de Kubernetes

Kubernetes usa el esquema de versiones estándar de [Versionamiento Semántico](https://semver.org/). Esto significa que cada versión de Kubernetes sigue este esquema de numeración:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
  1.13.7
```

Cada número en la versión indica compatibilidad general con la versión anterior:

* Las versiones principales cambian cuando cambia una API incompatible o la compatibilidad con versiones anteriores deja de funcionar.
* Las versiones secundarias cambian cuando se hacen cambios de funcionalidad que son compatibles con otras versiones anteriores secundarias.
* Las versiones de revisión cambian cuando se hacen correcciones de errores compatibles con versiones anteriores.

En general, los usuarios deben procurar ejecutar la última versión de revisión de la versión secundaria que están ejecutando, por ejemplo, si su clúster de producción usa la versión *1.13.6* y *1.13.7* es la última versión de revisión disponible para la serie *1.13*, debe actualizar a *1.13.7* tan pronto como pueda asegurarse de que el clúster esté completamente revisado y sea compatible.

## <a name="kubernetes-version-support-policy"></a>Directiva de soporte técnico de versión de Kubernetes

AKS es compatible con cuatro versiones secundarias de Kubernetes:

* La versión secundaria actual que está publicada en AKS (N)
* Tres versiones secundarias anteriores. Cada versión secundaria compatible también admite dos revisiones estables.

Esto se conoce como "N-3"-(N [versión más reciente] - 3 [versiones secundarias]).

Por ejemplo, si AKS presenta *1.13.x* hoy, también se proporciona compatibilidad para las versiones siguientes:

Nueva versión secundaria    |    Lista de versiones admitidas
-----------------    |    ----------------------
1.13.x               |    1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b

Donde "x" y ".a" y ".b" son las versiones de revisión representativas.

Para obtener más información sobre las comunicaciones con respecto a los cambios de versión y las expectativas, consulte "Comunicaciones" a continuación.

Cuando se introduce una nueva versión secundaria, la versión secundaria y la versión de revisión compatibles más antiguas quedan obsoletas y se retiran. Por ejemplo, si la lista de versiones que se admiten actualmente es:

```
1.12.a
1.12.b
1.11.a
1.11.b
1.10.a
1.10.b
1.9.a
1.9.b
```

Y AKS publica 1.13.x, esto significa que se quitarán las versiones 1.9. x (todas las versiones 1.9) y se quedarán sin soporte.

> [!NOTE]
> Tenga en cuenta que si los clientes ejecutan una versión incompatible de Kubernetes, se les pedirá que actualicen al solicitar soporte técnico para el clúster. Los clústeres que ejecutan versiones de Kubernetes no admitidas no están cubiertos por el [las directivas de soporte técnico de AKS](https://docs.microsoft.com/azure/aks/support-policies).


Además de lo anterior sobre las versiones secundarias, AKS admite las dos últimas versiones de *revisión** de una determinada versión secundaria. Por ejemplo, dadas las siguientes versiones admitidas:

```
Current Supported Version List
------------------------------
1.12.1, 1.12.2, 1.11.4, 1.11.5
```

Si anteriormente Kubernetes publicó 1.12.3 y 1.11.6 y AKS publica esas versiones de revisión, las versiones de revisión más antiguas quedan en desuso y se eliminan, y la lista de versiones admitidas pasa a ser:

```
New Supported Version List
----------------------
1.12.*2*, 1.12.*3*, 1.11.*5*, 1.11.*6*
```

> [!NOTE]
> Los clientes no deben fijar la creación de clústeres, la integración continua u otros trabajos automatizados a versiones de revisión específicas. 

### <a name="communications"></a>Comunicaciones

* Para nuevas versiones **secundarias** de Kubernetes
  * Todos los usuarios son notificados públicamente de la nueva versión y de la versión que se eliminará.
  * Cuando se publica una nueva versión de revisión, la versión de revisión más antigua se quita al mismo tiempo.
  * Los clientes tienen **60 días** desde la fecha de notificación pública para actualizar a una versión secundaria compatible.
* Para nuevas versiones de **revisión** de Kubernetes
  * Se notifica a todos los usuarios la publicación de la nueva versión de revisión y la necesidad de actualizar a la versión de revisión más reciente.
  * Los usuarios tienen **30 días** para actualizar a una versión de revisión más reciente que sea compatible. Los usuarios tienen **30 días** para actualizar a una versión de revisión compatible antes de quite la más antigua.

AKS define la "publicación" como disponibilidad general, habilitación en todas las mediciones de calidad de servicio/SLO y disponibilidad en todas las regiones.

> [!NOTE]
> Se notifica a los usuarios las publicaciones y obsolescencias de las versiones de Kubernetes; cuando una versión secundaria queda en desuso o se elimina, se da 60 a los usuarios para que actualicen a una versión admitida. En el caso de las versiones de revisión, los clientes reciben 30 días para actualizar a una versión compatible.

Las notificaciones se envían a través de:

* [Notas de la versión de AKS](https://aka.ms/aks/releasenotes)
* Notificaciones de Azure Portal
* [Canal de actualización de Azure][azure-update-channel]

### <a name="policy-exceptions"></a>Excepciones a la directiva

AKS se reserva el derecho de agregar o eliminar versiones nuevas/existentes donde se hayan identificado uno o varios problemas de seguridad o errores críticos que afecten a la producción sin previo aviso.

Las versiones de revisión específicas se pueden omitir, o su implantación puede verse acelerada en función de la gravedad del error o el problema de seguridad.

### <a name="azure-portal-and-cli-default-versions"></a>Versiones predeterminadas de Azure Portal y la CLI de Azure

Al implementar un clúster de AKS en Azure Portal o con la CLI de Azure, el clúster siempre se establece en la versión secundaria n-1 y la revisión más reciente. Por ejemplo, si AKS es compatible con *1.13.x*, *1.12.a* + *1.12.b*, *1.11.a* + *1.11.b*, *1.10.a* + *1.10b*, la versión predeterminada para los nuevos clústeres es *1.12.b*.

El valor predeterminado de AKS es N-1 (secundaria.últimaRevisión, por ejemplo, 1.12.b) para proporcionar a los clientes una versión conocida, estable y revisada de forma predeterminada.

## <a name="list-currently-supported-versions"></a>Lista de versiones admitidas actualmente

Para averiguar qué versiones están disponibles actualmente para su suscripción y región, utilice el comando [az aks get-versions][az-aks-get-versions]. En el ejemplo siguiente se enumeran las versiones de Kubernetes disponibles para la región *EastUS*:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

La salida es similar al ejemplo siguiente, que muestra que la versión de Kubernetes *1.13.5* es la más reciente disponible:

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

Si la suya es la versión *n-4*, se encuentra fuera de soporte técnico y se le pedirá que actualice. Si su actualización de la versión n-4 a n-3 se completa correctamente, estará dentro de nuestras directivas de soporte técnico. Por ejemplo:

- Si las versiones compatibles de AKS son *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c* + *1.11d* y *1.10.e* + *1.10f* y la suya es *1.9.g* o *1.9.h*, está fuera del soporte técnico.
- Si la actualización de *1.9.g* o *1.9.h* a *1.10.e* o *1.10.f* se realiza correctamente, estará de nuevo dentro de nuestras directivas de soporte técnico.

Las actualizaciones a las versiones anteriores a *n-4* no se admiten. En esos casos, se recomienda a los clientes que creen nuevos clústeres de AKS y vuelvan a implementar sus cargas de trabajo.

**¿Qué significa "fuera de soporte técnico"?**

"Fuera de soporte técnico" significa que la versión que está ejecutando está fuera de la lista de versiones admitidas, y se le pedirá que actualice el clúster a una versión compatible cuando solicite soporte técnico. Además, AKS no ofrece ningún entorno de ejecución ni otras garantías para los clústeres que estén fuera de la lista de versiones admitidas.

**¿Qué ocurre cuando un cliente escala un clúster de Kubernetes con una versión secundaria que no es compatible?**

Para las versiones secundarias que no son compatibles con AKS, el escalado sigue funcionando sin ningún problema.

**¿Puede un cliente permanecer en una versión de Kubernetes para siempre?**

Sí. No obstante, si el clúster no está en una de las versiones compatibles con AKS, el clúster está fuera de las directivas de soporte técnico de AKS. Azure no actualiza el clúster automáticamente ni lo elimina.

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
