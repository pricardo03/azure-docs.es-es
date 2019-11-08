---
title: 'Configuración de contenedores: Translator Text'
titleSuffix: Azure Cognitive Services
description: Translator Text proporciona a los contenedores un marco de configuración común, por lo que puede configurar y administrar fácilmente la configuración de almacenamiento, registro, telemetría y seguridad de los contenedores.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 3dbedcb5e5212091dc8906defa2b1cf5c7868d60
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506747"
---
# <a name="configure-translator-text-docker-containers"></a>Configuración de contenedores de Docker de Translator Text

Translator Text proporciona a cada contenedor un marco de configuración común, por lo que puede configurar y administrar fácilmente la configuración de almacenamiento, registro, telemetría y seguridad de los contenedores.

## <a name="configuration-settings"></a>Valores de configuración

Este contenedor tiene las siguientes opciones de configuración:

|Obligatorio|Configuración|Propósito|
|--|--|--|
|Sin|[Application Insights](#applicationinsights-setting)|Permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor.|
|Sí|[Eula](#eula-setting)| Indica que ha aceptado la licencia del contenedor.|
|Sin|[Fluentd](#fluentd-settings)|Escribe el registro y, opcionalmente, los datos de métricas en un servidor de Fluentd.|
|Sin|Proxy HTTP|Configura un proxy HTTP para realizar solicitudes de salida.|
|Sin|[Logging](#logging-settings)|Proporciona compatibilidad con el registro de ASP.NET Core al contenedor. |
|Sin|[Mounts](#mount-settings)|Lee y escribe los datos desde el equipo host al contenedor y del contenedor de nuevo al equipo host.|

> [!IMPORTANT]
> Se debe proporcionar el valor de [`Eula`](#eula-setting) con el valor de `accept`. De lo contrario, el contenedor no se iniciará.

## <a name="applicationinsights-setting"></a>Opción de configuración ApplicationInsights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

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

El contenedor de Translator Text no usa los montajes de entrada o salida para almacenar datos de entrenamiento o del servicio. 

La sintaxis exacta de la ubicación de montaje del host varía según el sistema operativo del host. Además, la ubicación de montaje del [equipo host](how-to-install-containers.md#the-host-computer) puede no estar accesible debido a un conflicto entre los permisos que utiliza la cuenta de servicio de Docker y los permisos de la ubicación de montaje del host. 

|Opcional| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|-------|------|-----------|-------------|
|No permitida| `Input` | Cadena | Los contenedores de Translator Text no usan esto.|
|Opcional| `Output` | Cadena | Destino del montaje de salida. El valor predeterminado es `/output`. Esta es la ubicación de los registros. Esto incluye los registros de contenedor. <br><br>Ejemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Comandos de ejemplo de docker run 

Los ejemplos siguientes usan las opciones de configuración para ilustrar cómo escribir y usar comandos `docker run`.  Una vez que se está ejecutando, el contenedor continúa ejecutándose hasta que lo [detenga](how-to-install-containers.md#stop-the-container).

* **Carácter de continuación de línea**: Los comandos de Docker de las secciones siguientes usan la barra diagonal inversa, `\`, como un carácter de continuación de línea. Puede quitarla o reemplazarla en función de los requisitos del sistema operativo del host. 
* **Orden de los argumentos**: No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.

## <a name="translator-text-container-docker-examples"></a>Ejemplos de Docker del contenedor de Translator Text

Los siguientes ejemplos de Docker son del contenedor de Translator Text.

### <a name="basic-example"></a>Ejemplo básico

```
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text \
Eula=accept
```

### <a name="logging-example"></a>Ejemplo de registro

```
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text \
Eula=accept \
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Instalación y ejecución de contenedores](how-to-install-containers.md)
* Use más [contenedores de Cognitive Services](../cognitive-services-container-support.md)
