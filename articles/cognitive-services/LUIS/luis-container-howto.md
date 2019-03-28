---
title: Contenedores de Docker
titleSuffix: Language Understanding - Azure Cognitive Services
description: El contenedor de LUIS carga la aplicación entrenada o publicada en un contenedor de Docker y proporciona acceso a las predicciones de consulta de los puntos de conexión de la API del contenedor.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/22/2019
ms.author: diberry
ms.openlocfilehash: edd035bc95cd2e694a7cfac39e447c63fce0f7d3
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520163"
---
# <a name="install-and-run-luis-docker-containers"></a>Instalar y ejecutar contenedores de docker de LUIS
 
El contenedor de Language Understanding (LUIS) carga el modelo de Language Understanding entrenado o publicado, lo que también se conoce como [aplicación de LUIS](https://www.luis.ai), en un contenedor de Docker y proporciona acceso a las predicciones de consulta de los puntos de conexión de la API del contenedor. Puede recopilar registros de consultas en el contenedor y cargarlos de nuevo en el modelo de Azure Language Understanding para mejorar la precisión de predicción de la aplicación.

En el siguiente vídeo, se explica cómo se utiliza este contenedor.

[![Demostración de contenedores de Cognitive Services](./media/luis-container-how-to/luis-containers-demo-video-still.png)](https://aka.ms/luis-container-demo).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para poder ejecutar el contenedor de LUIS, debe tener lo siguiente: 

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recurso y aplicación asociada de Language Understanding (LUIS) |Para poder usar el contenedor, debe tener:<br><br>* Un [recurso de Azure de _Language Understanding_](luis-how-to-azure-subscription.md), junto con la clave del punto de conexión asociado y el URI del punto de conexión (que se utiliza como punto de conexión de facturación).<br>* Una aplicación entrenada o publicada como una entrada montada en el contenedor junto con su identificador de aplicación asociado.<br>* La clave de creación para descargar el paquete de la aplicación, si esta operación se va a realizar desde la API.<br><br>Estos requisitos se usan para pasar argumentos de la línea de comandos a las siguientes variables:<br><br>**{AUTHORING_KEY}**: esta clave se usa para obtener la aplicación empaquetada del servicio LUIS en la nube y cargar los registros de consultas en la nube. El formato es `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.<br><br>**{APPLICATION_ID}**: este identificador se usa para seleccionar la aplicación. El formato es `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.<br><br>**{ENDPOINT_KEY}**: esta clave se usa para iniciar el contenedor. Puede encontrar la clave de punto de conexión en dos lugares. El primero de ellos es Azure Portal, en la lista de claves del recurso de _Language Understanding_. La clave de punto de conexión también está disponible en el portal de LUIS, en la página de configuración de puntos de conexión y claves. No utilice la clave de inicio.<br><br>**{BILLING_ENDPOINT}**: El valor del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Language Understanding. Un ejemplo sería `https://westus.api.cognitive.microsoft.com/luis/v2.0`.<br><br>El propósito de la [clave de creación y la clave de punto de conexión](luis-boundaries.md#key-limits) es diferente. No deben utilizarse indistintamente. |

### <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

Este contenedor admite los siguientes valores mínimos y recomendados:

|Contenedor| Mínima | Recomendado | TPS<br>(Mínimo, máximo)|
|-----------|---------|-------------|--|
|LUIS|1 núcleo, 2 GB de memoria|1 núcleo, 4 GB de memoria|20,40|

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.
* TP - transacciones por segundo

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Utilice el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de un contenedor del repositorio `mcr.microsoft.com/azure-cognitive-services/luis`:

```
docker pull mcr.microsoft.com/azure-cognitive-services/luis:latest
```

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor.

Para obtener una descripción completa de las etiquetas disponibles, como la etiqueta `latest` que se utilizó en el comando anterior, consulte [LUIS](https://go.microsoft.com/fwlink/?linkid=2043204) en Docker Hub.

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

![Proceso para utilizar el contenedor de Language Understanding (LUIS)](./media/luis-container-how-to/luis-flow-with-containers-diagram.jpg)

1. [Exporte el paquete](#export-packaged-app-from-luis) del contenedor desde el portal de LUIS o las API de LUIS.
1. Mueva el archivo del paquete al directorio de **entrada** correspondiente del [equipo host](#the-host-computer). No debe modificar el archivo del paquete de LUIS, descomprimirlo ni cambiar su nombre.
1. [Ejecute el contenedor](##run-the-container-with-docker-run) con la configuración de facturación y el _montaje de entrada_. Hay más [ejemplos](luis-container-configuration.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulte el punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 
1. Cuando haya terminado con el contenedor, [importe los registros de punto de conexión](#import-the-endpoint-logs-for-active-learning) desde la salida de montaje en el portal de LUIS y [detenga](#stop-the-container) el contenedor.
1. En el portal de LUIS, utilice el [aprendizaje activo ](luis-how-to-review-endoint-utt.md) de la página sobre la **revisión de expresiones de puntos de conexión** para mejorar la aplicación.

La aplicación que se ejecuta en el contenedor no se puede modificar. Para poder cambiar la aplicación del contenedor, tiene que modificarla en el servicio de LUIS utilizando el portal o las [API de creación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) de [LUIS](https://www.luis.ai). Después de realizar el entrenamiento o la publicación, descargue un nuevo paquete y ejecute de nuevo el contenedor.

La aplicación de LUIS que está dentro del contenedor no puede exportarse de nuevo al servicio de LUIS. Solo se pueden cargar los registros de consulta. 

## <a name="export-packaged-app-from-luis"></a>Exportación de la aplicación empaquetada desde LUIS

El contenedor de LUIS necesita una aplicación de LUIS que esté entrenada o publicada para responder a consultas de predicción de expresiones de los usuarios. Para obtener la aplicación de LUIS, utilice la API del paquete publicado o entrenado. 

La ubicación predeterminada es el subdirectorio `input` del lugar donde se ejecuta el comando `docker run`.  

Sitúe el archivo del paquete en un directorio y, cuando ejecute el contenedor de Docker, haga referencia a este directorio estableciéndolo como montaje de entrada. 

### <a name="package-types"></a>Tipos de paquetes

El directorio de montaje de entrada puede contener la versión de **producción**, la versión de **ensayo** y la versión **entrenada** de la aplicación al mismo tiempo. Todos los paquetes se montan. 

|Tipo de paquete|API de punto de conexión de consulta|Disponibilidad de consultas|Formato del nombre de archivo del paquete|
|--|--|--|--|
|Entrenada|Get, Post|Solo el contenedor|`{APPLICATION_ID}_v{APPLICATION_VERSION}.gz`|
|Ensayo|Get, Post|Azure y el contenedor|`{APPLICATION_ID}_STAGING.gz`|
|Producción|Get, Post|Azure y el contenedor|`{APPLICATION_ID}_PRODUCTION.gz`|

> [!IMPORTANT]
> No debe modificar los archivos del paquete de LUIS, descomprimirlos ni cambiar su nombre.

### <a name="packaging-prerequisites"></a>Requisitos previos de empaquetado

Para poder empaquetar una aplicación de LUIS, debe tener lo siguiente:

|Requisitos de empaquetado|Detalles|
|--|--|
|Instancia del recurso de Azure _Language Understanding_|Las regiones admitidas son<br><br>Oeste de EE. UU. (```westus```)<br>Europa Occidental (```westeurope```)<br>Este de Australia (```australiaeast```)|
|Aplicación de LUIS entrenada o publicada|Sin [dependencias no compatibles](#unsupported-dependencies). |
|Acceso al sistema de archivos del [equipo host](#the-host-computer) |El equipo host debe permitir un [montaje de entrada](luis-container-configuration.md#mount-settings).|
  
### <a name="export-app-package-from-luis-portal"></a>Exportación del paquete de aplicación desde el portal de LUIS

El [portal](https://www.luis.ai) de LUIS brinda la posibilidad de exportar el paquete de la aplicación publicada o entrenada. 

### <a name="export-published-apps-package-from-luis-portal"></a>Exportación del paquete de la aplicación publicada desde el portal de LUIS

El paquete de la aplicación publicada está disponible en la página **My Apps** (Mis aplicaciones). 

1. Inicie sesión en el [portal](https://www.luis.ai) de LUIS.
1. Active la casilla situada a la izquierda del nombre de la aplicación de la lista. 
1. Seleccione el elemento **Export** (Exportar) en la barra de herramientas contextual situada encima de la lista.
1. Seleccione **Export for container (GZIP)** [Exportar para contenedor (GZIP)].
1. Seleccione el entorno **Production slot** (Espacio de producción) o **Staging slot** (Espacio de ensayo).
1. El paquete se descarga desde el explorador.

![Exportación del paquete publicado del contenedor desde el menú Export (Exportar) de la página de la aplicación](./media/luis-container-how-to/export-published-package-for-container.png)

### <a name="export-trained-apps-package-from-luis-portal"></a>Exportación del paquete de la aplicación entrenada desde el portal de LUIS

El paquete de la aplicación entrenada está disponible en la página **Versions** (Versiones). 

1. Inicie sesión en el [portal](https://www.luis.ai) de LUIS.
1. Seleccione la aplicación en la lista. 
1. Seleccione **Manage** (Administrar) en la barra de navegación de la aplicación.
1. Seleccione **Versions** (Versiones) en la barra de navegación de la izquierda.
1. Active la casilla situada a la izquierda del nombre de la versión de la lista.
1. Seleccione el elemento **Export** (Exportar) en la barra de herramientas contextual situada encima de la lista.
1. Seleccione **Export for container (GZIP)** [Exportar para contenedor (GZIP)].
1. El paquete se descarga desde el explorador.

![Exportación del paquete entrenado del contenedor desde el menú Export (Exportar) de la página de versiones](./media/luis-container-how-to/export-trained-package-for-container.png)


### <a name="export-published-apps-package-from-api"></a>Exportación del paquete de la aplicación publicada desde la API

Utilice el siguiente método de la API REST para empaquetar una aplicación de LUIS que ya esté [publicada](luis-how-to-publish-app.md). Utilice sus proprios valores para sustituir los marcadores de posición de la llamada API; para ello, utilice la tabla situada bajo la especificación HTTP.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Marcador de posición | Valor |
|-------------|-------|
|{APPLICATION_ID} | Identificador de la aplicación de LUIS publicada. |
|{APPLICATION_ENVIRONMENT} | Entorno de la aplicación de LUIS publicada. Utilice uno de los valores siguientes:<br/>```PRODUCTION```<br/>```STAGING``` |
|{AUTHORING_KEY} | Clave de creación de la cuenta de LUIS para la aplicación de LUIS publicada.<br/>Puede obtener la clave de creación en la página **User Settings** (Configuración del usuario) del portal de LUIS. |
|{AZURE_REGION} | Región de Azure que corresponda:<br/><br/>```westus```: Oeste de EE. UU.<br/>```westeurope```: Europa Occidental<br/>```australiaeast```: Este de Australia |

Utilice el siguiente comando CURL para descargar el paquete publicado aplicando sus propios valores:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_{APPLICATION_ENVIRONMENT}.gz
```

Si el proceso se realiza correctamente, la respuesta será un archivo de paquete de LUIS. Guarde el archivo en la ubicación de almacenamiento especificada para el montaje de entrada del contenedor. 

### <a name="export-trained-apps-package-from-api"></a>Exportación del paquete de la aplicación entrenada desde la API

Utilice el siguiente método de la API REST para empaquetar una aplicación de LUIS que ya esté [entrenada](luis-how-to-train.md). Utilice sus proprios valores para sustituir los marcadores de posición de la llamada API; para ello, utilice la tabla situada bajo la especificación HTTP.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Marcador de posición | Valor |
|-------------|-------|
|{APPLICATION_ID} | Identificador de la aplicación de LUIS entrenada. |
|{APPLICATION_VERSION} | Versión de la aplicación de LUIS entrenada. |
|{AUTHORING_KEY} | Clave de creación de la cuenta de LUIS para la aplicación de LUIS publicada.<br/>Puede obtener la clave de creación en la página **User Settings** (Configuración del usuario) del portal de LUIS.  |
|{AZURE_REGION} | Región de Azure que corresponda:<br/><br/>```westus```: Oeste de EE. UU.<br/>```westeurope```: Europa Occidental<br/>```australiaeast```: Este de Australia |

Para descargar el paquete entrenado, utilice el siguiente comando CURL:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_v{APPLICATION_VERSION}.gz
```

Si el proceso se realiza correctamente, la respuesta será un archivo de paquete de LUIS. Guarde el archivo en la ubicación de almacenamiento especificada para el montaje de entrada del contenedor. 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar el contenedor. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{ENDPOINT_KEY} | Esta clave se usa para iniciar el contenedor. No utilice la clave de inicio. |
|{BILLING_ENDPOINT} | El valor del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Language Understanding.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> El comando anterior usa el directorio que está fuera de la unidad `c:` para evitar conflictos con los permisos de Windows. Si necesita usar un directorio específico como directorio de entrada, tal vez tenga que conceder permiso al servicio de Docker. El comando de Docker anterior utiliza la barra diagonal inversa, `\`, como carácter de continuación de línea. Puede quitarla o reemplazarla en función de los requisitos del sistema operativo del [equipo host](#the-host-computer). No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.


Este comando:

* Ejecuta un contenedor desde la imagen de contenedor de LUIS.
* Carga la aplicación de LUIS desde el montaje de entrada de c:\input, situado en el host del contenedor.
* Asigna dos núcleos de la CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Guarda los registros de LUIS y el contenedor en el montaje de salida de c:\output, situado en el host del contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

Hay más [ejemplos](luis-container-configuration.md#example-docker-run-commands) del comando `docker run` disponibles. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).
> El valor de ApiKey es la **clave** de la página de claves y puntos de conexión del portal de LUIS y también está disponible en la página de claves del recurso de Azure Language Understanding.  

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. Los puntos de conexión de las aplicaciones publicadas (en ensayo o producción), tienen una ruta _diferente_ a la de los puntos de conexión de las aplicaciones entrenadas. 

Utilice el host, `https://localhost:5000`, con las API de contenedor. 

|Tipo de paquete|Método|Enrutar|Parámetros de consulta|
|--|--|--|--|
|Publicado|[Get](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78), [Post](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)|/luis/v2.0/apps/{appId}?|q={q}<br>&staging<br>[&timezoneOffset]<br>[&verbose]<br>[&log]<br>|
|Entrenada|Get, Post|/luis/v2.0/apps/{appId}/versions/{versionId}?|q={q}<br>[&timezoneOffset]<br>[&verbose]<br>[&log]|

Los parámetros de consulta determinan cómo y qué se devuelve en la respuesta de la consulta:

|Parámetro de consulta|Type|Propósito|
|--|--|--|
|`q`|string|Expresión del usuario.|
|`timezoneOffset`|número|timezoneOffset le permite [cambiar la zona horaria](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) que se utiliza en la entidad datetimeV2 pregenerada.|
|`verbose`|boolean|Si está establecido en true, devuelve todas las intenciones y sus puntuaciones. El valor predeterminado es false, donde solo se devuelve la intención principal.|
|`staging`|boolean|Si está establecido en true, devuelve la consulta a partir de los resultados del entorno de ensayo. |
|`log`|boolean|Registra las consultas, lo que puede utilizarse después para el [aprendizaje activo](luis-how-to-review-endoint-utt.md). El valor predeterminado es true.|

### <a name="query-published-app"></a>Consulta de la aplicación publicada

A continuación, se muestra un ejemplo de un comando CURL para consultar el contenedor de una aplicación publicada:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}?q=turn%20on%20the%20lights&staging=false&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Para realizar consultas en el entorno de **ensayo**, cambie el valor del parámetro de la cadena de consulta **staging** a true: 

`staging=true`

### <a name="query-trained-app"></a>Consulta de la aplicación entrenada

A continuación, se muestra un ejemplo de un comando CURL para consultar el contenedor de una aplicación entrenada: 

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}/versions/{APPLICATION_VERSION}?q=turn%20on%20the%20lights&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
El nombre de la versión tiene un máximo de 10 caracteres y solamente contiene los caracteres que se permiten en las direcciones URL. 

## <a name="import-the-endpoint-logs-for-active-learning"></a>Importación de los registros de punto de conexión para el aprendizaje activo

Si se especifica un montaje de salida para el contenedor de LUIS, los archivos de registro de consultas de la aplicación se guardan en el directorio de salida, donde {INSTANCE_ID} es el identificador del contenedor. El registro de consultas de la aplicación contiene la consulta, la respuesta y las marcas de tiempo de cada consulta de predicción enviada al contenedor de LUIS. 

En la siguiente ubicación, se muestra la estructura anidada de directorios de los archivos de registro del contenedor.
`
/output/luis/{INSTANCE_ID}/
`
 
En el portal de LUIS, seleccione la aplicación y la opción **Import endpoint logs** (Importar registros de puntos de conexión) para cargar estos registros. 

![Importación de los archivos de registro del contenedor para el aprendizaje activo](./media/luis-container-how-to/upload-endpoint-log-files.png)

Después de cargar el registro, [revise las expresiones del punto de conexión](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-review-endpoint-utterances) en el portal de LUIS.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Detención del contenedor

Para apagar el contenedor, en el entorno de la línea de comandos en la que se ejecuta el contenedor, presione **Ctrl + C**.

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](luis-container-configuration.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor. 

## <a name="billing"></a>Facturación

El contenedor de LUIS envía información de facturación a Azure mediante un recurso de _Language Understanding_ de la cuenta de Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](luis-container-configuration.md) (Configuración de contenedores).

## <a name="unsupported-dependencies"></a>Dependencias no compatibles

Puede usar una aplicación de LUIS si **no incluye** ninguna de las siguientes dependencias:

Configuraciones de aplicaciones no admitidas|Detalles|
|--|--|
|Referencias culturales de contenedor no admitidas| Alemán (de-de)<br>Neerlandés (nl-NL)<br>Japonés (ja-JP)<br>|
|Dominios no admitidos|Dominios creados previamente, incluidas las entidades y las intenciones del dominio que se crearon previamente|
|Entidades no compatibles con ninguna referencia cultural|Entidad [KeyPhrase](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-keyphrase) pregenerada para todas las referencias culturales|
|Entidades no compatibles con la referencia cultural Inglés (en-US)|Entidades [GeographyV2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-geographyv2) pregeneradas|
|Preparación para la voz|El contenedor no admite dependencias externas.|
|análisis de opiniones|El contenedor no admite dependencias externas.|
|Bing Spell Check|El contenedor no admite dependencias externas.|

## <a name="summary"></a>Resumen

En este artículo, ha aprendido algunos conceptos y ha visto el flujo de trabajo de Language Understanding (LUIS) para descargar, instalar y ejecutar contenedores. En resumen:

* Language Understanding (LUIS) proporciona un contenedor de Linux para Docker que permite hacer predicciones de consultas de puntos de conexión de expresiones.
* Las imágenes del contenedor se descargan desde Microsoft Container Registry (MCR).
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST para consultar los puntos de conexión del contenedor especificando el URI que tiene el contenedor en el host.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](luis-container-configuration.md) (Configuración de contenedores) para ver las opciones de configuración.
* Consulte [Solución de problemas](troubleshooting.md) para resolver los problemas relacionados con la funcionalidad de LUIS.
* Uso de [Contenedores de Cognitive Services](../cognitive-services-container-support.md)
