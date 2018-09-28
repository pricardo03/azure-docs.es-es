---
title: Configurar un clúster de HDInsight con Enterprise Security Package mediante Azure AD DS
description: Obtenga información sobre cómo instalar y configurar un clúster de Enterprise Security Package de HDInsight mediante Azure Active Directory Domain Services
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a5b377381fd540c2a9f1d85e0cb7edce32c2dae8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968380"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Configurar un clúster de HDInsight con Enterprise Security Package mediante Azure Active Directory Domain Services

Los clústeres de Enterprise Security Package (ESP) proporcionan acceso multiusuario en clústeres de Azure HDInsight. Los clústeres de HDInsight con ESP se conectan a un dominio para que los usuarios del dominio puedan usar sus credenciales de dominio para autenticarse con los clústeres y ejecutar trabajos de macrodatos. 

En este artículo, aprenderá a configurar un clúster de HDInsight con ESP mediante Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>ESP está disponible en HDI 3.6+ para Spark, Interactive y Hadoop. ESP para el clúster de HBase está en versión preliminar.


## <a name="enable-azure-ad-ds"></a>Habilitar Azure AD DS

Habilitar Azure AD DS es un requisito previo para poder crear un clúster de HDInsight con ESP. Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Solo los administradores de inquilinos tienen los privilegios para crear una instancia de Azure AD DS. Si usa Azure Data Lake Storage Gen1 como almacenamiento predeterminado para HDInsight, asegúrese de que el inquilino de Azure AD predeterminado de Data Lake Storage Gen1 es el mismo que el del dominio del clúster de HDInsight. Como Hadoop se basa en Kerberos y en autenticación básica, la autenticación multifactor se debe deshabilitar para los usuarios que tendrán acceso al clúster.

LDAP seguro es para un dominio administrado de Azure AD DS. Al habilitar LDAPS, coloque el nombre de dominio en el nombre del firmante o el nombre alternativo del firmante del certificado. Para obtener más información, consulte [Configuración de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="add-managed-identity"></a>Agregar identidad administrada

Después de habilitar Azure AD DS, cree una identidad administrada y asígnela al rol **Colaborador de servicios de dominio de HDInsight** en el control de acceso de Azure AD DS.

![Control de acceso de Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Para obtener más información, vea [What is managed identities for Azure resources?](../../active-directory/managed-identities-azure-resources/overview.md) (¿Qué son las identidades administradas de Azure?).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Crear un clúster de HDInsight con ESP

El siguiente paso es crear el clúster de HDInsight con ESP habilitado mediante Azure AD DS.

Es más fácil colocar la instancia de Azure AD DS y el clúster de HDInsight en la misma red virtual de Azure. Si decide colocarlos en distintas redes virtuales, debe emparejar esas redes virtuales para que las máquinas virtuales de HDInsight tengan una línea de visión al controlador de dominio para unirse a las máquinas virtuales. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md).

Cuando cree un clúster de HDInsight, tiene la opción para habilitar Enterprise Security Package para que conecte su clúster con Azure AD DS. 

![Redes y seguridad de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Una vez que habilite ESP, se detectarán y validarán las configuraciones erróneas comunes relacionadas con Azure AD DS.

![Validación del dominio de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

La detección temprana ahorra tiempo al permitirle corregir los errores antes de crear el clúster.

![Validación de dominio con error de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Cuando crea un clúster de HDInsight mediante ESP, debe proporcionar los siguientes parámetros:

- **Usuario de administrador de clúster**: elija un administrador para el clúster desde su Azure AD DS sincronizado.

- **Grupos de acceso de clúster**: los grupos de seguridad cuyos usuarios desea sincronizar con el clúster deben estar sincronizados y disponibles en Azure AD DS. Por ejemplo, HiveUsers. Si desea especificar varios grupos de usuarios, sepárelos con un punto y coma ";". Los grupos deben existir en el directorio antes del aprovisionamiento. Para más información consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Si el grupo no existe, se produce un error que indica que el grupo HiveUsers no se ha encontrado en Active Directory.

- **Dirección URL de LDAPS**: un ejemplo es ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Escriba la dirección URL completa, incluido "ldaps://" y el número de puerto (:636).

En la siguiente captura de pantalla se muestran las configuraciones en Azure Portal:

   ![Configuración de Azure HDInsight ESP Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Pasos siguientes
* Para configurar las directivas de Hive y ejecutar las consultas de Hive, vea [Configurar directivas de Hive en HDInsight unido a un dominio](apache-domain-joined-run-hive.md).
* Para que usar SSH para conectarse a clústeres de HDInsight con ESP, vea [Usar SSH con Hadoop basado en Linux en HDInsight desde Linux, Unix u OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

