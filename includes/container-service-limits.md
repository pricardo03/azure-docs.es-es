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
ms.openlocfilehash: 23c25953d2f493d2dd799bfd11dbbb69db002d1b
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55735988"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Máximo de clústeres por suscripción | 100 |
| Máximo de nodos por clúster | 100 |
| Número máximo de pods por nodo: [Redes básicas][basic-networking] con Kubenet | 110 |
| Número máximo de pods por nodo: [Redes avanzadas][advanced-networking] con CNI de Azure | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup> Cuando implementa un clúster de AKS con la CLI de Azure o una plantilla de Resource Manager, este valor es configurable en hasta **110 pods por nodo**. No puede configurar un máximo de pods por nodo después de haber implementado un clúster AKS, o si implementa un clúster mediante Azure Portal.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
