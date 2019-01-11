---
title: 'Habilitar el registro de diagnósticos para las aplicaciones: Azure App Service'
description: Obtenga información acerca de cómo habilitar el registro de diagnóstico y agregar la instrumentación a su aplicación, así como la manera de acceder a la información registrada por Azure.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: d5a94258e8c17d13e15f22f9fa96ef0647105abe
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807880"
---
# <a name="enable-diagnostics-logging-for-apps-in-azure-app-service"></a>Habilitar el registro de diagnósticos para las aplicaciones de Azure App Service
## <a name="overview"></a>Información general
Azure integra diagnósticos para ayudar a depurar [aplicaciones de App Service](https://go.microsoft.com/fwlink/?LinkId=529714). En este artículo se ofrece información acerca de cómo habilitar el registro de diagnóstico, agregar instrumentación a la aplicación y obtener acceso a la información que registra Azure.

En este artículo se usa [Azure Portal](https://portal.azure.com) y la CLI de Azure para trabajar con registros de diagnóstico. Para obtener información acerca de cómo trabajar con registros de diagnóstico mediante Visual Studio, consulte [Solución de problemas de Azure en Visual Studio](troubleshoot-dotnet-visual-studio.md).

## <a name="whatisdiag"></a>Diagnóstico del servidor web y diagnóstico de aplicaciones
App Service ofrece la funcionalidad de diagnóstico para registrar información del servidor web y de la aplicación web. De forma lógica, estos diagnósticos se dividen en **diagnósticos del servidor web** y **diagnóstico de aplicaciones**.

### <a name="web-server-diagnostics"></a>Diagnósticos del servidor web
Puede habilitar o deshabilitar los siguientes tipos de registros:

* **Registro de errores detallado** : registra información detallada de errores para códigos de estado HTTP que indican un problema (código de estado 400 o superior). Puede contener información que puede ayudar a determinar por qué el servidor devolvió el código de error.
* **Seguimiento de solicitudes con error** : registra información detallada acerca de solicitudes con error, incluido un seguimiento de los componentes de IIS usados para procesar la solicitud y el tiempo dedicado a cada componente. Puede resultar útil si trata de aumentar el rendimiento del sitio o de aislar lo que causa la devolución de un error HTTP específico.
* **Registro del servidor web** : registra todas las transacciones HTTP con el [formato de archivo de registro extendido de W3C](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Resulta útil al determinar las métricas totales del sitio, como el número de solicitudes tramitadas o que proceden de una dirección IP específica.

### <a name="application-diagnostics"></a>Diagnósticos de aplicaciones
El diagnóstico de aplicaciones le permite capturar información generada por una aplicación web. Las aplicaciones de ASP.NET pueden usar la clase [System.Diagnostics.Trace](https://msdn.microsoft.com/library/36hhw2t6.aspx) para registrar información en el registro de diagnóstico de la aplicación. Por ejemplo: 

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

En tiempo de ejecución puede recuperar estos registros para ayudar a solucionar problemas. Para obtener más información, vea [Troubleshooting Azure App Service in Visual Studio](troubleshoot-dotnet-visual-studio.md) (Solución de problemas de Azure App Service en Visual Studio).

App Service también registra información de implementación al publicar contenido en una aplicación. Esta acción se lleva a cabo automáticamente, por lo que no es necesario realizar ninguna configuración para el registro de implementaciones. El registro de implementaciones le permite determinar por qué se ha producido un error con la implementación. Por ejemplo, si usa un script de implementación personalizado, puede usar el registro de implementaciones para determinar por qué se ha producido un error con el script.

## <a name="enablediag"></a>Habilitación de diagnósticos
Para habilitar el diagnóstico en [Azure Portal](https://portal.azure.com), vaya a la página de la aplicación y haga clic en **Configuración > Registros de diagnóstico**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Parte de los registros](./media/web-sites-enable-diagnostic-log/logspart.png)

Cuando habilite **Diagnóstico de aplicaciones**, elija también **Nivel**. Esta configuración le permite filtrar la información capturada como **informativa**, **advertencia** o **error**. Si establece esta opción en **Registros detallados**, se registrará toda la información que genere la aplicación.

> [!NOTE]
> Al contrario de lo que ocurre al cambiar el archivo web.config, habilitar Diagnóstico de aplicaciones o cambiar los niveles del registro de diagnóstico no recicla el dominio de la aplicación en el que esta se ejecuta.
>
>

Para la opción **Registro de aplicaciones**, puede activar temporalmente la opción del sistema de archivos con fines de depuración. Esta opción se desactiva automáticamente en 12 horas. También puede activar la opción de Blob Storage para seleccionar un contenedor de blob en el que escribir registros.

> [!NOTE]
> Actualmente, solo los registros de aplicación de .NET pueden escribirse en el almacenamiento de blobs. Los registros de aplicaciones de Java, PHP, Node.js, Python solo se pueden almacenar en el sistema de archivos (sin modificaciones de código para escribir registros en un almacenamiento externo).
>
>

Para la opción **Registro del servidor web**, puede seleccionar **Almacenamiento** o **Sistema de archivos**. Si selecciona **almacenamiento**, tiene la opción de seleccionar una cuenta de almacenamiento y, después, un contenedor de blobs en el que se escribirán los registros. 

Si almacena registros en el sistema de archivos, es posible obtener acceso a estos archivos por FTP, o bien se pueden descargar como un archivo ZIP mediante la CLI de Azure.

De forma predeterminada, los registros no se eliminan automáticamente (con la excepción del **Registro de aplicaciones [sistema de archivos]**). Para eliminar automáticamente los registros, establezca el campo **Período de retención (días)**.

> [!NOTE]
> Si se [regeneran las claves de acceso de su cuenta de almacenamiento](../storage/common/storage-create-storage-account.md), deberá restablecer la configuración de registro correspondiente para usar las claves actualizadas. Para ello, siga estos pasos:
>
> 1. En la pestaña **Configurar**, establezca la característica de registro correspondiente de **Desactivar**. Guarde la configuración.
> 2. Vuelva a habilitar el registro en el blob de la cuenta de almacenamiento. Guarde la configuración.
>
>

Se puede habilitar cualquier combinación de sistema de archivos o almacenamiento de blobs al mismo tiempo, y estas opciones tienen configuraciones de nivel de registro individuales. Por ejemplo, puede registrar errores y advertencias en el almacenamiento de blobs como una solución de registro a largo plazo, mientras habilita el registro en el sistema de archivos con un nivel detallado.

Si bien las ubicaciones de almacenamiento ofrecen la misma información básica de los eventos registrados, **blob storage** registra información adicional, como el id. de instancia, el id. del subproceso y una marca de tiempo más pormenorizada (formato de marca de graduación) que el registro en **file system**.

> [!NOTE]
> Solo se puede obtener acceso a la información almacenada en **blob storage** mediante una aplicación o un cliente de almacenamiento que pueda trabajar directamente con estos sistemas de almacenamiento. Por ejemplo, Visual Studio 2013 contiene un Explorador de Storage que se puede usar para explorar Blob Storage y HDInsight puede obtener acceso a los datos almacenados en Blob Storage. También puede escribir una aplicación que obtiene acceso a Azure Storage mediante algunos de los [SDK de Azure](https://azure.microsoft.com/downloads/).
>

## <a name="download"></a> Instrucciones: Descargar registros
Se puede acceder a la información de diagnóstico almacenada en el sistema de archivos de la aplicación directamente mediante FTP. También se puede descargar como un archivo ZIP mediante la CLI de Azure.

La estructura de directorios en que se almacenan los registros es la siguiente:

* **Registros de aplicaciones** : /LogFiles/Application/. Esta carpeta contiene uno o varios archivos de texto con información generada por el registro de aplicaciones.
* **Seguimientos de solicitudes con error** : /LogFiles/W3SVC#########/. Esta carpeta contiene un archivo XSL y uno o varios archivos XML. Asegúrese de descargar el archivo XSL en el mismo directorio de los archivos XML, porque el archivo XSL proporciona funcionalidad para dar formato y filtrar los contenidos de los archivos XML cuando se visualizan en Internet Explorer.
* **Registros detallados de errores** : /LogFiles/DetailedErrors/. Esta carpeta contiene uno o varios archivos .htm con información amplia de todos los errores HTTP que se han producido.
* **Registros de servidor web** : /LogFiles/http/RawLogs. Esta carpeta contiene uno o varios archivos de texto a los que se le aplica el [formato de archivo de registro extendido W3C](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Registros de implementaciones** : /LogFiles/Git. Esta carpeta contiene registros generados por los procesos de implementación internos usados por Azure App Service, además de los registros de las implementaciones Git. También puede encontrar registros de implementación en D:\home\site\deployments.

### <a name="ftp"></a>FTP

Para abrir una conexión FTP al servidor FTP de la aplicación, consulte [Implementación de la aplicación en Azure App Service mediante FTP/S](deploy-ftp.md).

Una vez conectado al servidor FTP/S de la aplicación, abra la carpeta **LogFiles**, donde se almacenan los archivos de registro.

### <a name="download-with-azure-cli"></a>Descarga con la CLI de Azure
Para descargar los archivos de registro mediante la interfaz de la línea de comandos de Azure, abra una sesión nueva del símbolo del sistema, PowerShell, Bash o Terminal y escriba el siguiente comando:

    az webapp log download --resource-group resourcegroupname --name appname

Este comando guarda los registros de la aplicación denominada "appname" en un archivo denominado **diagnostics.zip** en el directorio actual.

> [!NOTE]
> Si no tiene instalada la CLI de Azure o si no la ha configurado para que use la suscripción a Azure, consulte [Cómo usar la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Control de Visualización de registros en Application Insights
Visual Studio Application Insights proporciona herramientas para filtrar y buscar registros y para correlacionar los registros con solicitudes y otros eventos.

1. Incorporación del SDK de Application Insights al proyecto de Visual Studio
   * En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y elija Agregar Application Insights. La interfaz le guiará a través de pasos que incluyen la creación de un recurso de Application Insights. [Más información](../azure-monitor/app/asp-net.md)
2. Agregue el paquete del agente de escucha.
   * Haga clic con el botón derecho en el proyecto y elija Administrar paquetes NuGet. Seleccione `Microsoft.ApplicationInsights.TraceListener` [Más información](../azure-monitor/app/asp-net-trace-logs.md)
3. Cargue el proyecto y ejecútelo para generar datos de registro.
4. En [Azure Portal](https://portal.azure.com/), busque el nuevo recurso de Application Insights y abra **Buscar**. Verá los datos de registro, junto con la solicitud, el uso y otros datos de telemetría. Es posible que algunos datos de telemetría demoren unos minutos en aparecer: haga clic en Actualizar. [Más información](../azure-monitor/app/diagnostic-search.md)

[Obtenga más información acerca del seguimiento del rendimiento con Application Insights](../azure-monitor/app/azure-web-apps.md)

## <a name="streamlogs"></a> Instrucciones: Transmisión de registros
Al implementar una aplicación, suele resultar útil ver la información de registro casi en tiempo real. Puede transmitir información de registro al entorno de desarrollo mediante la CLI de Azure.

> [!NOTE]
> Algunos tipos de búfer de registros se escriben en el archivo de registro, lo que puede ocasionar la transmisión de eventos desordenados. Por ejemplo, una entrada de registro de aplicaciones que se genera cuando un usuario visita una página se puede visualizar en la transmisión antes de la entrada de registro HTTP correspondiente para la solicitud de la página.
>
> [!NOTE]
> La transmisión de registros también transmite información escrita en cualquier archivo de texto almacenado en la carpeta **D:\\home\\LogFiles\\**.
>
>

### <a name="streaming-with-azure-cli"></a>Streaming con la CLI de Azure
Para transmitir información de registro, abra una nueva sesión del símbolo del sistema, PowerShell, Bash o Terminal y escriba el siguiente comando:

    az webapp log tail --name appname --resource-group myResourceGroup

Este comando establece conexión con la aplicación denominada "appname" y comenzará a transmitir información a la ventana a medida que se produzcan los eventos de registro en la aplicación. Toda la información escrita en archivos terminados en .txt, .log o .htm almacenados en el directorio /LogFiles (d:/home/logfiles) se transmite a la consola local.

Para filtrar eventos específicos, como errores, use el parámetro **--Filter** . Por ejemplo: 

    az webapp log tail --name appname --resource-group myResourceGroup --filter Error

Para filtrar tipos de registros específicos, como HTTP, use el parámetro **--Path** . Por ejemplo: 

    az webapp log tail --name appname --resource-group myResourceGroup --path http

> [!NOTE]
> Si no tiene instalada la CLI de Azure o si no la ha configurado para que use la suscripción a Azure, consulte [Cómo usar la CLI de Azure](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Instrucciones: Información sobre los registros de diagnóstico
### <a name="application-diagnostics-logs"></a>Registros de diagnóstico de aplicaciones
El diagnóstico de aplicaciones almacena información con un formato específico para aplicaciones .NET, en función de si almacena los registros en el sistema de archivos o en Blob Storage. 

El conjunto base de datos almacenados es el mismo en ambos tipos de almacenamiento: la fecha y la hora en que se ha producido el evento, el id. del proceso que ha producido el evento, el tipo de evento (información, advertencia o error) y el mensaje del evento. El uso del sistema de archivos para el almacenamiento de registros es útil cuando necesita acceso inmediato para solucionar un problema porque los archivos de registro se actualizan casi al instante. Blob Storage se usa con fines de archivado porque los archivos se almacenan en caché y, a continuación, se vacían en el contenedor de almacenamiento según una programación.

**Sistema de archivos**

Cada línea registrada en el sistema de archivos o recibida mediante transmisión presentará el siguiente formato:

    {Date}  PID[{process ID}] {event type/level} {message}

Por ejemplo, un evento de error presentaría un formato similar al siguiente:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

El registro en el sistema de archivos ofrece la información más básica de los tres métodos disponibles, ya que ofrece solo la hora, el identificador del proceso, el nivel de evento y el mensaje.

**Blob Storage**

Al realizar registros en el almacenamiento de blobs, los datos se almacenan con un formato de valores separados por comas (CSV). Se registran campos adicionales para ofrecer información más pormenorizada acerca del evento. Las siguientes propiedades se usan para cada fila con el formato CSV:

| Nombre de propiedad | Valor/formato |
| --- | --- |
| Date |La fecha y la hora en que se ha producido el evento |
| Nivel |Nivel del evento (por ejemplo, error, advertencia, información). |
| ApplicationName |El nombre de la aplicación |
| InstanceId |Instancia de la aplicación en la que se ha producido el evento |
| EventTickCount |La fecha y hora en que se ha producido el evento, con formato de marca de graduación (mayor precisión) |
| EventId |El identificador de este evento<p><p>El valor predeterminado es 0 si no se ha especificado ninguno |
| Pid |Identificador del proceso |
| Tid |El identificador del subproceso que ha generado el evento |
| Message |Mensaje detallado del evento |

Los datos almacenados en un blob serían similares a los siguientes:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> En el caso de ASP.NET Core, el registro se realiza mediante el proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices). Este proveedor deposita archivos de registro adicionales en el contenedor de blobs. Para más información, consulte el artículo sobre el [registro de ASP.NET Core en Azure](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#logging-in-azure).
>
>

### <a name="failed-request-traces"></a>Seguimientos de solicitudes con error
El seguimiento de solicitudes con error se almacena en archivos XML con nombre **fr######.xml**. Para facilitar la visualización de la información registrada, se proporciona una hoja de estilo XSL con nombre **freb.xsl** en el mismo directorio que los archivos XML. Si abre uno de los archivos XML en Internet Explorer, se usará la hoja de estilo XSL para ofrecer una visualización con formato de la información de seguimiento, similar a la siguiente:

![solicitud con error visualizada en el explorador](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Registros detallados de errores
Los registros de error detallados son documentos HTML que ofrecen información más detallada sobre los errores HTTP que se han producido. Habida cuenta de que se trata sencillamente de documentos HTML, se pueden ver en un explorador web.

### <a name="web-server-logs"></a>Registros de servidor web
A los registros del servidor web se les aplica el [formato de archivo de registro extendido W3C](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Esta información se puede leer con un editor de texto o analizarse con utilidades como el [analizador del registro](https://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Los registros generados por Azure App Service no admiten los campos **s-computername**, **s-ip** o **cs-version**.
>
>

## <a name="nextsteps"></a> Pasos siguientes
* [Control de Supervisión de aplicaciones en Azure App Service](web-sites-monitor.md)
* [Troubleshooting Azure App Service in Visual Studio](troubleshoot-dotnet-visual-studio.md) (Solución de problemas de Azure App Service en Visual Studio)
* [Analyze app Logs in HDInsight](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413) (Análisis de registros de aplicación en HDInsight)
