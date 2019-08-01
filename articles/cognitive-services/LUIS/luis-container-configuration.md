---
title: Configuración del contenedor de Docker
titleSuffix: Language Understanding - Azure Cognitive Services
description: El entorno en tiempo de ejecución del contenedor de LUIS se configura mediante los argumentos del comando `docker run`. LUIS tiene varias opciones necesarias, junto con algunas opciones de configuración opcionales.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: dapine
ms.openlocfilehash: 4a9f7762b7960c74acad8203f70bc1e7c7cbd90f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063224"
---
# <a name="configure-language-understanding-docker-containers"></a>Configuración de los contenedores de Docker de Language Understanding 

El entorno en tiempo de ejecución del contenedor de **Language Understanding** (LUIS) se configura mediante los argumentos del comando `docker run`. LUIS tiene varias opciones necesarias, junto con algunas opciones de configuración opcionales. Hay disponibles varios [ejemplos](#example-docker-run-commands) del comando. La configuración específica del contenedor son la [configuración de montaje](#mount-settings) de entrada y la configuración de facturación. 

## <a name="configuration-settings"></a>Valores de configuración

Este contenedor tiene las siguientes opciones de configuración:

|Obligatorio|Configuración|Propósito|
|--|--|--|
|Sí|[ApiKey](#apikey-setting)|Se usa para realizar un seguimiento de la información de facturación.|
|Sin|[Application Insights](#applicationinsights-setting)|Le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor.|
|Sí|[Facturación](#billing-setting)|Especifica el URI del punto de conexión del recurso de servicio en Azure.|
|Sí|[Eula](#eula-setting)| Indica que ha aceptado la licencia del contenedor.|
|Sin|[Fluentd](#fluentd-settings)|Escribe el registro y, opcionalmente, los datos de métricas en un servidor de Fluentd.|
|Sin|[Proxy Http](#http-proxy-credentials-settings)|Configure un proxy HTTP para hacer solicitudes salientes.|
|Sin|[Logging](#logging-settings)|Proporciona compatibilidad con el registro de ASP.NET Core al contenedor. |
|Sí|[Mounts](#mount-settings)|Leer y escribir datos desde el equipo host al contenedor y del contenedor al equipo host.|

> [!IMPORTANT]
> Las opciones [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting) y [`Eula`](#eula-setting) se usan en conjunto y debe proporcionar valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>Opción de configuración ApiKey

La opción de configuración `ApiKey` especifica la clave de recurso de Azure usada para realizar un seguimiento de la información de facturación del contenedor. Debe especificar un valor para ApiKey que debe ser una clave válida para el recurso de _Cognitive Services_ especificado para la opción de configuración [`Billing`](#billing-setting).

Este valor se puede encontrar en los siguientes lugares:

* Azure Portal: Administración de recursos de **Cognitivas Services**, en **Claves**
* Portal de LUIS: en la página de configuración **Claves y puntos de conexión**. 

No utilice la clave de inicio ni la clave de creación. 

## <a name="applicationinsights-setting"></a>Opción de configuración ApplicationInsights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Opción de configuración Billing

La opción de configuración `Billing` especifica el URI del punto de conexión del recurso de _Cognitive Services_ que se usa para medir la información de facturación del contenedor. Debe especificar un valor para esta opción de configuración, que debe ser un URI de punto de conexión válido para un recurso de _Cognitive Services_ en Azure. El contenedor informa sobre el uso cada 10 a 15 minutos.

Este valor se puede encontrar en los siguientes lugares:

* Azure Portal: Información general de **Cognitivas Services**, con la etiqueta `Endpoint`
* Portal de LUIS: en la página de configuración **Claves y puntos de conexión**, como parte del identificador URI del punto de conexión.

No olvide incluir la ruta `luis/v2.0` en la dirección URL como se muestra en la tabla siguiente:


|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|Sí| `Billing` | Cadena | Identificador URI del punto de conexión de facturación<br><br>Ejemplo:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

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

El contenedor de LUIS no usa los montajes de entrada o salida para almacenar datos de entrenamiento o del servicio. 

La sintaxis exacta de la ubicación de montaje del host varía según el sistema operativo del host. Además, la ubicación de montaje del [equipo host](luis-container-howto.md#the-host-computer) puede no estar accesible debido a un conflicto entre los permisos que utiliza la cuenta de servicio de Docker y los permisos de la ubicación de montaje del host. 

En la tabla siguiente se describen las configuraciones admitidas.

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|-------|------|-----------|-------------|
|Sí| `Input` | Cadena | Destino del montaje de entrada. El valor predeterminado es `/input`. Esta es la ubicación de los archivos de paquete de LUIS. <br><br>Ejemplo:<br>`--mount type=bind,src=c:\input,target=/input`|
|Sin| `Output` | Cadena | Destino del montaje de salida. El valor predeterminado es `/output`. Esta es la ubicación de los registros. Incluye los registros de consultas de LUIS y los registros de contenedor. <br><br>Ejemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Comandos de ejemplo de docker run

Los ejemplos siguientes usan las opciones de configuración para ilustrar cómo escribir y usar comandos `docker run`.  Una vez que se está ejecutando, el contenedor continúa ejecutándose hasta que lo [detenga](luis-container-howto.md#stop-the-container).

* Los ejemplos de `c:` usan el directorio que está en la unidad para evitar conflictos con los permisos de Windows. Si necesita usar un directorio específico como directorio de entrada, tal vez tenga que conceder permiso al servicio de Docker. 
* No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.
* Si usa otro sistema operativo, utilice la consola o el terminal, la sintaxis de carpeta de los montajes y el carácter de continuación de línea que sean adecuados para su sistema. En estos ejemplos se supone que se usa una consola de Windows con un carácter de continuación de línea `^`. Dado que el contenedor es un sistema operativo Linux, el montaje de destino usa una sintaxis de carpeta basada en Linux.

No olvide incluir la ruta `luis/v2.0` en la dirección URL como se muestra en la tabla siguiente.

Reemplace {_argument_name_} por sus propios valores:

| Marcador de posición | Valor | Formato o ejemplo |
|-------------|-------|---|
|{ENDPOINT_KEY} | Clave del punto de conexión de la aplicación de LUIS entrenada. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | El valor del punto de conexión de facturación está disponible en la página Información general de Azure `Cognitive Services`. |https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](luis-container-howto.md#billing).
> El valor de ApiKey es la **clave** de la página de claves y puntos de conexión del portal de LUIS y también está disponible en la página de claves del recurso de Azure `Cognitive Services`. 

### <a name="basic-example"></a>Ejemplo básico

El ejemplo siguiente tiene los argumentos mínimos posibles para ejecutar el contenedor:

```console
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY}
```

### <a name="applicationinsights-example"></a>Ejemplo de ApplicationInsights

El ejemplo siguiente establece el argumento ApplicationInsights para enviar datos de telemetría a Application Insights mientras se está ejecutando el contenedor:

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY} ^
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example"></a>Ejemplo de registro 

El siguiente comando establece el nivel de registro, `Logging:Console:LogLevel`, para configurar el nivel de registro en [`Information`](https://msdn.microsoft.com). 

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY} ^
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Instalación y ejecución de contenedores](luis-container-howto.md)
* Consulte [Solución de problemas](troubleshooting.md) para resolver los problemas relacionados con la funcionalidad de LUIS.
* Uso de [Contenedores de Cognitive Services](../cognitive-services-container-support.md)
