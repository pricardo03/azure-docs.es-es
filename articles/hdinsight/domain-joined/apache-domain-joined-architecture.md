---
title: Arquitectura de Azure HDInsight con Enterprise Security Package
description: Aprenda a planear la seguridad de HDInsight con Enterprise Security Package.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 975a4f7b15d1e1c13767cd7026e961e9d4227603
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998944"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Uso de Enterprise Security Package en HDInsight

El clúster de Azure HDInsight estándar es de un único usuario. Es apropiado para la mayor parte de las empresas que tienen equipos de aplicaciones pequeños que generan grandes cargas de trabajo de datos. Cada usuario puede crear un clúster dedicado a petición y destruirlo cuando ya no sea necesario. 

Muchas empresas han migrado a un modelo en el que los equipos de TI administran los clústeres y varios equipos de aplicaciones comparten los clústeres. Estas empresas más grandes necesitan acceso multiusuario a cada clúster de Azure HDInsight.

HDInsight se basa en un proveedor de identidades conocido (Active Directory) de una manera administrada. Mediante la integración de HDInsight con [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), puede acceder a los clústeres con sus credenciales de dominio. 

Las máquinas virtuales (VM) de HDInsight están unidas al dominio proporcionado. De este modo, todos los servicios que se ejecutan en HDInsight (Ambari, servidor de Hive, Ranger, servidor Thrift de Spark, etc.) funcionan perfectamente para el usuario autenticado. Los administradores pueden crear entonces directivas de autorización seguras con Apache Ranger para proporcionar control de acceso basado en rol para los recursos del clúster.


## <a name="integrate-hdinsight-with-active-directory"></a>Integración de HDInsight con Active Directory

Hadoop de código abierto se basa en Kerberos para proporcionar autenticación y seguridad. Por consiguiente, los nodos del clúster de HDInsight con Enterprise Security Package (ESP) se combinan en un dominio que administra Azure AD DS. Se configura la seguridad de Kerberos para los componentes de Hadoop en el clúster. 

Para cada componente de Hadoop, se crea una entidad de servicio automáticamente. También se crea una entidad de seguridad de máquina correspondiente para cada máquina unida al dominio. Para almacenar estas entidades de seguridad de servicio o máquina, debe proporcionar una unidad organizativa (OU) en el controlador de dominio (Azure AD DS), donde se colocan estas entidades de seguridad. 

En resumen, es necesario configurar un entorno con:

- Un dominio de Active Directory (administrado por Azure AD DS).
- LDAP seguro (LDAPS) habilitado en Azure AD DS.
- Conectividad de red adecuada desde la red virtual de HDInsight a la red virtual de Azure AD DS, si elige redes virtuales independientes para ellas. Una máquina virtual dentro de la red virtual de HDInsight debe tener una línea de visión a Azure AD DS a través del emparejamiento de la red virtual. Si HDInsight y Azure AD DS se implementan en la misma red virtual, la conectividad se proporciona de manera automática y no es necesaria ninguna otra acción.
- Una unidad organizativa [creada en Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).
- Una cuenta de servicio que tenga permisos para:
    - Crear entidades de servicio en la unidad organizativa.
    - Unir máquinas al dominio y crear entidades de seguridad de máquina en la unidad organizativa.

En la siguiente captura de pantalla se muestra una unidad organizativa creada en contoso.com. También muestra algunas de las entidades de servicio y de las entidades de seguridad de máquina.

![Unidad organizativa para clústeres de HDInsight con ESP](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

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
* [Configuración de directivas de Hive en clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-run-hive.md)
* [Administración de clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-manage.md) 
