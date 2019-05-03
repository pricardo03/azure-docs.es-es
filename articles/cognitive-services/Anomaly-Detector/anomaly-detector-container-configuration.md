---
title: Configurar los contenedores - detector de anomalías
titleSuffix: Azure Cognitive Services
description: El entorno de tiempo de ejecución del contenedor de Detector de anomalías se configura mediante el `docker run` argumentos de comandos. Este contenedor tiene varias opciones de configuración necesarias, así como otras opcionales.
services: cognitive-services
author: aahill
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/07/2019
ms.author: aahi
ms.openlocfilehash: 0d09ce29aa5431de3eb82e5d9fe7440d4e3352e1
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026395"
---
# <a name="configure-anomaly-detector-containers"></a>Configurar los contenedores del Detector de anomalías

El **Detector de anomalías** entorno en tiempo de ejecución del contenedor se configura mediante el `docker run` argumentos de comandos. Este contenedor tiene varias opciones de configuración necesarias, así como otras opcionales. Hay disponibles varios [ejemplos](#example-docker-run-commands) del comando. La configuración específica del contenedor es la configuración de facturación. 

# <a name="configuration-settings"></a>Valores de configuración

Este contenedor tiene las siguientes opciones de configuración:

|Obligatorio|Configuración|Propósito|
|--|--|--|
|Sí|[ApiKey](#apikey-configuration-setting)|Se usa para realizar un seguimiento de la información de facturación.|
|Sin |[Application Insights](#applicationinsights-setting)|Le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor.|
|Sí|[Facturación](#billing-configuration-setting)|Especifica el URI del punto de conexión del recurso de servicio en Azure.|
|Sí|[Eula](#eula-setting)| Indica que ha aceptado la licencia del contenedor.|
|Sin |[Fluentd](#fluentd-settings)|Escribe el registro y, opcionalmente, los datos de métricas en un servidor de Fluentd.|
|Sin |[Proxy Http](#http-proxy-credentials-settings)|Configure un proxy HTTP para hacer solicitudes salientes.|
|Sin |[Logging](#logging-settings)|Proporciona compatibilidad con el registro de ASP.NET Core al contenedor. |
|Sin |[Mounts](#mount-settings)|Leer y escribir datos desde el equipo host al contenedor y del contenedor al equipo host.|

> [!IMPORTANT]
> Las opciones [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) y [`Eula`](#eula-setting) se usan en conjunto y debe proporcionar valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](anomaly-detector-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>Opción de configuración ApiKey

La opción de configuración `ApiKey` especifica la clave de recurso de Azure usada para realizar un seguimiento de la información de facturación del contenedor. Debe especificar un valor para la ApiKey y el valor debe ser una clave válida para el _Detector de anomalías_ recurso especificado para el [ `Billing` ](#billing-configuration-setting) opción de configuración.

Este valor se puede encontrar en el siguiente lugar:

* Azure Portal: **Detector de anomalías** administración de recursos, en **claves**

## <a name="applicationinsights-setting"></a>Opción de configuración ApplicationInsights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Opción de configuración Billing

El `Billing` configuración especifica el URI del extremo de la _Detector de anomalías_ recursos en Azure se usan para medir la información de facturación para el contenedor. Debe especificar un valor para esta opción de configuración y el valor debe ser un URI de extremo válido para un _Detector de anomalías_ recursos en Azure.

Este valor se puede encontrar en el siguiente lugar:

* Azure Portal: **Detector de anomalías** información general, con la etiqueta `Endpoint`

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|Sí| `Billing` | string | Identificador URI del punto de conexión de facturación<br><br>Ejemplo:<br>`Billing=https://westus2.api.cognitive.microsoft.com` |

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

Los contenedores del Detector de anomalías no utiliza la entrada o salida monta para almacenar el entrenamiento o servicio de datos. 

La sintaxis exacta de la ubicación de montaje del host varía según el sistema operativo del host. Además, la ubicación de montaje del [equipo host](anomaly-detector-container-howto.md#the-host-computer) puede no ser accesible debido a un conflicto entre los permisos que usa la cuenta de servicio de Docker y los permisos de la ubicación de montaje del host. 

|Opcional| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|-------|------|-----------|-------------|
|No permitida| `Input` | String | Los contenedores del Detector de anomalías no usan.|
|Opcional| `Output` | string | Destino del montaje de salida. El valor predeterminado es `/output`. Esta es la ubicación de los registros. Esto incluye los registros de contenedor. <br><br>Ejemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Comandos de ejemplo de docker run 

Los ejemplos siguientes usan las opciones de configuración para ilustrar cómo escribir y usar comandos `docker run`.  Una vez que se está ejecutando, el contenedor continúa ejecutándose hasta que lo [detenga](anomaly-detector-container-howto.md#stop-the-container).

* **Carácter de continuación de línea**: Los comandos de Docker en las secciones siguientes usan la barra diagonal inversa, `\`, como un carácter de continuación de línea para un shell de bash. Puede quitarla o reemplazarla en función de los requisitos del sistema operativo del host. Por ejemplo, el carácter de continuación de línea para windows es un símbolo de intercalación, `^`. Reemplazar la barra diagonal inversa con el símbolo de intercalación. 
* **Orden de los argumentos**: No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.

Reemplace el valor entre corchetes, `{}`, con sus propios valores:

| Marcador de posición | Valor | Formato o ejemplo |
|-------------|-------|---|
|{BILLING_KEY} | La clave de punto de conexión del recurso Detector de anomalías. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | El valor del punto de conexión de facturación, incluida la región.|`https://westus2.api.cognitive.microsoft.com`|

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](anomaly-detector-container-howto.md#billing).
> El valor de ApiKey es el **clave** en la página de claves de recurso de Detector de anomalías de Azure. 

## <a name="anomaly-detector-container-docker-examples"></a>Ejemplos de Docker de contenedor de Detector de anomalías

Los siguientes ejemplos de Docker son para el contenedor del Detector de anomalías. 

### <a name="basic-example"></a>Ejemplo básico 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Ejemplo de registro con argumentos de la línea de comandos

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```
