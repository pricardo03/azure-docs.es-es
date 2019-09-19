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
ms.openlocfilehash: a2729af6a689daa551fc01f585324d53a8770a9b
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "67186678"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Número máximo de clústeres por suscripción | 100 |
| Número máximo de nodos por clúster | 100 |
| Número máximo de pods por nodo: [Redes básicas][basic-networking] con Kubenet | 110 |
| Número máximo de pods por nodo: [Conexiones de red avanzadas][advanced-networking] con la interfaz de red de contenedor de Azure | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Azure Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup>Cuando implementa un clúster de Azure Kubernetes Service (AKS) con la CLI de Azure o una plantilla de Resource Manager, este valor es configurable en hasta 250 pods por nodo. No puede configurar un número máximo de pods por nodo después de haber implementado un clúster AKS, o si implementa un clúster mediante Azure Portal.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
