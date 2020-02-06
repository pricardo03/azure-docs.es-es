---
title: Cifrado de disco mediante claves administradas por el cliente para Azure HDInsight
description: En este artículo se describe cómo puede usar su propia clave de cifrado desde Azure Key Vault a fin de cifrar los datos almacenados en discos administrados en clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/06/2019
ms.openlocfilehash: b452cb986e6f662aeb33c2a475f18695ebc75745
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846078"
---
# <a name="customer-managed-key-disk-encryption"></a>Cifrado de disco mediante claves administradas por el cliente

Azure HDInsight admite claves administradas por el cliente, lo que también se conoce como cifrado de Bring Your Own Key (BYOK) para los datos de los discos administrados y los discos de recursos conectados a las máquinas virtuales del clúster de HDInsight. Esta característica permite usar Azure Key Vault para administrar las claves de cifrado que protegen los datos en reposo en los clústeres de HDInsight. Los clústeres pueden tener una o varias cuentas de Azure Storage asociadas, en las que el cliente o Microsoft también puede administrar las claves de cifrado, sin embargo, el servicio de cifrado es diferente.

En este documento no se tratan los datos almacenados en la cuenta de Azure Storage. Para obtener más información sobre ASE, consulte [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md).

Todos los discos administrados en HDInsight están protegidos con Azure Storage Service Encryption (SSE). De forma predeterminada, los datos en esos discos se cifran mediante claves administradas por Microsoft. Si habilita las claves administradas por el cliente para HDInsight, debe proporcionar las claves de cifrado para que HDInsight las use y administre mediante Azure Key Vault.

El cifrado de claves administradas por el cliente consiste en un proceso de un paso controlado durante la creación del clúster sin ningún costo adicional. Solo necesita registrar HDInsight como una identidad administrada con Azure Key Vault y agregar la clave de cifrado al crear el clúster.

Tanto el disco de recursos como los discos administrados de cada nodo del clúster están cifrados con una clave de cifrado de datos (DEK) simétrica. La DEK está protegida mediante la clave de cifrado de claves (KEK) del almacén de claves. Azure HDInsight controla completamente los procesos de cifrado y descifrado.

Puede usar Azure Portal o la CLI de Azure para rotar las claves en el almacén de claves de forma segura. Cuando se gira una clave, el clúster de HDInsight comienza a usar la clave nueva en cuestión de minutos. Habilite la característica de protección de claves "Eliminación temporal" para protegerse frente a los escenarios de ransomware y la eliminación accidental. No se admiten almacenes de claves sin esta característica de protección.

## <a name="get-started-with-customer-managed-keys"></a>Introducción a las claves administradas por el cliente

Para crear un clúster de HDInsight habilitado para claves administradas por el cliente, repasaremos los pasos siguientes:

1. Creación de identidades administradas para los recursos de Azure
2. Configuración de Azure Key Vault y las claves
3. Creación de un clúster de HDInsight con la clave administrada por el cliente habilitada
4. Rotación de la clave de cifrado

## <a name="create-managed-identities-for-azure-resources"></a>Creación de identidades administradas para los recursos de Azure

