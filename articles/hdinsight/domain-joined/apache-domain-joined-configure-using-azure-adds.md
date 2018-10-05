---
title: Configurar un clúster de HDInsight con Enterprise Security Package mediante Azure AD DS
description: Aprenda a instalar y configurar un clúster de Enterprise Security Package de HDInsight mediante Azure Active Directory Domain Services.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: eb24aa0471604696de99f4878baef764cfef0a8b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408361"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Configurar un clúster de HDInsight con Enterprise Security Package mediante Azure Active Directory Domain Services

Los clústeres de Enterprise Security Package (ESP) proporcionan acceso multiusuario en clústeres de Azure HDInsight. Los clústeres de HDInsight con ESP se conectan a un dominio para que los usuarios del dominio puedan usar sus credenciales de dominio para autenticarse con los clústeres y ejecutar trabajos de macrodatos. 

En este artículo, aprenderá a configurar un clúster de HDInsight con ESP mediante Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>ESP está disponible con carácter general en HDI 3.6 para Spark, Interactive y Hadoop. ESP para los tipos de clúster de HBase y Kafka se encuentra en versión preliminar.

## <a name="enable-azure-ad-ds"></a>Habilitar Azure AD DS

Habilitar Azure AD DS es un requisito previo para poder crear un clúster de HDInsight con ESP. Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Cuando está habilitado Azure AD-DS, todos los usuarios y objetos comienzan a sincronizarse desde Azure Active Directory (AAD) hasta Azure AD-DS de forma predeterminada. La longitud de la operación de sincronización depende del número de objetos en AAD. La sincronización puede tardar algunos días cuando hay cientos de miles de objetos. 

Los clientes pueden elegir sincronizar solo los grupos que necesitan acceder a los clústeres de HDInsight. Esta opción de sincronizar solo determinados grupos se conoce como *sincronización con ámbito*. Para instrucciones, consulte [Configuración de la sincronización con ámbito desde Azure AD a un dominio administrado](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization).

> [!NOTE]
> Solo los administradores de inquilinos tienen los privilegios para crear una instancia de Azure AD DS. Solo los usuarios que accedan al clúster deberán tener deshabilitada la autenticación multifactor.

Al habilitar LDAP seguro, coloque el nombre de dominio en el nombre del firmante o el nombre alternativo del firmante del certificado. Por ejemplo, si el nombre de dominio es *contoso.com*, asegúrese de que existe ese nombre exacto en el nombre del firmante o el nombre alternativo del firmante del certificado. Para obtener más información, consulte [Configuración de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="check-aad-ds-health-status"></a>Comprobar el estado de mantenimiento de AAD-DS

Para ver el estado de mantenimiento de su instancia de Azure Active Directory Domain Services, seleccione **Mantenimiento** en la categoría **Administrar**. Asegúrese de que el estado de AAD-DS sea verde (en ejecución) y que la sincronización haya finalizado.

![Mantenimiento de Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="add-managed-identity"></a>Agregar identidad administrada

Después de habilitar Azure AD-DS, cree una identidad administrada asignada por el usuario y asígnela al rol **Colaborador de Domain Services para HDInsight** en el control de acceso de Azure AD DS.

![Control de acceso de Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

La asignación de una identidad administrada al rol **Colaborador de Domain Services para HDInsight** garantiza que la identidad tiene el acceso correcto para realizar determinadas operaciones de servicios de dominio en el dominio AAD-DS. Para obtener más información, vea [What is managed identities for Azure resources?](../../active-directory/managed-identities-azure-resources/overview.md) (¿Qué son las identidades administradas de Azure?).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Crear un clúster de HDInsight con ESP

El siguiente paso es crear el clúster de HDInsight con ESP habilitado mediante Azure AD DS.

Es más fácil colocar la instancia de Azure AD DS y el clúster de HDInsight en la misma red virtual de Azure. Si decide colocarlos en distintas redes virtuales, debe emparejar esas redes virtuales para que las máquinas virtuales de HDInsight tengan una línea de visión al controlador de dominio para unirse a las máquinas virtuales. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md). Para probar si el emparejamiento se realiza correctamente, puede unir una máquina virtual a la red virtual o la subred de HDInsight y hacer ping al nombre de dominio o ejecutar **ldp.exe** para acceder al dominio de AAD-DS.

Cuando crea un clúster de HDInsight, tiene la opción de habilitar Enterprise Security Package en la pestaña de personalización. 

![Redes y seguridad de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Una vez que habilite ESP, se detectarán y validarán las configuraciones erróneas comunes relacionadas con Azure AD DS.

![Validación del dominio de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

La detección temprana ahorra tiempo al permitirle corregir los errores antes de crear el clúster.

![Validación de dominio con error de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Cuando crea un clúster de HDInsight mediante ESP, debe proporcionar los siguientes parámetros:

- **Usuario de administrador de clúster**: elija un administrador para el clúster desde su Azure AD DS sincronizado. Esta cuenta debe estar ya sincronizada y disponible en AAD-DS.

- **Grupos de acceso de clúster**: los grupos de seguridad cuyos usuarios desea sincronizar con el clúster deben estar disponibles en Azure AD DS. Por ejemplo, el grupo HiveUsers. Para más información consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **Dirección URL de LDAPS**: un ejemplo es ldaps://contoso.onmicrosoft.com:636.

En la siguiente captura de pantalla se muestran las configuraciones correctas en Azure Portal:

![Configuración de Azure HDInsight ESP Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

La identidad administrada que se ha creado se puede elegir en la lista desplegable de identidades administradas asignadas por el usuario al crear un nuevo clúster.

![Configuración de Azure HDInsight ESP Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Pasos siguientes
* Para configurar las directivas de Hive y ejecutar las consultas de Hive, vea [Configurar directivas de Hive en HDInsight unido a un dominio](apache-domain-joined-run-hive.md).
* Para que usar SSH para conectarse a clústeres de HDInsight con ESP, vea [Usar SSH con Hadoop basado en Linux en HDInsight desde Linux, Unix u OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).