---
title: Configurar un clúster de HDInsight con Enterprise Security Package mediante Azure AD DS
description: Aprenda a instalar y configurar un clúster de Enterprise Security Package de HDInsight mediante Azure Active Directory Domain Services.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 10/9/2018
ms.openlocfilehash: 851fa7c6a970d725a52bc84d7d057472e09c3ee9
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388347"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Configurar un clúster de HDInsight con Enterprise Security Package mediante Azure Active Directory Domain Services

Los clústeres de Enterprise Security Package (ESP) proporcionan acceso multiusuario en clústeres de Azure HDInsight. Los clústeres de HDInsight con ESP se conectan a un dominio para que los usuarios del dominio puedan usar sus credenciales de dominio para autenticarse con los clústeres y ejecutar trabajos de macrodatos. 

En este artículo, aprenderá a configurar un clúster de HDInsight con ESP mediante Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>ESP está disponible con carácter general en HDI 3.6 para Spark, Interactive y Hadoop. ESP para los tipos de clúster de HBase y Kafka se encuentra en versión preliminar.

## <a name="enable-azure-ad-ds"></a>Habilitar Azure AD DS

> [!NOTE]
> Solo los administradores de inquilinos tienen los privilegios para crear una instancia de Azure AD DS. Si el almacenamiento del clúster es Azure Data Lake Store (ADLS) Gen1 o Gen2, deshabilite Multi-Factor Authentication (MFA) solo para los usuarios que obtendrán acceso al clúster. Si el almacenamiento de clúster es Azure Blob Storage (WASB), no deshabilite MFA.

Habilitar Azure AD DS es un requisito previo para poder crear un clúster de HDInsight con ESP. Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Cuando está habilitado Azure AD DS, todos los usuarios y objetos comienzan a sincronizarse desde Azure Active Directory hasta Azure AD DS de forma predeterminada. La longitud de la operación de sincronización depende del número de objetos en Azure AD. La sincronización puede tardar algunos días cuando hay cientos de miles de objetos. 

Los clientes pueden elegir sincronizar solo los grupos que necesitan acceder a los clústeres de HDInsight. Esta opción de sincronizar solo determinados grupos se conoce como *sincronización con ámbito*. Para instrucciones, consulte [Configuración de la sincronización con ámbito desde Azure AD a un dominio administrado](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization).

Después de habilitar Azure AD DS, se ejecuta un servidor local del Servicio de nombres de dominio (DNS) en las máquinas virtuales (VM) de AD. Configure su red virtual (VNET) de Azure AD DS para usar estos servidores DNS personalizados. Para buscar las direcciones IP correctas, seleccione **Propiedades** en la categoría **Administrar** y observe las direcciones IP enumeradas debajo de la **dirección IP en la red virtual**.

![Localice las direcciones IP de los servidores DNS locales.](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns.png)

Cambie la configuración de los servidores DNS en la red virtual de Azure AD DS para usar estas IP personalizadas; para ello, seleccione **Servidores DNS** en la categoría **Configuración**. A continuación, haga clic en el botón de radio al lado de la opción **Personalizado**; escriba la primera dirección IP en el cuadro de texto y, a continuación, haga clic en **Guardar**. Agregue direcciones IP adicionales siguiendo los mismos pasos.

![Actualización de la configuración de DNS de la red virtual](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)



