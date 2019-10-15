---
title: 'Tutorial: Creación de una canalización de ETL integral para obtener información de ventas'
description: Aprenda a crear canalizaciones de ETL con Azure HDInsight para obtener información sobre los datos de ventas mediante clústeres a petición de Spark y Power BI.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 09/30/2019
ms.author: hrasheed
ms.openlocfilehash: b9bcaf4b7497e8beba377eb7e47a44a6eb061299
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72178005"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights"></a>Tutorial: Creación de una canalización de datos integral para obtener información de ventas

En este tutorial compilará una canalización de datos integral que realice operaciones de extracción, transformación y carga de datos (ETL). La canalización usará clústeres de Apache Spark y Apache Hive en ejecución en Azure HDInsight para consultar y manipular los datos. También usará tecnologías como Azure Data Lake Storage Gen2 para el almacenamiento de datos y Power BI para la visualización.

Esta canalización de datos combina los datos de varios almacenes, quita los datos no deseados, y anexa nuevos datos y los carga de nuevo en su almacenamiento para visualizar información empresarial. Consulte [Extracción, transformación y carga de datos (ETL) a escala](./hadoop/apache-hadoop-etl-at-scale.md) para más información sobre las canalizaciones ETL.

![Arquitectura de ETL](./media/hdinsight-sales-insights-etl/architecture.png)

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

Descargue [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331) para visualizar información empresarial al final de este tutorial.

## <a name="create-resources"></a>Crear recursos

### <a name="clone-the-repository-with-scripts-and-data"></a>Clonación del repositorio con scripts y datos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Abra Azure Cloud Shell desde la barra de menús superior. Seleccione la suscripción para crear un recurso compartido de archivos si Azure Cloud Shell se lo solicita.

   ![Apertura de Azure Cloud Shell](./media/hdinsight-sales-insights-etl/hdinsight-sales-insights-etl-click-cloud-shell.png)
1. Seleccione **Bash** en el menú desplegable **Seleccionar entorno**.
1. Inicie sesión en la cuenta de Azure y establezca la suscripción. 
1. Configure el grupo de recursos para el proyecto.
   1. Elija un nombre único para el grupo de recursos.
   1. Ejecute el siguiente fragmento de código en Cloud Shell para establecer las variables que se usarán en pasos posteriores:

       ```azurecli-interactive 
       resourceGroup="<RESOURCE GROUP NAME>"
       subscriptionID="<SUBSCRIPTION ID>"
        
       az account set --subscription $subscriptionID
       az group create --name $resourceGroup --location westus
       ```

