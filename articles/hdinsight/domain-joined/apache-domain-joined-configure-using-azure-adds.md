---
title: Configuración de clústeres de HDInsight unidos a un dominio con AAD-DS
description: Aprender a configurar y definir clústeres de HDInsight unidos a un dominio con Azure Active Directory Domain Services
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 8064940e0d0f035010a2521752d6f32f3f9ccd9f
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849843"
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Configurar clústeres de HDInsight unidos a un dominio con Azure Active Directory Domain Services

Los clústeres unidos a un dominio proporcionan el acceso multiusuario en clústeres de HDInsight. Los clústeres de HDInsight unidos a un dominio se conectan a un dominio, lo que permite a los usuarios del dominio usar sus credenciales de dominio para autenticarse en los clústeres y ejecutar trabajos de macrodatos. 

En este artículo, aprenderá a configurar un clúster de HDInsight unido a un dominio con Azure Active Directory Domain Services.

## <a name="create-azure-adds"></a>Creación de una instancia de Azure AD DS

La habilitación de Azure AD Domain Services (AAD-DS) es un requisito previo para poder crear un clúster de HDInsight unido a un dominio. Para habilitar una instancia de AAD DS, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Solo los administradores de inquilinos tienen privilegios para crear instancias de AAD-DS. Si usa Azure Data Lake Storage (ADLS) como almacenamiento predeterminado para HDInsight, asegúrese de que el inquilino de Azure AD predeterminado de ADLS es el mismo que el del dominio del clúster de HDInsight. Dado que Hadoop se basa en Kerberos y en autenticación básica, se debe deshabilitar la autenticación multifactor para los usuarios que tengan acceso al clúster.

Después de aprovisionar la instancia de AAD-DS, debe crear una cuenta de servicio en AAD (que se sincronizará con AAD-DS) con los permisos correctos. Si la cuenta de servicio ya existe, tiene que restablecer su contraseña y esperar hasta que se sincronice con AAD DS (este restablecimiento dará como resultado la creación del hash de contraseñas de Kerberos y la sincronización con AAD-DS podría tardar hasta 30 minutos). Esta cuenta de servicio debe tener los privilegios siguientes:

- Unir máquinas al dominio y colocar las entidades de seguridad de la máquina en la unidad organizativa que se especifique durante la creación del clúster.
- Crear entidades de servicio dentro de la unidad organizativa que especifique durante la creación del clúster.

> [!NOTE]
> Dado que Apache Zeppeling usa el nombre de dominio para autenticar la cuenta de servicio administrativa, dicha cuenta DEBE tener el mismo nombre de dominio que su sufijo de UPN para que Apache Zeppeling funcione correctamente.

Para más información sobre las unidades organizativas y cómo administrarlos, consulte [Creación de una unidad organizativa en un dominio administrado de Servicios de dominio de Azure AD](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Se requiere LDAP seguro para dominio administrado con AAD-DS. Para habilitar LDAP seguro, consulte [Configuración de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Creación de un clúster de HDInsight unido a un dominio

El siguiente paso es crear el clúster de HDInsight con AAD-DS y la cuenta de servicio que creó en la sección anterior.

Es más fácil colocar AAD-DS y el clúster de HDInsight en la misma red virtual (VNet) de Azure. En caso de que eligiera ubicarlos en diferentes redes virtuales, debe emparejar esas redes virtuales para que las máquinas virtuales HDI tengan una línea de visión con el controlador de dominio para unirse a las máquinas virtuales. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md).

Al crear un clúster de HDInsight unidos a un dominio, debe proporcionar los parámetros siguientes:

- **Nombre de dominio**: nombre de dominio asociado con AAD-DS. Por ejemplo, contoso.onmicrosoft.com.
- **Nombre de usuario de dominio**: cuenta de servicio del dominio administrado creada en la sección anterior. Por ejemplo, hdiadmin@contoso.onmicrosoft.com. Este usuario de dominio será el administrador de este clúster de HDInsight.
- **Contraseña de dominio**: contraseña de la cuenta de servicio.
- **Unidad organizativa**: nombre distintivo de la unidad organizativa (UO) que quiere utilizar con el clúster de HDInsight. Por ejemplo: OU=HDInsightOU,DC=contoso,DC=onmicrosohift,DC=com. Si no existe esta unidad organizativa, el clúster de HDInsight intentará crearla mediante los privilegios que tiene la cuenta de servicio. (Por ejemplo, si la cuenta de servicio está en el grupo de administradores de AAD-DS, tiene los permisos adecuados para crear una unidad organizativa; en caso contrario, deberá crear primero la unidad organizativa y asignar control total a la cuenta de servicio a través de esa unidad organizativa; consulte [Creación de una unidad organizativa en un dominio administrado de Servicios de dominio de Azure AD](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)).

    > [!IMPORTANT]
    > Es importante incluir todos lo controladores de dominio separados por coma después de la unidad organizativa (por ejemplo, OU=HDInsightOU,DC=contoso, DC=onmicrosohift,DC=com).

- **Dirección URL de LDAPS**: ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Escriba la dirección URL completa, incluido ldaps:// y el número de puerto (:636)

- **Grupo de usuarios de acceso**: grupos de seguridad cuyos usuarios quiere que se sincronicen con el clúster. Por ejemplo, HiveUsers. Si desea especificar varios grupos de usuarios, sepárelos con una coma ','.
 
En la siguiente captura de pantalla se muestran las configuraciones en Azure Portal:

![Configuración de Active Directory Domain Services unido a un dominio de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Pasos siguientes
* Para configurar directivas de Hive y ejecución de consultas de Hive, consulte [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Configuración de directivas de los clústeres de HDInsight unidos a dominio).
* Para utilizar SSH para conectarse a clústeres de HDInsight unidos a dominio, consulte [Utilización de SSH con Hadoop en HDInsight basado en Linux desde Linux, Unix u OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

