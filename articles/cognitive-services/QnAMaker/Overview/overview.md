---
title: ¿Qué es QnA Maker?
titleSuffix: Azure Cognitive Services
description: QnA Maker es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a una pregunta formulada en lenguaje natural del usuario para proporcionar la mejor respuesta.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: 742c18815445b038e85c33a96743790491976945
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901114"
---
# <a name="what-is-qna-maker"></a>¿Qué es QnA Maker?

QnA Maker es un servicio de base de conocimiento (KB) de preguntas y respuestas que aplica inteligencia de aprendizaje automático personalizado a una pregunta formulada en lenguaje natural del usuario para determinar la mejor respuesta.

QnA Maker le permite alimentar un servicio basado en la nube a partir de su contenido semiestructurado, como documentos de preguntas frecuentes, direcciones URL, manuales de productos, y preguntas y respuestas personalizadas. El [portal web](https://qnamaker.ai) es fácil de usar y le permite crear, administrar, entrenar y publicar su servicio sin tener ninguna experiencia como desarrollador. Una vez que el servicio se publica en un punto de conexión, una aplicación cliente como un bot de chat puede administrar la conversación con un usuario para recibir preguntas y ofrecer las respuestas. 

![Información general](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Principales procesos de QnA Maker

QnA Maker proporciona dos servicios clave para los datos:

* **Extracción**: los datos de preguntas y respuestas estructurados se extraen de [orígenes de datos](../Concepts/data-sources-supported.md) semiestructurados como preguntas frecuentes y manuales de productos. Esta extracción se puede realizar como parte de la [creación](https://aka.ms/qnamaker-docs-createkb) de la base de conocimiento o, posteriormente, como parte del proceso de edición.

* **Coincidencia**: una vez que la base de conocimientos se ha [entrenado y probado](https://aka.ms/qnamaker-docs-trainkb), se [publica](https://aka.ms/qnamaker-docs-publishkb). Esto habilita un punto de conexión a la base de conocimiento de QnA Maker, que puede usar en su bot o aplicación cliente. Este punto de conexión acepta una pregunta del usuario y responde con la mejor respuesta de la base de conocimiento, junto con una puntuación de confianza para la coincidencia.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>Arquitectura de QnA Maker

QnA Maker consta de los siguientes servicios de API:

1. **Servicios de administración de QnA Maker**: la experiencia de administración para una base de conocimiento de QnA Maker, que incluye la creación inicial, actualización, entrenamiento y publicación. Estas actividades se pueden realizar mediante el [portal](https://qnamaker.ai) o las [API de administración](https://aka.ms/qnamaker-v4-apis). 

2. **Servicio de predicción de QnA Maker**: este se implementa en la suscripción de Azure en la región que especifique. El contenido de la base de conocimiento del cliente se almacena en [Azure Search](https://azure.microsoft.com/services/search/) y el punto de conexión se implementa como una instancia de [App Service](https://azure.microsoft.com/services/app-service/). También puede elegir implementar un recurso de [Application Insights](https://azure.microsoft.com/services/application-insights/) para el análisis.

![Arquitectura](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Servicios destacados

- Una experiencia **sin código** completa para [crear un bot de preguntas frecuentes](https://aka.ms/qnamaker-docs-create-faqbot).
- **No hay límite de red para las predicciones**. Pague por el hospedaje del servicio y no por el número de transacciones. Consulte la [página de precios](https://aka.ms/qnamaker-docs-pricing) para más información.
- **Escale según sea necesario**. Seleccione las SKU apropiadas de los componentes individuales que se adapten a su escenario. Consulte cómo [elegir la capacidad](https://aka.ms/qnamaker-docs-capacity) para el servicio QnA Maker.
- **Compatibilidad completa de datos**. Los componentes del servicio de predicción se implementan en la suscripción de Azure y dentro del límite de cumplimiento.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un servicio QnA Maker](../how-to/set-up-qnamaker-service-azure.md)
