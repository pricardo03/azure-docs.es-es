---
title: Aprenda los conceptos de la herramienta de revisión - Content Moderator
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de la herramienta de revisión de Content Moderator, un sitio Web que coordina un combinado de inteligencia artificial y el esfuerzo de moderación de revisión humana.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.date: 03/15/2019
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.author: sajagtap
ms.openlocfilehash: b7ec997fd3e9bfe294050893d80fd57a96a47aae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270011"
---
# <a name="content-moderator-review-tool"></a>Herramienta de Content Moderator Review

Azure Content Moderator proporciona servicios para combinar la moderación de contenido de aprendizaje de máquina con revisiones humanas y el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) sitio Web es un front-end fácil de usar que ofrece acceso detallado a estos servicios.

![El panel de la herramienta de revisión en un explorador](./images/0-dashboard.png)

## <a name="what-it-does"></a>Qué hace

El [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com), cuando se usa junto con la moderación de las API, permite realizar las tareas siguientes en el proceso de moderación de contenido:

- Use un conjunto de herramientas para moderar contenido en varios formatos (texto, imagen y vídeo).
- Automatizar la creación de humanos [revisa](../review-api.md#reviews) cuando las API de moderación da como resultado vienen en.
- Asignar o remitir las revisiones de contenido para varios equipos de revisión, organizadas por nivel de categoría o la experiencia de contenido.
- Usar filtros de lógica personalizada o predeterminada ([flujos de trabajo](../review-api.md#workflows)) para ordenar y realizar un seguimiento de contenido, sin escribir ningún código.
- Use [conectores](./configure.md#connectors) para procesar contenido con Microsoft PhotoDNA, análisis de texto y Face API, además de las API del moderador de contenido.
- Crear su propio conector para crear flujos de trabajo para cualquier API o el proceso empresarial.
- Obtenga métricas clave de rendimiento de los procesos de moderación de contenido.

## <a name="review-tool-dashboard"></a>Panel de la herramienta de revisión

En el **panel** ficha, puede ver las métricas clave de contenido revisiones que se realiza dentro de la herramienta. Ver el número de total, completa y en espera de las revisiones de la imagen, texto y contenido de vídeo. También puede ver el desglose de los usuarios y equipos que se han completado las revisiones, así como las etiquetas de moderación en el que se han aplicado.

![Visualización del panel](images/0-dashboard.png)

## <a name="review-tool-credentials"></a>Credenciales de la herramienta de revisión

Cuando se registra con el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com), se pedirá que seleccione una región de Azure para que la cuenta. Esto es porque el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) genera una clave de prueba gratuita para los servicios de Azure Content Moderator; necesitará esta clave para tener acceso a cualquiera de los servicios de una llamada de REST o SDK de cliente. Puede ver su clave y la API de dirección URL del extremo seleccionando **configuración** > **credenciales**.

![Credenciales de Content Moderator](images/settings-6-credentials.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte [configurar la herramienta de revisión](./configure.md) para obtener información sobre cómo tener acceso a recursos de la herramienta de revisión y cambiar la configuración.