---
title: Aporte su propia clave para Apache Kafka en Azure HDInsight (versión preliminar)
description: En este artículo se describe el método para que use su propia clave desde Azure Key Vault para cifrar los datos almacenados en Apache Kafka en Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 26e4b921b4050efa5217e3b599b9dc942a003090
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58173930"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight-preview"></a>Aporte su propia clave para Apache Kafka en Azure HDInsight (versión preliminar)

Azure HDInsight incluye compatibilidad con Bring Your Own Key (BYOK) para Apache Kafka. Esta funcionalidad le permite ser el propietario y administrar las claves utilizadas para cifrar datos en reposo. 

Todos los discos administrados en HDInsight están protegidos con Azure Storage Service Encryption (SSE). De forma predeterminada, los datos en esos discos se cifran mediante claves administradas por Microsoft. Al habilitar BYOK, le proporciona a HDInsight la clave de cifrado para que la use y administre con Azure Key Vault. 

El cifrado de BYOK es un proceso de un paso controlado durante la creación del clúster sin ningún costo adicional. Solo necesita registrar HDInsight como una identidad administrada con Azure Key Vault y agregar la clave de cifrado al crear el clúster.

Todos los mensajes en el clúster de Kafka (incluidas las réplicas mantenidas por Kafka) se cifran con una clave de cifrado de datos (DEK) simétrica. La DEK está protegida mediante la clave de cifrado de claves (KEK) del almacén de claves. Azure HDInsight controla completamente los procesos de cifrado y descifrado. 

Puede usar Azure Portal o la CLI de Azure para rotar las claves en el almacén de claves de forma segura. Cuando se rota una clave, el clúster de HDInsight Kafka comienza a usar la clave nueva en cuestión de minutos. Habilite las características de protección de claves "No purgar" y "Eliminación temporal" para protegerse frente a los escenarios de ransomware y la eliminación accidental. No se admiten claves sin estas funciones de protección.

## <a name="get-started-with-byok"></a>Introducción a BYOK

