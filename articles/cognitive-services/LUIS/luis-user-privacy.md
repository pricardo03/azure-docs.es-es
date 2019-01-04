---
title: Exportación y eliminación de datos
titleSuffix: Azure Cognitive Services
description: Language Understanding Intelligent Service (LUIS) conserva el contenido del cliente para que el servicio funcione, pero el usuario de LUIS tiene control total sobre la visualización, la exportación y la eliminación de sus datos. Esto se puede realizar a través del portal web de LUIS o las API de programación de LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 0aba859fc0c5e2de64b2bbdb9c06f2434937f4e8
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140683"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exportación y eliminación de datos del cliente en Language Understanding (LUIS) en Cognitive Services

## <a name="summary-of-customer-data-request-features"></a>Resumen de características de solicitud de datos del cliente
Language Understanding Intelligent Service (LUIS) conserva el contenido del cliente para que el servicio funcione, pero el usuario de LUIS tiene control total sobre la visualización, la exportación y la eliminación de sus datos. Esto puede realizarse a través del [portal](luis-reference-regions.md) web de LUIS o las [API de programación de LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

El contenido del cliente se almacena cifrado en el almacenamiento regional de Microsoft Azure e incluye:

- El contenido de la cuenta de usuario recopilado en el registro.
- Datos de entrenamiento necesarios para generar los modelos (es decir, intenciones y entidades).
- Consultas de usuarios registradas durante el tiempo de ejecución para ayudar a mejorar los modelos del usuario.
  - Los usuarios pueden desactivar el registro de consultas mediante la anexión de `&log=false` a la solicitud. Puede obtener más información [aquí](luis-resources-faq.md#how-can-i-disable-the-logging-of-utterances).

## <a name="deleting-customer-data"></a>Eliminación de datos del cliente
Los usuarios de LUIS tienen control total para eliminar cualquier contenido del usuario, ya sea a través del portal web de LUIS o las API de programación de LUIS. En la siguiente tabla se muestran vínculos para proporcionar asistencia con ambos:

| | **Cuenta de usuario** | **Aplicación** | **Expresiones** | **Consultas de usuario final** |
| --- | --- | --- | --- | --- |
| **Portal** | [Vínculo](luis-how-to-account-settings.md) | [Vínculo](luis-how-to-start-new-app.md#delete-app) | [Vínculo](luis-how-to-start-new-app.md#delete-app) | [Vínculo](luis-how-to-start-new-app.md#delete-app) |
| **API** | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exportación de datos del cliente
Los usuarios de LUIS tienen control total para ver los datos en el portal, sin embargo, debe exportarse a través de las API de programación de LUIS. En la siguiente tabla se muestra vínculos para proporcionar asistencia con las exportaciones de datos a través de las API de programación de LUIS:

| | **Cuenta de usuario** | **Aplicación** | **Expresiones** | **Consultas de usuario final** |
| --- | --- | --- | --- | --- |
| **API** | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de las regiones de LUIS](./luis-reference-regions.md)
