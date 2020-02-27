---
title: 'Plan de continuidad empresarial: QnA Maker'
titleSuffix: Azure Cognitive Services
description: El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: f920a789f1ec631c5d70863b10d8364b3eb81b6c
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650476"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Cómo crear un plan de continuidad empresarial para el servicio QnA Maker

El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.

![Plan de continuidad empresarial de QnA Maker](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

La idea de alto nivel como se representa anteriormente es la siguiente:

1. Configure dos [servicios QnA Maker](../How-To/set-up-qnamaker-service-azure.md) paralelos en [Regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Mantenga sincronizados los índices principal y secundario de Azure Search. Use el ejemplo de GitHub [aquí](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver cómo realizar una copia de seguridad de los índices de Azure y cómo restaurarlos.

3. Realice una copia de seguridad de Application Insights con la [exportación continua](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Una vez configuradas las pilas principal y secundaria, use [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) para configurar los dos puntos de conexión y establecer un método de enrutamiento.

5. Debe crear un certificado de capa de sockets seguros (SSL) para el punto de conexión de Traffic Manager. [Enlace el certificado SSL](https://docs.microsoft.com/azure/app-service/configure-ssl-bindings) en App Service.

6. Por último, use el punto de conexión de Traffic Manager del bot o de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Selección de la capacidad](./improve-knowledge-base.md)
