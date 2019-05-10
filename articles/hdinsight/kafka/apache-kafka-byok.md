---
title: Traiga su propia clave de Apache Kafka en HDInsight de Azure
description: En este artículo se describe el método para que use su propia clave desde Azure Key Vault para cifrar los datos almacenados en Apache Kafka en Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 6108bfd9e39b37507ec7e113bf2c489e890f0ca0
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233568"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight"></a>Traiga su propia clave de Apache Kafka en HDInsight de Azure

Azure HDInsight incluye compatibilidad con Bring Your Own Key (BYOK) para Apache Kafka. Esta funcionalidad le permite ser el propietario y administrar las claves utilizadas para cifrar datos en reposo.

Todos los discos administrados en HDInsight están protegidos con Azure Storage Service Encryption (SSE). De forma predeterminada, los datos en esos discos se cifran mediante claves administradas por Microsoft. Al habilitar BYOK, le proporciona a HDInsight la clave de cifrado para que la use y administre con Azure Key Vault.

El cifrado de BYOK es un proceso de un paso controlado durante la creación del clúster sin ningún costo adicional. Solo necesita registrar HDInsight como una identidad administrada con Azure Key Vault y agregar la clave de cifrado al crear el clúster.

Todos los mensajes en el clúster de Kafka (incluidas las réplicas mantenidas por Kafka) se cifran con una clave de cifrado de datos (DEK) simétrica. La DEK está protegida mediante la clave de cifrado de claves (KEK) del almacén de claves. Azure HDInsight controla completamente los procesos de cifrado y descifrado. 

Puede usar Azure Portal o la CLI de Azure para rotar las claves en el almacén de claves de forma segura. Cuando se rota una clave, el clúster de HDInsight Kafka comienza a usar la clave nueva en cuestión de minutos. Habilitar las características de protección de claves "Eliminación temporal" para protegerse frente a ransomware escenarios y la eliminación accidental. Los almacenes de claves sin esta característica de protección no se admiten.

## <a name="get-started-with-byok"></a>Introducción a BYOK
Para crear un BYOK habilitado el clúster de Kafka, describiremos los pasos siguientes:
1. Crear identidades administradas para los recursos de Azure
2. Azure Key Vault y claves de instalación
3. Creación de clúster de HDInsight Kafka con BYOK habilitado
4. Rotación de la clave de cifrado

