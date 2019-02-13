---
title: Escalado del clúster del Explorador de datos de Azure para ajustarse a los cambios en la demanda
description: En este artículo se describen los pasos para escalar y reducir horizontalmente un clúster del Explorador de datos de Azure en función de los cambios en la demanda.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 213a49d87d5e9f801bb17604a322c231a4e3dee2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735318"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Administración del escalado de clúster para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión del 100 %. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal. Un enfoque mejor es *escalar* un clúster, es decir, agregar y eliminar capacidad y CPU con los cambios en la demanda. En este artículo se muestra cómo administrar el escalado de clústeres.

Vaya al clúster y, en **Configuración**, seleccione **Escalar verticalmente**.

Se le dará la lista de las SKU que están disponibles. Puede elegir en la lista de tarjetas habilitadas. Por ejemplo, en la ilustración siguiente hay solo una SKU que se puede elegir en D14_vs.

![Escalado vertical](media/manage-cluster-scale-up/scale-up.png)

D13_v2 está deshabilitada porque se trata de la SKU actual del clúster. L8 y L16 están deshabilitadas porque no están disponibles en la región donde está el clúster.

Para cambiar una SKU no tiene más que hacer clic en la SKU que le gustaría usar y luego hacer clic en el botón **Seleccionar**.

[!NOTE] La escalada vertical del proceso puede tardar unos minutos y, durante ese tiempo se suspenderá el clúster. Tenga en cuenta que la reducción vertical puede perjudicar el rendimiento del clúster.

Si necesita ayuda por problemas relacionados con el escalado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).