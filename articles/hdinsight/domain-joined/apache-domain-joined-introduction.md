---
title: Introducción a la seguridad de Hadoop con clústeres de Azure HDInsight unidos a un dominio
description: Obtenga información sobre cómo los clústeres de Azure HDInsight unidos a un dominio son compatibles con los cuatro pilares de la seguridad empresarial.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.openlocfilehash: c13fd979562cc89831d031c5050fe9dbb184267b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041139"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Introducción a la seguridad de Hadoop con clústeres de HDInsight unidos a dominio

En el pasado, Azure HDInsight solo era compatible con un único usuario: el administrador local. Esto funcionaba muy bien para departamentos o equipos de aplicaciones menores. A medida que la popularidad de las cargas de trabajo basadas en Hadoop aumentaba en el sector empresarial, fue creciendo la necesidad de funcionalidades de tipo empresarial, tales como la autenticación basada en Active Directory, la compatibilidad con múltiples usuarios y el control de acceso basado en rol. 

Puede crear un clúster de HDInsight que esté unido a un dominio de Active Directory. Luego puede configurar una lista de empleados de la empresa que pueden autenticarse a través de Azure Active Directory para iniciar sesión en el clúster de HDInsight. Ninguna persona fuera de la empresa puede iniciar sesión ni acceder al clúster de HDInsight. 

El administrador de la empresa puede configurar el control de acceso basado en rol (RBAC) para la seguridad de Hive mediante el uso de [Apache Ranger](http://hortonworks.com/apache/ranger/). Configurar el RBAC restringe el acceso solo a los datos necesarios. Por último, el administrador puede auditar el acceso a los datos de los empleados y todos los cambios que se realizan en las directivas de control de acceso. El administrador puede lograr un alto grado de gobierno de sus recursos corporativos.

> [!NOTE]
> Las nuevas características que se describen en este artículo solo están disponibles en versión preliminar en los siguientes tipos de clúster: Hadoop, Spark e Interactive Query. Oozie ya está habilitado en clústeres unidos a un dominio. Para tener acceso a la interfaz de usuario web de Oozie, los usuarios deben habilitar la [tunelización](../hdinsight-linux-ambari-ssh-tunnel.md).

La seguridad empresarial incluye cuatro pilares importantes: seguridad del perímetro, autenticación, autorización y cifrado.

![Ventajas de los clústeres de HDInsight unidos a un dominio en los cuatro pilares de la seguridad empresarial](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

## <a name="perimeter-security"></a>Seguridad del perímetro
La seguridad del perímetro en HDInsight se logra a través de redes virtuales y el servicio de Azure VPN Gateway. Un administrador de empresa puede crear un clúster de HDInsight dentro de una red virtual y usar los grupos de seguridad de red (reglas de firewall) para restringir el acceso a la red virtual. Solo las direcciones IP definidas en las reglas de firewall de entrada podrán comunicarse con el clúster de HDInsight. Esta configuración proporciona la seguridad del perímetro.

Otro nivel de seguridad del perímetro se logra mediante el servicio de VPN Gateway. La puerta de enlace actúa como primera línea de defensa para cualquier solicitud entrante al clúster de HDInsight. Acepta la solicitud, la valida y solo entonces permite que la solicitud pase a los otros nodos del clúster. De este modo, la puerta de enlace proporciona la seguridad del perímetro a otros nodos de nombre y datos en el clúster.

## <a name="authentication"></a>Autenticación
Un administrador de empresa puede crear un clúster de HDInsight unido a dominio en una [red virtual](https://azure.microsoft.com/services/virtual-network/). Todos los nodos del clúster de HDInsight se unen al dominio administrado por la empresa. Esto se logra mediante el uso de [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). 

Con esta configuración, los empleados de la empresa pueden iniciar sesión en los nodos del clúster con sus credenciales de dominio. También pueden usar sus credenciales de dominio para autenticarse con otros puntos de conexión aprobados tales como Ambari Views, ODBC, JDBC, PowerShell y las API REST para interactuar con el clúster. El administrador tiene control total sobre la limitación del número de usuarios que interactúan con el clúster mediante estos puntos de conexión.

## <a name="authorization"></a>Autorización
Un procedimiento recomendado que la mayoría de las empresas sigue es que no todos los empleados tengan acceso a todos los recursos de la empresa. Asimismo, el administrador puede definir directivas de control de acceso basado en rol para los recursos del clúster. 

Por ejemplo, puede configurar el administrador [Apache Ranger](http://hortonworks.com/apache/ranger/) para establecer las directivas de control de acceso para Hive. Esta funcionalidad garantiza que los empleados solo pueden acceder a los datos que necesitan para realizar correctamente sus trabajos. El acceso SSH al clúster también está restringido solo al administrador.

## <a name="auditing"></a>Auditoría
Auditar todos los acceso a los recursos de clúster, y los datos, es necesario para hacer seguimiento del acceso no autorizado o accidental de los recursos. Es tan importante como proteger los recursos de clúster de HDInsight contra usuarios no autorizados y proteger los datos. 

El administrador puede ver y notificar todos los accesos a los recursos y los datos del clúster de HDInsight. El administrador también puede ver y notificar todos los cambios en las directivas de control de acceso creados en los puntos de conexión de Apache Ranger compatibles. 

Un clúster de HDInsight unido a dominio utiliza la familiar interfaz de usuario de Apache Ranger para buscar registros de auditoría. En el back-end, Ranger usa [Apache Solr](http://hortonworks.com/apache/solr/) para almacenar y buscar en los registros.

## <a name="encryption"></a>Cifrado
Proteger los datos es importante para cumplir con los requisitos de cumplimiento y de seguridad de la organización. Además de restringir el acceso de empleados no autorizados a los datos, es necesario cifrarlos. 

Ambos almacenes de datos para clústeres de HDInsight (Azure Blob Storage y Azure Data Lake Storage Gen1) admiten el [cifrado de datos](../../storage/common/storage-service-encryption.md) en reposo en el lado servidor transparente. Los clústeres de HDInsight seguros funcionarán perfectamente con esta funcionalidad de cifrado de los datos en reposo en el lado servidor.

## <a name="next-steps"></a>Pasos siguientes
* [Planeamiento de clústeres de HDInsight unidos a un dominio](apache-domain-joined-architecture.md)
* [Configuración de clústeres de HDInsight unidos a un dominio](apache-domain-joined-configure.md)
* [Administración de clústeres de HDInsight unidos a dominio](apache-domain-joined-manage.md)
* [Configuración de directivas de Hive en clústeres de HDInsight unidos a un dominio](apache-domain-joined-run-hive.md)
* [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)

