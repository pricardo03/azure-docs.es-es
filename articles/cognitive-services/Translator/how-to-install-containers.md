---
title: 'Instalación y ejecución de contenedores: Translator Text'
titleSuffix: Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Translator Text en este tutorial paso a paso.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 7b2b07f71d00e6da66062d1826f8c5c88bb6be7a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506823"
---
# <a name="install-and-run-translator-text-containers"></a>Instalar y ejecutar contenedores de Translator Text

<!--
    ACOM forward link:
    https://docs.microsoft.com/azure/cognitive-services/translator/howto-install-containers
-->

Los contenedores permiten ejecutar Translator Text API en su propio entorno y son ideales para sus requisitos específicos de seguridad y gobernanza de datos.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos para poder usar contenedores de Translator Text:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.|

## <a name="request-access-to-the-container-registry"></a>Solicitud de acceso al registro de contenedor

Primero debe completar y enviar el [formulario de solicitud de contenedores de Translator Text](https://aka.ms/translatorcontainerform) para solicitar acceso al contenedor.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

### <a name="the-host-computer"></a>El equipo host

El host es un equipo basado en x64 con un sistema operativo Linux que ejecuta el contenedor de Docker. Puede ser un equipo del entorno local o un servicio de hospedaje de Docker incluido en Azure, como:

* Azure Kubernetes Service.
* Azure Container Instances.
* Un clúster de [Kubernetes](https://kubernetes.io/) implementado en Azure Stack.

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente, se describe la cantidad mínima y recomendada de núcleos de CPU, por lo menos 2,6 gigahercios (GHz) o más rápida, y de memoria, en gigabytes (GB), para asignar a cada contenedor de Translator Text.

| Contenedor | Mínima | Par de idiomas |
|-----------|---------|---------------|
| Translator Text | 4 núcleos, 4 GB de memoria | 4 |

Para cada par de idioma, se recomienda tener 1 GB de memoria. De forma predeterminada, el contenedor de Translator Text tiene 3 o 4 pares de idioma, dependiendo de la etiqueta `<image-tag>` que se esté ejecutando. Consulte [idiomas admitidos y traducción](#supported-languages-and-translation) para obtener los detalles. El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Las imágenes de contenedor para Translator Text están disponibles en el repositorio de contenedores siguiente. En la tabla también se asignan las etiquetas de imagen del contenedor y sus idiomas admitidos correspondientes.

| Contenedor | Etiqueta de imagen | Idiomas admitidos |
|-----------|-----------|---------------------|
| `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` | `ar_de_en_ru_zh_1.0.0` | `ar-SA`, `zh-CN`, `de-DE` y `ru-RU` |
| `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` | `de_en_es_fr_1.0.0` | `de-DE`, `fr-FR` y `es-ES` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-translator-text-container"></a>Comando docker pull para el contenedor Translator Text

Para ejecutar el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), primero necesita acceder al registro de contenedor. Desde la CLI de Azure, puede iniciar sesión en Azure Container Registry mediante el comando [`az acr login`](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az-acr-login).

```azureinteractive
az acr login --name containerpreview.azurecr.io
```

Este comando autenticará el usuario actual con la instancia de Azure Container Registry correspondiente. Ahora, puede ejecutar el comando `docker pull`.

> [!NOTE]
> En función de la compatibilidad de idioma que necesite, proporcione la etiqueta `<image-tag>` correspondiente.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-translator-text:<image-tag>
```

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run). Hay más [ejemplos](translator-text-container-config.md#example-docker-run-commands) del comando `docker run` disponibles.
1. [Consulte el punto de conexión de traducción del contenedor](#query-the-containers-translate-endpoint).

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. Hay disponibles [ejemplos](translator-text-container-config.md#example-docker-run-commands) del comando `docker run`.

### <a name="run-container-example-of-docker-run-command"></a>Ejecución del ejemplo de contenedor del comando de ejecución de Docker

```Docker
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text:ar_de_en_ru_zh_1.0.0 \
Eula=accept
```

Este comando:

* Ejecuta un contenedor de Translator Text desde la imagen de contenedor.
* Asigna un núcleo de 4 CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Acepta el contrato de usuario final (EULA).
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen de contenedor sigue estando disponible en el equipo host.

### <a name="how-to-collect-docker-logs"></a>Cómo recopilar registros de Docker

Para solucionar problemas, puede ser útil ver los registros de Docker desde la ejecución del contenedor. En primer lugar, ejecute el comando [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) con la marca de formato para limitar los detalles que se muestran para todos los contenedores.

```Docker
docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.Status}}"

