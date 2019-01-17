---
title: 'Glosario: QnA Maker'
titleSuffix: Azure Cognitive Services
description: El servicio QnA Maker tiene muchos nuevos términos de aprendizaje automático y procesamiento de lenguaje natural, además de términos específicos del servicio. Esta lista le ayudará a comprender los términos.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 73504cff7a35dfe475b2de90a6b38a9527a800e1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259945"
---
# <a name="glossary-for-qna-maker-knowledge-base-and-service"></a>Glosario del servicio y la base de conocimiento de QnA Maker

## <a name="qna-maker-service"></a>Servicio QnA Maker
Un servicio QnA Maker es un requisito previo para empezar a usar QnA Maker. La compra de nivel de QnA Maker configura los recursos de su suscripción de Azure para crear y administrar su base de conocimiento. Todas las cuentas de usuario de QnA Maker pueden crear varios servicios QnA Maker en su suscripción de Azure.

## <a name="knowledge-base"></a>Knowledge Base
Una base de conocimientos es el repositorio de preguntas y respuestas que se crea, se mantiene y se usa en QnA Maker. Cada nivel QnA Maker se puede usar para varias bases de conocimiento.

## <a name="endpoint"></a>Punto de conexión
Un punto de conexión HTTP basado en REST que presta servicio a una base de conocimientos que se puede integrar en una aplicación, normalmente un bot de chat. 

## <a name="test-knowledge-base"></a>Base de conocimientos de prueba
Una base de conocimientos tiene dos estados: prueba y publicada. La base de conocimientos de prueba es la versión que se edita, se guarda y se prueba para conocer la precisión e integridad de las respuestas. Los cambios realizados en la base de conocimientos de prueba no afectan al usuario final de la aplicación o bot de chat.

## <a name="published-knowledge-base"></a>Base de conocimientos publicada
Una base de conocimientos tiene dos estados: prueba y publicada.  La base de conocimiento publicada es la versión que se utiliza en la aplicación o en el bot de chat. La acción de publicar una base de conocimientos coloca el contenido de la base de conocimientos de prueba en la versión publicada de la base de conocimientos. Dado que la base de conocimientos publicada es la versión que la aplicación usa a través del punto de conexión, es preciso asegurarse de que el contenido es correcto y que se ha probado profusamente.

## <a name="query"></a>Consultar
Una consulta de usuario es la pregunta que el usuario final o el evaluador formulan en la base de conocimientos. A menudo la consulta está en un formato de lenguaje natural o se usan unas pocas palabras clave que representan la pregunta.

## <a name="response"></a>Response
La respuesta es la respuesta que se recupera de la base de conocimientos, en función de la coincidencia más alta para una consulta de usuario dada.

## <a name="confidence-score"></a>Puntuación de confianza
La puntuación de confianza de la respuesta es un valor numérico entre 0 y 100, siendo 100 coincidencia exacta entre la consulta del usuario y una pregunta de la base de conocimientos, que indica si la respuesta que se ha proporcionado es la correcta y adecuada para una consulta de usuario determinada. Las respuestas se suelen clasificar por la puntuación de confianza y la de mayor puntuación es la que se sirve como respuesta predeterminada.