1. Descargue los datos y los scripts de este tutorial del [repositorio de ETL de información de ventas de HDInsight](https://github.com/Azure-Samples/hdinsight-sales-insights-etl); para ello, escriba los siguientes comandos en Cloud Shell:

    ```azurecli-interactive 
    git clone https://github.com/Azure-Samples/hdinsight-sales-insights-etl.git
    cd hdinsight-sales-insights-etl
    ```

1. Escriba `ls` en el símbolo del sistema del shell para ver que se han creado los siguientes archivos y directorios:

   ```output
   /salesdata/
   /scripts/
   /templates/
   ```

### <a name="deploy-azure-resources-needed-for-the-pipeline"></a>Implementación de recursos de Azure necesarios para la canalización 

1. Agregue permisos de ejecución para el script `chmod +x scripts/*.sh`.
1. Use el comando `./scripts/resources.sh <RESOURCE_GROUP_NAME> <LOCATION>` para ejecutar el script a fin de implementar los siguientes recursos en Azure:

   1. Una cuenta de Azure Blob Storage. Esta cuenta contendrá los datos de ventas de la empresa.
   2. Una cuenta de Azure Data Lake Storage Gen2. Esta cuenta servirá como cuenta de almacenamiento para ambos clústeres de HDInsight. Consulte [Integración de Azure HDInsight con Data Lake Storage Gen2](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/) para más información sobre HDInsight y Data Lake Storage Gen2.
   3. Una identidad administrada asignada por el usuario. Esta cuenta proporciona a los clústeres de HDInsight acceso a la cuenta de Data Lake Storage Gen2.
   4. Un clúster Apache Spark. Este clúster se usará para limpiar y transformar los datos sin procesar.
   5. Un clúster de Apache Hive Interactive Query. Este clúster permitirá consultar los datos de ventas y visualizarlos con Power BI.
   6. Una red virtual de Azure compatible con las reglas del grupo de seguridad de red (NSG). Esta red virtual permite a los clústeres comunicarse y proteger sus comunicaciones. 

La creación del clúster puede tardar unos 20 minutos.

El script `resources.sh` contiene el comando siguiente, que usa una plantilla de Azure Resource Manager (`resourcestemplate.json`) para crear los recursos especificados con la configuración deseada.

```azurecli-interactive 
az group deployment create --name ResourcesDeployment \
    --resource-group $resourceGroup \
    --template-file resourcestemplate.json \
    --parameters "@resourceparameters.json"
```

El script `resources.sh` también carga los archivos CSV de datos de ventas en la cuenta de almacenamiento de blobs recién creada mediante el comando:

```
az storage blob upload-batch -d rawdata \
    --account-name <BLOB STORAGE NAME> -s ./ --pattern *.csv
```

La contraseña predeterminada para el acceso SSH a los clústeres es `Thisisapassword1`. Si desea cambiar la contraseña, vaya al archivo `resourcesparameters.json` y cámbiela para los parámetros `sparksshPassword`, `sparkClusterLoginPassword`, `llapClusterLoginPassword` y `llapsshPassword`.

### <a name="verify-deployment-and-collect-resource-information"></a>Comprobación de la implementación y recopilación de información de recursos

1. Si desea comprobar el estado de la implementación, vaya al grupo de recursos en Azure Portal. Seleccione **Implementaciones** en **Configuración**. Seleccione el nombre de la implementación, `ResourcesDeployment`. Aquí puede ver los recursos que se han implementado correctamente y los que todavía están en curso.
1. Una vez finalizada la implementación, vaya a Azure Portal > **Grupos de recursos** > <NOMBRE_DEL_GRUPO_DE_RECURSOS>.
1. Busque la nueva cuenta de almacenamiento de Azure que se creó para almacenar los archivos de ventas. El nombre de la cuenta de almacenamiento comienza por `blob` y, a continuación, contiene una cadena aleatoria. Haga lo siguiente:
   1. Anote el nombre de la cuenta de almacenamiento para más adelante.
   1. Seleccione el nombre de la cuenta de almacenamiento de blobs.
   1. En el lado izquierdo del portal, en **Configuración**, seleccione **Claves de acceso**.
   1. Copie la cadena del cuadro **Key1** y guárdela para usarla más adelante.
1. Busque la cuenta de Data Lake Storage Gen2 que se creó como almacenamiento para los clústeres de HDInsight. Esta cuenta se encuentra en el mismo grupo de recursos que la cuenta de almacenamiento de blobs, pero comienza por `adlsgen2`. Haga lo siguiente:
   1. Anote el nombre de la cuenta de Data Lake Storage Gen2.
   1. Seleccione el nombre de la cuenta de Data Lake Storage Gen2.
   1. En el lado izquierdo del portal, en **Configuración**, seleccione **Claves de acceso**.
   1. Copie la cadena del cuadro **Key1** y guárdela para usarla más adelante.

> [!Note]
> Cuando sepa los nombres de las cuentas de almacenamiento, puede obtener las claves de cuenta con el siguiente comando en el símbolo del sistema de Azure Cloud Shell:
> ```azurecli-interactive
> az storage account keys list \
>    --account-name <STORAGE NAME> \
>    --resource-group $rg \
>    --output table
> ```

### <a name="create-a-data-factory"></a>Crear una factoría de datos

Azure Data Factory es una herramienta que ayuda a automatizar las canalizaciones de Azure. No es la única forma de realizar estas tareas, pero es una excelente manera de automatizar los procesos. Consulte la [documentación de Azure Data Factory](https://azure.microsoft.com/en-us/services/data-factory/) para más información sobre Azure Data Factory. 

Esta factoría de datos tendrá una canalización con dos actividades: 

- La primera actividad copiará los datos de Azure Blob Storage en la cuenta de almacenamiento de Data Lake Storage Gen 2 para simular la ingesta de datos.
- La segunda actividad transformará los datos en el clúster de Spark. El script transforma los datos al quitar las columnas no deseadas. También anexa una nueva columna que calcula los ingresos que genera una transacción.

Para configurar la canalización de Azure Data Factory, ejecute el script `adf.sh`:

1. Use `chmod +x adf.sh` para agregar permisos de ejecución al archivo.
1. Use `./adf.sh` para ejecutar el script. 

El script hace lo siguiente:

1. Crea una entidad de servicio con permisos de `Storage Blob Data Contributor` en la cuenta de almacenamiento de Data Lake Storage Gen2.
1. Obtiene un token de autenticación para autorizar solicitudes POST a la [API REST del sistema de archivos de Data Lake Storage Gen2](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create).
1. Rellena el nombre de la cuenta de almacenamiento de Data Lake Storage Gen2 en los archivos `sparktransform.py` y `query.hql`.
1. Obtiene las claves de almacenamiento para las cuentas de Data Lake Storage Gen2 y Blob Storage.
1. Crea otra implementación de recursos para crear una canalización de Azure Data Factory, con sus actividades y servicios vinculados asociados. Pasa las claves de almacenamiento como parámetros al archivo de plantilla para que los servicios vinculados puedan acceder a las cuentas de almacenamiento correctamente.

La canalización de Data Factory se implementa con el siguiente comando:

```azurecli-interactive
az group deployment create --name ADFDeployment \
    --resource-group $resourceGroup \
    --template-file adftemplate.json \
    --parameters "@adfparameters.json"
```

## <a name="run-the-data-pipeline"></a>Ejecución de la canalización de datos

### <a name="trigger-the-data-factory-activities"></a>Desencadenador de las actividades de Data Factory

La primera actividad de la canalización de Data Factory que ha creado mueve los datos de Blob Storage a Data Lake Storage Gen2. La segunda actividad aplica las transformaciones de Spark a los datos y guarda los archivos CSV transformados en una nueva ubicación. La canalización puede tardar unos minutos en completarse.

Para desencadenar las canalizaciones, puede:

- Ejecutar los siguientes comandos para desencadenar las canalizaciones de Data Factory en PowerShell: 

    ```powershell
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "CopyPipeline_k8z" 
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "sparkTransformPipeline"
    ```

- Abra la factoría de datos y seleccione **Author & Monitor** (Creación y supervisión). Desencadene la canalización de copia y después la de Spark desde el portal. Consulte [Creación de clústeres de Apache Hadoop a petición en HDInsight mediante Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md#trigger-a-pipeline) para más información sobre cómo desencadenar canalizaciones mediante el portal.

Para comprobar que se han ejecutado las canalizaciones, puede realizar uno de los pasos siguientes:

- Vaya a la sección **Monitor** (Supervisar) de la factoría de datos mediante el portal.
- En Explorador de Azure Storage, vaya a la cuenta de almacenamiento de Data Lake Storage Gen 2. Vaya al sistema de archivos `files` y a la carpeta `transformed`, y compruebe su contenido para ver si la canalización se ha realizado correctamente.

Consulte [este artículo sobre el uso de Jupyter Notebook](/azure/hdinsight/spark/apache-spark-load-data-run-query) para otras maneras de transformar datos con HDInsight.

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Creación de una tabla en el clúster de Interactive Query para ver datos en Power BI

1. Copie el archivo `query.hql` en el clúster de LLAP mediante SCP:

    ```
    scp scripts/query.hql sshuser@<clustername>-ssh.azurehdinsight.net:/home/sshuser/
    ```

2. Acceda mediante SSH al clúster de LLAP con el siguiente comando y escriba la contraseña. Si no ha modificado el archivo `resourcesparameters.json`, la contraseña es `Thisisapassword1`.

    ```
    ssh sshuser@<clustername>-ssh.azurehdinsight.net
    ```

3. Use el siguiente comando para ejecutar el script:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

Este script creará una tabla administrada en el clúster de Interactive Query a la que puede acceder desde Power BI. 

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Creación de un panel de Power BI a partir de datos de ventas

1. Abra Power BI Desktop.
1. Seleccione **Obtener datos**.
1. Busque un **clúster de HDInsight Interactive Query**.
1. Pegue el URI del clúster ahí. Debe tener este formato: `https://<LLAP CLUSTER NAME>.azurehdinsight.net`.

   Escriba `default` para la base de datos.
1. Escriba el nombre de usuario y la contraseña que usa para acceder al clúster.

Una vez cargados los datos, puede experimentar con el panel que desee crear. Consulte los siguientes vínculos para ver una introducción a los paneles de Power BI:

* [Introducción a los paneles para diseñadores de Power BI](https://docs.microsoft.com/power-bi/service-dashboards)
* [Tutorial: introducción al servicio Power BI](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine todos los recursos asociados mediante el comando siguiente para que no se le facturen.

```azurecli-interactive 
az group delete -n $resourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Extracción, transformación y carga de datos (ETL) a escala](./hadoop/apache-hadoop-etl-at-scale.md)
