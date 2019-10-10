---
title: Creación y claves de tiempo de ejecución - LUIS
titleSuffix: Azure Cognitive Services
description: 'LUIS usa dos claves: la clave de creación para crear el modelo y la clave de tiempo de ejecución para consultar el punto de conexión de predicción con expresiones del usuario.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 70e58077fa40ce685324cd24b447886ec3411034
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703181"
---
# <a name="authoring-and-runtime-keys"></a>Creación y claves en tiempo de ejecución


>[!NOTE]
>[Migre](luis-migration-authoring.md) cualquier aplicación que no use el recurso de creación de Azure antes de continuar.

LUIS usa dos tipos de recursos de Azure, y cada tipo tiene estas claves: 
 
* [Creación](#programmatic-key) para crear intenciones y entidades, etiquetar expresiones, entrenar y publicar contenido. Cuando esté listo para publicar la aplicación de LUIS, necesitará una [clave de punto de conexión de predicción para el tiempo de ejecución](luis-how-to-azure-subscription.md) asignado a la aplicación.
* [Clave de punto de conexión de predicción para el tiempo de ejecución](#prediction-endpoint-runtime-key). Las aplicaciones cliente, como un bot de chat, necesitan obtener acceso al **punto de conexión de predicción de consulta** del tiempo de ejecución a través de esta clave. 

|Clave|Propósito|Cognitive Service `kind`|Cognitive Service `type`|
|--|--|--|--|
|[Clave de creación](#programmatic-key)|Creación, entrenamiento, publicación, pruebas.|`LUIS.Authoring`|`Cognitive Services`|
|[Clave de tiempo de ejecución del punto de conexión de predicción](#prediction-endpoint-runtime-key)| Tiempo de ejecución del punto de conexión de predicción de consulta con una expresión de usuario para determinar las intenciones y las entidades.|`LUIS`|`Cognitive Services`|

LUIS también proporciona una [clave de inicio](luis-how-to-azure-subscription.md#starter-key) con una cuota de punto de conexión de predicción de 1000 transacciones al mes. 

Aunque no es necesario crear ambas claves al mismo tiempo, es mucho más fácil si lo hace.

Es importante crear las aplicaciones de LUIS en [regiones](luis-reference-regions.md#publishing-regions) en las que también quiera publicar y consultar contenido.

<a name="programmatic-key" ></a>

## <a name="authoring-key"></a>Clave de creación

Una clave de creación se crea automáticamente al crear una cuenta de LUIS y es gratuita. Cuando empiece a usar LUIS, tendrá una clave de inicio en todas las aplicaciones de LUIS para cada [región](luis-reference-regions.md) de creación. El propósito de la clave de creación es proporcionar una autenticación para administrar la aplicación de LUIS o para probar las consultas de punto de conexión de predicción. 

La creación de claves de creación en Azure Portal le permite controlar los permisos para el recurso de creación mediante la asignación de usuarios al [rol de colaborador](#contributions-from-other-authors). Necesita permiso en el nivel de suscripción de Azure para agregar colaboradores. 

Para encontrar la clave de creación, inicie sesión en [LUIS](luis-reference-regions.md#luis-website) y haga clic en el nombre de cuenta en la barra de navegación superior derecha para abrir **Configuración de la cuenta**.

![Clave de creación](./media/luis-concept-keys/authoring-key.png)

Si quiere realizar **consultas de tiempo de ejecución**, cree el [recurso de LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) de Azure. 

> [!CAUTION]
> Para mayor comodidad, en muchos de los ejemplos se usa la [clave de inicio](#starter-prediction-endpoint-runtime-key), ya que ofrece algunas llamadas de punto de conexión de predicción en su [cuota](luis-boundaries.md#key-limits).  

<a name="endpoint-key"></a>

## <a name="prediction-endpoint-runtime-key"></a>Clave de tiempo de ejecución del punto de conexión de predicción 

Cuando necesite usar las **consultas de puntos de conexión de tiempo de ejecución**, cree un recurso de Language Understanding (LUIS) y asígnelo a la aplicación de LUIS. 

[!INCLUDE [Azure runtime resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Cuando finalice el proceso de creación del recurso, [asigne la clave](luis-how-to-azure-subscription.md) a la aplicación. 

* La clave (punto de conexión de predicción de consulta) de tiempo de ejecución, le permite tener una cuota de visitas de punto de conexión basada en el plan de uso que especificó al crear la clave de tiempo de ejecución. Vea [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) para obtener información sobre los precios.

* La clave de tiempo de ejecución se puede usar en todas las aplicaciones de LUIS o en aplicaciones de LUIS específicas. 
* No use la clave de tiempo de ejecución para crear aplicaciones de LUIS. 

### <a name="starter-prediction-endpoint-runtime-key"></a>Clave de tiempo de ejecución del punto de conexión de predicción de inicio

La clave de punto de conexión de predicción de **inicio** se proporciona de forma gratuita e incluye 1000 consultas de punto de conexión de predicción. Después de usar estas consultas, debe crear su propio recurso de punto de conexión de predicción para Language Understanding.  

Se trata de un recurso especial creado para usted. No aparece en la lista de recursos de Azure porque está pensado como una clave inicial temporal. 

<a name="use-endpoint-key-in-query"></a>

### <a name="use-runtime-key-in-query"></a>Uso de la clave de tiempo de ejecución en la consulta
El punto de conexión de tiempo de ejecución de LUIS acepta dos estilos de consulta y ambos usan la clave de tiempo de ejecución del punto de conexión, pero en distintos lugares.

El punto de conexión que se usa para obtener acceso al tiempo de ejecución, usa un subdominio que es único en la región del recurso, que se indica con `{region}` en la tabla siguiente. 


#### <a name="v2-prediction-endpointtabv2"></a>[Punto de conexión de predicción de V2](#tab/V2)

|Verbo|Ejemplo de URL y ubicación de la clave|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?runtime-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`|

#### <a name="v3-prediction-endpointtabv3"></a>[Punto de conexión de predicción de V3](#tab/V3)

|Verbo|Ejemplo de URL y ubicación de la clave|
|--|--|
|[GET](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a91e54c9db63d589f433)|`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?runtime-key=your-endpoint-key-here&query=turn%20on%20the%20lights`|
|[POST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a5830f741b27cd03a061)| `https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict`| 

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * * 

**GET**. Cambie el valor de la consulta de punto de conexión de `runtime-key` de la clave de creación (inicio) a la nueva clave de punto de conexión para usar la tasa de cuota de la clave de punto de conexión de LUIS. Si crea la clave y la asigna, pero no cambia el valor de la consulta de punto de conexión de `runtime-key`, no está usando la cuota de la clave de punto de conexión.

**POST**: Cambie el valor de encabezado de `Ocp-Apim-Subscription-Key`.<br>Si crea la clave de tiempo de ejecución y la asigna, pero no cambia el valor de la consulta de punto de conexión de `Ocp-Apim-Subscription-Key`, no estará usando la clave de tiempo de ejecución.

El identificador de la aplicación que se usó en las direcciones URL anteriores, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, es la aplicación de IoT pública que se usa para la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="assignment-of-the-runtime-key"></a>Asignación de la clave de tiempo de ejecución

Puede [asignar](luis-how-to-azure-subscription.md) la clave de tiempo de ejecución en el [portal de LUIS](https://www.luis.ai) o a través de las API correspondientes. 

## <a name="key-limits"></a>Límites de la clave

Puede crear hasta 10 claves de creación por región y por suscripción. 

Consulte [Límites de la clave](luis-boundaries.md#key-limits) y [Regiones de Azure](luis-reference-regions.md). 

Las regiones de publicación son diferentes de las regiones de creación. Asegúrese de que crea una aplicación en la región de creación correspondiente a la región de publicación donde quiera ubicar su aplicación cliente.

## <a name="key-limit-errors"></a>Errores del límite de la clave
Si supera su cuota de transacciones por segundo (TPS), recibirá un error HTTP 429. Si supera su cuota de transacciones por mes (TPS), recibirá un error HTTP 403. 

## <a name="contributions-from-other-authors"></a>Contribuciones de otros autores



La administración de las contribuciones de los colaboradores depende del estado actual de la aplicación.

**Para la [creación de aplicaciones migradas](luis-migration-authoring.md) de recursos**: los _colaboradores_ se administran en Azure Portal para el recurso de creación, mediante la página del **control de acceso (IAM)** . Obtenga información acerca de [cómo agregar un usuario](luis-how-to-collaborate.md) mediante la dirección de correo electrónico del colaborador y el rol del _colaborador_. 

**En el caso de las aplicaciones que no se han migrado todavía**: todos los _colaboradores_ se administran en el portal de LUIS desde la página **Manage -> Collaborators** (Administrar -> Colaboradores).

### <a name="contributor-roles-vs-entity-roles"></a>Roles de colaborador frente a roles de entidad

Los [roles de entidad](luis-concept-roles.md) se aplican al modelo de datos de la aplicación de LUIS. Los roles de colaborador se aplican a los niveles de acceso de creación. 

## <a name="moving-or-changing-ownership"></a>Mover o cambiar la propiedad

Una aplicación se define mediante sus recursos de Azure, que se determinan en función de la suscripción del propietario. 

Puede mover la aplicación de LUIS. Use los siguientes recursos de documentación en Azure Portal o la CLI de Azure:

* [Mover la aplicación entre los recursos de creación de LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Mover los recursos a un nuevo grupo de recursos o a una nueva suscripción](../../azure-resource-manager/resource-group-move-resources.md)
* [Mueve el recurso dentro de la misma suscripción o entre suscripciones](../../azure-resource-manager/move-limitations/app-service-move-limitations.md)
* [Transferencia de la propiedad](../../billing/billing-subscription-transfer.md) de la suscripción 

## <a name="access-for-private-and-public-apps"></a>Acceso para aplicaciones públicas y privadas

Para una aplicación **privada**, el acceso al tiempo de ejecución está disponible para los propietarios y los colaboradores. Para una aplicación **pública**, el acceso al tiempo de ejecución está disponible para todos los usuarios que tengan su propio recurso de tiempo de ejecución de Azure [Cognitive Services](../cognitive-services-apis-create-account.md) o de [LUIS](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) y el identificador de la aplicación pública. 

Actualmente no hay un catálogo de aplicaciones públicas.

### <a name="authoring-access"></a>Acceso de creación
El acceso a la aplicación desde el portal de [LUIS](luis-reference-regions.md#luis-website) o las [API de creación](https://go.microsoft.com/fwlink/?linkid=2092087) lo controla el recurso de creación de Azure. 

El propietario y todos los colaboradores tienen acceso a la creación de la aplicación. 

|El acceso de creación incluye|Notas|
|--|--|
|Agregar o quitar claves del punto de conexión||
|Exportar la versión||
|Exportar los registros de punto de conexión||
|Importar la versión||
|Hacer pública la aplicación|Cuando una aplicación es pública, cualquiera que tenga una clave de creación o del punto de conexión puede consultar la aplicación.|
|Modificar el modelo|
|Publicar|
|Revisar las expresiones de punto de conexión para un [aprendizaje activo](luis-how-to-review-endpoint-utterances.md)|
|Train|

### <a name="prediction-endpoint-runtime-access"></a>Acceso de tiempo de ejecución del punto de conexión de predicción

El acceso para consultar el punto de conexión de predicción se controla mediante una opción de configuración de la página **Application Information** (Información de la aplicación) en la sección **Manage** (Administrar). 

![Configurar la aplicación como pública](./media/luis-concept-security/set-application-as-public.png)

|[Punto de conexión privado](#runtime-security-for-private-apps)|[Punto de conexión público](#runtime-security-for-public-apps)|
|:--|:--|
|Disponible para el propietario y los colaboradores.|Disponible para el propietario, los colaboradores y cualquier otra persona que conozca el identificador de la aplicación.|

Puede controlar quién puede ver su clave de tiempo de ejecución de LUIS llamándola en un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de servidor (por ejemplo, una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API del lado servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Aunque esta estrategia no impide los ataques de tipo "Man in the middle", ofusca la clave y la dirección URL del punto de conexión de los usuarios, le permite realizar un seguimiento del acceso y agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).

#### <a name="runtime-security-for-private-apps"></a>Seguridad del tiempo de ejecución para aplicaciones privadas

El tiempo de ejecución de una aplicación privada solo está disponible para lo siguiente:

|Clave y usuario|Explicación|
|--|--|
|Clave de creación del propietario| Hasta 1000 visitas del punto de conexión|
|Claves de creación de colaborador| Hasta 1000 visitas del punto de conexión|
|Cualquier clave que un autor o colaborador haya asignado a LUIS|Según el nivel de uso de las claves|

#### <a name="runtime-security-for-public-apps"></a>Seguridad del tiempo de ejecución para aplicaciones públicas

Una vez que una aplicación se ha configurado como pública, _cualquier_ clave de creación de LUIS válida o clave del punto de conexión de LUIS puede consultar la aplicación, siempre y cuando la clave no haya alcanzado el límite de cuota del punto de conexión.

Un usuario que no sea propietario ni colaborador solo podrá obtener acceso al tiempo de ejecución de una aplicación pública si se le proporciona el identificador de la aplicación. LUIS no tiene un _mercado_ público u otro medio para buscar una aplicación pública.  

Una aplicación pública se pone a disposición de los usuarios en todas las regiones para que los que tengan una clave de recurso de LUIS basada en regiones puedan acceder a la aplicación en cualquier región que esté asociada a la clave de recurso.

## <a name="transfer-of-ownership"></a>Transferencia de propiedad

**Para [crear aplicaciones migradas](luis-migration-authoring.md) de recursos**: Como propietario del recurso, puede agregar un `contributor`.

**En el caso de las aplicaciones que no se han migrado todavía**: Exporte la aplicación como un archivo JSON. Otro usuario de LUIS puede importar la aplicación y, por tanto, se convierte en el propietario de la aplicación. La nueva aplicación tendrá un identificador de aplicación diferente.  

## <a name="securing-the-endpoint"></a>Proteger el punto de conexión 

Puede controlar quién puede ver su clave del punto de conexión de tiempo de ejecución de predicción de LUIS llamándola a un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de servidor (por ejemplo, una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API de servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Aunque esta estrategia no impide los ataques de tipo "Man in the middle", ofusca el punto de conexión de los usuarios, le permite realizar un seguimiento de acceso y agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="next-steps"></a>Pasos siguientes

* Descripción de los conceptos de [control de versiones](luis-concept-version.md). 
* Obtenga más información sobre [cómo crear claves](luis-how-to-azure-subscription.md).
