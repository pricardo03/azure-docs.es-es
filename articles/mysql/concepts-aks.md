---
title: Conexión de Azure Kubernetes Service (AKS) con Azure Database for MySQL
description: Aprenda a conectar Azure Kubernetes Service con Azure Database for MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 11/28/2018
ms.openlocfilehash: 54deae9fcf9fdc786aa917bae518a2177a7acaff
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52577186"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-mysql"></a>Conexión de Azure Kubernetes Service y Azure Database for MySQL

Azure Kubernetes Service (AKS) proporciona un clúster de Kubernetes administrado que puede usar en Azure. A continuación se muestran algunas opciones que se deben tener en cuenta al usar AKS y Azure Database for MySQL juntos para crear una aplicación.


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

Con OSBA, puede crear un servidor Azure Database for MySQL y enlazarlo a su clúster de AKS mediante el lenguaje nativo de Kubernetes. Aprenda a usar OSBA y Azure Database for MySQL juntos en la [página de GitHub sobre OSBA](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/mysql.md). 



## <a name="next-steps"></a>Pasos siguientes
- [Creación de un clúster de Azure Kubernetes Service](../aks/kubernetes-walkthrough.md)
- Aprenda a [instalar WordPress desde un gráfico de Helm mediante OSBA y Azure Database for MySQL](../aks/integrate-azure.md).
