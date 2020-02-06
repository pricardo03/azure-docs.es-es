---
title: Método Bring Your Own Key para Apache Kafka en Azure HDInsight
description: En este artículo se describe el método para que use su propia clave desde Azure Key Vault para cifrar los datos almacenados en Apache Kafka en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 72fd23e4283925b91d749fef0afac4e87e93405c
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841691"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight"></a>Método Bring Your Own Key para Apache Kafka en Azure HDInsight

Azure HDInsight incluye compatibilidad con Bring Your Own Key (BYOK) para Apache Kafka. Esta funcionalidad le permite ser el propietario y administrar las claves utilizadas para cifrar datos en reposo.

Todos los discos administrados en HDInsight están protegidos con Azure Storage Service Encryption (SSE). De forma predeterminada, los datos en esos discos se cifran mediante claves administradas por Microsoft. Al habilitar BYOK, le proporciona a HDInsight la clave de cifrado para que la use y administre con Azure Key Vault.

El cifrado de BYOK es un proceso de un paso controlado durante la creación del clúster sin ningún costo adicional. Solo necesita registrar HDInsight como una identidad administrada con Azure Key Vault y agregar la clave de cifrado al crear el clúster.

Todos los mensajes en el clúster de Kafka (incluidas las réplicas mantenidas por Kafka) se cifran con una clave de cifrado de datos (DEK) simétrica. La DEK está protegida mediante la clave de cifrado de claves (KEK) del almacén de claves. Azure HDInsight controla completamente los procesos de cifrado y descifrado.

Puede usar Azure Portal o la CLI de Azure para rotar las claves en el almacén de claves de forma segura. Cuando se rota una clave, el clúster de HDInsight Kafka comienza a usar la clave nueva en cuestión de minutos. Habilite la característica de protección de claves "Eliminación temporal" para protegerse frente a los escenarios de ransomware y la eliminación accidental. No se admiten almacenes de claves sin esta característica de protección.

## <a name="get-started-with-byok"></a>Introducción a BYOK

Para crear un clúster de Kafka habilitado para BYOK, realizaremos los pasos siguientes:

1. Creación de identidades administradas para los recursos de Azure
2. Configuración de Azure Key Vault y las claves
3. Creación de clúster de Kafka HDInsight con BYOK habilitado
4. Rotación de la clave de cifrado

## <a name="create-managed-identities-for-azure-resources"></a>Creación de identidades administradas para los recursos de Azure

