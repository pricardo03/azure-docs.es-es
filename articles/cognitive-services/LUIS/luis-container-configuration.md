---
title: Configuración del contenedor de Docker
titleSuffix: Language Understanding - Azure Cognitive Services
description: El entorno en tiempo de ejecución del contenedor de LUIS se configura mediante los argumentos del comando `docker run`. LUIS tiene varias opciones necesarias, junto con algunas opciones de configuración opcionales.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: 98828589832d69ada11205e471314a153a566766
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53080272"
---
# <a name="configure-containers"></a>Configuración de contenedores

El entorno en tiempo de ejecución del contenedor de Language Understanding (LUIS) se configura mediante los argumentos del comando `docker run`. LUIS tiene varias opciones necesarias, junto con algunas opciones de configuración opcionales. Hay disponibles varios [ejemplos](#example-docker-run-commands) del comando. La configuración específica del contenedor son la [configuración de montaje](#mount-settings) de entrada y la configuración de facturación. 

La configuración del contenedor es [jerárquica](#hierarchical-settings) y se puede establecer con [variables de entorno](#environment-variable-settings) o [argumentos de la línea de comandos](#command-line-argument-settings) de Docker.

## <a name="configuration-settings"></a>Valores de configuración

Este contenedor tiene las siguientes opciones de configuración:

|Obligatorio|Configuración|Propósito|
|--|--|--|
|SÍ|[ApiKey](#apikey-setting)|Se usa para realizar un seguimiento de la información de facturación.|
|Sin |[Application Insights](#applicationinsights-setting)|Le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor.|
|SÍ|[Facturación](#billing-setting)|Especifica el identificador URI del punto de conexión del recurso de _Language Understanding_ en Azure.|
|SÍ|[Eula](#eula-setting)| Indica que ha aceptado la licencia del contenedor.|
|Sin |[Fluentd](#fluentd-settings)|Escribe el registro y, opcionalmente, los datos de métricas en un servidor de Fluentd.|
|Sin |[Registro](#logging-settings)|Proporciona compatibilidad con el registro de ASP.NET Core al contenedor. |
|SÍ|[Mounts](#mount-settings)|Leer y escribir datos desde el [equipo host](luis-container-howto.md#the-host-computer) al contenedor y del contenedor al equipo host.|

> [!IMPORTANT]
> Las opciones [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting) y [`Eula`](#eula-setting) se usan en conjunto y debe proporcionar valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>Opción de configuración ApiKey

La opción de configuración `ApiKey` especifica la clave de recurso de Azure usada para realizar un seguimiento de la información de facturación del contenedor. Debe especificar un valor para ApiKey que debe ser una clave válida para el recurso de _Language Understanding_ especificado para la opción de configuración [`Billing`](#billing-setting).

Este valor se puede encontrar en dos lugares:

* Azure Portal: en la Administración de recursos de **Language Understanding**, en **Claves**
* Portal de LUIS: en la página de configuración **Claves y puntos de conexión**. 

No utilice la clave de inicio ni la clave de creación. 

## <a name="applicationinsights-setting"></a>Opción de configuración ApplicationInsights

La opción de configuración `ApplicationInsights` le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor. Application Insights proporciona una supervisión detallada del contenedor. Puede supervisar fácilmente la disponibilidad, el rendimiento y el uso del contenedor. También puede identificar y diagnosticar errores en el contenedor rápidamente.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `ApplicationInsights`.

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|Sin | `InstrumentationKey` | string | Clave de instrumentación de la instancia de Application Insights para la que se envían los datos de telemetría del contenedor. Para más información, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Ejemplo:<br>`InstrumentationKey=123456789`|


## <a name="billing-setting"></a>Opción de configuración Billing

La opción de configuración `Billing` especifica el identificador URI del punto de conexión del recurso de _Language Understanding_ de Azure que se utiliza para medir la información de facturación del contenedor. Debe especificar un valor para esta opción de configuración y el valor debe ser un identificador URI de punto de conexión válido para un recurso de _Language Understanding_ de Azure.

Este valor se puede encontrar en dos lugares:

* Azure Portal: en la Información general de **Language Understanding**, con la etiqueta `Endpoint`
* Portal de LUIS: en la página de configuración **Claves y puntos de conexión**, como parte del identificador URI del punto de conexión.

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|SÍ| `Billing` | string | Identificador URI del punto de conexión de facturación<br><br>Ejemplo:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>Opción de configuración Eula

La opción de configuración `Eula` indica que ha aceptado la licencia del contenedor. Debe especificar un valor para esta opción de configuración y el valor debe establecerse en `accept`.

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|SÍ| `Eula` | string | Aceptación de la licencia<br><br>Ejemplo:<br>`Eula=accept` |

Los contenedores de Cognitive Services tienen una licencia sujeta al [contrato](https://go.microsoft.com/fwlink/?linkid=2018657) que rige el uso de Azure. Si no tiene ningún contrato que rija el uso de Azure, acepta que el contrato que rige el uso de Azure es el [Contrato Microsoft Online Subscription](https://go.microsoft.com/fwlink/?linkid=2018755), que incorpora los [Términos de Online Services](https://go.microsoft.com/fwlink/?linkid=2018760). En el caso de las versiones preliminares, acepta también los [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://go.microsoft.com/fwlink/?linkid=2018815). Al usar el contenedor, acepta estos términos.

## <a name="fluentd-settings"></a>Opción de configuración Fluentd

Fluentd es un recopilador de datos de código abierto para el registro unificado. La opción de configuración `Fluentd` administra la conexión del contenedor a un servidor de [Fluentd](https://www.fluentd.org). El contenedor de LUIS incluye un proveedor de registro de Fluentd que permite que el contenedor escriba los registros y, opcionalmente, los datos de métricas en un servidor de Fluentd.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `Fluentd`.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Host` | string | Dirección IP o nombre de host DNS del servidor de Fluentd. |
| `Port` | Entero | Puerto del servidor de Fluentd.<br/> El valor predeterminado es 24 224. |
| `HeartbeatMs` | Entero | Intervalo de latidos (en milisegundos). Si no se envía ningún tráfico de evento antes de que este intervalo expire, se envía un latido al servidor de Fluentd. El valor predeterminado es 60 000 milisegundos (1 minuto). |
| `SendBufferSize` | Entero | Espacio en búfer de red (en bytes) asignado para las operaciones de envío. El valor predeterminado es 32 768 bytes (32 kilobytes). |
| `TlsConnectionEstablishmentTimeoutMs` | Entero | Tiempo de expiración (en milisegundos) para establecer una conexión SSL/TLS con el servidor de Fluentd. El valor predeterminado es 10 000 milisegundos (10 segundos).<br/> Si `UseTLS` se establece en false, este valor se ignora. |
| `UseTLS` | boolean | Indica si el contenedor debe utilizar SSL/TLS para comunicarse con el servidor de Fluentd. El valor predeterminado es false. |

## <a name="logging-settings"></a>Opción de configuración Logging

La opción de configuración `Logging` administra la compatibilidad con el registro de ASP.NET Core del contenedor. Puede usar los mismos valores y opciones de configuración para el contenedor que los que usa para una aplicación ASP.NET Core. 

Los siguientes proveedores de registro son compatibles con el contenedor de LUIS:

|Proveedor|Propósito|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|Proveedor de registro de `Console` de ASP.NET Core. Se admiten todos los valores predeterminados y las opciones de configuración de ASP.NET Core para este proveedor de registro.|
|[Depuración](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|Proveedor de registro de `Debug` de ASP.NET Core. Se admiten todos los valores predeterminados y las opciones de configuración de ASP.NET Core para este proveedor de registro.|
|[Disco](#disk-logging)|Proveedor de registro JSON. Este proveedor de registro escribe datos de registro para el montaje de salida.|

### <a name="disk-logging"></a>Registro del disco
  
El proveedor de registro `Disk` admite la configuración siguiente:  

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Format` | string | Formato de salida de los archivos de registro.<br/> **Nota:** Este valor debe establecerse en `json` para habilitar el proveedor de registro. Si se especifica este valor sin especificar también un montaje de salida al crear una instancia de un contenedor, se produce un error. |
| `MaxFileSize` | Entero | Tamaño máximo en megabytes (MB) de un archivo de registro. Cuando el tamaño del archivo de registro actual cumple este valor o lo supera, el proveedor de registro inicia un nuevo archivo de registro. Si se especifica -1, el tamaño del archivo de registro solo está limitado por el tamaño máximo de archivo, si existe, para el montaje de salida. El valor predeterminado es 1. |

Para obtener más información acerca de cómo configurar la compatibilidad con el registro de ASP.NET Core, consulte [Configuración del archivo de configuración](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mount-settings"></a>Configuración de montaje

Utilice montajes de enlace para leer y escribir datos hacia y desde el contenedor. Puede especificar un montaje de entrada o un montaje de salida mediante la opción `--mount` del comando [docker run](https://docs.docker.com/engine/reference/commandline/run/). 

El contenedor de LUIS no usa los montajes de entrada o salida para almacenar datos de entrenamiento o del servicio. 

La sintaxis exacta de la ubicación de montaje del host varía según el sistema operativo del host. Además, la ubicación de montaje del [equipo host](luis-container-howto.md#the-host-computer) puede no estar accesible debido a un conflicto entre los permisos que utiliza la cuenta de servicio de Docker y los permisos de la ubicación de montaje del host. 

En la tabla siguiente se describen las configuraciones admitidas.

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|-------|------|-----------|-------------|
|SÍ| `Input` | string | Destino del montaje de entrada. El valor predeterminado es `/input`. Esta es la ubicación de los archivos de paquete de LUIS. <br><br>Ejemplo:<br>`--mount type=bind,src=c:\input,target=/input`|
|Sin | `Output` | string | Destino del montaje de salida. El valor predeterminado es `/output`. Esta es la ubicación de los registros. Incluye los registros de consultas de LUIS y los registros de contenedor. <br><br>Ejemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Configuración jerárquica

La configuración para el contenedor de LUIS es jerárquica y todos los contenedores del [equipo host](luis-container-howto.md#the-host-computer) usan una jerarquía compartida.

Puede usar cualquiera de las siguientes opciones para especificar la configuración:

* [Variables de entorno](#environment-variable-settings)
* [Argumentos de la línea de comandos](#command-line-argument-settings)

Los valores de variable de entorno invalidan los valores de argumento de la línea de comandos, que a su vez invalidan los valores predeterminados para la imagen de contenedor. Si especifica valores diferentes en una variable de entorno y un argumento de la línea de comandos para la misma opción de configuración, el contenedor del que se crea la instancia usa el valor de la variable de entorno.

|Prioridad|Ubicación de la configuración|
|--|--|
|1|Variable de entorno| 
|2|Línea de comandos|
|3|Valor predeterminado de la imagen de contenedor|

### <a name="environment-variable-settings"></a>Configuración de variables de entorno

Las ventajas del uso de variables de entorno son:

* Se pueden configurar varias opciones.
* Varios contenedores pueden usar la misma configuración.

### <a name="command-line-argument-settings"></a>Configuración de argumentos de la línea de comandos

La ventaja de utilizar argumentos de la línea de comandos es que cada contenedor puede usar una configuración diferente.

## <a name="example-docker-run-commands"></a>Comandos de ejemplo de docker run

Los ejemplos siguientes usan las opciones de configuración para ilustrar cómo escribir y usar comandos `docker run`.  Una vez que se está ejecutando, el contenedor continúa ejecutándose hasta que lo [detenga](luis-container-howto.md#stop-the-container).


* **Carácter de continuación de línea**: Los comandos de Docker de las secciones siguientes usan la barra diagonal inversa, `\`, como un carácter de continuación de línea. Puede quitarla o reemplazarla en función de los requisitos del sistema operativo del host. 
* **Orden de los argumentos**: No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.

Reemplace {_argument_name_} por sus propios valores:

| Marcador de posición | Valor | Formato o ejemplo |
|-------------|-------|---|
|{ENDPOINT_KEY} | Clave del punto de conexión de la aplicación de LUIS entrenada. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | El valor del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Language Understanding.|https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](luis-container-howto.md#billing).
> El valor de ApiKey es la **clave** de la página de claves y puntos de conexión del portal de LUIS y también está disponible en la página de claves del recurso de Azure Language Understanding. 

### <a name="basic-example"></a>Ejemplo básico

El ejemplo siguiente tiene los argumentos mínimos posibles para ejecutar el contenedor:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> El comando anterior usa el directorio que está fuera de la unidad `c:` para evitar conflictos con los permisos de Windows. Si necesita usar un directorio específico como directorio de entrada, tal vez tenga que conceder permiso al servicio de Docker. El comando de Docker anterior utiliza la barra diagonal inversa, `\`, como carácter de continuación de línea. Puede quitarla o reemplazarla en función de los requisitos del sistema operativo del [equipo host](luis-container-howto.md#the-host-computer). No cambie el orden de los argumentos a menos que esté muy familiarizado con los contenedores de Docker.


### <a name="applicationinsights-example"></a>Ejemplo de ApplicationInsights

El ejemplo siguiente establece el argumento ApplicationInsights para enviar datos de telemetría a Application Insights mientras se está ejecutando el contenedor:

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example-with-command-line-arguments"></a>Ejemplo de registro con argumentos de la línea de comandos

El siguiente comando establece el nivel de registro, `Logging:Console:LogLevel`, para configurar el nivel de registro en [`Information`](https://msdn.microsoft.com). 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel=Information
```

### <a name="logging-example-with-environment-variable"></a>Ejemplo de registro con variable de entorno

Los siguientes comandos usan una variable de entorno llamada `Logging:Console:LogLevel` para configurar el nivel de registro en [`Information`](https://msdn.microsoft.com). 

```bash
SET Logging:Console:LogLevel=Information
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={APPLICATION_ID} \
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Instalación y ejecución de contenedores](luis-container-howto.md)
* Consulte las [preguntas frecuentes (P+F)](luis-resources-faq.md) para resolver problemas relacionados con la funcionalidad de LUIS.