---
title: Configuraciones regionales e idiomas admitidos en Custom Speech Service en Azure | Microsoft Docs
description: Información general de los idiomas admitidos de Custom Speech Service en Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: e5c1f7d9648f404512f366be4f4d16ea0cb0f778
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217125"
---
# <a name="supported-locales-in-custom-speech-service"></a>Configuraciones regionales admitidas en Custom Speech Service

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Custom Speech Service admite actualmente la personalización de modelos en las siguientes configuraciones regionales:

| Tipo de modelo | Compatibilidad con lenguajes |
|----|-----|
| Modelos acústicos | Inglés de Estados Unidos (en-US) |
| Modelos de idiomas | Inglés de Estados Unidos (en-US), chino (zh-CN) |

Aunque la personalización de modelos acústicos solo se admite en inglés de Estados Unidos, la importación de datos acústicos en chino se admite para realizar pruebas sin conexión de modelos de idiomas en chino personalizados.

Debe seleccionar la configuración regional adecuada antes de realizar cualquier acción. La configuración regional actual se indica en el título de la tabla en todas las páginas de la implementación, el modelo y los datos. Para cambiar la configuración regional, haga clic en el botón "Change Locale" (Cambiar configuración regional) que se encuentra bajo el título de la tabla. Esto le llevará a una página de confirmación de configuración regional. Haga clic en "OK" (Aceptar) para volver a la tabla.

Debe seguir estos pasos:
* Aprenda a [crear un modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md) para mejorar la precisión del reconocimiento.
* Aprenda a [crear un modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md) para mejorar la velocidad del reconocimiento.
* Siga las [directrices de transcripción](cognitive-services-custom-speech-transcription-guidelines.md) para preparar los datos.
