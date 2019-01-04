---
title: Conexión de Azure Kubernetes Service (AKS) con Azure Database for PostgreSQL
description: Aprenda a conectar Azure Kubernetes Service con Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.date: 11/27/2018
ms.topic: article
ms.openlocfilehash: 770a583ad09ec1aaa8191f5dafb3db61b2386e68
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166241"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql"></a>Conexión de Azure Kubernetes Service y Azure Database for PostgreSQL

Azure Kubernetes Service (AKS) proporciona un clúster de Kubernetes administrado que puede usar en Azure. A continuación se muestran algunas opciones que se deben tener en cuenta al usar AKS y Azure Database for PostgreSQL juntos para crear una aplicación.


## <a name="accelerated-networking"></a>Redes aceleradas
Use máquinas virtuales subyacentes habilitadas para redes aceleradas en el clúster de AKS. Cuando las redes aceleradas están habilitadas en una máquina virtual, la latencia, la inestabilidad y el uso de CPU son menores en la máquina virtual. Aprenda más sobre cómo funcionan las redes aceleradas, las versiones de SO compatibles y las instancias de máquina virtual admitidas en [Linux](../virtual-network/create-vm-accelerated-networking-cli.md).

A partir de noviembre de 2018, AKS admite redes aceleradas en esas instancias de máquina virtual compatibles. Las redes aceleradas están habilitadas de forma predeterminada en los nuevos clústeres de AKS que usan esas máquinas virtuales.

Puede confirmar si el clúster de AKS tiene redes aceleradas de la manera siguiente:
1. Vaya a Azure Portal y seleccione su clúster de AKS.
2. Seleccione la pestaña Propiedades.
3. Copie el nombre del **grupo de recursos de infraestructura**.
4. Use la barra de búsqueda del portal para buscar y abrir el grupo de recursos de infraestructura.
5. Seleccione una máquina virtual de ese grupo de recursos.
6. Vaya a la pestaña **Redes** de la máquina virtual.
7. Confirme que **Redes aceleradas** esté habilitado.


## <a name="open-service-broker-for-azure"></a>Abrir Service Broker para Azure 
[Open Service Broker para Azure](https://github.com/Azure/open-service-broker-azure/blob/master/README.md) (OSBA) le permite aprovisionar servicios de Azure directamente desde Kubernetes o Cloud Foundry. Es una implementación de [Open Service Broker API](https://www.openservicebrokerapi.org/) para Azure.

Con OSBA, puede crear un servidor Azure Database for PostgreSQL y enlazarlo a su clúster de AKS mediante el lenguaje nativo de Kubernetes. Aprenda a usar OSBA y Azure Database for PostgreSQL juntos en la [página de GitHub sobre OSBA](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md). 


## <a name="connection-pooling"></a>Agrupación de conexiones
Un concentrador de conexión minimiza el costo y el tiempo necesarios para la creación y el cierre de nuevas conexiones a la base de datos. El grupo es una colección de conexiones que se pueden reutilizar. 

Existen varios concentradores de conexión que puede usar con PostgreSQL. Uno de ellos es [PgBouncer](https://pgbouncer.github.io/). En Microsoft Container Registry, proporcionamos un PgBouncer en contenedores ligero que puede usarse en un sidecar para agrupar conexiones de AKS a Azure Database for PostgreSQL. Visite la [página de Docker Hub](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/) para obtener información acerca de cómo acceder a esta imagen y usarla. 


## <a name="next-steps"></a>Pasos siguientes
-  [Creación de un clúster de Azure Kubernetes Service](../aks/kubernetes-walkthrough.md)