## <a name="create-managed-identities-for-azure-resources"></a>Crear identidades administradas para los recursos de Azure

   Para autenticarse en Key Vault, cree una identidad administrada asignada por el usuario utilizando el [portal Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md), o [ CLI de Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Para obtener más información sobre cómo administrado trabajo de identidades en Azure HDInsight, consulte [administra identidades en Azure HDInsight](../hdinsight-managed-identities.md). Azure Active Directory es un requisito para las identidades administradas y BYOK a Kafka, pero Enterprise Security Package no lo es. Asegúrese de guardar el identificador de recurso de identidad administrada para cuando lo agregue a la directiva de acceso de Key Vault.

   ![Creación de una identidad administrada asignada por el usuario en Azure Portal](./media/apache-kafka-byok/user-managed-identity-portal.png)

## <a name="setup-the-key-vault-and-keys"></a>Configurar el almacén de claves y claves

   HDInsight solo es compatible con Azure Key Vault. Si tiene su propio almacén de claves, puede importar las claves a Azure Key Vault. Recuerde que las claves deben tener "Eliminación temporal". La característica de "Eliminación temporal" está disponible en el resto, .NET /C#, interfaces de PowerShell y CLI de Azure.

   1. Para crear un nuevo almacén de claves, siga la guía de inicio rápido de [Azure Key Vault](../../key-vault/key-vault-overview.md). Para obtener más información sobre cómo importar claves existentes, visite [Información acerca de claves, secretos y certificados](../../key-vault/about-keys-secrets-and-certificates.md).

   2. Habilite "eliminación temporal" en el almacén de claves utilizando la [actualización de az keyvault](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) comando de cli.
        ''' Actualización del almacén de claves de azure az CLI--nombre <Key Vault Name> --enable-soft-delete
        ```

   3. Create keys

        a. To create a new key, select **Generate/Import** from the **Keys** menu under **Settings**.

        ![Generate a new key in Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png)

        b. Set **Options** to **Generate** and give the key a name.

        ![Generate a new key in Azure Key Vault](./media/apache-kafka-byok/kafka-create-a-key.png)

        c. Select the key you created from the list of keys.

        ![Azure Key Vault key list](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

        d. When you use your own key for Kafka cluster encryption, you need to provide the key URI. Copy the **Key identifier** and save it somewhere until you're ready to create your cluster.

        ![Copy key identifier](./media/apache-kafka-byok/kafka-get-key-identifier.png)
   
    4. Add managed identity to the key vault access policy.

        a. Create a new Azure Key Vault access policy.

        ![Create new Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy.png)

        b. Under **Select Principal**, choose the user-assigned managed identity you created.

        ![Set Select Principal for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

        c. Set **Key Permissions** to **Get**, **Unwrap Key**, and **Wrap Key**.

        ![Set Key Permissions for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

        d. Set **Secret Permissions** to **Get**, **Set**, and **Delete**.

        ![Set Key Permissions for Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

        e. Click on **Save**. 

        ![Save Azure Key Vault access policy](./media/apache-kafka-byok/add-key-vault-access-policy-save.png)

## Create HDInsight cluster

   You're now ready to create a new HDInsight cluster. BYOK can only be applied to new clusters during cluster creation. Encryption can't be removed from BYOK clusters, and BYOK can't be added to existing clusters.

   ![Kafka disk encryption in Azure portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   During cluster creation, provide the full key URL, including the key version. For example, `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. You also need to assign the managed identity to the cluster and provide the key URI.

## Rotating the Encryption key
   There might be scenarios where you might want to change the encryption keys used by the Kafka cluster after it has been created. This can be easily via the portal. For this operation, the cluster must have access to both the current key and the intended new key, otherwise the rotate key operation will fail.

   To rotate the key, you must have the full url of the new key (See Step 3 of [Setup the Key Vault and Keys](#setup-the-key-vault-and-keys)). Once you have that, go to the Kafka cluster properties section in the portal and click on **Change Key** under **Disk Encryption Key URL**. Enter in the new key url and submit to rotate the key.

   ![Kafka rotate disk encryption key](./media/apache-kafka-byok/kafka-change-key.png)

## FAQ for BYOK to Apache Kafka

**How does the Kafka cluster access my key vault?**

   Associate a managed identity with the HDInsight Kafka cluster during cluster creation. This managed identity can be created before or during cluster creation. You also need to grant the managed identity access to the key vault where the key is stored.

**Is this feature available for all Kafka clusters on HDInsight?**

   BYOK encryption is only possible for Kafka 1.1 and above clusters.

**Can I have different keys for different topics/partitions?**

   No, all managed disks in the cluster are encrypted by the same key.

**What happens if the cluster loses access to the key vault or the key?**
   If the cluster loses access to the key, warnings will be shown in the Apache Ambari portal. In this state, the **Change Key** operation will fail. Once key access is restored, Ambari warnings will go away and operations such as key rotation can be successfully performed.

   ![Kafka key access Ambari alert](./media/apache-kafka-byok/kafka-byok-ambari-alert.png)

**How can I recover the cluster if the keys are deleted?**

   Since only “Soft Delete” enabled keys are supported, if the keys are recovered in the key vault, the cluster should regain access to the keys. To recover an Azure Key Vault key, see [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) or [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Can I have producer/consumer applications working with a BYOK cluster and a non-BYOK cluster simultaneously?**

   Yes. The use of BYOK is transparent to producer/consumer applications. Encryption happens at the OS layer. No changes need to be made to existing producer/consumer Kafka applications.

**Are OS disks/Resource disks also encrypted?**

   No. OS disks and Resource disks are not encrypted.

**If a cluster is scaled up, will the new brokers support BYOK seamlessly?**

   Yes. The cluster needs access to the key in the key vault during scale up. The same key is used to encrypt all managed disks in the cluster.

**Is BYOK available in my location?**

   Kafka BYOK is available in all public clouds.

## Next steps

* For more information about Azure Key Vault, see [What is Azure Key Vault](../../key-vault/key-vault-whatis.md)?
* To get started with Azure Key Vault, see [Getting Started with Azure Key Vault](../../key-vault/key-vault-overview.md).