CONTAINER ID    IMAGE                                                                       STATUS
ed6f115697f3    containerpreview.azurecr.io/microsoft/cognitive-services-translator-text    Up 4 minutes
```

A continuación, ejecute el comando [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) con el `<Container ID>` del contenedor correspondiente en cuestión para ver sus registros.

```Docker
docker logs <Container ID> --timestamps --since=4h | grep Error
```

El comando de registros de Docker anterior recopilará los registros de "error" de las últimas cuatro horas.

## <a name="supported-languages-and-translation"></a>Idiomas admitidos y traducción

El método `POST /translate` admite las siguientes conversiones de idiomas, con el cambio de *inglés* a un idioma de destino y viceversa. Observe que, aunque puede intercambiar el inglés con uno de los idiomas de la lista, *no puede* pasar de un idioma *que no sea inglés* a otro idioma *que no sea inglés*.

> [!NOTE]
> Para obtener una calidad óptima, los consumidores solo deben enviar una oración por solicitud.

| Conversión de idioma | Conversión ISO de idioma | Etiquetas de imagen |
|--|--|:--|
| Inglés :left_right_arrow: Chino | `en-US` :left_right_arrow: `zh-CN` | `ar_de_en_ru_zh_1.0.0` |
| Inglés :left_right_arrow: Ruso | `en-US` :left_right_arrow: `ru-RU` | `ar_de_en_ru_zh_1.0.0` |
| Inglés :left_right_arrow: Árabe | `en-US` :left_right_arrow: `ar-SA` | `ar_de_en_ru_zh_1.0.0` |
| Inglés :left_right_arrow: Alemán | `en-US` :left_right_arrow: `de-DE` | `ar_de_en_ru_zh_1.0.0` y `de_en_es_fr_1.0.0` |
| Inglés :left_right_arrow: Español | `en-US` :left_right_arrow: `es-ES` | `de_en_es_fr_1.0.0` |
| Inglés :left_right_arrow: Francés | `en-US` :left_right_arrow: `fr-FR` | `de_en_es_fr_1.0.0` |

> [!IMPORTANT]
> Para poder ejecutar el contenedor, la opción `Eula` debe estar especificada; de lo contrario, el contenedor no se iniciará.

## <a name="query-the-containers-translate-endpoint"></a>Consulta del punto de conexión de traducción del contenedor

El contenedor proporciona una API de punto de conexión de traducción basada en REST. A continuación se indican algunos usos de ejemplo de este punto de conexión:

# <a name="curltabcurl"></a>[cURL](#tab/curl)


Ejecute el siguiente comando cURL, desde la CLI deseada.

```curl
curl -X POST "http://localhost:5000/translate?api-version=3.0&from=en-US&to=de-DE"
    -H "Content-Type: application/json" -d "[{'Text':'hello, how are you'}]"
```

> [!TIP]
> Si intenta esta solicitud POST cURL antes de que el contenedor esté listo, terminará obteniendo la respuesta "El servicio no está disponible temporalmente". Simplemente espere hasta que el contenedor esté listo e inténtelo de nuevo.

La salida siguiente se imprimirá en la consola.

```console
"translations": [
    {
        "text": "hallo, wie geht es dir",
        "to": "de-DE"
    }
]
```

# <a name="swaggertabswagger"></a>[Swagger](#tab/Swagger)

Vaya a la página de Swagger, http://localhost:5000/swagger/index.html.

1. Seleccione **POST /translate**.
1. Haga clic en **Probar**.
1. Introduzca el parámetro **Desde** como `en-US`.
1. Introduzca el parámetro **Hasta** como `de-DE`.
1. Introduzca el parámetro **api-version** como `3.0`.
1. En **texts**, reemplace `string` por el siguiente código JSON.
    ```json
    [
        {
            "text": "hello, how are you"
        }
    ]
    ```
1. Seleccione **Ejecutar** y las traducciones resultantes se mostrarán en **Cuerpo de respuesta**. Debería obtener un resultado parecido al siguiente:
    ```json
    "translations": [
      {
          "text": "hallo, wie geht es dir",
          "to": "de-DE"
      }
    ]
    ```

# <a name="net-coretabnetcore"></a>[.NET Core](#tab/netcore)

1. Inicie Visual Studio y cree una aplicación de consola.
1. Edite el archivo `*.csproj` para agregar el nodo `<LangVersion>7.1</LangVersion>` (especifica C# 7.1).
1. Agregue el paquete NuGet [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/), versión 11.0.2.
1. En `Program.cs`, reemplace el código existente por lo siguiente:
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    namespace TranslateContainer
    {
        class Program
        {
            const string ApiHostEndpoint = "http://localhost:5000";
            const string TranslateApi = "/translate?api-version=3.0&from=en-US&to=de-DE";

            static async Task Main(string[] args)
            {
                var textToTranslate = "hello, how are you";
                var result = await TranslateTextAsync(textToTranslate);

                Console.WriteLine(result);
                Console.ReadLine();
            }

            static async Task<string> TranslateTextAsync(string textToTranslate)
            {
                var body = new object[] { new { Text = textToTranslate } };
                var requestBody = JsonConvert.SerializeObject(body);

                var client = new HttpClient();
                using (var request =
                    new HttpRequestMessage
                    {
                        Method = HttpMethod.Post,
                        RequestUri = new Uri($"{ApiHostEndpoint}{TranslateApi}"),
                        Content = new StringContent(requestBody, Encoding.UTF8, "application/json")
                    })
                {
                    // Send the request and await a response.
                    var response = await client.SendAsync(request);

                    return await response.Content.ReadAsStringAsync();
                }
            }
        }
    }
    ```
1. Presione **F5** para ejecutar el programa.
1. La salida siguiente se imprimirá en la consola.
    ```console
    "translations": [
      {
          "text": "hallo, wie geht es dir",
          "to": "de-DE"
      }
    ]
    ```

***

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](translator-text-container-config.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor.

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Translator Text. En resumen:

* Translator Text proporciona varios contenedores de Linux para Docker y encapsula varios pares de idioma.
* Las imágenes de contenedor se descargan desde el registro de "versión preliminar del contenedor".
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Translator Text mediante la especificación del URI del host del contenedor.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](translator-text-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Consulte las [preguntas más frecuentes (P+F) sobre los contenedores](../containers/container-faq.md) para resolver problemas relacionados con la funcionalidad.
