---
title: Configuración de contenedor (Form Recognizer)
titleSuffix: Azure Cognitive Services
description: Vea cómo configurar el contenedor Form Recognizer para analizar los datos de formularios y tablas.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 05/15/2019
ms.author: pafarley
ms.openlocfilehash: 17cf1d88701370c4f81eab4f0d2df33ee2e94af5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796412"
---
# <a name="configure-form-recognizer-containers"></a>Configuración de contenedores de Form Recognizer

Los contenedores de Form Recognizer permiten a los clientes crear una arquitectura de aplicación optimizada para aprovechar las sólidas funcionalidades de la nube y la localidad del perímetro.

El entorno del runtime de los contenedores de **Form Recognizer** se configura mediante los argumentos del comando `docker run`. Este contenedor tiene varias opciones de configuración necesarias, así como otras opcionales. Hay disponibles varios [ejemplos](#example-docker-run-commands) del comando. La configuración específica del contenedor es la configuración de facturación.

## <a name="configuration-settings"></a>Valores de configuración

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> Las opciones [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) y [`Eula`](#eula-setting) se usan en conjunto y debe proporcionar valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](form-recognizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>Opción de configuración ApiKey

La opción de configuración `ApiKey` especifica la clave de recurso de Azure usada para realizar un seguimiento de la información de facturación del contenedor. Debe especificar un valor para ApiKey, que debe ser una clave válida para el recurso de _Form Recognizer_ especificado para la opción de configuración [`Billing`](#billing-configuration-setting).

Este valor se puede encontrar en el siguiente lugar:

* Azure Portal: Administración de recursos de **Form Recognizer**, en **Claves**

## <a name="applicationinsights-setting"></a>Opción de configuración ApplicationInsights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Opción de configuración Billing

El valor de configuración `Billing` especifica el identificador URI del punto de conexión del recurso de _Form Recognizer_ en Azure que se usa para medir la información de facturación del contenedor. Debe especificar un valor para esta opción de configuración, y el valor debe ser un URI de punto de conexión válido para un recurso de _Form Recognizer_ en Azure. El contenedor informa sobre el uso cada 10 a 15 minutos.

Este valor se puede encontrar en el siguiente lugar:

* Azure Portal: Información general de **Form Recognizer**, con la etiqueta `Endpoint`

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|Sí| `Billing` | string | Identificador URI del punto de conexión de facturación<br><br>Ejemplo:<br>`Billing=https://westus2.api.cognitive.microsoft.com/` |

## <a name="eula-setting"></a>Opción de configuración Eula

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Opción de configuración Fluentd

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Configuración de las credenciales del proxy HTTP

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Opción de configuración Logging

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Configuración de montaje

Utilice montajes de enlace para leer y escribir datos hacia y desde el contenedor. Puede especificar un montaje de entrada o un montaje de salida mediante la opción `--mount` del comando [docker run](https://docs.docker.com/engine/reference/commandline/run/).

El contenedor Form Recognizer requiere un montaje de entrada y salida. El de entrada puede ser de solo lectura y es necesario para acceder a los datos que se usarán para el entrenamiento y la puntuación. En el de salida se debe poder escribir y se usará para almacenar los modelos y los datos temporales.

La sintaxis exacta de la ubicación de montaje del host varía según el sistema operativo del host. Además, la ubicación de montaje del [equipo host](form-recognizer-container-howto.md#the-host-computer) puede no ser accesible debido a un conflicto entre los permisos que usa la cuenta de servicio de Docker y los permisos de la ubicación de montaje del host.

|Opcional| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|-------|------|-----------|-------------|
|Obligatorio| `Input` | string | Destino del montaje de entrada. El valor predeterminado es `/input`.    <br><br>Ejemplo:<br>`--mount type=bind,src=c:\input,target=/input`|
|Obligatorio| `Output` | string | Destino del montaje de salida. El valor predeterminado es `/output`.  <br><br>Ejemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Comandos de ejemplo de docker run

Los ejemplos siguientes usan las opciones de configuración para ilustrar cómo escribir y usar comandos `docker run`.  Una vez que se está ejecutando, el contenedor continúa ejecutándose hasta que lo [detenga](form-recognizer-container-howto.md#stop-the-container).

* **Carácter de continuación de línea**: Los comandos de Docker de las secciones siguientes usan la barra diagonal inversa (`\`) como un carácter de continuación de línea. Puede quitarla o reemplazarla en función de los requisitos del sistema operativo del host.
* **Orden de los argumentos**: No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.

Reemplace {_argument_name_} por sus propios valores:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en página de claves de Form Recognizer en Azure Portal.  |
|{BILLING_ENDPOINT_URI} | El valor de URI del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Form Recognizer.|
|{COMPUTER_VISION_API_KEY}| La clave está disponible en la página Claves de la API Computer Vision de Azure Portal.|
|{COMPUTER_VISION_ENDPOINT_URI}|El punto de conexión de facturación. Si está utilizando un recurso de Computer Vision en la nube, el valor del identificador URI está disponible en la página de introducción de la API Computer Vision de Azure Portal. Si utiliza un contenedor `cognitive-services-recognize-text`, use la dirección URL del punto de conexión de facturación pasada al contenedor en el comando `docker run`.|

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing-configuration-setting).
> El valor de ApiKey es la **clave** de la página de claves del recurso de Azure Form Recognizer.

## <a name="form-recognizer-container-docker-examples"></a>Ejemplos de Docker del contenedor de Form Recognizer

Los siguientes ejemplos de Docker son para el contenedor de Form Recognizer.

### <a name="basic-example-for-form-recognizer"></a>Ejemplo básico de Form Recognizer

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

### <a name="logging-example-for-form-recognizer"></a>Ejemplo de registro de Form Recognizer

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
Logging:Console:LogLevel:Default=Information
```


## <a name="next-steps"></a>Pasos siguientes

* Consulte [Instalación y ejecución de contenedores](form-recognizer-container-howto.md)