Al habilitar LDAP seguro, coloque el nombre de dominio en el nombre del firmante o el nombre alternativo del firmante del certificado. Por ejemplo, si el nombre de dominio es *contoso.com*, asegúrese de que existe ese nombre exacto en el nombre del firmante o el nombre alternativo del firmante del certificado. Para obtener más información, consulte [Configuración de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="check-azure-ad-ds-health-status"></a>Comprobar el estado de mantenimiento de Azure AD DS
Para ver el estado de mantenimiento de su instancia de Azure Active Directory Domain Services, seleccione **Mantenimiento** en la categoría **Administrar**. Asegúrese de que el estado de Azure AD DS esté en verde (en ejecución) y que la sincronización haya finalizado.

![Mantenimiento de Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

Si un grupo de seguridad de red se encarga de proteger AAD DS, debe asegurarse de que todos los [puertos necesarios](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) estén en la lista blanca que se encuentra en las reglas NSG de la subred AAD DS. 

## <a name="create-and-authorize-a-managed-identity"></a>Crear y autorizar una identidad gestionada
> [!NOTE]
> Solo los administradores de AAD DS tienen los privilegios necesarios para autorizar esta identidad administrada.

Se usa una **identidad administrada que asignó el usuario** para simplificar las operaciones de servicios de dominio. Cuando asigna la identidad administrada al rol de colaborador de Domain Services para HDInsight, este puede leer, crear, modificar y eliminar operaciones de servicios de dominio. Ciertas operaciones de servicios de dominio, como la creación de unidades organizativas y los principios de servicio, son necesarias para Enterprise Security Package de HDInsight. Las identidades administradas se pueden crear en cualquier suscripción. Para obtener más información, consulte [Managed identities for Azure resources](../../active-directory/managed-identities-azure-resources/overview.md) (Identidades administradas para los recursos de Azure).

Para configurar una identidad administrada para usarla con los clústeres de ESP de HDInsight, cree una identidad administrada que haya asignado el usuario, si aún no la tiene. Consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada que haya asignado el usuario mediante Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) para obtener más detalles. A continuación, asigne la identidad administrada al rol de **colaborador de Domain Services para HDInsight** en el control de acceso de Azure AD DS (es necesario tener privilegios de administrador de AAD DS para realizar esta asignación de roles).

![Control de acceso de Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

La asignación de una identidad administrada al rol **Colaborador de Domain Services para HDInsight** garantiza que la identidad tiene el acceso correcto para realizar determinadas operaciones de servicios de dominio en el dominio AAD-DS.

Una vez que se crea la identidad administrada y se le asigna el rol correcto, el administrador de AAD DS puede configurar quién puede usar esta identidad administrada. Para configurar los usuarios de la identidad administrada, el administrador debe seleccionar la identidad administrada en el portal y hacer clic en **Control de acceso (IAM)** en **Información general**. A continuación, en el lado derecho, asigne el rol "Operador de identidades administradas" a los usuarios o grupos que quieran crear clústeres de ESP de HDInsight. Por ejemplo, el administrador de AAD DS puede asignar este rol al grupo "MarketingTeam" para la identidad administrada "sjmsi", tal como se muestra en la siguiente imagen.

![Asignación de roles del operador de identidades administradas de HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)


## <a name="create-a-hdinsight-cluster-with-esp"></a>Crear un clúster de HDInsight con ESP

El siguiente paso es crear el clúster de HDInsight con ESP habilitado mediante Azure AD DS.

Es más fácil colocar la instancia de Azure AD DS y el clúster de HDInsight en la misma red virtual de Azure. Si están en redes virtuales diferentes, debe emparejar esas redes virtuales para que las máquinas virtuales de HDInsight sean visibles para el controlador de dominio y se puedan agregar al dominio. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md). 

Una vez emparejadas las redes virtuales, configure la red virtual de HDInsight para que use un servidor DNS personalizado y escriba las direcciones IP privadas de Azure AD DS como direcciones del servidor DNS. Cuando ambas redes virtuales utilizan los mismos servidores DNS, su nombre de dominio personalizado se resolverá con la IP correcta y podrá obtener acceso a él desde HDInsight. Por ejemplo, si el nombre de dominio es "contoso.com", después de realizar este paso, debe hacer ping en "contoso.com" para resolver la dirección IP correcta de Azure AD DS. Para comprobar si el emparejamiento se realizó correctamente, puede agregar una máquina virtual de Windows a la red virtual o la subred de HDInsight y hacer ping al nombre de dominio o ejecutar **ldp.exe** para acceder al dominio de Azure AD-DS. A continuación, agregue esta máquina virtual de Windows al dominio para confirmar que todas las llamadas de RPC requeridas se realizaron correctamente entre el cliente y el servidor.

![Configuración de servidores DNS personalizados para la red virtual emparejada](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Cuando cree un clúster de HDInsight, puede habilitar Enterprise Security Package en la pestaña personalizada.

![Redes y seguridad de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Una vez que habilite ESP, se detectarán y validarán las configuraciones erróneas comunes relacionadas con Azure AD DS.

![Validación del dominio de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

La detección temprana ahorra tiempo al permitirle corregir los errores antes de crear el clúster.

![Validación de dominio con error de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Cuando crea un clúster de HDInsight mediante ESP, debe proporcionar los siguientes parámetros:

- **Usuario de administrador de clúster**: elija un administrador para el clúster desde su Azure AD DS sincronizado. Esta cuenta ya debe estar sincronizada y disponible en Azure AD DS.

- **Grupos de acceso de clúster**: los grupos de seguridad cuyos usuarios desea sincronizar con el clúster deben estar disponibles en Azure AD DS. Por ejemplo, el grupo HiveUsers. Para más información consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **Dirección URL de LDAPS**: un ejemplo es ldaps://contoso.onmicrosoft.com:636.

En la siguiente captura de pantalla se muestra una configuración correcta en Azure Portal:

![Configuración de Azure HDInsight ESP Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

La identidad administrada que se ha creado se puede elegir en la lista desplegable de identidades administradas asignadas por el usuario al crear un nuevo clúster.

![Configuración de Azure HDInsight ESP Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Pasos siguientes
* Para configurar las directivas de Hive y ejecutar las consultas de Hive, vea [Configurar directivas de Hive en HDInsight unido a un dominio](apache-domain-joined-run-hive.md).
* Para que usar SSH para conectarse a clústeres de HDInsight con ESP, vea [Usar SSH con Hadoop basado en Linux en HDInsight desde Linux, Unix u OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