Para autenticarse en Key Vault, cree una identidad administrada asignada por el usuario mediante [Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md) o la [CLI de Azure](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Para obtener más información sobre cómo funcionan las identidades administradas en Azure HDInsight, vea [Identidades administradas en Azure HDInsight](hdinsight-managed-identities.md). Asegúrese de guardar el identificador de recurso de identidad administrada para cuando lo agregue a la directiva de acceso de Key Vault.

## <a name="set-up-the-key-vault-and-keys"></a>Configuración de Key Vault y claves

HDInsight solo es compatible con Azure Key Vault. Si tiene su propio almacén de claves, puede importar las claves a Azure Key Vault. Recuerde que las claves deben tener habilitada la característica "Eliminación temporal". Dicha característica está disponible a través de las interfaces de REST, .NET/C#, PowerShell y la CLI de Azure.

1. Para crear un nuevo almacén de claves, siga la guía de inicio rápido de [Azure Key Vault](../key-vault/key-vault-overview.md). Para obtener más información sobre cómo importar claves existentes, visite [Información acerca de claves, secretos y certificados](../key-vault/about-keys-secrets-and-certificates.md).

1. Habilite la característica "Eliminación temporal" en el almacén de claves mediante el comando [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) de la CLI.

    ```azurecli
    az keyvault update --name <Key Vault Name> --enable-soft-delete
    ```

1. Crear claves.

    a. Para crear una nueva clave, seleccione **Generar/Importar** en el menú **Claves** en **Configuración**.

    ![Generación de una clave nueva en Azure Key Vault](./media/disk-encryption/create-new-key.png "Generar una clave nueva en Azure Key Vault")

    b. Establezca **Opciones** en **Generar** y asigne un nombre a la clave.

    ![generación de un nombre de clave](./media/disk-encryption/create-key.png "Generación de un nombre de clave")

    c. Seleccione la clave que creó en la lista de claves.

    ![lista de claves de Key Vault](./media/disk-encryption/key-vault-key-list.png)

    d. Cuando usa su propia clave para el cifrado del clúster de HDInsight, tiene que proporcionar el URI de la clave. Copie el **identificador de clave** y guárdelo en algún lugar hasta que vaya a crear el clúster.

    ![obtención del identificador de clave](./media/disk-encryption/get-key-identifier.png)

1. Agregue identidades administradas a la directiva de acceso del almacén de claves.

    a. Cree una directiva de acceso de Azure Key Vault.

    ![Creación de una directiva de acceso de Azure Key Vault](./media/disk-encryption/add-key-vault-access-policy.png)

    b. En **Seleccionar entidad de seguridad**, elija la identidad administrada asignada por el usuario que creó.

    ![Establecimiento de la entidad de seguridad para la directiva de acceso de Azure Key Vault](./media/disk-encryption/azure-portal-add-access-policy.png)

    c. Establezca los **Permisos de clave** en **Obtener**, **Desencapsular clave** y **Encapsular clave**.

    ![Establecimiento de los permisos de clave para la directiva de acceso 1 de Azure Key Vault](./media/disk-encryption/add-key-vault-access-policy-keys.png "Establecimiento de los permisos de clave para la directiva de acceso 1 de Azure Key Vault")

    d. Establezca los **Permisos de secretos** en **Obtener**, **Establecer** y **Eliminar**.

    ![Establecimiento de los permisos de clave para la directiva de acceso 2 de Azure Key Vault](./media/disk-encryption/add-key-vault-access-policy-secrets.png "Establecimiento de los permisos de clave para la directiva de acceso 2 de Azure Key Vault")

    e. Seleccione **Guardar**.

    ![Guardar la directiva de acceso de Azure Key Vault](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Creación de un clúster con el cifrado de disco mediante claves administradas por el cliente

Ya está listo para crear un clúster de HDInsight. La clave administrada por el cliente solo puede aplicarse a los clústeres nuevos durante la creación del clúster. No se puede quitar el cifrado de los clústeres de las claves administradas por el cliente y no se puede agregar la clave administrada por el cliente a los clústeres existentes.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Durante la creación del clúster, proporcione la dirección URL completa de la clave, de forma que incluya también la versión de la clave. Por ejemplo, `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. También deberá asignar la identidad administrada al clúster y proporcionar el URI de la clave.

![Creación de un clúster](./media/disk-encryption/create-cluster-portal.png)

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

En el ejemplo siguiente se muestra cómo usar la CLI de Azure para crear un nuevo clúster de Apache Spark con el cifrado de disco habilitado. Consulte la documentación de [az hdinsight create de la CLI de Azure](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) para obtener más información.

```azurecli
az hdinsight create -t spark -g MyResourceGroup -n MyCluster \
-p "HttpPassword1234!" --workernode-data-disks-per-node 2 \
--storage-account MyStorageAccount \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--assign-identity MyMSI
```

## <a name="rotating-the-encryption-key"></a>Rotación de la clave de cifrado

Puede haber escenarios en los que quiera cambiar las claves de cifrado que se usan en el clúster de HDInsight una vez creado. Esta tarea se puede realizar fácilmente en el portal. En esta operación, el clúster debe tener acceso a la clave actual y a la clave nueva deseada; si no, se producirá un error en la operación de rotación de la clave.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Para rotar la clave, debe tener la dirección URL completa de la nueva clave (vea el paso 3 de [Configuración de Key Vault y las claves](#set-up-the-key-vault-and-keys)). Una vez lo haya hecho, vaya a la sección de propiedades del clúster de HDInsight en el portal y haga clic en **Cambiar clave** en **URL de clave de cifrado de disco**. Escriba la dirección URL de la nueva clave y envíela para rotar la clave.

![rotación de la clave de cifrado de disco](./media/disk-encryption/change-key.png)

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

En el ejemplo siguiente se muestra cómo girar la clave de cifrado de disco de un clúster de HDInsight existente. Consulte [az hdinsight rotate-disk-encryption-key de la CLI de Azure](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-rotate-disk-encryption-key) para obtener más detalles.

```azurecli
az hdinsight rotate-disk-encryption-key \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--name MyCluster \
--resource-group MyResourceGroup
```

## <a name="faq-for-customer-managed-key-encryption"></a>Preguntas más frecuentes del cifrado de claves administradas por el cliente

**¿Cómo accede el clúster de HDInsight a mi almacén de claves?**

HDInsight accede a su instancia de Azure Key Vault mediante la identidad administrada que se asocia con el clúster de HDInsight. Esta identidad administrada puede crearse antes o durante la creación del clúster. También debe conceder acceso a la identidad administrada para que pueda usar el almacén de claves donde se almacena la clave.

**¿Esta característica está disponible para todos los clústeres de HDInsight?**

El cifrado de claves administradas por el cliente está disponible para todos los tipos de clúster, excepto Spark 2.1 y 2.2.

**¿Puedo usar varias claves para cifrar diferentes discos o carpetas?**

No, todos los discos administrados y los discos de recursos se cifran con la misma clave.

**¿Qué ocurre si el clúster pierde el acceso al almacén de claves o a la clave?**

Si el clúster pierde acceso a la clave, se mostrarán advertencias en el portal de Apache Ambari. En este estado, la operación **Cambiar clave** dará error. Cuando se restaura el acceso a la clave, las advertencias de Ambari desaparecen y operaciones como la rotación de claves se pueden realizar correctamente.

![alerta de Ambari de acceso a la clave](./media/disk-encryption/ambari-alert.png)

**¿Cómo puedo recuperar el clúster si se eliminan las claves?**

Dado que solo se admiten claves habilitadas para eliminación temporal, si las claves se recuperan en el almacén de claves, el clúster debe volver a acceder a las claves. Para recuperar una clave de Azure Key Vault, consulte [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) o [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**¿Qué tipos de disco están cifrados? ¿Los discos de sistema operativo o de recursos también se cifran?**

Los discos de recursos, los datos y los discos administrados están cifrados. Los discos de sistema operativo no se cifran.

**Si un clúster se escala verticalmente, ¿los nuevos nodos admitirán las claves administradas por el cliente sin problemas?**

Sí. El clúster necesita tener acceso a la clave del almacén de claves durante el escalado vertical. Se usa la misma clave para cifrar los discos administrados y de recursos en el clúster.

**Las claves administradas por el cliente están disponibles en mi ubicación?**

Las claves administradas por el cliente de HDInsight están disponibles en todas las nubes públicas y en las nubes nacionales.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la seguridad de la empresa en Azure HDInsight](./domain-joined/hdinsight-security-overview.md)
