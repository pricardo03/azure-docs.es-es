---
title: Información general sobre Azure sin servidor | Microsoft Docs
description: Más información sobre la creación de soluciones eficaces en la nube sin preocuparse por la infraestructura
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 9cd1946d8fa670764bcc95b82298d07cc70417bd
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191330"
---
# <a name="overview-azure-serverless-with-azure-logic-apps-and-azure-functions"></a>Información general: Azure sin servidor con Azure Logic Apps y Azure Functions

Las aplicaciones [sin servidor](https://azure.microsoft.com/solutions/serverless/) proporcionan las ventajas de aumento en la velocidad de desarrollo, reducción del código necesario y simplicidad con la escala.  En este artículo se analizan los diferentes atributos de las soluciones sin servidor y las ofertas de Azure sin servidor.

## <a name="what-is-serverless"></a>¿Qué significa sin servidor?

Sin servidor no significa que no haya ningún servidor, sino que el desarrollador no tiene que preocuparse por los servidores.  Gran parte del desarrollo tradicional de aplicaciones es dar respuesta a preguntas en torno a las soluciones de escalado, hospedaje y supervisión para satisfacer las demandas de la aplicación.  Las soluciones sin servidor se ocupan de estas preguntas como parte de la solución.  Además, las aplicaciones sin servidor se facturan según un plan de consumo.  Si la aplicación no se usa nunca, nunca se incurrirá en ningún gasto.  Estas características permiten a los desarrolladores centrarse únicamente en la lógica de negocios de la solución.

Los principales servicios de Azure en torno al concepto sin servidor son [Azure Functions](https://azure.microsoft.com/services/functions/) y [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Ambas solucione siguen los principios anteriormente mencionados y permiten a los desarrolladores crear sólidas aplicaciones de nube con muy poco código.

## <a name="what-are-azure-functions"></a>¿Qué es Azure Functions?

Azure Functions es una solución para ejecutar fácilmente pequeños fragmentos de código, o "funciones", en la nube. Simplemente, puede escribir el código que necesita para el problema en cuestión, sin preocuparse de toda la aplicación o la infraestructura para ejecutarlo. Functions puede hacer que el desarrollo sea aún más productivo y, además, le permite utilizar el lenguaje de desarrollo que prefiera, como C#, F#, Node.js, Python o PHP. Pague solo por el tiempo que se ejecuta el código y que se escala Azure según sea necesario.

Si desea comenzar de inmediato y empezar a trabajar con Azure Functions, comience con el artículo [Creación de su primera función de Azure](../azure-functions/functions-create-first-azure-function.md). Si busca información más técnica acerca de las Funciones, consulte [Referencia para desarrolladores de Funciones de Azure](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>¿Qué es Azure Logic Apps?

Azure Logic Apps ofrece una manera de simplificar e implementar flujos de trabajo e integraciones escalables en la nube. Proporciona un diseñador visual para modelar y automatizar el proceso en una serie de pasos denominada flujo de trabajo.  Existen [muchos conectores](../connectors/apis-list.md) en servicio locales y en la nube para conectar rápidamente una aplicación sin servidor a otras API.  Una aplicación lógica comienza con un desencadenador (como cuando se agrega una cuenta a Dynamics CRM) y la activación puede empezar muchas combinaciones, acciones, conversiones y lógicas de condiciones.  Logic Apps es una buena opción al orquestar diferentes instancias de Azure Functions en un proceso, en especial cuando el proceso requiere la interacción con un sistema o una API externos.

Para empezar a trabajar con Logic Apps, comience por la [creación de su primera aplicación lógica](quickstart-create-first-logic-app-workflow.md).  Si busca información más técnica acerca de Logic Apps, consulte la [referencia para desarrolladores](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>¿Cómo puedo crear e implementar aplicaciones sin servidor en Azure?

Azure proporciona un conjunto abundante de herramientas para el desarrollo, la implementación y la administración de aplicaciones sin servidor.  Las aplicaciones se pueden crear directamente en el portal de Azure o con [herramientas de Visual Studio](logic-apps-serverless-get-started-vs.md).  Una vez que se ha desarrollado una aplicación, se puede [implementar al instante](logic-apps-create-deploy-template.md).  Azure también proporciona supervisión para aplicaciones sin servidor.  Esta supervisión se puede acceder desde el portal de Azure, a través de la API o SDK o herramientas integradas con registros de Azure Monitor y Application Insights.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la creación de una aplicación sin servidor en Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Creación de un panel de Customer Insights con una aplicación sin servidor](logic-apps-scenario-social-serverless.md)
* [Creación de una plantilla de implementación para una aplicación lógica](logic-apps-create-deploy-template.md)
