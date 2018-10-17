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
ms.date: 09/18/2018
ms.author: tomfitz
ms.openlocfilehash: c462334597b41b914b6a0a0e3c8a67ad97e2bd7a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295589"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Plantillas de Azure Resource Manager para Event Grid

En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Event Grid.

| | |
|-|-|
|**Suscripciones de Event Grid**||
| [Personalización de tema y suscripción con el punto de conexión de WebHook](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Implementa un tema personalizado de Event Grid. Crea una suscripción a ese tema personalizado que usa un punto de conexión de WebHook. |
| [Personalización de la suscripción a un tema con el punto de conexión de EventHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Crea una suscripción de Event Grid a un tema personalizado. La suscripción usa un centro de eventos para el punto de conexión. |
| [Suscripción a Azure o suscripción al grupo de recursos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Se suscribe a eventos para una suscripción al grupo de recursos o a Azure. El grupo de recursos que especifique como destino durante la implementación es el origen de los eventos. La suscripción usa Webhook para el punto de conexión. |
| [Cuenta y suscripción de Blob Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Implementa una cuenta de Azure Blob Storage y crea una suscripción a eventos de dicha cuenta. |
| | |
