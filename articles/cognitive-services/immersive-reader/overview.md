---
title: Acerca del Lector inmersivo
titlesuffix: Azure Cognitive Services
description: Información sobre el Lector inmersivo
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 38056f701b76f3fc2cf1f617a6977321445f78ff
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296910"
---
# <a name="what-is-immersive-reader"></a>¿Qué es Lector inmersivo?

El [Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de manera inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, estudiantes de idiomas y personas con diferencias de aprendizaje, como la dislexia.

Puede usar Lector inmersivo en la aplicación web mediante el SDK correspondiente.

## <a name="what-does-immersive-reader-do"></a>¿Qué hace el Lector inmersivo?

El Lector inmersivo está diseñado para hacer que la lectura sea más accesible para todos.

* Muestra el contenido en una vista de lectura mínima.

  ![Lector inmersivo](./media/immersive-reader.png)

* Muestra imágenes de las palabras más usadas.

  ![Diccionario de imágenes](./media/picture-dictionary.png)

* Resalta los sustantivos, verbos, adjetivos y adverbios.

  ![Elementos de la oración](./media/parts-of-speech.png)

* Lee el contenido en voz alta.

  ![Lectura en voz alta](./media/read-aloud.png)

* Traduce el contenido a otro idioma.

  ![Traducción](./media/translation.png)

* Divide las palabras en sílabas.

  ![Separación de sílabas](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>¿Cómo funciona el Lector inmersivo?

El Lector inmersivo es una aplicación web independiente que, cuando se invoca mediante el SDK de JavaScript para Lector inmersivo, aparece en la parte superior de la aplicación web existente en una`iframe`. Cuando se llama a la API para iniciar el Lector inmersivo, puede especificar el contenido que quiere mostrar en el Lector inmersivo. El SDK controla la creación y el estilo de `iframe` y la comunicación con el servicio back-end del Lector inmersivo, que procesa el contenido de los elementos de la oración, el texto a voz, la traducción, etc.

## <a name="next-steps"></a>Pasos siguientes

Introducción a Lector inmersivo:

* Vaya al [inicio rápido](./quickstart.md).
* Explore el [SDK del Lector inmersivo en GitHub](https://github.com/Microsoft/immersive-reader-sdk).
* Lea la [referencia del SDK del Lector inmersivo](./reference.md).