Para autenticarse en Key Vault, cree una identidad administrada asignada por el usuario mediante [Azure Portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md) o la [CLI de Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Para obtener más información sobre cómo funcionan las identidades administradas en Azure HDInsight, vea [Identidades administradas en Azure HDInsight](../hdinsight-managed-identities.md). Azure Active Directory es un requisito para las identidades administradas y BYOK a Kafka, pero Enterprise Security Package no lo es. Asegúrese de guardar el identificador de recurso de identidad administrada para cuando lo agregue a la directiva de acceso de Key Vault.

![Creación de una identidad administrada asignada por el usuario en Azure Portal](./media/apache-kafka-byok/azure-portal-create-managed-identity.png)

## <a name="set-up-the-key-vault-and-keys"></a>Configuración de Key Vault y claves

HDInsight solo es compatible con Azure Key Vault. Si tiene su propio almacén de claves, puede importar las claves a Azure Key Vault. Recuerde que las claves deben tener habilitada la característica "Eliminación temporal". Dicha característica está disponible a través de las interfaces de REST, .NET/C#, PowerShell y la CLI de Azure.

1. Para crear un nuevo almacén de claves, siga la guía de inicio rápido de [Azure Key Vault](../../key-vault/quick-create-cli.md). Para obtener más información sobre cómo importar claves existentes, visite [Información acerca de claves, secretos y certificados](../../key-vault/about-keys-secrets-and-certificates.md).

1. Habilite la característica "Eliminación temporal" en el almacén de claves mediante el comando [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) de la CLI.

    ```azurecli
    az keyvault update --name <Key Vault Name> --enable-soft-delete
    ```

1. Crear claves.

    a. Para crear una nueva clave, seleccione **Generar/Importar** en el menú **Claves** en **Configuración**.

    ![Generación de una clave nueva en Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png "Generar una clave nueva en Azure Key Vault")

    b. Establezca **Opciones** en **Generar** y asigne un nombre a la clave.

    ![Apache Kafka genera el nombre de la clave](./media/apache-kafka-byok/apache-kafka-create-key.png "Generación de un nombre de clave")

    c. Seleccione la clave que creó en la lista de claves.

    ![Apache Kafka: lista de claves del almacén de claves](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

    d. Cuando usa su propia clave de cifrado del clúster de Kafka, tiene que proporcionar la URI de la clave. Copie el **identificador de clave** y guárdelo en algún lugar hasta que vaya a crear el clúster.

    ![Apache Kafka: obtener el identificador de la clave](./media/apache-kafka-byok/kafka-get-key-identifier.png)

1. Agregue identidades administradas a la directiva de acceso del almacén de claves.

    a. Cree una directiva de acceso de Azure Key Vault.

    ![Creación de una directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy.png)

    b. En **Seleccionar entidad de seguridad**, elija la identidad administrada asignada por el usuario que creó.

    ![Establecimiento de la entidad de seguridad para la directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/azure-portal-add-access-policy.png)

    c. Establezca los **Permisos de clave** en **Obtener**, **Desencapsular clave** y **Encapsular clave**.

    ![Establecimiento de los permisos de clave para la directiva de acceso 1 de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png "Establecimiento de los permisos de clave para la directiva de acceso 1 de Azure Key Vault")

    d. Establezca los **Permisos de secretos** en **Obtener**, **Establecer** y **Eliminar**.

    ![Establecimiento de los permisos de clave para la directiva de acceso 2 de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png "Establecimiento de los permisos de clave para la directiva de acceso 2 de Azure Key Vault")

    e. Seleccione **Guardar**.

    ![Guardar la directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy-save.png)

## <a name="create-hdinsight-cluster"></a>Creación de un clúster de HDInsight

Ya está listo para crear un clúster de HDInsight. BYOK sólo puede aplicarse a los clústeres nuevos durante la creación del clúster. No se puede quitar el cifrado de los clústeres BYOK, y no puede agregar BYOK a los clústeres existentes.

![Cifrado de disco de Kafka en Azure Portal](./media/apache-kafka-byok/azure-portal-cluster-security-networking-kafka.png)

Durante la creación del clúster, proporcione la dirección URL completa de la clave, de forma que incluya también la versión de la clave. Por ejemplo, `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. También deberá asignar la identidad administrada al clúster y proporcionar el URI de la clave. Para obtener todos los detalles sobre la creación de clústeres, consulte [Creación de clústeres de Apache Hadoop mediante Azure Portal](./apache-kafka-get-started.md)

## <a name="rotating-the-encryption-key"></a>Rotación de la clave de cifrado

Puede haber escenarios en los que quiera cambiar las claves de cifrado que se usan en el clúster de Kafka una vez creado. Esta tarea se puede realizar fácilmente en el portal. En esta operación, el clúster debe tener acceso a la clave actual y a la clave nueva deseada; si no, se producirá un error en la operación de rotación de la clave.

Para rotar la clave, debe tener la dirección URL completa de la nueva clave (vea el paso 3 de [Configuración de Key Vault y las claves](#set-up-the-key-vault-and-keys)). Cuando lo tenga, vaya a la sección de propiedades del clúster de Kafka en el portal y haga clic en **Cambiar clave** en **URL de clave de cifrado de disco**. Escriba la dirección URL de la nueva clave y envíela para rotar la clave.

![Rotación de la clave de cifrado de disco de Kafka](./media/apache-kafka-byok/apache-kafka-change-key.png)

## <a name="faq-for-byok-to-apache-kafka"></a>Preguntas más frecuentes de BYOK a Apache Kafka

**¿Cómo accede el clúster de Kafka a mi almacén de claves?**

Asocie una identidad administrada con el clúster de Kafka en HDInsight durante la creación del clúster. Esta identidad administrada puede crearse antes o durante la creación del clúster. También debe conceder acceso a la identidad administrada para que pueda usar el almacén de claves donde se almacena la clave.

**¿Esta característica está disponible para todos los clústeres de Kafka en HDInsight?**

Solo los clústeres para Kafka 1.1 y versiones posteriores cuentan con el cifrado BYOK.

**¿Puedo tener claves diferentes para distintos temas o particiones?**

No, todos los discos administrados en el clúster se cifran con la misma clave.

**¿Qué ocurre si el clúster pierde el acceso al almacén de claves o a la clave?**

Si el clúster pierde acceso a la clave, se mostrarán advertencias en el portal de Apache Ambari. En este estado, la operación **Cambiar clave** dará error. Cuando se restaura el acceso a la clave, las advertencias de Ambari desaparecen y operaciones como la rotación de claves se pueden realizar correctamente.

![Apache Kafka: alerta de Ambari de acceso a la clave](./media/apache-kafka-byok/kafka-byok-ambari-alert.png)

**¿Cómo puedo recuperar el clúster si se eliminan las claves?**

Dado que solo se admiten claves habilitadas para eliminación temporal, si las claves se recuperan en el almacén de claves, el clúster debe volver a acceder a las claves. Para recuperar una clave de Azure Key Vault, consulte [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) o [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**¿Las aplicaciones de productor/consumidor pueden trabajar simultáneamente con un clúster BYOK y un clúster sin BYOK?**

Sí. El uso de BYOK es transparente para las aplicaciones de productor/consumidor. El cifrado se produce en el nivel del sistema operativo. No es necesario realizar cambios en las aplicaciones de productor/consumidor de Kafka existentes.

**¿Los discos de sistema operativo o los de recursos también se cifran?**

No. No se cifran los discos del sistema operativo ni los discos de recursos.

**¿Si un clúster se escala verticalmente, los agentes nuevos admitirán BYOK a la perfección?**

Sí. El clúster necesita tener acceso a la clave del almacén de claves durante el escalado vertical. Se usa la misma clave para cifrar todos los discos administrados en el clúster.

**¿BYOK está disponible en mi ubicación?**

BYOK Kafka está disponible en todas las nubes públicas.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md)
* Para empezar a trabajar con Azure Key Vault, consulte [Introducción a Azure Key Vault](../../key-vault/key-vault-overview.md).
