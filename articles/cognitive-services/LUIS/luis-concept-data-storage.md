---
title: Almacenamiento de datos
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS almacena datos cifrados en un almacén de datos de Azure correspondiente a la región especificada por la clave.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: a382c3b3f9949e95ebc09f9db5072a123c59f970
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223211"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Almacenamiento y eliminación de datos en Cognitive Services de Language Understanding (LUIS)
LUIS almacena datos cifrados en un almacén de datos de Azure correspondiente a la región especificada por la clave. Estos datos se almacenan durante 30 días. 

## <a name="export-and-delete-app"></a>Exportar y eliminar la aplicación
Los usuarios tienen un control total sobre la [exportación](luis-how-to-start-new-app.md#export-app) y la [eliminación](luis-how-to-start-new-app.md#delete-app) de la aplicación. 

## <a name="utterances-in-an-intent"></a>Expresiones en una intención
Elimine expresiones de ejemplo que se han usado para entrenar a [LUIS](luis-reference-regions.md). Si elimina una expresión de ejemplo desde la aplicación de LUIS, se eliminará del servicio web de LUIS y no estará disponible para la exportación.

## <a name="utterances-in-review"></a>Expresiones en revisión
Puede eliminar expresiones de la lista de expresiones de usuario que LUIS sugiere en la página **[Review endpoint utterances](luis-how-to-review-endoint-utt.md)** (Revisar expresiones de punto de conexión). La eliminación de las expresiones de esta lista evita que se sugieran, pero no las elimina de los registros.

## <a name="accounts"></a>Cuentas
Si elimina una cuenta, se eliminarán todas las aplicaciones, incluso las expresiones de ejemplo y los registros. Los datos se conservan durante 60 días antes de que la cuenta y los datos se eliminen de forma permanente.

Para eliminar la cuenta debe ir a la página **Configuración**. Seleccione el nombre de la cuenta en la barra de navegación situada en la parte superior derecha para ir a la página **Configuración**.

## <a name="data-inactivity-as-an-expired-subscription"></a>Inactividad de datos como suscripción caducada
Para eliminar y retener datos, una aplicación de LUIS inactiva podría tratarse como una suscripción caducada a _discreción de Microsoft_. Una aplicación se considera inactiva si cumple los siguientes criterios durante los últimos 90 días: 

* **No** ha recibido ninguna llamada.
* No se ha modificado.
* No tiene ninguna clave actual asignada.
* Ningún usuario ha iniciado sesión en ella.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtenga información sobre cómo exportar y eliminar una aplicación](luis-how-to-start-new-app.md)
