---
title: Agregar cuentas de almacenamiento de Azure adicionales a HDInsight
description: Aprenda a agregar cuentas de Azure Storage adicionales a un clúster de HDInsight existente.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: hrasheed
ms.openlocfilehash: 6b9577bcf8b527abb0cb7b8720ed83ec8321655b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724468"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>Adición de más cuentas de almacenamiento a HDInsight

Obtenga información sobre cómo usar acciones de script para agregar almacenamiento de Azure adicionales *cuentas* a HDInsight. Los pasos descritos en este documento agregan un almacenamiento *cuenta* a un clúster de HDInsight basado en Linux existente. En este artículo se aplica al almacenamiento *cuentas* (no el clúster cuenta de almacenamiento predeterminada) y el almacenamiento adicional no como [Gen1 de almacenamiento de Azure Data Lake](hdinsight-hadoop-use-data-lake-store.md) y [Azure Data Lake Storage Gen2 ](hdinsight-hadoop-use-data-lake-storage-gen2.md).

> [!IMPORTANT]  
> La información de este documento trata sobre cómo agregar almacenamiento adicional a un clúster después de que se ha creado. Para información sobre cómo agregar cuentas de almacenamiento durante la creación de clústeres, consulte [Configuración de clústeres en HDInsight con Apache Hadoop, Apache Spark, Apache Kafka, etc](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="prerequisites"></a>Requisitos previos

* Un clúster de Hadoop en HDInsight. Consulte [empezar a trabajar con HDInsight en Linux](./hadoop/apache-hadoop-linux-tutorial-get-started.md).
* Nombre de la cuenta de almacenamiento y la clave. Consulte [Administrar configuración de la cuenta de almacenamiento en Azure portal](../storage/common/storage-account-manage.md).
* [Nombre del clúster de mayúsculas y minúsculas correctamente](hdinsight-hadoop-manage-ambari-rest-api.md#identify-correctly-cased-cluster-name).
* Si usa PowerShell, necesitará el módulo de AZ.  Consulte [Introducción a Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
* Si no ha instalado la CLI de Azure, consulte [interfaz de línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).
* Si usa bash o un símbolo del sistema de windows, también necesitará **jq**, un procesador JSON de línea de comandos.  Consulte [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/). Para bash en Ubuntu en Windows 10, consulte [subsistema de Windows para la Guía de instalación de Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="how-it-works"></a>Cómo funciona

Este script toma los parámetros siguientes:

* __Nombre de la cuenta de Azure Storage__: el nombre de la cuenta de almacenamiento que se agregará al clúster de HDInsight. Después de ejecutar el script, HDInsight podrá leer y escribir los datos almacenados en esta cuenta de almacenamiento.

* __Clave de la cuenta de Azure Storage__: una clave que concede acceso a la cuenta de almacenamiento.

* __-p__ (opcional): si se especifica, la clave no se cifra y se almacena en el archivo core-site.xml como texto sin formato.

Durante el procesamiento, el script realiza las siguientes acciones:

* Si la cuenta de almacenamiento ya existe en la configuración de core-site.xml para el clúster, el script se cierra y no se lleva a cabo ninguna otra acción.

* Comprueba que la cuenta de almacenamiento existe y que se puede acceder a ella mediante la clave.

* Cifra la clave con la credencial del clúster.

* Agrega la cuenta de almacenamiento al archivo core-site.xml.

* Detiene y reinicia los servicios [Apache Oozie](https://oozie.apache.org/), [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) y [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html). Detener e iniciar estos servicios permite que usen la nueva cuenta de almacenamiento.

> [!WARNING]  
> No se admite el uso de una cuenta de almacenamiento en una ubicación diferente a la del clúster de HDInsight.

## <a name="the-script"></a>La secuencia de comandos

__Ubicación del script__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Requisitos__:  El script se debe aplicar en los __nodos principales__. No es necesario marcar este script como __Guardado__, porque actualiza directamente la configuración de Ambari para el clúster.

## <a name="to-use-the-script"></a>Para usar el script

Este script puede usarse desde el portal de Azure, CLI de Azure o Azure PowerShell.

### <a name="powershell"></a>PowerShell

Uso de [AzHDInsightScriptAction enviar](https://docs.microsoft.com/powershell/module/az.hdinsight/submit-azhdinsightscriptaction). Reemplace `CLUSTERNAME`, `ACCOUNTNAME`, y `ACCOUNTKEY` con los valores adecuados.

```powershell
# Update these parameters
$clusterName = "CLUSTERNAME"
$parameters = "ACCOUNTNAME ACCOUNTKEY"

$scriptActionName = "addStorage"
$scriptActionUri = "https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh"

# Execute script
Submit-AzHDInsightScriptAction `
    -ClusterName $clusterName `
    -Name $scriptActionName `
    -Uri $scriptActionUri `
    -NodeTypes "headnode" `
    -Parameters $parameters
```

### <a name="azure-cli"></a>Azure CLI

Uso de [ejecutar la acción de script de hdinsight az](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-execute).  Reemplace `CLUSTERNAME`, `RESOURCEGROUP`, `ACCOUNTNAME`, y `ACCOUNTKEY` con los valores adecuados.

```cli
az hdinsight script-action execute ^
    --name CLUSTERNAME ^
    --resource-group RESOURCEGROUP ^
    --roles headnode ^
    --script-action-name addStorage ^
    --script-uri "https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh" ^
    --script-parameters "ACCOUNTNAME ACCOUNTKEY"
```

### <a name="azure-portal"></a>Azure Portal

Consulte [aplicar una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster).

## <a name="known-issues"></a>Problemas conocidos

### <a name="storage-firewall"></a>Firewall de almacenamiento

Si decide proteger la cuenta de almacenamiento con el **Firewalls y redes virtuales** restricciones en **redes seleccionadas**, asegúrese de habilitar la excepción **permitir confianza de Microsoft servicios...**  para que HDInsight puede acceder a su cuenta de almacenamiento.

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Cuentas de almacenamiento que no se muestran en Azure Portal o las herramientas

Cuando vea el clúster de HDInsight en Azure Portal, si selecciona la entrada __Cuentas de almacenamiento__ en __Propiedades__, no se mostrarán las cuentas de almacenamiento agregadas mediante esta acción de script. Azure PowerShell y la CLI de Azure tampoco mostrarán la cuenta de almacenamiento adicional.

Esto se debe a que el script solo modifica la configuración de core-site.xml del clúster. Esta información no se usa al recuperar la información del clúster mediante las API de administración de Azure.

Para ver información de la cuenta de almacenamiento agregada al clúster mediante este script, use la API de REST de Ambari. Use los siguientes comandos para recuperar esta información para su clúster:

### <a name="powershell"></a>PowerShell

Reemplace `CLUSTERNAME` con el nombre del clúster de mayúsculas y minúsculas correctamente. En primer lugar identifique la versión de configuración de servicio usa escribiendo el comando siguiente:

```powershell
# getting service_config_version
$clusterName = "CLUSTERNAME"

$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_service_config_versions/HDFS" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.desired_service_config_versions.HDFS.service_config_version
```

Reemplace `ACCOUNTNAME` con los nombres reales. A continuación, reemplace `4` con los datos reales versión de configuración de servicio y escriba el comando. Cuando se le solicite, escriba la contraseña de inicio de sesión del clúster.

```powershell
# Update values
$accountName = "ACCOUNTNAME"
$version = 4

$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=$version" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$accountName.blob.core.windows.net"
```

### <a name="bash"></a>Bash
Reemplace `myCluster` con el nombre del clúster de mayúsculas y minúsculas correctamente.

```bash
export CLUSTERNAME='myCluster'

curl --silent -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_service_config_versions/HDFS" \
| jq ".Clusters.desired_service_config_versions.HDFS[].service_config_version" 
```

Reemplace `myAccount` con el nombre de cuenta de almacenamiento real. A continuación, reemplace `4` con los datos reales versión de configuración de servicio y escriba el comando:

```bash
export ACCOUNTNAME='"fs.azure.account.key.myAccount.blob.core.windows.net"'
export VERSION='4'

curl --silent -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=$VERSION" \
| jq ".items[].configurations[].properties[$ACCOUNTNAME] | select(. != null)"
```

### <a name="cmd"></a>cmd

Reemplace `CLUSTERNAME` con el nombre del clúster de mayúsculas y minúsculas correctamente en ambas secuencias de comandos. En primer lugar identifique la versión de configuración de servicio usa escribiendo el comando siguiente:

```cmd
curl --silent -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_service_config_versions/HDFS" | ^
jq-win64 ".Clusters.desired_service_config_versions.HDFS[].service_config_version" 
```

Reemplace `ACCOUNTNAME` con el nombre de cuenta de almacenamiento real. A continuación, reemplace `4` con los datos reales versión de configuración de servicio y escriba el comando:

```cmd
curl --silent -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=4" | ^
jq-win64 ".items[].configurations[].properties["""fs.azure.account.key.ACCOUNTNAME.blob.core.windows.net"""] | select(. != null)"
```
---

 La información que devuelve este comando es similar al siguiente texto:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Este texto es un ejemplo de una clave cifrada que se usa para acceder a la cuenta de almacenamiento.

### <a name="unable-to-access-storage-after-changing-key"></a>No se puede acceder a almacenamiento después de cambiar la clave

Si cambia la clave de una cuenta de almacenamiento, HDInsight ya no podrá acceder a dicha cuenta. HDInsight usa una copia en caché de clave del archivo core-site.xml para el clúster. Esta copia en caché debe actualizarse para que coincida con la nueva.

La ejecución nuevamente de la acción de script __no__ actualizará la clave, ya que el script comprueba si ya existe una entrada para la cuenta de almacenamiento. Si ya existe una entrada, no realice ningún cambio.

Para solucionar este problema, debe quitar la entrada existente para la cuenta de almacenamiento. Realice los siguientes pasos para quitar la entrada:

1. Abra un explorador web y abra la interfaz de usuario web de Ambari de su clúster de HDInsight. El URI es `https://CLUSTERNAME.azurehdinsight.net`. Reemplace `CLUSTERNAME` por el nombre del clúster.

    Cuando se le solicite, escriba el usuario de inicio de sesión HTTP y la contraseña para el clúster.

2. En la lista de servicios situada a la izquierda de la página, seleccione __HDFS__. A continuación, seleccione la pestaña __Configs__ (Configuraciones) en el centro de la página.

3. En el campo __Filter...__ (Filtro), escriba un valor de __fs.azure.account__. Esta acción devolverá entradas para las cuentas de almacenamiento adicionales que se hayan agregado al clúster. Hay dos tipos de entradas; __keyprovider__ y __key__. Ambas contendrán el nombre de la cuenta de almacenamiento como parte del nombre de clave.

    Los siguientes son entradas de ejemplo para una cuenta de almacenamiento denominada __mystorage__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Después de haber identificado las claves de la cuenta de almacenamiento que quiere quitar, use el icono rojo '-' a la derecha de la entrada para eliminarlas. Haga clic en el botón __Guardar__ para guardar los cambios.

5. Después de guardar los cambios, use la acción de secuencia de comandos para agregar la cuenta de almacenamiento y el nuevo valor de clave a clúster.

### <a name="poor-performance"></a>Rendimiento deficiente

Si la cuenta de almacenamiento está en una región distinta a la del clúster de HDInsight, puede que experimente un rendimiento deficiente. Al acceder a los datos en una región diferente, el tráfico de red se envía fuera del centro de datos regional de Azure y en la red pública de Internet, lo que puede producir latencia.

### <a name="additional-charges"></a>Cargos adicionales

Si la cuenta de almacenamiento se encuentra en una región distinta a la del clúster de HDInsight, puede que observe cargos de salida adicionales en su facturación de Azure. Se aplica un cargo de salida cuando los datos salen de un centro de datos regional. Este cargo se aplica incluso si el tráfico va destinado a otro centro de datos de Azure en una región distinta.

## <a name="next-steps"></a>Pasos siguientes

En este documento ha aprendido a agregar más cuentas de almacenamiento a un clúster de HDInsight. Para más información sobre las acciones de script, consulte [Personalización de clústeres de HDInsight basados en Linux mediante la acción de script](hdinsight-hadoop-customize-cluster-linux.md).