1. Cree identidades administradas para los recursos de Azure.

   Para autenticarse en Key Vault, cree una identidad administrada asignada por el usuario utilizando el [portal Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md), o [ CLI de Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Para obtener más información sobre cómo administrado trabajo de identidades en Azure HDInsight, consulte [administra identidades en Azure HDInsight](../hdinsight-managed-identities.md). Azure Active Directory es un requisito para las identidades administradas y BYOK a Kafka, pero Enterprise Security Package no lo es. Asegúrese de guardar el identificador de recurso de identidad administrada para cuando lo agregue a la directiva de acceso de Key Vault.

   ![Creación de una identidad administrada asignada por el usuario en Azure Portal](./media/apache-kafka-byok/user-managed-identity-portal.png)

2. Importe un almacén de claves existente o cree uno nuevo.

   HDInsight solo es compatible con Azure Key Vault. Si tiene su propio almacén de claves, puede importar las claves a Azure Key Vault. Recuerde que las claves deben tener habilitadas las opciones "Eliminación temporal" y "No purgar". Las características "Eliminación temporal" y "No purgar" están disponibles a través de las interfaces de REST, .NET/C#, PowerShell y la CLI de Azure.

   Para crear un nuevo almacén de claves, siga la guía de inicio rápido de [Azure Key Vault](../../key-vault/key-vault-overview.md). Para obtener más información sobre cómo importar claves existentes, visite [Información acerca de claves, secretos y certificados](../../key-vault/about-keys-secrets-and-certificates.md).

   Para crear una nueva clave, seleccione **Generar/Importar** en el menú **Claves** en **Configuración**.

   ![Generar una clave nueva en Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png)

   Establezca **Opciones** en **Generar** y asigne un nombre a la clave.

   ![Generar una clave nueva en Azure Key Vault](./media/apache-kafka-byok/kafka-create-a-key.png)

   Seleccione la clave que creó en la lista de claves.

   ![Lista de claves de Azure Key Vault](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

   Cuando usa su propia clave de cifrado del clúster de Kafka, tiene que proporcionar la URI de la clave. Copie el **identificador de clave** y guárdelo en algún lugar hasta que vaya a crear el clúster.

   ![Copiar el identificador de clave](./media/apache-kafka-byok/kafka-get-key-identifier.png)
   
3. Agregue identidades administradas a la directiva de acceso del almacén de claves.

   Cree una directiva de acceso de Azure Key Vault.

   ![Creación de una directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy.png)

   En **Seleccionar entidad de seguridad**, elija la identidad administrada asignada por el usuario que creó.

   ![Establecimiento de la entidad de seguridad para la directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

   Establezca los **Permisos de clave** en **Obtener**, **Desencapsular clave** y **Encapsular clave**.

   ![Establecimiento de los permisos de clave para la directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

   Establezca los **Permisos de secretos** en **Obtener**, **Establecer** y **Eliminar**.

   ![Establecimiento de los permisos de clave para la directiva de acceso de Azure Key Vault](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

4. Creación de un clúster de HDInsight

   Ya está listo para crear un clúster de HDInsight. BYOK sólo puede aplicarse a los clústeres nuevos durante la creación del clúster. No se puede quitar el cifrado de los clústeres BYOK, y no puede agregar BYOK a los clústeres existentes.

   ![Cifrado de disco de Kafka en Azure Portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   Durante la creación del clúster, proporcione la dirección URL completa de la clave, de forma que incluya también la versión de la clave. Por ejemplo, `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. También deberá asignar la identidad administrada al clúster y proporcionar el URI de la clave.

## <a name="faq-for-byok-to-apache-kafka"></a>Preguntas más frecuentes de BYOK a Apache Kafka

**¿Cómo accede el clúster de Kafka a mi almacén de claves?**

   Asocie una identidad administrada con el clúster de Kafka en HDInsight durante la creación del clúster. Esta identidad administrada puede crearse antes o durante la creación del clúster. También debe conceder acceso a la identidad administrada para que pueda usar el almacén de claves donde se almacena la clave.

**¿Esta característica está disponible para todos los clústeres de Kafka en HDInsight?**

   Solo los clústeres para Kafka 1.1 y versiones posteriores cuentan con el cifrado BYOK.

**¿Puedo tener claves diferentes para distintos temas o particiones?**

   No, todos los discos administrados en el clúster se cifran con la misma clave.

**¿Cómo puedo recuperar el clúster si se eliminan las claves?**

   Dado que se admiten solo las claves habilitadas para la "Eliminación temporal", si las claves se restauran en el almacén de claves, el clúster debe volver a obtener acceso a las claves. Para restaurar una clave de Azure Key Vault, consulte [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey).

**¿Las aplicaciones de productor/consumidor pueden trabajar simultáneamente con un clúster BYOK y un clúster sin BYOK?**

   Sí. El uso de BYOK es transparente para las aplicaciones de productor/consumidor. El cifrado se produce en el nivel del sistema operativo. No es necesario realizar cambios en las aplicaciones de productor/consumidor de Kafka existentes.

**¿Los discos de sistema operativo o los de recursos también se cifran?**

    No. No se cifran los discos del sistema operativo ni los discos de recursos.

**¿Si un clúster se escala verticalmente, los agentes nuevos admitirán BYOK a la perfección?**

   Sí. El clúster necesita tener acceso a la clave del almacén de claves durante el escalado vertical. Se usa la misma clave para cifrar todos los discos administrados en el clúster.

**¿BYOK está disponible en mi ubicación?**

   BYOK Kafka está disponible en todas las nubes públicas.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../../key-vault/key-vault-whatis.md)
* Para empezar a trabajar con Azure Key Vault, consulte [Introducción a Azure Key Vault](../../key-vault/key-vault-overview.md).
