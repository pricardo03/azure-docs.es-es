---
title: Escalado del clúster del Explorador de datos de Azure para ajustarse a los cambios en la demanda
description: En este artículo se describen los pasos para escalar y reducir verticalmente un clúster de Azure Data Explorer en función de los cambios en la demanda.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: bc3f97c798f5e040908e8103c00d3f015f8c824d
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415341"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Administración del escalado vertical de clúster para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión del 100 %. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal. Un enfoque mejor es *escalar* un clúster, es decir, agregar y eliminar capacidad y CPU con los cambios en la demanda. Existen dos flujos de trabajo de escalado: la escalabilidad vertical y la horizontal. En este artículo se muestra cómo administrar el escalado de los clústeres.

1. Vaya al clúster y, en **Configuración**, seleccione **Escalar verticalmente**.

    Se le asigna una lista de las SKU disponibles. Por ejemplo, en la siguiente ilustración solo hay una SKU disponible: D14_V2.

    ![Escalado vertical](media/manage-cluster-scale-up/scale-up.png)

    D13_V2 está deshabilitada porque se trata de la SKU actual del clúster. L8 y L16 están deshabilitadas porque no están disponibles en la región donde se encuentra el clúster.

1. Para cambiar la SKU, seleccione la que quiera y presione el botón **Seleccionar**.

> [!NOTE]
> La escalada vertical del proceso puede tardar unos minutos y, durante ese tiempo se suspenderá el clúster. Tenga en cuenta que la reducción vertical puede perjudicar el rendimiento del clúster.

Ha realizado una operación de escalado o reducción verticales para el clúster de Azure Data Explorer. También puede hacer un [escalado horizontal del clúster](manage-cluster-scale-out.md), también conocido como escalado automático, para escalar dinámicamente en función de las métricas que especifique.

Si necesita ayuda por problemas relacionados con el escalado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
