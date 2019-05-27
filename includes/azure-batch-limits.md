---
title: archivo de inclusión
description: archivo de inclusión
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 9ffb02fce41e8805dfccf1dfd6e982cf107039ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127538"
---
| **Recurso** | **Límite predeterminado** | **Límite máximo** |
| --- | --- | --- |
| Cuentas de Azure Batch por región y suscripción | 1-3 |50 |
| Núcleos dedicados por cuenta de Batch | 10-100 | N/A<sup>1</sup> |
| Núcleos de baja prioridad por cuenta de Batch | 10-100 | N/A<sup>2</sup> |
| Trabajos activos y programaciones de trabajos<sup>3</sup> por cuenta de Batch | 100-300 | 1,000<sup>4</sup> |
| Grupos por cuenta de Batch | 20-100 | 500<sup>4</sup> |

> [!NOTE]
> Los límites predeterminados varían según el tipo de suscripción que se use para crear una cuenta de Batch. Las cuotas de núcleos mostradas son para las cuentas de Batch del modo de servicio Batch. [Vea las cuotas de su cuenta de Batch](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup>se puede aumentar el número de núcleos dedicados por cuenta de Batch, pero se ha especificado el número máximo. Para más información acerca de las opciones de aumento, póngase en contacto con soporte técnico de Azure.

<sup>2</sup>se puede aumentar el número de núcleos de baja prioridad por cuenta de Batch, pero se ha especificado el número máximo. Para más información acerca de las opciones de aumento, póngase en contacto con soporte técnico de Azure.

<sup>3</sup>los trabajos completados ni las programaciones de trabajos no están limitadas.

<sup>4</sup>para solicitar un aumento más allá de este límite, póngase en contacto con soporte técnico de Azure.
