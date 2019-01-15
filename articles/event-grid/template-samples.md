---
title: 'Plantillas de ejemplo de Azure Resource Manager: Event Grid | Microsoft Docs'
description: Plantillas de ejemplo de Azure Resource Manager para Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 01/04/2019
ms.author: tomfitz
ms.openlocfilehash: 788d23c7bddd90c1e12a118742c651eb9759529c
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062968"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Plantillas de Azure Resource Manager para Event Grid

Para la sintaxis y las propiedades de JSON que se usan en una plantilla, consulte [Tipos de recursos de Microsoft.EventGrid](/azure/templates/microsoft.eventgrid/allversions). En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Event Grid.

| | |
|-|-|
|**Suscripciones de Event Grid**||
| [Personalización de tema y suscripción con el punto de conexión de WebHook](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Implementa un tema personalizado de Event Grid. Crea una suscripción a ese tema personalizado que usa un punto de conexión de WebHook. |
| [Personalización de la suscripción a un tema con el punto de conexión de EventHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Crea una suscripción de Event Grid a un tema personalizado. La suscripción usa un centro de eventos para el punto de conexión. |
| [Suscripción a Azure o suscripción al grupo de recursos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Se suscribe a eventos para una suscripción al grupo de recursos o a Azure. El grupo de recursos que especifique como destino durante la implementación es el origen de los eventos. La suscripción usa Webhook para el punto de conexión. |
| [Cuenta y suscripción de Blob Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Implementa una cuenta de Azure Blob Storage y crea una suscripción a eventos de dicha cuenta. |
| | |
