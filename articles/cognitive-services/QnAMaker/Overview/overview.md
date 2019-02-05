---
title: ¿Qué es QnA Maker?
titleSuffix: Azure Cognitive Services
description: QnA Maker es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a una pregunta formulada en lenguaje natural del usuario para proporcionar la mejor respuesta.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 01/24/2019
ms.author: tulasim
ms.openlocfilehash: 1295d071fa3ad99aabd2649f7b7e0b7fe1f86584
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226866"
---
# <a name="what-is-qna-maker"></a>¿Qué es QnA Maker?

QnA Maker es un servicio de API basado en la nube que crea una capa de conversación, preguntas y respuestas mediante los datos. 

QnA Maker permite crear una base de conocimiento a partir de su contenido semiestructurado, como preguntas frecuentes, direcciones URL, manuales de producto, documentos de soporte técnico y preguntas y respuestas personalizadas. El servicio QnA Maker responde a preguntas en el lenguaje natural de los usuarios, para lo que las hace coincidir con la mejor respuesta posible de los QnA de su base de conocimiento.

El [portal web](https://qnamaker.ai) es fácil de usar y le permite crear, administrar, entrenar y publicar su servicio sin tener ninguna experiencia como desarrollador. Una vez que el servicio se publica en un punto de conexión, una aplicación cliente como un bot de chat puede administrar la conversación con un usuario para recibir preguntas y ofrecer las respuestas. 

![Información general](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Principales procesos de QnA Maker

QnA Maker proporciona dos servicios clave para los datos:

* **Extracción**: los datos de preguntas y respuestas estructurados se extraen de [orígenes de datos](../Concepts/data-sources-supported.md) estructurados y semiestructurados como preguntas frecuentes y manuales de producto. Esta extracción se puede realizar como parte de la [creación](https://aka.ms/qnamaker-docs-createkb) de la base de conocimiento o, posteriormente, como parte del proceso de edición.

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

La arquitectura de QnA Maker consta de los dos siguientes componentes:

1. **Servicios de administración de QnA Maker**: la experiencia de administración para una base de conocimiento de QnA Maker, que incluye la creación inicial, la actualización, el entrenamiento y la publicación. Estas actividades se pueden realizar mediante el [portal](https://qnamaker.ai) o las [API de administración](https://aka.ms/qnamaker-v4-apis). 

2. **Datos y runtime de QnA Maker**: se implementa en la suscripción de Azure de la región que especifique. El contenido de la base de conocimiento se almacena en [Azure Search](https://azure.microsoft.com/services/search/) y el punto de conexión se implementa como una instancia de [App Service](https://azure.microsoft.com/services/app-service/). También puede elegir implementar un recurso de [Application Insights](https://azure.microsoft.com/services/application-insights/) para el análisis.

![Arquitectura](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Servicios destacados

- Una experiencia **sin código** completa para [crear un bot de preguntas frecuentes](https://aka.ms/qnamaker-docs-create-faqbot).
- **No hay límite de red para las predicciones**. Pague por el hospedaje del servicio y no por el número de transacciones. Consulte la [página de precios](https://aka.ms/qnamaker-docs-pricing) para más información.
- **Escale según sea necesario**. Seleccione las SKU apropiadas de los componentes individuales que se adapten a su escenario. Consulte cómo [elegir la capacidad](https://aka.ms/qnamaker-docs-capacity) para el servicio QnA Maker.
- **Compatibilidad completa de datos**. Los componentes del servicio de predicción se implementan en la suscripción de Azure y dentro del límite de cumplimiento.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un servicio QnA Maker](../how-to/set-up-qnamaker-service-azure.md)
