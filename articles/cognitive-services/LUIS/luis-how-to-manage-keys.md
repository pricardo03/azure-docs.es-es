---
title: Administración de claves
titleSuffix: Language Understanding - Azure Cognitive Services
description: Después de crear una clave de punto de conexión de LUIS en Azure Portal, asigne la clave a la aplicación de LUIS y obtenga la URL de punto de conexión correcto. Use esta dirección URL de punto de conexión para obtener predicciones de LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 0a278ded7ce290347645f345e4eee0b15972787f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088109"
---
# <a name="add-an-azure-luis-resource-to-app"></a>Adición de un recurso de LUIS para Azure a la aplicación

Después de crear un recurso de LUIS en Azure Portal, asigne el recurso de LUIS y obtenga la URL de punto de conexión correcto. Use esta dirección URL de punto de conexión para obtener predicciones de LUIS.

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>
<a name="assign-resource"></a>


## <a name="assign-resource-in-luis-portal"></a>Asignación del recurso en el portal de LUIS

1. Cree una clave de LUIS en [Azure Portal](https://portal.azure.com). Para más instrucciones, consulte [Creating an endpoint key using Azure](luis-how-to-azure-subscription.md) (Creación de una clave de punto de conexión mediante Azure).
 
2. Seleccione **Administrar** en el menú superior derecho y, a continuación, seleccione **Claves y puntos de conexión**.

    [ ![Página Claves y puntos de conexión](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

3. Para agregar LUIS, seleccione **Asignar recurso +**.

    ![Asignación de un recurso a la aplicación](./media/luis-manage-keys/assign-key.png)

4. Seleccione a un inquilino en el cuadro de diálogo asociado con la dirección de correo electrónico con la cual inicia sesión en el sitio web de LUIS.  

5. Elija el **Nombre de suscripción** asociado con el recurso de Azure que quiera agregar.

6. Seleccione el **Nombre de recurso de LUIS**. 

7. Seleccione **Asignar recurso**. 

8. Busque la nueva fila en la tabla y copie la dirección URL de punto de conexión. Se construye correctamente para realizar una solicitud HTTP GET en el punto de conexión de LUIS para una predicción. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

### <a name="unassign-resource"></a>Anulación de la asignación de un recurso
Al anular la asignación de la clave de punto de conexión, no se elimina de Azure. Solo se desvincula de LUIS. 

Cuando una clave de punto de conexión está sin asignar o no está asignada a la aplicación, cualquier solicitud a la dirección URL de punto de conexión devuelve un error: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Incluir todas las puntuaciones de intención pronosticada
La casilla de verificación **Include all predicted intent scores** (Incluir todas las puntuaciones de intención pronosticada) permite que la respuesta a la consulta del punto de conexión incluya la puntuación de predicción para cada intención. 

Esta opción permite que el bot de chat o la aplicación que llama a LUIS tome decisiones mediante programación basadas en las puntuaciones de las intenciones devueltas. Por lo general, las dos primeras intenciones son las más interesantes. Si la mejor puntuación es la intención None, el bot de chat puede hacer una pregunta de seguimiento que elegirá definitivamente entre la intención None y la otra intención con una puntuación alta. 

Las intenciones y sus puntuaciones también se incluyen en los registros de punto de conexión. También puede [exportar](luis-how-to-start-new-app.md#export-app) esos registros y analizar los resultados. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Habilitar el corrector ortográfico de Bing 
En **Endpoint url settings** (Configuración de dirección URL de punto de conexión), el conmutador **Bing spell checker** (Corrector ortográfico de Bing) permite a LUIS corregir las palabras mal escritas antes de realizar la predicción. Cree una **[clave de Bing Spell Check](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. 

Agregue el parámetro de cadena de consulta **spellCheck=true** y **bing-spell-check-subscription-key={SU_CLAVE_DE_BING}**. Reemplace `{YOUR_BING_KEY_HERE}` con su clave del corrector ortográfico de Bing.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```


### <a name="publishing-regions"></a>Regiones de publicación

Obtenga más información sobre las [regiones](luis-reference-regions.md) de publicación, incluida la publicación en [Europa](luis-reference-regions.md#publishing-to-europe) y [Australia](luis-reference-regions.md#publishing-to-australia). Las regiones de publicación son diferentes de las regiones de creación. Cree una aplicación en la región de creación correspondiente a la región de publicación que quiera para el punto de conexión de consulta.

## <a name="assign-resource-without-luis-portal"></a>Asignación del recurso sin el portal de LUIS

Para fines de automatización, como una canalización de CI/CD, puede automatizar la asignación de un recurso de LUIS para una aplicación de LUIS. Para ello, tiene que realizar los siguientes pasos:

1. Obtenga un token de Azure Resource Manager en este [sitio web](https://resources.azure.com/api/token?plaintext=true). Este token expira, por lo que debe usarlo de inmediato. La solicitud devuelve un token de Azure Resource Manager.

    ![Solicitud de un token de Azure Resource Manager y recepción del token de Azure Resource Manager](./media/luis-manage-keys/get-arm-token.png)

1. Use el token para solicitar los recursos de LUIS en distintas suscripciones desde la [API Get LUIS azure accounts](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), su cuenta de usuario tiene acceso. 

    Esta API POST requiere la siguiente configuración:

    |Encabezado|Valor|
    |--|--|
    |`Authorization`|El valor de `Authorization` es `Bearer {token}`. Tenga en cuenta que el valor del token debe ir precedido de la palabra `Bearer` y un espacio.| 
    |`Ocp-Apim-Subscription-Key`|Su [clave de creación](luis-how-to-account-settings.md).|

    Esta API devuelve una matriz de objetos JSON con las suscripciones de LUIS, incluidos el identificador de suscripción, el grupo de recursos y el nombre del recurso, que se devuelve como nombre de cuenta. Busque el elemento de la matriz que es el recurso de LUIS que se va a asignar a la aplicación de LUIS. 

1. Asigne el token al recurso de LUIS con la API [Assign a LUIS azure accounts to an application](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515). 

    Esta API POST requiere la siguiente configuración:

    |Escriba|Configuración|Valor|
    |--|--|--|
    |Encabezado|`Authorization`|El valor de `Authorization` es `Bearer {token}`. Tenga en cuenta que el valor del token debe ir precedido de la palabra `Bearer` y un espacio.|
    |Encabezado|`Ocp-Apim-Subscription-Key`|Su [clave de creación](luis-how-to-account-settings.md).|
    |Encabezado|`Content-type`|`application/json`|
    |QueryString|`appid`|El id. de la aplicación LUIS. 
    |Cuerpo||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Cuando esta API finaliza correctamente, devuelve un estado 201: creado. 

## <a name="next-steps"></a>Pasos siguientes

Use su clave para publicar la aplicación en la página **Publicar aplicación**. Para obtener instrucciones sobre cómo publicarla, vea [Publish app](luis-how-to-publish-app.md) (Publicar la aplicación).

Vea [Keys in LUIS](luis-concept-keys.md) (Claves en LUIS) para comprender los conceptos de claves de creación y punto de conexión de LUIS.
