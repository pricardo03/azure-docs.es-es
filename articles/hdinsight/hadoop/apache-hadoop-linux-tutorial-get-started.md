---
title: 'Inicio rápido: Creación de clústeres de Apache Hadoop con Resource Manager: Azure HDInsight'
description: Aprenda a crear clústeres de HDInsight.
keywords: introducción a hadoop, hadoop linux, inicio rápido de hadoop, introducción a hive, inicio rápido en hive
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 05/14/2019
ms.openlocfilehash: 9084e8e0fe7707b5f115df80792cea102772b8e9
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861648"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-resource-manager-template"></a>Inicio rápido: Creación de clústeres de Apache Hadoop en Azure HDInsight con plantillas de Resource Manager

En este inicio rápido, aprenderá a crear un clúster de Apache Hadoop en Azure HDInsight con una plantilla de Resource Manager.

Puede ver plantillas similares en [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular). Puede encontrar la referencia de plantilla [aquí](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/allversions).  También puede crear un clúster mediante [Azure Portal](apache-hadoop-linux-create-cluster-get-started-portal.md).  

Actualmente HDInsight tiene [siete tipos diferentes de clúster](./apache-hadoop-introduction.md#cluster-types-in-hdinsight). Cada uno de estos tipos de clúster es compatible con un conjunto de componentes diferente. Todos los tipos de clúster son compatibles con Hive. Para ver una lista de los componentes compatibles con HDInsight, consulte [Novedades en las versiones de clústeres de Hadoop proporcionadas por HDInsight](../hdinsight-component-versioning.md)  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

<a name="create-cluster"></a>
## <a name="create-a-hadoop-cluster"></a>Creación de un clúster de Hadoop

1. Seleccione el botón **Implementar en Azure** siguiente para iniciar sesión en Azure y abrir la plantilla de Resource Manager en Azure Portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Escriba o seleccione los siguientes valores:

    |Propiedad  |Descripción  |
    |---------|---------|
    |**Suscripción**     |  Seleccione su suscripción a Azure. |
    |**Grupos de recursos**     | Cree un grupo de recursos o seleccione uno existente.  Un grupo de recursos es un contenedor de componentes de Azure.  En este caso, el grupo de recursos contiene el clúster de HDInsight y la cuenta de Azure Storage dependiente. |
    |**Ubicación**     | Seleccione una ubicación de Azure en la que quiera crear el clúster.  Elija una ubicación más cercana a usted para mejorar el rendimiento. |
    |**Nombre del clúster**     | Escriba el nombre del clúster de Hadoop. Dado que todos los clústeres de HDInsight comparten el mismo espacio de nombres de DNS, este nombre debe ser único. El nombre solo puede incluir letras minúsculas, números, guiones y debe empezar por una letra.  Antes y después de cada guion debe ir un carácter que no sea otro guión.  El nombre debe tener entre 3 y 59 caracteres. |
    |**Tipo de clúster**     | Seleccione **hadoop**. |
    |**Nombre de inicio de sesión y contraseña del clúster**     | El nombre de inicio de sesión predeterminado es **admin**. La contraseña debe tener un mínimo de 10 caracteres y contener al menos un dígito, una letra mayúscula y una letra minúscula, y un carácter no alfanumérico (excepto los caracteres ' " y `\). Asegúrese de **no proporcionar** contraseñas comunes, como "Pass@word1".|
    |**Nombre de usuario y contraseña de SSH**     | El nombre de usuario predeterminado es **sshuser**.  El nombre de usuario de SSH se puede cambiar.  La contraseña de usuario de SSH tiene los mismos requisitos que la contraseña de inicio de sesión del clúster.|

    Algunas propiedades se han codificado de forma rígida en la plantilla.  Puede configurar estos valores desde la plantilla. Para más información acerca de estas propiedades, consulte este artículo sobre la [creación de clústeres de Apache Hadoop en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

    > [!NOTE]  
    > Los valores que proporcione deben ser exclusivos y deben seguir las directrices de nomenclatura. La plantilla no realiza comprobaciones de validación. Si los valores proporcionados ya están en uso o no sigue las directrices, obtendrá un error después de haber enviado la plantilla.  

    ![Introducción de HDInsight Linux a la plantilla de Resource Manager en el portal](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Implementación de un clúster de Hadoop en HDInsight mediante Azure Portal y una plantilla de Resource Manager")

3. Seleccione **Acepto los términos y condiciones indicadas anteriormente** y, después, seleccione **Comprar**. Recibirá una notificación de que la implementación está en curso.  Se tarda aproximadamente 20 minutos en crear un clúster.

4. Una vez que se cree el clúster, recibirá una notificación de **Implementación correcta** con un vínculo **Ir al grupo de recursos**.  La página **Grupo de recursos** mostrará el nuevo clúster de HDInsight y el almacenamiento predeterminado asociado con el clúster. Cada clúster depende de una [cuenta de Azure Storage](../hdinsight-hadoop-use-blob-storage.md) o de una [cuenta de Azure Data Lake Storage](../hdinsight-hadoop-use-data-lake-store.md). Se conoce como cuenta de almacenamiento predeterminada. El clúster de HDInsight y su cuenta de almacenamiento predeterminada deben estar en la misma región de Azure. Al eliminar los clústeres no se elimina la cuenta de almacenamiento.

> [!NOTE]  
> Para conocer otros métodos de creación de clústeres y las propiedades que se usan en este inicio rápido, vea [Creación de clústeres en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="troubleshoot"></a>Solución de problemas

Si experimenta problemas con la creación de clústeres de HDInsight, consulte los [requisitos de control de acceso](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="clean-up-resources"></a>Limpieza de recursos
Después de completar el inicio rápido, puede ser conveniente eliminar el clúster. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no se esté usando. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando.

> [!NOTE]  
> Si avanza de *inmediato* al siguiente tutorial para aprender a ejecutar operaciones de ETL con Hadoop en HDInsight, es posible que quiera mantener el clúster en ejecución. Esto es porque en el tutorial tendrá que crear un clúster de Hadoop de nuevo. Sin embargo, si no va a continuar con el próximo tutorial de inmediato, debe eliminar el clúster ahora.

**Para eliminar el clúster o la cuenta de almacenamiento predeterminada**

1. Vuelva a la pestaña de explorador en la que tenga Azure Portal. Estará en la página de información general del clúster. Si solo quiere eliminar el clúster, pero desea seguir conservando la cuenta de almacenamiento predeterminada, seleccione **Eliminar**.

    ![Eliminación de un clúster de HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "Delete HDInsight cluster")

2. Si quiere eliminar el clúster y la cuenta de almacenamiento predeterminada, seleccione el nombre del grupo de recursos (resaltado en la captura de pantalla anterior) para abrir la página del grupo de recursos.

3. Seleccione **Eliminar grupo de recursos** para eliminar el grupo de recursos, que contiene el clúster y la cuenta de almacenamiento predeterminada. Tenga en cuenta que, al eliminar el grupo de recursos, se elimina también la cuenta de almacenamiento. Si desea mantener la cuenta de almacenamiento, elija eliminar solo el clúster.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a crear un clúster de Apache Hadoop en HDInsight con una plantilla de Resource Manager. En el siguiente artículo, aprenderá a realizar una operación ETL (extraer, transformar y cargar) con Hadoop en HDInsight.

> [!div class="nextstepaction"]
>[Extracción, transformación y carga de datos mediante Apache Hive en HDInsight](../hdinsight-analyze-flight-delay-data-linux.md)

Si está preparado para empezar a trabajar con sus propios datos y necesita más información acerca de la forma en que HDInsight almacena los datos o de cómo obtener datos en HDInsight, consulte los siguientes artículos:

* Para más información sobre cómo HDInsight usa Azure Storage, consulte [Uso de Azure Storage con HDInsight](../hdinsight-hadoop-use-blob-storage.md).
* Para obtener información sobre cómo crear un clúster de HDInsight con Data Lake Storage, consulte [Quickstart: Set up clusters in HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) (Inicio rápido: Configuración de clústeres en HDInsight).
* Para más información sobre cómo cargar datos en HDInsight, consulte [Carga de datos en HDInsight](../hdinsight-upload-data.md).
* [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md)

Para más información sobre el análisis de datos con HDInsight, consulte los siguientes artículos:

* Para más información sobre el uso de Hive con HDInsight, como la ejecución de consultas de Hive desde Visual Studio, consulte este artículo sobre el [uso de Apache Hive con HDInsight](hdinsight-use-hive.md).
* Para más información sobre Pig, un lenguaje que se utiliza para transformar datos, consulte este artículo sobre el [uso de Apache Pig con HDInsight](hdinsight-use-pig.md).
* Para más información sobre MapReduce, una manera de escribir programas que procesan datos en Hadoop, consulte [Uso de MapReduce con HDInsight](hdinsight-use-mapreduce.md).
* Para más información sobre el uso de las herramientas de HDInsight para Visual Studio para analizar datos en HDInsight, consulte [Introducción al uso de herramientas de Hadoop en Visual Studio para HDInsight para ejecutar una consulta de Hive](apache-hadoop-visual-studio-tools-get-started.md).
* Para más información sobre el uso de las herramientas de HDInsight para VSCode para analizar datos en HDInsight, consulte [Uso de las herramientas de Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).


Si desea más información acerca de cómo crear o administrar un clúster de HDInsight, consulte los siguientes artículos:

* Para más información sobre cómo administrar el clúster de HDInsight basado en Linux, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](../hdinsight-hadoop-manage-ambari.md).
* Para más información acerca de las opciones que se pueden seleccionar al crear un clúster de HDInsight, consulte [Creación de clústeres de Hadoop basados en Linux en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

Para más información sobre la creación de un clúster de HDInsight con las plantillas de Azure Resource Manager, consulte:

* [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular).
* [Referencia sobre las plantillas de Azure](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/allversions).

[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md