---
title: Arquitectura Azure HDInsight unida a un dominio | Microsoft Docs
description: Aprenda a planear clústeres de HDInsight unidos a un dominio.
services: hdinsight
documentationcenter: ''
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: f4380f5d6ec379d5807f697294623a672bd270ae
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715248"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planeamiento de clústeres de Hadoop unidos a un dominio de Azure en HDInsight

El clúster de HDInsight estándar es de un único usuario. Es apropiado para la mayor parte de las empresas que tienen equipos de aplicaciones pequeños que generan grandes cargas de trabajo de datos. Cada usuario puede tener otro clúster dedicado a sí mismo a petición y eliminarlo cuando ya no sea necesario. Sin embargo, muchas empresas empezaron a usar un modelo en el que los equipos de TI administran clústeres y varios equipos de aplicaciones comparten los clústeres. Por lo tanto, es necesario el acceso multiusuario al clúster en HDInsight de Azure para estas empresas más grandes.

HDInsight se basa en el proveedor de identidades más conocido: Active Directory (AD) de una manera administrada. Mediante la integración de HDInsight con [Azure Active Directory Domain Services (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md), puede acceder a los clústeres con sus credenciales de dominio. Las máquinas virtuales de HDInsight están unidas al dominio proporcionado, así que todos los servicio que se ejecutan en HDInsight (Ambari, Hive server, Ranger, Spark Thrift server, etc.) funcionan perfectamente para el usuario autenticado. Los administradores pueden crear entonces directivas de autorización seguras con Apache Ranger para proporcionar control de acceso basado en rol para los recursos del clúster.


## <a name="integrate-hdinsight-with-active-directory"></a>Integración de HDInsight con Active Directory

Hadoop de código abierto se basa en Kerberos para proporcionar autenticación y seguridad. Por lo tanto, los nodos del clúster de HDInsight están unidos a un dominio administrado por AAD-DS. Se configura la seguridad de Kerberos para los componentes de Hadoop en el clúster. Para cada uno de los componentes de Hadoop, se crea una entidad de servicio automáticamente. También se crea una entidad de seguridad de máquina correspondiente para cada máquina unida al dominio. Para almacenar estas entidades de seguridad de servicio o máquina, es necesario proporcionar una unidad organizativa (OU) en el controlador de dominio (AAD-DS), donde se colocan estas entidades de seguridad. 

En resumen, es necesario configurar un entorno con:

- Un dominio de Active Directory (administrado por AAD-DS).
- LDAP seguro (LDAPS) habilitado en AAD-DS.
- Conectividad de red adecuada desde la red virtual de HDInsight hasta la red virtual de AAD-DS, en caso de que elija distintas redes virtuales para ellas. Una máquina virtual dentro de la red virtual de HDI debe tener una línea de visión a AAD-DS mediante el emparejamiento de VNET. Si HDI y AAD-DS se implementan en la misma red virtual, la conectividad se proporciona automáticamente y no es necesaria ninguna otra acción.
- Una unidad organizativa (OU) [creada en AAD-DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)
- Una cuenta de servicio que tenga permisos para:
    - Crear entidades de servicio en la unidad organizativa.
    - Unir máquinas al dominio y crear entidades de seguridad de máquina en la unidad organizativa.

En la siguiente captura de pantalla se muestra una unidad organizativa creada en contoso.com. Algunas de las entidades de servicio y entidades de seguridad de máquina también se muestran en la captura de pantalla.

![Unidad organizativa de clústeres de HDInsight unidos a un dominio](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="different-domain-controllers-setup"></a>Distintas configuraciones de controladores de dominio:
HDInsight solo admite actualmente AAD-DS como controlador de dominio principal que el clúster usará para comunicarse con Kerberos. Sin embargo, también son posibles otras configuraciones complejas de AD siempre y cuando conduzcan a habilitar AAD-DS para el acceso a HDI.

- **[Azure Active Directory Domain Services (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md)**: este servicio proporciona un dominio administrado, que es completamente compatible con Windows Server Active Directory. Microsoft se encarga de administrar y supervisar el dominio, y de aplicarle los parches, en una configuración de Alta disponibilidad (HA). Puede implementar el clúster sin preocuparse por mantener los controladores de dominio. Los usuarios, los grupos y las contraseñas se sincronizan desde su instancia de Azure Active Directory (AAD) [sincronización unidireccional desde AAD hasta AAD-DS], lo que permite a los usuarios iniciar sesión en el clúster con las mismas credenciales corporativas. Para más información, consulte [Configurar clústeres de HDInsight unidos a un dominio con Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).
- **AD local o AD en máquinas virtuales de IaaS**: si tiene una instancia de AD local u otras configuraciones de AD más complejas para el dominio, puede sincronizar esas entidades con AAD mediante AD Connect y, luego, habilitar AAD-DS en ese inquilino de AD. Puesto que Kerberos se basa en valores hash de contraseña, deberá [habilitar la sincronización de hash de contraseñas en AAD-DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Si va a usar la federación con los servicios de federación de AD (ADFS), puede configurar opcionalmente la sincronización de hash de contraseña como apoyo en caso de que se produzca un error en la infraestructura de ADFS. Para más información, consulte la sección sobre [cómo habilitar la sincronización de hash de contraseña con la sincronización de AAD Connect](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). El uso de AD local o de AD en máquinas virtuales de IaaS de forma aislada, sin AAD ni AAD-DS, no es una configuración admitida para la unión a dominios de clúster de HDI.

## <a name="next-steps"></a>Pasos siguientes
* [Configuración de clústeres de HDInsight unidos a un dominio](apache-domain-joined-configure-using-azure-adds.md)
* [Configuración de directivas de Hive en clústeres de HDInsight unidos a un dominio](apache-domain-joined-run-hive.md)
* [Administración de clústeres de HDInsight unidos a dominio](apache-domain-joined-manage.md) 
