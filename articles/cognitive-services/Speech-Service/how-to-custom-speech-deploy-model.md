---
title: Implementar un modelo de voz personalizados - servicios de voz
titlesuffix: Azure Cognitive Services
description: En este documento, obtendrá información sobre cómo crear e implementar un punto de conexión mediante el portal de voz personalizado.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: fc51c1d9d47340da85d42f7c398c8ee21c601beb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025875"
---
# <a name="deploy-a-custom-model"></a>Implementar un modelo personalizado

Una vez haya cargado e inspeccionar datos, evalúa la precisión y entrena un modelo personalizado, puede implementar un punto de conexión personalizado para usar con los productos, herramientas y aplicaciones. En este documento, obtendrá información sobre cómo crear e implementar un punto de conexión mediante el portal de voz personalizado.

## <a name="create-a-custom-endpoint"></a>Crear un extremo personalizado

Para crear un nuevo punto de conexión personalizado, seleccione **implementación** en el menú de voz personalizados en la parte superior de la página. Si se trata de la primera ejecución, observará que no hay ningún punto de conexión que se muestran en la tabla. Después de crear un punto de conexión, usará esta página para realizar el seguimiento de cada punto de conexión implementado.

A continuación, seleccione **agregar punto de conexión** y escriba un **nombre** y **descripción** para el punto de conexión personalizado. A continuación, seleccione el modelo personalizado que le gustaría asociar a este punto de conexión. Desde esta página, también puede habilitar el registro. El registro le permite supervisar el tráfico de extremo. Si se deshabilita, no se almacenará el tráfico.

![Cómo implementar un modelo](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> No olvide que acepten los términos de uso y los detalles de precios.

A continuación, seleccione **Crear**. Esta acción vuelve a la **implementación** página. La tabla incluye ahora una entrada que corresponde al punto de conexión personalizado. Estado del punto de conexión muestra su estado actual. Puede tardar hasta 30 minutos para crear instancias de un nuevo punto de conexión con sus modelos personalizados. Cuando se cambia el estado de la implementación a **completar**, está listo para usar el punto de conexión.

Una vez implementado el punto de conexión, el nombre de punto de conexión aparece como un vínculo. Haga clic en el vínculo para mostrar información específica para el punto de conexión, como la clave de punto de conexión, la dirección URL del extremo y el código de ejemplo.

## <a name="view-logging-data"></a>Ver datos de registro

Datos de registro están disponibles para su descarga en **punto de conexión > detalles**.

## <a name="next-steps"></a>Pasos siguientes

* Utilice el punto de conexión personalizado con el [Speech SDK](speech-sdk.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparar y probar los datos](how-to-custom-speech-test-data.md)
* [Inspeccione los datos](how-to-custom-speech-inspect-data.md)
* [Evaluar los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenar el modelo.](how-to-custom-speech-train-model.md)
* [Implementar el modelo](how-to-custom-speech-deploy-model.md)
