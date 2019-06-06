---
title: Soluciones de Oracle en Microsoft Azure | Microsoft Docs
description: Obtenga información acerca de las opciones para implementar aplicaciones de Oracle y soluciones en Microsoft Azure, incluidos los que se ejecuta completamente en la infraestructura de Azure o con conectividad entre la nube con infraestructura de nube de Oracle (OCI).
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.openlocfilehash: b5e40975fca491f289d949ee273d13053897fc6d
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743641"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Información general de las aplicaciones de Oracle y las soluciones en Azure

Este artículo presentan las capacidades para ejecutar soluciones de Oracle mediante la infraestructura de Azure. Vea también una introducción detallada a disponible [imágenes de máquina virtual de Oracle](oracle-vm-solutions.md) en Azure Marketplace y la capacidad de vista previa [interconexión con infraestructura de nube de Oracle (OCI) de Azure](oracle-oci-overview.md).

## <a name="oracle-databases-on-azure-infrastructure"></a>Bases de datos de Oracle en la infraestructura de Azure

Ejecute las bases de datos de Oracle en la infraestructura de Azure con imágenes de Linux disponibles en Azure Marketplace:

* Oracle Database 12.1 12.2 y 18,3 Enterprise Edition 

* Oracle Database 18,3, 12.2 y 12.1 Standard Edition 

También puede basar una solución en una imagen personalizada que cree desde cero en Azure o cargue una imagen personalizada desde el entorno local.

Opcionalmente, configure con varios discos conectados y mejorar el rendimiento de la base de datos mediante la instalación de Oracle Automated Storage Management (ASM).

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Aplicaciones de Linux de Oracle y WebLogic Server

Ejecutar aplicaciones empresariales en Azure en sistemas operativos de Oracle. Las siguientes imágenes están disponibles en Azure Marketplace:

* Oracle WebLogic Server 12.1.2

* Oracle Linux (UEK), 6.8, 6.9, 6.10, 7.3, 7.4, 7.5 y 7.6

## <a name="high-availability-and-disaster-recovery-options"></a>Opciones de recuperación ante desastres y disponibilidad alta

* Configurar Oracle Data Guard, protección de datos activa o GoldenGate en infraestructura de Azure junto con [zonas de disponibilidad](../../../availability-zones/az-overview.md) para lograr alta disponibilidad.

* Use [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) para organizar y administrar la recuperación ante desastres para las máquinas virtuales Linux de Oracle en Azure y sus locales o servidores físicos. 

* Habilitar Oracle Real Application Clusters (RAC) en Azure mediante [FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="integration-of-azure-with-oci-preview"></a>Integración de Azure con OCI (versión preliminar)

Ejecutar aplicaciones de Oracle en la infraestructura de Azure, conectado a las bases de datos back-end en infraestructura de nube de Oracle (OCI). Esta solución usa las siguientes funcionalidades: 

* **La nube entre redes** -uso la directa de interconexión entre Azure ExpressRoute y Oracle FastConnect para establecer conexiones de gran ancho de banda, privadas y baja latencia entre la aplicación y la capa de base de datos.
* **Integra identidad** -configuración de identidad federada entre Azure AD y IDCS de Oracle para crear un origen de la identidad única para las soluciones. Habilitar el inicio de sesión único administrar los recursos en OCI y Azure.

### <a name="deploy-oracle-applications-on-azure"></a>Implementar aplicaciones de Oracle en Azure

Utilice las plantillas de Terraform para configurar la infraestructura de Azure e instalar aplicaciones de Oracle para ejecutarse con la configuración de toda la nube validado y admite:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Aplicaciones comerciales de Oracle
* Administración financiera de Oracle Hyperion

También implementar aplicaciones personalizadas en Azure que se conectan con OCI y otros servicios de Azure.

### <a name="set-up-oracle-databases-in-oci"></a>Configurar bases de datos Oracle OCI

Usar servicios de nube de base de datos de Oracle (base de datos autónomos, RAC, Exadata, DBaaS, nodo único) junto con aplicaciones de Oracle que se ejecutan en Azure. Obtenga más información sobre [opciones de base de datos OCI](https://docs.cloud.oracle.com/iaas/Content/Database/Concepts/databaseoverview.htm). 
 

## <a name="licensing"></a>Licencias

Implementación de aplicaciones de Oracle en Azure se basa en un modelo "traiga su propia licencia". Se supone que tiene la licencia correcta para utilizar software de Oracle y que tiene un contrato de soporte actual con Oracle. Oracle ha garantizado la movilidad de licencias del entorno local a Azure. Ver el Azure Oracle [preguntas más frecuentes sobre](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html).

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de la implementación [imágenes de máquina virtual de Oracle](oracle-vm-solutions.md) en infraestructura de Azure.

* Más información acerca de cómo [interconexión de Azure con OCI](oracle-oci-overview.md).