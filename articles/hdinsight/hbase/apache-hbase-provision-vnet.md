---
title: Creación de clústeres de HBase en Azure Virtual Network (Azure)
description: Introducción al uso de HBase en HDInsight de Azure Aprenda a crear clústeres de HBase de HDInsight en Azure Virtual Network.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: 60a7afb6e610294ccaa535eaa7371ff8d5015db3
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71077204"
---
# <a name="create-apache-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Creación de clústeres de Apache HBase en HDInsight en Azure Virtual Network
Aprenda a crear clústeres de Apache HBase de Azure HDInsight en [Azure Virtual Network][1].

Con la integración de Virtual Network, los clústeres de Apache HBase se pueden implementar en la misma red virtual que sus aplicaciones para que estas puedan comunicarse directamente con HBase. Entre las ventajas se incluye lo siguiente:

* Conectividad directa entre la aplicación web y los nodos del clúster de HBase, lo cual permite la comunicación mediante las API de llamada a procedimiento remoto (RPC) de Java de HBase.
* Rendimiento mejorado gracias a que el tráfico ya no tiene que examinar varias puertas de enlace y equilibradores de carga.
* La posibilidad de procesar información confidencial de una manera segura sin exponer un extremo público.

### <a name="prerequisites"></a>Requisitos previos
Antes de empezar este artículo, debe tener los siguientes elementos:

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Una estación de trabajo con Azure PowerShell**. Consulte [Install and use Azure PowerShell (Instalación y uso de Azure PowerShell)](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-apache-hbase-cluster-into-virtual-network"></a>Creación de un clúster de Apache HBase en Virtual Network
En esta sección, crea un clúster de Apache HBase basado en Linux con la cuenta de Azure Storage dependiente en Azure Virtual Network mediante una [plantilla de Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy.md). Para conocer otros métodos de creación de clústeres y la descripción de la configuración, consulte [Creación de clústeres de Hadoop basados en Windows en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Para más información acerca de cómo utilizar una plantilla para crear clústeres de Apache Hadoop en HDInsight, consulte [Creación de clústeres de Apache Hadoop en HDInsight con plantillas de Resource Manager](../hdinsight-hadoop-create-linux-clusters-arm-templates.md).

> [!NOTE]  
> Algunas propiedades se han codificado de forma rígida en la plantilla. Por ejemplo:
>
> * **Ubicación**: Este de EE. UU. 2
> * **Versión del clúster**: 3.6
> * **Número de nodos de trabajo en el clúster**: 2
> * **Nombre de la cuenta de almacenamiento predeterminada**: una cadena única
> * **Nombre de la red virtual**: &lt;Nombre del clúster>-vnet
> * **Espacio de direcciones de red virtual**: 10.0.0.0/16
> * **Nombre de subred**: subnet1
> * **Intervalo de direcciones de subred**: 10.0.0.0/24
>
> &lt;Nombre del clúster&gt; se reemplaza con el nombre de clúster que proporcione al utilizar la plantilla.

1. Haga clic en la imagen siguiente para abrir la plantilla en el Portal de Azure. La plantilla se encuentra en las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-provision-vnet/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

2. En la hoja **Implementación personalizada**, escriba las propiedades siguientes:

   * **Suscripción**: seleccione una suscripción de Azure usada para crear el clúster de HDInsight, la cuenta de Storage dependiente y Azure Virtual Network.
   * **Grupos de recursos**: seleccione **Crear nuevo** y especifique un nuevo nombre al grupo de recursos.
   * **Ubicación**: Seleccione una ubicación para el grupo de recursos.
   * **ClusterName**: escriba un nombre para el clúster de Hadoop que se va a crear.
   * **Nombre de inicio de sesión y contraseña del clúster**: El nombre de inicio de sesión predeterminado es **admin**.
   * **Nombre de usuario y contraseña de SSH**: El nombre de usuario predeterminado es **sshuser**.  Puede cambiarlo.
   * **Acepto los términos y condiciones indicados anteriormente**: (Seleccionar)
3. Haga clic en **Comprar**. Tarda aproximadamente 20 minutos en crear un clúster. Una vez creado el clúster, puede hacer clic en la hoja del clúster en el portal para abrirlo.

Cuando termine el artículo, puede eliminar el clúster. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no se esté usando. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando. Para ver instrucciones sobre cómo eliminar un clúster, consulte [Administración de clústeres de Apache Hadoop en HDInsight mediante Azure Portal](../hdinsight-administer-use-portal-linux.md#delete-clusters).

Para comenzar a trabajar con el nuevo clúster de HBase, utilice los procedimientos que encontrará en [Introducción al uso de Apache HBase con Apache Hadoop en HDInsight](./apache-hbase-tutorial-get-started-linux.md).

## <a name="connect-to-the-apache-hbase-cluster-using-apache-hbase-java-rpc-apis"></a>Conexión al clúster de Apache HBase mediante las API de RPC de Java de Apache HBase
1. Cree una máquina virtual de infraestructura como servicio (IaaS) en la misma red virtual de Azure y la misma subred. Para obtener instrucciones sobre cómo crear una nueva máquina virtual de IaaS consulte el tutorial sobre la [creación de una máquina virtual que ejecuta Windows Server](../../virtual-machines/windows/quick-create-portal.md). Al seguir los pasos descritos en este documento, tiene que usar los valores siguientes para la configuración de red:

   * **Red virtual**: &lt;Nombre del clúster>-vnet
   * **Subred**: subnet1

   > [!IMPORTANT]  
   > Reemplace &lt;Nombre del clúster > con el nombre que utilizó al crear el clúster de HDInsight en los pasos anteriores.

   Con estos valores, se colocará la máquina virtual en la misma red virtual y subred que el clúster de HDInsight. Esta configuración les permitirá comunicarse directamente entre sí. No hay una manera de crear un clúster de HDInsight con un nodo perimetral vacío. El nodo perimetral puede usarse para administrar el clúster.  Para obtener más información, consulte [Uso de nodos perimetrales vacíos en HDInsight](../hdinsight-apps-use-edge-node.md).

2. Cuando use una aplicación Java para conectarse a HBase en modo remoto, debe usar el nombre de dominio completo (FQDN). Para determinarlo, debe obtener el sufijo DNS específico de la conexión del clúster de HBase. Para ello, use uno de los siguientes métodos:

   * Utilice un explorador web para realizar una llamada [Apache Ambari](https://ambari.apache.org/):

     Vaya a https://&lt;Nombre del clúster>.azurehdinsight.net/api/v1/clusters/&lt;Nombre del clúster>/hosts?minimal_response=true. Convierte un archivo JSON con los sufijos DNS.
   * Utilice el sitio web de Ambari:

     1. Vaya a https://&lt;Nombre del clúster>.azurehdinsight.net.
     2. Haga clic en **Hosts** en el menú superior.
   * Use Curl para realizar llamadas REST:

     ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
     ```

     En los datos de notación de objetos JavaScript (JSON) devueltos, busque la entrada "host_name". Contiene el nombre de dominio completo (FQDN) de los nodos del clúster. Por ejemplo:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     La parte del nombre de dominio que comienza con el nombre del clúster es el sufijo DNS. Por ejemplo, mycluster.b1.cloudapp.net.
   * Uso de Azure PowerShell

     Use el siguiente script de Azure PowerShell para registrar la función **Get-ClusterDetail** , que se puede usar para devolver el sufijo DNS:

     ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
            .Description
            This command shows the following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run the HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows the FQDN suffix of hosts in the cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows the value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run the HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows the FQDN suffix of hosts in the cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
     ```

     Después de ejecutar el script de Azure PowerShell, use el siguiente comando para devolver el sufijo DNS usando la función **Get-ClusterDetail** . Especifique el nombre del clúster de HBase de HDInsight, el nombre y la contraseña de administrador cuando use este comando.

     ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
     ```

     Este comando devuelve el sufijo DNS. Por ejemplo, **yourclustername.b4.internal.cloudapp.net**.


<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix](./media/apache-hbase-provision-vnet/hdi-primary-dns-suffix.png)
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Para comprobar que la máquina virtual puede comunicarse con el clúster de HBase, use el comando `ping headnode0.<dns suffix>` desde la máquina virtual. Por ejemplo, haga ping a headnode0.mycluster.b1.cloudapp.net

Para usar esta información en una aplicación Java, puede seguir los pasos que se indican en [Uso de Apache Maven para crear aplicaciones Java que utilicen Apache HBase con HDInsight (Hadoop)](./apache-hbase-build-java-maven-linux.md) para crear una aplicación. Para que la aplicación se conecte a un servidor HBase remoto, modifique el archivo **hbase-site.xml** de este ejemplo para que use el FQDN de ZooKeeper. Por ejemplo:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]  
> Para más información sobre la resolución de nombres en redes virtuales de Azure, incluido el uso de su propio servidor DNS, vea [Resolución de nombres (DNS)](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a crear un clúster de Apache HBase. Para obtener más información, consulte:

* [Introducción a HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Uso de nodos perimetrales vacíos en HDInsight](../hdinsight-apps-use-edge-node.md)
* [Configuración de la replicación de Apache HBase en HDInsight](apache-hbase-replication.md)
* [Creación de clústeres de Apache Hadoop en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Introducción al uso de Apache HBase con Apache Hadoop en HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Información general sobre redes virtuales](../../virtual-network/virtual-networks-overview.md)

[1]: https://azure.microsoft.com/services/virtual-network/
[2]: https://technet.microsoft.com/library/ee176961.aspx
[3]: https://technet.microsoft.com/library/hh847889.aspx

