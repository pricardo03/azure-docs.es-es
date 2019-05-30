---
title: Uso de blob storage para IIS y table storage para eventos en Azure Monitor | Microsoft Docs
description: Monitor de Azure puede leer los registros de servicios de Azure que escriben diagnósticos en table storage o registros ISS en blob storage.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: 901544886e0a0c90c29e83fc71f7a7a25ffc6862
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244880"
---
# <a name="collect-azure-diagnostic-logs-from-azure-storage"></a>Recopilar registros de diagnóstico de Azure desde Azure Storage

Monitor de Azure puede leer los registros de los siguientes servicios que escriben diagnósticos en table storage o registros ISS en blob storage:

* Clústeres de Service Fabric (versión preliminar)
* Virtual Machines
* Roles web y de trabajo

Antes de que Azure Monitor puede recopilar datos en un área de trabajo de Log Analytics para estos recursos, deben estar habilitados diagnósticos de Azure.

Una vez que se habilitan los diagnósticos, puede usar el portal de Azure o PowerShell para configurar el área de trabajo para recopilar los registros.

Diagnósticos de Azure es una extensión de Azure que le permite recopilar datos de diagnóstico de un rol de trabajo, un rol web o una máquina virtual en ejecución en Azure. Los datos se almacenan en una cuenta de almacenamiento de Azure y, a continuación, se pueden recopilar mediante Azure Monitor.

Monitor de Azure recopilar estos registros de diagnósticos de Azure, los registros deben estar en las siguientes ubicaciones:

| Tipo de registro | Tipo de recurso | Ubicación |
| --- | --- | --- |
| Registros IIS |Virtual Machines <br> Roles web <br> Roles de trabajo |wad-iis-logfiles (Blob Storage) |
| syslog |Virtual Machines |LinuxsyslogVer2v0 (Table Storage) |
| Eventos operativos de Service Fabric |Nodos de Service Fabric |WADServiceFabricSystemEventTable |
| Service Fabric Reliable Actor Events |Nodos de Service Fabric |WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Service Events |Nodos de Service Fabric |WADServiceFabricReliableServiceEventTable |
| Registros de eventos de Windows |Nodos de Service Fabric <br> Virtual Machines <br> Roles web <br> Roles de trabajo |WADWindowsEventLogsTable (Table Storage) |
| Registros de ETW de Windows |Nodos de Service Fabric <br> Virtual Machines <br> Roles web <br> Roles de trabajo |WADETWEventTable (Table Storage) |

> [!NOTE]
> Los registros de IIS de Azure Websites no son compatibles actualmente.
>
>

Para máquinas virtuales, también tiene la opción de instalar el [agente de Log Analytics](../../azure-monitor/learn/quick-collect-azurevm.md) en la máquina virtual para permitir información adicional. Además de poder analizar registros de IIS y registros de eventos, también podrá realizar análisis adicionales, como seguimiento de cambios de configuración, evaluación de SQL y evaluación de actualizaciones.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Habilitación de Diagnósticos de Azure en una máquina virtual para la recopilación de registros de IIS y de eventos

Use el siguiente procedimiento para habilitar Diagnósticos de Azure en una máquina virtual para la recopilación de registros de IIS y de eventos mediante Microsoft Azure Portal.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Pasos para habilitar Diagnósticos de Azure en una máquina virtual con Azure Portal

1. Instale al agente de máquina virtual cuando cree una máquina virtual. Si la máquina virtual ya existe, compruebe que el agente de máquina virtual ya está instalado.

   * En Azure Portal, acceda a la máquina virtual, seleccione **Configuración opcional**, luego **Diagnósticos** y establezca **Estado** en **Activado**.

     Tras la finalización, la máquina virtual tendrá la extensión de Diagnósticos de Azure instalada y ejecutándose. Esta extensión se encarga de recopilar datos de diagnóstico.
2. Habilite la supervisión y configure el registro de eventos en una máquina virtual existente. Puede habilitar el diagnóstico en el nivel de máquina virtual. Para activar el diagnóstico y, a continuación, configurar el registro de eventos, realice los siguientes pasos:

   1. Seleccione la máquina virtual.
   2. Haga clic en **Supervisión**.
   3. Haga clic en **Diagnósticos**.
   4. Establezca el **Estado** en **Activado**.
   5. Seleccione los registros de diagnóstico que desee recopilar.
   6. Haga clic en **OK**.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Activación de Diagnósticos de Azure en un rol web para la recopilación de eventos y registros de IIS

Consulte [Cómo habilitar diagnósticos en un servicio en la nube](../../cloud-services/cloud-services-dotnet-diagnostics.md) para conocer los pasos generales para habilitar Diagnósticos de Azure. Las instrucciones siguientes utilizan esta información y la personalizan para utilizarse con Log Analytics.

