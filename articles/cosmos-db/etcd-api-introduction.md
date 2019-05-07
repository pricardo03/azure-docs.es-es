---
title: Introducción a la API de Azure Cosmos DB etcd
description: Este artículo proporciona una información general y la clave de los beneficios de etcd API en Azure Cosmos DB
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: 6c7fcb1429438ee024cb226b63cfcdcab05ed9f8
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205804"
---
# <a name="introduction-to-the-azure-cosmos-db-etcd-api-preview"></a>Introducción a la API (versión preliminar) de Azure Cosmos DB etcd

Azure Cosmos DB es el servicio de base de datos multimodelo distribuida globalmente de Microsoft para aplicaciones de misión crítica. Ofrece una distribución global llave en mano, escalado elástica de rendimiento y almacenamiento, latencias de un solo dígito en el percentil 99 y alta disponibilidad garantizada, todo ello respaldada por un SLA de líderes del sector.

[Etcd](https://github.com/etcd-io/etcd) es un almacén distribuido de pares clave-valor. En [Kubernetes](https://kubernetes.io/), etcd se usa para almacenar el estado y la configuración de los clústeres de Kubernetes. Lo que garantiza la disponibilidad, confiabilidad y rendimiento de etcd es crucial para el estado general del clúster, escalabilidad, disponibilidad de elasticidad y rendimiento de un clúster de Kubernetes. 

La API etcd en Azure Cosmos DB le permite usar Azure Cosmos DB como almacén de back-end para [Azure Kubernetes](../aks/index.yml). etcd API en Azure Cosmos DB está actualmente en versión preliminar. Azure Cosmos DB implementa el protocolo de conexión etcd. Con etcd API en Azure Cosmos DB, los desarrolladores obtendrán automáticamente altamente fiables, [disponibles](high-availability.md), [distribuida globalmente](distribute-data-globally.md) Kubernetes. Esta API permite a los desarrolladores escalar la administración del estado de Kubernetes en un servicio de PaaS nativo en la nube totalmente administrado. 

> [!NOTE]
> A diferencia de otras API de Azure Cosmos DB, no se puede aprovisionar una cuenta de API etcd a través del portal de Azure, CLI o SDK. Puede aprovisionar una cuenta de API etcd mediante la implementación de la plantilla de Resource Manager Para obtener instrucciones detalladas, consulte [cómo aprovisionar Azure Kubernetes con Azure Cosmos DB](bootstrap-kubernetes-cluster.md) artículo. API de Azure Cosmos DB etcd está actualmente en versión preliminar limitada. También puede [Regístrese para obtener la versión preliminar](https://aka.ms/cosmosetcdapi-signup), rellenando el formulario de registro.

## <a name="wire-level-compatibility"></a>Compatibilidad de nivel de conexión

Azure Cosmos DB implementa el protocolo de transferencia de etcd versión 3 y permite la [del nodo maestro](https://kubernetes.io/docs/concepts/overview/components/) servidores de API para usar Azure Cosmos DB, tal como lo haría en un entorno etcd instalada localmente. La API etcd admite la autenticación mutua de TLS. 

El siguiente diagrama muestra los componentes de un clúster de Kubernetes. En el patrón del clúster, el servidor de la API usa la API de Azure Cosmos DB etcd, en lugar de etcd instalada localmente. 

![Azure Cosmos DB implementa el protocolo de conexión etcd](./media/etcd-api-introduction/etcd-api-wire-protocol.png)

## <a name="key-benefits"></a>Ventajas principales

### <a name="no-etcd-operations-management"></a>Sin administración de operaciones etcd

Como un servicio totalmente administrado nativas en la nube, Azure Cosmos DB elimina la necesidad de Kubernetes a los desarrolladores configurar y administrar etcd. La API etcd en Azure Cosmos DB es escalable y altamente disponible, tolerante a errores y ofrece un alto rendimiento. La sobrecarga de configurar la replicación en varios nodos, realizar las sucesivas actualizaciones, revisiones de seguridad, y la supervisión del estado etcd se controlan mediante Azure Cosmos DB.

### <a name="global-distribution--high-availability"></a>Alta disponibilidad y distribución global 

Mediante API etcd, Azure Cosmos DB garantiza una disponibilidad del 99,99% para las lecturas de datos y los escribe en una sola región y disponibilidad del 99,999% en varias regiones. 

### <a name="elastic-scalability"></a>Escalabilidad elástica

Azure Cosmos DB ofrece escalabilidad elástica para leer y escribir las solicitudes de diferentes regiones.
A medida que crece el clúster de Kubernetes, la cuenta etcd API en Azure Cosmos DB se amplía elásticamente sin tiempo de inactividad. Almacenamiento de datos de etcd en Azure Cosmos DB, en lugar de los nodos de Kubernetes maestros también permite escalar más flexible de nodo principal. 

### <a name="security--enterprise-readiness"></a>Preparación de la seguridad y empresa

Cuando los datos de etcd se almacenan en Azure Cosmos DB, Kubernetes a los desarrolladores obtienen automáticamente la [cifrado integrado en reposo](database-encryption-at-rest.md), [certificaciones y cumplimiento de normas](compliance.md), y [de copia de seguridad y restauración capacidades](online-backup-and-restore.md) compatibles con Azure Cosmos DB. 

## <a name="next-steps"></a>Pasos siguientes

* [Cómo usar Azure Kubernetes con Azure Cosmos DB](bootstrap-kubernetes-cluster.md)
* [Principales ventajas de Azure Cosmos DB](introduction.md)
* [Guía de inicio rápido AKS motor](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md)