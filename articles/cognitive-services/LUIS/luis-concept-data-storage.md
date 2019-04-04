---
title: Almacenamiento de datos
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS almacena datos cifrados en un almacén de datos de Azure correspondiente a la región especificada por la clave.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: diberry
ms.openlocfilehash: a1093c2a6303b453a17a52058303913de5ecfa8d
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893204"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Almacenamiento y eliminación de datos en Cognitive Services de Language Understanding (LUIS)
LUIS almacena datos cifrados en un almacén de datos de Azure correspondiente a la región especificada por la clave. Estos datos se almacenan durante 30 días. 

## <a name="export-and-delete-app"></a>Exportar y eliminar la aplicación
Los usuarios tienen un control total sobre la [exportación](luis-how-to-start-new-app.md#export-app) y la [eliminación](luis-how-to-start-new-app.md#delete-app) de la aplicación. 

## <a name="utterances"></a>Grabaciones de voz

Grabaciones de voz se pueden almacenar en dos lugares diferentes. 

* Durante la **el proceso de creación**, grabaciones de voz se crean y almacenan en la intención. Grabaciones de voz en intenciones son necesarios para una aplicación de LUIS correctamente. Una vez que la aplicación se publica y se recibe en el punto de conexión, cadena de consulta de la solicitud de punto de conexión, las consultas `log=false`, determina si la declaración de punto de conexión se almacena. Si se almacena el punto de conexión, se convierte en parte de las declaraciones de aprendizaje activo se encuentra en la **compilar** sección del portal, en la **revisar grabaciones de voz de punto de conexión** sección. 
* Cuando se **revisar grabaciones de voz de punto de conexión**y agregue una declaración para una intención, la declaración ya no se almacena como parte de las declaraciones de punto de conexión para su revisión. Se agrega a las intenciones de la aplicación. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Eliminar grabaciones de voz de ejemplo de una intención
Elimine expresiones de ejemplo que se han usado para entrenar a [LUIS](luis-reference-regions.md). Si elimina una expresión de ejemplo desde la aplicación de LUIS, se eliminará del servicio web de LUIS y no estará disponible para la exportación.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Eliminar grabaciones de voz en la revisión de aprendizaje activo

Puede eliminar expresiones de la lista de expresiones de usuario que LUIS sugiere en la página **[Review endpoint utterances](luis-how-to-review-endpoint-utterances.md)** (Revisar expresiones de punto de conexión). La eliminación de las expresiones de esta lista evita que se sugieran, pero no las elimina de los registros.

Si no desea que las declaraciones de aprendizaje activo, puede [deshabilitar aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Deshabilitar el aprendizaje activo también deshabilita el registro.

### <a name="disable-logging-utterances"></a>Deshabilitar registro grabaciones de voz
[Deshabilitar el aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning) se deshabilita el registro.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Eliminación de una cuenta
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