Con el diagnóstico de Azure habilitado:

* Los registros de IIS se almacenan de forma predeterminada, con los datos transferidos en el intervalo de transferencia de scheduledTransferPeriod.
* Los registros de eventos de Windows no se transfieren de forma predeterminada.

### <a name="to-enable-diagnostics"></a>Para habilitar diagnósticos

Para habilitar los registros de eventos de Windows, o para cambiar scheduledTransferPeriod, configure Microsoft Azure Diagnostics con el archivo de configuración XML (diagnostics.wadcfg) como se muestra en el [Paso 4: Crear el archivo de configuración de Diagnostics e instalar la extensión](../../cloud-services/cloud-services-dotnet-diagnostics.md)

El siguiente archivo de configuración de ejemplo recopila los registros de IIS y todos los eventos desde los registros de aplicación y sistema:

```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Asegúrese de que ConfigurationSettings especifica una cuenta de almacenamiento, como en el ejemplo siguiente:

```xml
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Los valores de **AccountName** y **AccountKey** se encuentran en el panel de cuentas de almacenamiento de Azure Portal, en Administrar claves de acceso. El protocolo de la cadena de conexión debe ser **https**.

Una vez que se aplica la configuración de diagnóstico actualizada al servicio de nube y se escribe el diagnóstico en Azure Storage, está listo para configurar el área de trabajo de Log Analytics.

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Uso de Azure Portal para recopilar registros de Azure Storage

Puede usar el portal de Azure para configurar un área de trabajo de Log Analytics en Azure Monitor para recopilar los registros para los siguientes servicios de Azure:

* Clústeres de Service Fabric
* Virtual Machines
* Roles web y de trabajo

En Azure Portal, vaya hasta el área de trabajo de Log Analytics y realice las siguientes tareas:

1. Haga clic en *Storage accounts logs* (Registros de las cuentas de almacenamiento)
2. Haga clic en la tarea *Agregar*
3. Seleccione la cuenta de almacenamiento que contenga los registros de diagnóstico
   * Esta cuenta puede ser una cuenta de almacenamiento clásico o una cuenta de almacenamiento de Azure Resource Manager
4. Seleccione el tipo de datos de los cuales desea recopilar registros
   * Las opciones serán registros de IIS; eventos; Syslog (Linux); registros de ETW; eventos de Service Fabric
5. El valor de origen se rellenará automáticamente según el tipo de datos y no se puede cambiar
6. Haga clic en Aceptar para guardar la configuración

Repita los pasos 2 a 6 para los tipos de datos que desea recopilar en el área de trabajo y cuentas de almacenamiento adicionales.

En aproximadamente 30 minutos, es posible ver los datos de la cuenta de almacenamiento en el área de trabajo de Log Analytics. Solo verá los datos que se escriban en el almacenamiento de una vez aplicada la configuración. El área de trabajo no lee los datos preexistentes de la cuenta de almacenamiento.

> [!NOTE]
> El portal no valida la existencia del origen en la cuenta de almacenamiento o si se escriben nuevos datos.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Habilitación de Diagnósticos de Azure en una máquina virtual para la recopilación de registros de IIS y de eventos con PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Siga los pasos de [configurar Azure Monitor para indizar diagnósticos de Azure](powershell-workspace-configuration.md#configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage) usar PowerShell para leer desde diagnósticos de Azure que se escriben en table storage.

Con PowerShell de Azure puede especificar con mayor precisión los eventos que se escriben en Azure Storage.
Para obtener más información, vea[Habilitación de Diagnósticos en Azure](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

Puede habilitar y actualizar Diagnósticos de Azure mediante el siguiente script de PowerShell.
También puede utilizar este script con una configuración de registro personalizada.
Modifique el script para establecer la cuenta de almacenamiento, el nombre del servicio y el nombre de máquina virtual.
El script usa cmdlets para máquinas virtuales clásicas.

Revise el siguiente ejemplo de script, cópielo, modifíquelo según sea necesario, guarde el ejemplo como un archivo de script de PowerShell y, a continuación, ejecute el script.

```powershell
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Pasos siguientes

* [Recopilación de registros y métricas de los servicios de Azure](collect-azure-metrics-logs.md) para los servicios compatibles de Azure.
* [Incorporación de soluciones de Log Analytics desde la galería de soluciones](../../azure-monitor/insights/solutions.md) para más información sobre los datos.
* [Búsquedas de registros en Log Analytics](../../azure-monitor/log-query/log-query-overview.md) para analizar los datos.