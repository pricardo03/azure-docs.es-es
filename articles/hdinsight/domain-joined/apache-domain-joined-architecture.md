---
title: Arquitectura de Azure HDInsight con Enterprise Security Package
description: Aprenda a planear la seguridad de HDInsight con Enterprise Security Package.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 50c5838f576b6fd6775373f2dbe3c46d751545c1
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437595"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Uso de Enterprise Security Package en HDInsight

El clúster de Azure HDInsight estándar es de un único usuario. Es apropiado para la mayor parte de las empresas que tienen equipos de aplicaciones pequeños que generan grandes cargas de trabajo de datos. Cada usuario puede crear un clúster dedicado a petición y destruirlo cuando ya no sea necesario. 

Muchas empresas han migrado a un modelo en el que los equipos de TI administran los clústeres y varios equipos de aplicaciones comparten los clústeres. Estas empresas más grandes necesitan acceso multiusuario a cada clúster de Azure HDInsight.

HDInsight se basa en un proveedor de identidades conocido (Active Directory) de una manera administrada. Mediante la integración de HDInsight con [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), puede acceder a los clústeres con sus credenciales de dominio. 

Las máquinas virtuales (VM) de HDInsight están unidas al dominio proporcionado. De este modo, todos los servicios que se ejecutan en HDInsight (Apache Ambari, servidor de Apache Hive, Apache Ranger, servidor Thrift de Apache Spark, etc.) funcionan perfectamente para el usuario autenticado. Los administradores pueden crear entonces directivas de autorización seguras con Apache Ranger para proporcionar control de acceso basado en rol para los recursos del clúster.

## <a name="integrate-hdinsight-with-active-directory"></a>Integración de HDInsight con Active Directory

Apache Hadoop de código abierto se basa en Kerberos para proporcionar autenticación y seguridad. Por consiguiente, los nodos del clúster de HDInsight con Enterprise Security Package (ESP) se combinan en un dominio que administra Azure AD DS. Se configura la seguridad de Kerberos para los componentes de Hadoop en el clúster. 

Las siguientes cosas se crean automáticamente:
- una entidad de servicio para cada componente de Hadoop 
- una entidad de seguridad de máquina para cada máquina unida al dominio
- una unidad organizativa (UO) para cada clúster para almacenar estas entidades de servicio y de máquina 

En resumen, es necesario configurar un entorno con:

- Un dominio de Active Directory (administrado por Azure AD DS).
- LDAP seguro (LDAPS) habilitado en Azure AD DS.
- Conectividad de red adecuada desde la red virtual de HDInsight a la red virtual de Azure AD DS, si elige redes virtuales independientes para ellas. Una máquina virtual dentro de la red virtual de HDInsight debe tener una línea de visión a Azure AD DS a través del emparejamiento de la red virtual. Si HDInsight y Azure AD DS se implementan en la misma red virtual, la conectividad se proporciona de manera automática y no es necesaria ninguna otra acción.

## <a name="set-up-different-domain-controllers"></a>Configuración de distintos controladores de dominio
HDInsight actualmente solo admite Azure AD DS como el controlador de dominio principal que el clúster usa para la comunicación de Kerberos. Pero hay otras configuraciones complejas de Active Directory que son posibles, siempre que dicha configuración lleve a habilitar Azure AD DS para el acceso de HDInsight.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) proporciona un dominio administrado que es totalmente compatible con Windows Server Active Directory. Microsoft se encarga de administrar y supervisar el dominio, y de aplicarle los parches, en una configuración de alta disponibilidad (HA). Puede implementar el clúster sin preocuparse por mantener los controladores de dominio. 

Los usuarios, los grupos y las contraseñas se sincronizan desde Azure Active Directory (Azure AD). La sincronización unidireccional desde la instancia de Azure AD a Azure AD DS permite que los usuarios inicien sesión en el clúster con las mismas credenciales corporativas. 

Para más información, consulte [Configuración de clústeres de HDInsight con Enterprise Security Package mediante Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>Active Directory local o Active Directory en máquinas virtuales de IaaS

Si tiene una instancia de Active Directory local o configuraciones más complejas de Active Directory para el dominio, puede sincronizar esas identidades con Azure AD mediante Azure AD Connect. Luego puede habilitar Azure AD DS en ese inquilino de Active Directory. 

Como Kerberos se basa en valores hash de contraseña, deberá [habilitar la sincronización de hash de contraseñas en Azure AD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Si usa la federación con Servicios de federación de Active Directory (AD FS), tiene la posibilidad de configurar la sincronización de hash de contraseñas como copia de seguridad en caso de error en la infraestructura de AD FS. Para más información, consulte [Implementación de la sincronización de hash de contraseñas con la sincronización de Azure AD Connect](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

El uso de Active Directory local o de Active Directory solo en máquinas virtuales de IaaS, sin Azure AD ni Azure AD DS, no es una configuración compatible con los clústeres de HDInsight con ESP.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-configure-using-azure-adds.md)
* [Configuración de directivas de Apache Hive en clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-run-hive.md)
* [Administración de clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-manage.md) 
