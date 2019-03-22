---
title: archivo de inclusión
description: archivo de inclusión
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 62eb75ef18d3ac81be65783e57c21c0aefd7a429
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554627"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Clústeres máximos por suscripción | 100 |
| Nodos máximos por clúster | 100 |
| Máximos pods por nodo: [Redes básicas][basic-networking] con Kubenet | 110 |
| Máximos pods por nodo: [Conexiones de red avanzadas] [ advanced-networking] con la interfaz de red de contenedor de Azure | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Azure Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup>al implementar un clúster de Azure Kubernetes Service (AKS) con la CLI de Azure o una plantilla de Resource Manager, este valor es configurable hasta 110 pods por nodo. No se puede configurar máximos pods por nodo después de que ya ha implementado un clúster de AKS, o si implementa un clúster mediante el portal de Azure.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
