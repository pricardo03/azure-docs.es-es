---
title: Claves de suscripción
titleSuffix: Language Understanding - Azure Cognitive Services
description: No es necesario crear claves de suscripción para usar las primeras 1000 consultas de punto de conexión. Si recibe un error de que se ha quedado _sin cuota_ como un error HTTP 403 o 429, debe crear una clave y asignarla a la aplicación.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/29/2019
ms.author: diberry
ms.openlocfilehash: 26954c8f839ff0bfb2da484e4fb535f33d4e07ed
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239186"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>Uso de claves de suscripción con la aplicación de LUIS

No es necesario crear claves de suscripción para usar las primeras 1000 consultas de punto de conexión. Una vez que se usan esas consultas de punto de conexión, cree un recurso de Azure en [Azure Portal](http://portal.azure.com) y luego asigne ese recurso a una aplicación de LUIS en el [portal de LUIS](https://www.luis.ai).

Si recibe un error de que se ha quedado _sin cuota_ como un error HTTP 403 o 429, debe crear una clave y asignarla a la aplicación. 

Para pruebas y prototipos solo, use el plan gratuito (F0). Para los sistemas de producción, use un plan de [pago](https://aka.ms/luis-price-tier). No use la [clave de creación](luis-concept-keys.md#authoring-key) para las consultas de punto de conexión en producción.

<a name="create-luis-service"></a>

## <a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>Creación de la clave de punto de conexión de Language Understanding en Azure Portal

Este procedimiento crea un recurso de **Language Understanding**. Si quiere que un recurso se pueda usar en todo Cognitive Services, cree la clave todo en uno **[Cognitive Service](../cognitive-services-apis-create-account.md)** en lugar del recurso de Language Understanding. 

Esta clave solo se debe usar para las consultas de predicción de punto de conexión. No utilice esta clave para los cambios en el modelo o la aplicación. 

1. Inicie sesión en **[Azure Portal](https://ms.portal.azure.com/)**. 
1. Seleccione el signo **+** de color verde en el panel de la parte superior izquierda y busque `Language Understanding` en Marketplace y, después, seleccione **Language Understanding** y siga la **experiencia de creación** para crear una cuenta de suscripción de LUIS. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

1. Configure la suscripción con la configuración, incluido el nombre de cuenta, los planes de tarifa, etc. 

    ![Elección de la API de Azure](./media/luis-azure-subscription/azure-api-choice.png) 

1. Una vez que se crea el recurso de Language Understanding, puede ver las claves de acceso generadas en **Administración de recursos -> Claves**. En la siguiente sección verá cómo conectar este recurso nuevo a una aplicación de LUIS en el portal de LUIS. Necesita el nombre del recurso de LUIS del paso 3.

    ![Claves de Azure](./media/luis-azure-subscription/azure-keys.png)

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


## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>Asignación de la clave de recurso a la aplicación de LUIS en el portal de LUIS

1. Inicie sesión en el portal de LUIS, elija una aplicación en la cual agregar la clave nueva, seleccione **Administrar** en el menú que se encuentra en la parte superior derecha y, luego, seleccione **Claves y puntos de conexión**.

    [ ![Página Claves y puntos de conexión](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

1. Para agregar LUIS, seleccione **Asignar recurso +**.

    ![Asignación de un recurso a la aplicación](./media/luis-manage-keys/assign-key.png)

1. Seleccione a un inquilino en el cuadro de diálogo asociado con la dirección de correo electrónico que se usó para iniciar sesión en el sitio web de LUIS.  

1. Elija el **Nombre de suscripción** asociado con el recurso de Azure que quiera agregar.

1. Seleccione el **Nombre de recurso de LUIS**. 

1. Seleccione **Asignar recurso**. 

1. Busque la nueva fila en la tabla y copie la dirección URL de punto de conexión. Se construye correctamente para realizar una solicitud HTTP GET en el punto de conexión de LUIS para una predicción. 

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

    |Type|Configuración|Valor|
    |--|--|--|
    |Encabezado|`Authorization`|El valor de `Authorization` es `Bearer {token}`. Tenga en cuenta que el valor del token debe ir precedido de la palabra `Bearer` y un espacio.|
    |Encabezado|`Ocp-Apim-Subscription-Key`|Su [clave de creación](luis-how-to-account-settings.md).|
    |Encabezado|`Content-type`|`application/json`|
    |QueryString|`appid`|El id. de la aplicación LUIS. 
    |Cuerpo||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Cuando esta API finaliza correctamente, devuelve un estado 201: creado. 

## <a name="change-pricing-tier"></a>Cambiar el plan de tarifa

1.  En [Azure](https://portal.azure.com), busque la suscripción de LUIS. Seleccione la suscripción de LUIS.
    ![Buscar la suscripción de LUIS](./media/luis-usage-tiers/find.png)
1.  Seleccione **Plan de tarifa** para ver los planes de tarifa disponibles. 
    ![Ver los planes de tarifa](./media/luis-usage-tiers/subscription.png)
1.  Seleccione el plan de tarifa y después **Seleccionar** para guardar el cambio. 
    ![Cambiar el plan de pago de LUIS](./media/luis-usage-tiers/plans.png)
1.  Una vez completado el cambio de tarifa, el plan de tarifa nuevo se comprueba en una ventana emergente. 
    ![Comprobar el plan de pago de LUIS](./media/luis-usage-tiers/updated.png)
1. No olvide [asignar esta clave de punto de conexión](#assign-endpoint-key) en la página **Publicar** y usarla en todas las consultas de punto de conexión. 

## <a name="how-to-fix-out-of-quota-errors-when-the-key-exceeds-pricing-tier-usage"></a>Solución de los errores por quedarse sin cuota cuando la clave excede el uso permitido por el plan de tarifa
Cada plan permite solicitudes de punto de conexión a la cuenta de LUIS a una tasa específica. Si la tasa de solicitudes es mayor que la permitida para la cuenta de uso medido por minuto o por mes, las solicitudes reciben un error HTTP de "429: demasiadas solicitudes".

Cada plan permite solicitudes acumulativas por mes. Si el número total de solicitudes es más alto que la tasa permitida, las solicitudes reciben un error HTTP "403: prohibido".  

## <a name="viewing-summary-usage"></a>Ver uso de resumen
En Azure puede ver la información de uso de LUIS. En la página **Información general** se muestra información de resumen reciente incluidos los errores y las llamadas. Si realiza una solicitud de punto de conexión de LUIS, consulte inmediatamente la **página Información general**, y deje que pasen hasta cinco minutos hasta que se muestre el uso.

![Ver uso de resumen](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Personalización de los gráficos de uso
Las métricas proporcionan una vista más detallada de los datos.

![Métricas predeterminadas](./media/luis-usage-tiers/metrics-default.png)

Puede configurar los gráficos de métricas para el período de tiempo y el tipo de métrica. 

![Métricas personalizadas](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Alerta de umbral de transacciones totales
Si le gustaría saber cuándo se alcanza un umbral de transacciones determinado (por ejemplo 10.000) puede crear una alerta. 

![Alertas predeterminadas](./media/luis-usage-tiers/alert-default.png)

Agregue una alerta de métrica para la métrica **Llamadas totales** para un período de tiempo concreto. Agregue las direcciones de correo electrónico de todos los usuarios que deben recibir la alerta. Agregue webhooks para todos los sistemas que deben recibir la alerta. También puede ejecutar una aplicación lógica cuando se desencadene la alerta. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo usar las [versiones](luis-how-to-manage-versions.md) para administrar los cambios en la aplicación de LUIS.
