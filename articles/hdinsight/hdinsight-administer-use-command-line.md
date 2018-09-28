---
title: 'Administrar clústeres de Hadoop mediante la CLI clásica de Azure: Azure HDInsight'
description: Obtenga información sobre cómo usar la CLI clásica de Azure para administrar clústeres de Hadoop en Azure HDInsight.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 2586b9219eb145b2033fe2d8fc64b8ae72f34eda
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958295"
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-classic-cli"></a>Administración de clústeres de Hadoop en HDInsight mediante la CLI clásica de Azure
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Obtenga información sobre cómo usar la [CLI clásica de Azure](../cli-install-nodejs.md) para administrar clústeres de Hadoop en Azure HDInsight. La CLI clásica se implementa en Node.js. y se puede usar en cualquier plataforma compatible con Node.js, entre las que se incluyen Windows, Mac y Linux.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar este artículo, debe tener lo siguiente:

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **CLI clásica de Azure**: vea [Install and configure the Azure Classic CLI](../cli-install-nodejs.md) (Instalar y configurar la CLI clásica de Azure) para obtener información de instalación y configuración.
* **Conectarse a Azure**mediante el comando siguiente:

    ```cli
    azure login
    ```
  
    Para obtener más información sobre la autenticación mediante una cuenta profesional o educativa, vea [Conectarse a una suscripción de Azure desde la CLI clásica de Azure](/cli/azure/authenticate-azure-cli).
* **Cambiar al modo de Administrador de recursos de Azure**con el siguiente comando:
  
    ```cli
    azure config mode arm
    ```

Para obtener ayuda, use el modificador **-h** .  Por ejemplo: 

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>Creación de clústeres con la CLI
Vea [Creación de clústeres de HDInsight mediante la CLI de Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Enumeración y visualización de los detalles del clúster
Use los comandos siguientes para enumerar y mostrar los detalles del clúster:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Vista de línea de comandos de la lista de clústeres][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Eliminación de clústeres
Use el comando siguiente para eliminar un clúster:

```cli
azure hdinsight cluster delete <Cluster Name>
```

También puede eliminar un clúster eliminando el grupo de recursos que lo contiene. Tenga en cuenta que esto eliminará todos los recursos del grupo, incluida la cuenta de almacenamiento predeterminada.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Escalado de clústeres
Para cambiar el tamaño del clúster de Hadoop:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>Habilitar/deshabilitar el acceso HTTP para un clúster

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Habilitar/deshabilitar el acceso RDP para un clúster

```cli
azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
azure hdinsight cluster disable-rdp-access [options] <Cluster Name>
```

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a realizar diferentes tareas administrativas en clústeres de HDInsight. Para obtener más información, consulte los artículos siguientes:

* [Administración de HDInsight mediante Azure Portal][hdinsight-admin-portal]
* [Administración de HDInsight con Azure PowerShell][hdinsight-admin-powershell]
* [Introducción a Azure HDInsight][hdinsight-get-started]
* [Cómo usar la CLI clásica de Azure][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Enumeración y visualización de clústeres"
