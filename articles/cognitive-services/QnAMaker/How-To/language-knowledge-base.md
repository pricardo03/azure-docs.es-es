---
title: 'Conceptos del lenguaje: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker admite el contenido de la base de conocimiento en varios idiomas. Sin embargo, cada servicio QnA Maker se debe reservar para un único idioma. La primera base de conocimiento creada, cuyo destino es un servicio QnA Maker concreto, establece el idioma de dicho servicio.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: diberry
ms.openlocfilehash: 849c919950c57a1df3b0fb76021de6e10254c7b4
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286382"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Compatibilidad de idioma del contenido de la base de conocimiento para QnA Maker

El idioma del servicio se selecciona al crear la primera base de conocimiento en el recurso. Todas las bases de conocimiento adicionales del recurso deben estar en el mismo idioma. 

El idioma determina la relevancia de los resultados que QnA Maker proporciona en respuesta a las consultas de los usuarios.

## <a name="one-language-for-all-knowledge-bases-in-resource"></a>Un idioma para todas las bases de conocimiento del recurso

QnA Maker le permite seleccionar el idioma del servicio QnA al crear la primera base de conocimiento. Todas las bases de conocimiento de un recurso QnA Maker deben estar en el mismo idioma. Dicho idioma no se puede cambiar.

La creación de bases de conocimiento en distintos idiomas en un recurso afecta negativamente a la relevancia de los resultados que QnA Maker proporciona en respuesta a las consultas de los usuarios.

Consulte una lista de los [idiomas compatibles](../overview/language-support.md#languages-supported) y cómo afectan [a la coincidencia y relevancia](#query-matching-and-relevance). 

## <a name="select-language-when-creating-first-knowledge-base"></a>Selección del idioma al crear la primera base de conocimiento

La selección del idioma forma parte de los pasos necesarios para crear la primera base de conocimiento en un recurso. 

![Captura de pantalla del portal de QnA Maker, donde se selecciona el idioma de la primera base de conocimiento](../media/language-support/select-language-when-creating-knowledge-base.png)

## <a name="query-matching-and-relevance"></a>Coincidencia y relevancia de las consultas
QnA Maker depende de los [analizadores de idioma de Azure Search](https://docs.microsoft.com/rest/api/searchservice/language-support) para proporcionar resultados. 

Aunque las funcionalidades de Azure Search están en el mismo nivel que los idiomas admitidos, QnA Maker tiene un clasificador adicional que está por encima de los resultados de búsqueda de Azure. En este modelo de clasificador, se usan características semánticas y basadas en palabras especiales en los siguientes idiomas. 

|Idiomas con un clasificador adicional|
|--|
|Chino|
|Checo|
|Neerlandés|
|English|
|Francés|
|Alemán|
|Húngaro|
|Italiano|
|Japonés|
|Coreano|
|Polaco|
|Portugués|
|Español|
|Sueco|

Esta clasificación adicional es un elemento de trabajo interno del clasificador de QnA Maker.

## <a name="verify-language"></a>Comprobación del idioma

Puede comprobar el idioma del recurso de QnA Maker desde la página de configuración del servicio de QnA Maker.

![Captura de pantalla de la página Configuración del servicio del portal de QnA Maker](../media/language-support/language-knowledge-base.png) 


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un bot QnA con Azure Bot Service](../Tutorials/create-qna-bot.md)
