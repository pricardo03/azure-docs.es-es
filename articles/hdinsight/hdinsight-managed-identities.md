---
title: Identidades administradas en Azure HDInsight
description: Proporciona información general de la implementación de las identidades administradas en Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: hrasheed
ms.openlocfilehash: 9386bcb8e455bff5ceed1fccdf55874caf7b6507
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58175809"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Identidades administradas en Azure HDInsight

Una identidad administrada es una identidad registrada en Azure Active Directory (Azure AD) cuyas credenciales se administran mediante Azure. Con las identidades administradas, no es necesario registrar a las entidades de servicio en Azure AD o mantener credenciales, como certificados.

Las identidades administradas pueden utilizarse en Azure HDInsight para permitir que los clústeres para tener acceso a servicios de dominio de Azure AD, acceder a Azure Key Vault o tener acceso a archivos en Azure Data Lake Storage Gen2.

Hay dos tipos de identidades administradas: asignada por el usuario y asignado por el sistema. Azure HDInsight usa identidades administradas asignada por el usuario. Se crea una identidad administrada asignada por el usuario como un recurso de Azure que, a continuación, puede asignar a una o varias instancias de servicio de Azure independiente. En cambio, una identidad administrada asignado por el sistema está creada en Azure AD y, a continuación, se habilita directamente en una instancia de servicio de Azure determinado automáticamente. La vida útil de esa identidad administrada asignado por el sistema, a continuación, se asocia a la vida de la instancia de servicio que se ha habilitado en.

## <a name="hdinsight-managed-identity-implementation"></a>Implementación de la identidad administrada de HDInsight

En HDInsight de Azure, las identidades administradas se aprovisionan en cada nodo del clúster. Estos componentes de identidad, sin embargo, sólo son utilizables por el servicio HDInsight. Actualmente no hay ningún método admitido para generar tokens de acceso con las identidades administradas instaladas en nodos de clúster de HDInsight. Para algunos servicios de Azure, las identidades administradas se implementan con un punto de conexión que puede usar para adquirir tokens de acceso para interactuar con otros servicios de Azure por su cuenta.

## <a name="create-a-managed-identity"></a>Cree una identidad administrada

Las identidades administradas se pueden crear con cualquiera de los métodos siguientes:

* [Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [CLI de Azure](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

Los pasos restantes para configurar la identidad administrada dependen del escenario donde se utilizará.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Escenarios de identidad administrada en Azure HDInsight

Las identidades administradas se usan en HDInsight de Azure en varios escenarios. Consulte los documentos relacionados para el programa de instalación detallada e instrucciones de configuración:

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-managed-identity)
* [Paquete de seguridad de la empresa](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Kafka Bring Your Own Key (BYOK)](kafka/apache-kafka-byok.md#get-started-with-byok)

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué son las identidades administradas de los recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md)