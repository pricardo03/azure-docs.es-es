---
title: Error al crear el clúster con InvalidNetworkConfigurationErrorCode en Azure HDInsight
description: Varias razones para no poder crear un clúster con InvalidNetworkConfigurationErrorCode en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/05/2019
ms.openlocfilehash: 9d55041e05101c610a050574f2e940c40dac991a
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816907"
---
# <a name="cluster-creation-fails-with-invalidnetworkconfigurationerrorcode-in-azure-hdinsight"></a>Error al crear el clúster con InvalidNetworkConfigurationErrorCode en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

Si ve el código de error `InvalidNetworkConfigurationErrorCode` con la descripción "Virtual Network configuration is not compatible with HDInsight Requirement" (La configuración de Virtual Network no es compatible con el requisito de HDInsight), suele indicar un problema con la [configuración de la red virtual](../hdinsight-plan-virtual-network-deployment.md) del clúster. Según el resto de la descripción del error, lea las secciones siguientes para resolver el problema.

## <a name="hostname-resolution-failed"></a>"Error de resolución de nombre de host"

### <a name="issue"></a>Problema

La descripción del error contiene "Error de resolución de nombre de host".

### <a name="cause"></a>Causa

Este error indica un problema con la configuración de DNS personalizada. Los servidores DNS de una red virtual pueden reenviar consultas de DNS a resoluciones recursivas de Azure para resolver los nombres de host dentro de esa red virtual (consulte los detalles en [Resolución de nombres de recursos en redes virtuales de Azure](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)). El acceso a las resoluciones recursivas de Azure se proporciona a través de la IP virtual 168.63.129.16. Esta dirección IP solo es accesible desde las máquinas virtuales de Azure. Por lo tanto, no funcionará si usa un servidor DNS local o si el servidor DNS es una máquina virtual de Azure que no forma parte de la red virtual del clúster.

### <a name="resolution"></a>Resolución

1. SSH en la máquina virtual que forma parte del clúster y ejecute el comando `hostname -f`. De esta forma, se devolverá el nombre de dominio completo del host (denominado `<host_fqdn>` en las instrucciones siguientes).

1. A continuación, ejecute el comando `nslookup <host_fqdn>` (por ejemplo, `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net`). Si este comando resuelve el nombre en una dirección IP, significa que el servidor DNS funciona correctamente. En este caso, genere una incidencia de soporte técnico con HDInsight e investigaremos su problema. En la incidencia, incluya los pasos de solución de problemas que ha ejecutado. Nos ayudará a resolver el problema con más rapidez.

1. Si el comando anterior no devuelve una dirección IP, ejecute `nslookup <host_fqdn> 168.63.129.16` (por ejemplo, `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net 168.63.129.16`). Si este comando es capaz de resolver la dirección IP, significa que el servidor DNS no reenvía la consulta al DNS de Azure o bien no es una máquina virtual que forme parte de la misma red virtual que el clúster.

1. Si no tiene una máquina virtual de Azure que pueda actuar como un servidor DNS personalizado en la red virtual del clúster, debe agregar esto primero. Cree una máquina virtual en la red virtual, que se configurará como reenviador de DNS.

1. Una vez que haya implementado una máquina virtual en la red virtual, configure las reglas de reenvío de DNS en esta máquina virtual. Reenvíe todas las solicitudes de resolución de nombres de iDNS a 168.63.129.16 y el resto al servidor DNS. [Este](../hdinsight-plan-virtual-network-deployment.md) es un ejemplo de esta instalación para un servidor DNS personalizado.

1. Agregue la dirección IP de esta máquina virtual como primera entrada DNS para la configuración de DNS de Virtual Network.

---

## <a name="failed-to-connect-to-azure-storage-account"></a>"No se pudo establecer conexión con la cuenta de Azure Storage"

### <a name="issue"></a>Problema

La descripción del error contiene el mensaje "Failed to connect to Azure Storage Account” ("No se pudo conectar a la cuenta de Azure Storage") o “Failed to connect to Azure SQL" (No se pudo conectar a Azure SQL).

### <a name="cause"></a>Causa

Azure Storage y SQL no tienen direcciones IP fijas, por lo que es necesario permitir las conexiones salientes a todas las direcciones IP para permitir el acceso a estos servicios. Los pasos exactos de la resolución dependen de si ha configurado un grupo de seguridad de red (NSG) o reglas definidas por el usuario (UDR). Consulte la sección sobre [el control del tráfico de red con HDInsight con grupos de seguridad de red y rutas definidas por el usuario ](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip) para más información sobre estas configuraciones.

### <a name="resolution"></a>Resolución

* Si el clúster usa un [grupo de seguridad de red (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md).

    Vaya a Azure Portal e identifique el grupo de seguridad de red que está asociado a la subred donde se va a implementar el clúster. En la sección **Reglas de seguridad de salida**, permita el acceso de salida a Internet sin límite (tenga en cuenta que un número menor de **prioridad** aquí es más prioritario). Además, en la sección **Subredes**, confirme si este grupo de seguridad de red se aplica a la subred del clúster.

* Si el clúster usa [rutas definidas por el usuario (UDR)](../../virtual-network/virtual-networks-udr-overview.md).

    Vaya a Azure Portal e identifique la tabla de rutas que está asociada a la subred donde se va a implementar el clúster. Una vez que encuentre la tabla de rutas para la subred, revise la sección de **rutas** que contiene.

    Si hay rutas definidas, asegúrese de que existan para las direcciones IP de la región en la que se implementó el clúster y de que el **NextHopType** de cada ruta sea **Internet**. Debe haber una ruta definida para cada dirección IP requerida documentada en el artículo mencionado anteriormente.

---

### <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener más información, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
