---
title: Moderación de contenido asistida automáticamente y revisiones humanas en Content Moderator | Microsoft Docs
description: Se describe cómo las máquinas y los humanos proporcionan juntos los mejores resultados para la moderación de contenido.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.date: 03/16/2018
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.author: sajagtap
ms.openlocfilehash: 1c08b30c0e59aac21d24f75d88530ba51b67d6b4
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023940"
---
# <a name="human-in-the-loop"></a>Revisión humana en bucle

Los mejores resultados se obtienen cuando los humanos y las máquinas trabajan juntos en la moderación de contenido. En la práctica, las máquinas aumentan las revisiones humanas en los casos en que la confianza en la predicción necesita ayuda o se debe alterar dentro de un contexto del mundo real. El resultado es un proceso de moderación de contenido híbrido que funciona mejor que si los seres humanos o las máquinas trabajaran solos.

## <a name="how-the-review-tool-helps"></a>Cómo ayuda la herramienta de revisión ##

La herramienta de revisión humana, cuando se usa en combinación con las API de moderación asistida automáticamente, le permite acometer estas tareas importantes en relación con el ciclo de vida de la moderación de contenido.

1. Automatización de la creación de revisiones humanas a partir de los resultados de la API de moderación subyacente
2. Uso de una herramienta (herramienta de revisión y API) para moderar varios formatos (texto, imagen y vídeo)
3. Asigne o escale las revisiones de contenido a varios equipos de revisión organizados por categoría de contenido o nivel de experiencia.
4. Use flujos de trabajo predeterminados o defina flujos de trabajo personalizados con reglas flexibles y sin escribir código.
5. Agregue revisión humana a cualquier API o proceso de negocio con solo compilar un conector.
6. Use los conectores predeterminados para revisar los resultados de Microsoft PhotoDNA, Text Analytics y Face API.
7. Obtenga métricas clave de rendimiento de los procesos de moderación de contenido.
