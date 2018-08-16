---
title: Configuración de un clúster de HDInsight unido a un dominio mediante Azure AD DS
description: Obtenga información sobre cómo configurar un clúster de HDInsight unido a un dominio mediante Azure Active Directory Domain Services
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
editor: jasonwhowell
ms.topic: conceptual
ms.date: 07/17/2018
ms.openlocfilehash: 0d44812c92fd14bf87aac9a942241f8de55f2eec
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590599"
---
# <a name="configure-a-domain-joined-hdinsight-cluster-by-using-azure-active-directory-domain-services"></a>Configuración de un clúster de HDInsight unido a un dominio mediante Azure Active Directory Domain Services

Los clústeres unidos a un dominio proporcionan acceso multiusuario en los clústeres de Azure HDInsight. Los clústeres de HDInsight unidos a un dominio se conectan a un dominio, lo que permite a los usuarios del dominio usar sus credenciales de dominio para autenticarse en los clústeres y ejecutar trabajos de macrodatos. 

En este artículo, aprenderá a configurar un clúster de HDInsight unido a un dominio con Azure Active Directory Domain Services (Azure AD DS).

## <a name="enable-azure-ad-ds"></a>Habilitación de Azure AD DS

La habilitación de Azure AD DS es un requisito previo para poder crear un clúster de HDInsight unido a un dominio. Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Solo los administradores de inquilinos tienen privilegios para crear instancias de Azure AD DS. Si usa Azure Data Lake Storage Gen1 como almacenamiento predeterminado para HDInsight, asegúrese de que el inquilino de Azure AD predeterminado de Data Lake Storage Gen1 es el mismo que el del dominio del clúster de HDInsight. Como Hadoop se basa en Kerberos y en autenticación básica, la autenticación multifactor se debe deshabilitar para los usuarios que tendrán acceso al clúster.

Después de aprovisionar la instancia de Azure AD DS, cree una cuenta de servicio en Azure Active Directory (Azure AD) con los permisos adecuados. Si ya existe esta cuenta de servicio, restablezca la contraseña y espere hasta que se sincronice con Azure AD DS. Este restablecimiento dará como resultado la creación del hash de contraseña de Kerberos y puede tardar hasta 30 minutos en sincronizarse con Azure AD DS. 

La cuenta de servicio debe tener los privilegios siguientes:

- Unir máquinas al dominio y colocar las entidades de seguridad de la máquina en la unidad organizativa que se especifique durante la creación del clúster.
- Crear entidades de servicio dentro de la unidad organizativa que especifique durante la creación del clúster.

> [!NOTE]
> Dado que Apache Zeppelin usa el nombre de dominio para autenticar la cuenta de servicio administrativa, dicha cuenta *debe* tener el mismo nombre de dominio que su sufijo de UPN para que Apache Zeppelin funcione correctamente.

Para más información sobre las unidades organizativas y cómo administrarlas, consulte [Creación de una unidad organizativa en un dominio administrado de Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

LDAP seguro es para un dominio administrado de Azure AD DS. Para más información, consulte [Configuración de LDAP seguro para un dominio administrado de Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Creación de un clúster de HDInsight unido a un dominio

El paso siguiente es crear el clúster de HDInsight con Azure AD DS y la cuenta de servicio que creó en la sección anterior.

Es más sencillo colocar tanto la instancia de Azure AD DS como el clúster de HDInsight en la misma red virtual de Azure. Si decide colocarlos en distintas redes virtuales, debe emparejar esas redes virtuales para que las máquinas virtuales de HDInsight tengan una línea de visión al controlador de dominio para unirse a las máquinas virtuales. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md).

Al crear un clúster de HDInsight unidos a un dominio, debe proporcionar los parámetros siguientes:

- **Nombre de dominio**: nombre de dominio asociado con Azure AD DS. Un ejemplo es contoso.onmicrosoft.com.

- **Nombre de usuario de dominio**: cuenta de servicio del dominio administrado del controlador de dominio de Azure ADDS que creó en la sección anterior. Un ejemplo es hdiadmin@contoso.onmicrosoft.com. Este usuario de dominio será el administrador de este clúster de HDInsight.

- **Contraseña de dominio**: contraseña de la cuenta de servicio.

- **Unidad organizativa**: nombre distintivo de la unidad organizativa que quiere utilizar con el clúster de HDInsight. Un ejemplo es OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com. Si no existe esta unidad organizativa, el clúster de HDInsight intenta crearla mediante los privilegios que tiene la cuenta de servicio. Por ejemplo, si la cuenta de servicio está en el grupo de administradores de Azure AD DS, tiene los permisos adecuados para crear una unidad organizativa. En caso contrario, debe crear primero la unidad organizativa y proporcionar a la cuenta de servicio el control total sobre esa unidad organizativa. Para más información, consulte [Creación de una unidad organizativa en un dominio administrado de Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

    > [!IMPORTANT]
    > Incluya todos los controladores de dominio, separados con comas, después de la unidad organizativa (por ejemplo, OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com).

- **Dirección URL de LDAPS**: un ejemplo es ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Escriba la dirección URL completa, incluido "ldaps://" y el número de puerto (:636).

- **Grupo de usuarios de acceso**: grupos de seguridad cuyos usuarios quiere que se sincronicen con el clúster. Por ejemplo, HiveUsers. Si desea especificar varios grupos de usuarios, sepárelos con un punto y coma ";". Los grupos deben existir en el directorio antes del aprovisionamiento. Para más información consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Si el grupo no existe, se produce un error que indica que el grupo HiveUsers no se ha encontrado en Active Directory.

En la siguiente captura de pantalla se muestran las configuraciones en Azure Portal:

   ![Configuración de Active Directory Domain Services unido a un dominio de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Pasos siguientes
* Para configurar las directivas de Hive y ejecutar las consultas de Hive, consulte [Configuración de directivas de Hive en clústeres de HDInsight unidos a un dominio](apache-domain-joined-run-hive.md).
* Para utilizar SSH para conectarse a clústeres de HDInsight unidos a dominio, consulte [Utilización de SSH con Hadoop en HDInsight basado en Linux desde Linux, Unix u OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

