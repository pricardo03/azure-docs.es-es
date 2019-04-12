---
title: Introducción a SAP en máquinas virtuales de Azure | Microsoft Docs
description: Aprenda sobre las soluciones SAP que se ejecutan en máquinas virtuales (VM) en Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/10/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e07824db683b55cf5827962c1030d6cfd6c9f66
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490436"
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Uso de Azure para hospedar y ejecutar escenarios de carga de trabajo SAP

Al elegir Microsoft Azure, puede ejecutar de forma confiable los escenarios y las cargas de trabajo SAP críticas en una plataforma compatible, ampliable y de uso demostrado en la empresa.  Obtenga la escalabilidad, la flexibilidad y el ahorro de costos de Azure. Con la ampliación de la asociación entre Microsoft y SAP, puede ejecutar las aplicaciones SAP en escenarios de desarrollo y pruebas, y de producción en Azure (y serán totalmente compatibles). De SAP NetWeaver a SAP S/4HANA, BI de SAP, Linux a Windows, SAP HANA a SQL, con cobertura para todo.

Además de hospedar escenarios de SAP NetWeaver con diferentes DBMS en Azure, puede hospedar otros escenarios de cargas de trabajo SAP distintos, como BI de SAP en Azure. 

La unicidad de Azure para SAP HANA es una oferta que distingue a Azure de la competencia. Para habilitar el hospedaje de escenarios SAP que requieren más memoria y más recursos de CPU como SAP HANA, Azure ofrece el uso de hardware de reconstrucción completa dedicado al cliente para fines de ejecución de implementaciones de SAP HANA que necesitan 24 TB (con escalabilidad horizontal de 120 TB) de memoria para S/4HANA u otras cargas de trabajo SAP HANA. Esta solución exclusiva de Azure de SAP HANA en Azure (instancias grandes) permite ejecutar SAP HANA en el hardware de reconstrucción completa dedicado con el nivel de aplicación de SAP o el nivel de middleware de carga de trabajo hospedados en instancias nativas de Azure Virtual Machines. Esta solución se documenta en varios documentos en la sección "SAP HANA en Azure (instancias grandes)".   

Hospedaje de escenarios de carga de trabajo SAP en Azure también puede crear requisitos de integración de identidades y Single-Sign-On con Azure Active Directory a los diferentes componentes SAP y SAP SaaS o PaaS ofrece. Una lista de tales escenarios de integración e inicio de sesión único con entidades de Azure Active Directory (AAD) y SAP se describe y documenta en la sección "Integración de identidades de AAD SAP e inicio de sesión único".

## <a name="latest-changes"></a>Últimos cambios

Versión de [controlar instancias grandes de HANA de Azure a través del portal de Azure](hana-li-portal.md)

Versión de [alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server con Azure Files de NetApp para las aplicaciones de SAP](high-availability-guide-suse-netapp-files.md)

Aclaración en **net.ipv4.tcp_timestamps de parámetro del sistema operativo Linux** equilibrador de carga de configuración junto con Azure

Lanzamiento de [configuraciones de cargas de trabajo de SAP con Azure Availability Zones](sap-ha-availability-zones.md)

Publicación de la [lista de comprobación de planeación e implementación de carga de trabajo de SAP](sap-deployment-checklist.md)




## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA en SAP HANA en Azure (Instancias grandes)

Una serie de artículos de documentación le guía a través de SAP HANA en Azure (instancias grandes) o de forma breve por las instancias grandes de HANA. Los documentos tratan sobre las siguientes áreas de las instancias grandes de HANA:

- [Información general de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Infraestructura y conectividad con SAP HANA en Azure (Instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [Instalación de SAP HANA en SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (Instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Solución de problemas y supervisión de SAP HANA en Azure (Instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Pasos siguientes:

- Consulte la [Introducción y arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA en Azure Virtual Machines
Esta sección de la documentación trata diferentes aspectos de SAP HANA. Como requisito previo, debe estar familiarizado con los servicios principales de Azure que proporcionan servicios básicos de IaaS de Azure, así que principalmente debe conocer el proceso, almacenamiento y redes de Azure. Muchos de estos temas se tratan en la [guía de planeamiento de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) relacionada con SAP NetWeaver. 

La documentación específica para HANA en Azure consta de los siguientes artículos, junto con sus artículos secundarios:

- [Inicio rápido: instalación manual de una única instancia de SAP HANA en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Implementación de SAP S/4HANA o BW/4HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [Configuraciones y operaciones de infraestructura de SAP HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Alta disponibilidad de SAP HANA para máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Disponibilidad de SAP HANA dentro de una región de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Disponibilidad de SAP HANA entre regiones de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Alta disponibilidad de SAP HANA en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Guía de copia de seguridad de SAP HANA en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Azure Backup de SAP HANA en el nivel de archivo](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Copia de seguridad de SAP HANA basada en instantáneas de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver implementadas en Azure Virtual Machines
En esta sección, encontrará documentación de la planeación e implementación para SAP NetWeaver y Business One en Azure. La documentación en este capítulo se centra principalmente en los aspectos básicos y el uso de bases de datos no pertenecientes a HANA con cargas de trabajo de SAP en Azure. Mientras que los documentos y los artículos para la alta disponibilidad también son la base para la alta disponibilidad de HANA en Azure. los artículos son los siguientes:

- [SAP Business One en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Implementación de SAP IDES EHP7 SP3 para SAP ERP 6.0 en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [Ejecución de SAP NetWeaver en máquinas virtuales de SUSE Linux de Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Implementación y planeamiento de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Implementación de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Protección de la implementación de una aplicación SAP NetWeaver de niveles múltiples mediante Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Conector de SAP LaMa para Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Respecto a las bases de datos no pertenecientes a HANA en la carga de trabajo SAP en Azure, los documentos son los siguientes:

- [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [Implementación de DBMS de Azure Virtual Machines de SQL Server para la carga de trabajo de SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [Implementación de DBMS de Azure Virtual Machines de Oracle para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [Implementación de DBMS de Azure Virtual Machines de IBM DB2 para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [Implementación de DBMS de Azure Virtual Machines de SAP ASE para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Implementación de SAP MaxDB, liveCache y del servidor de contenido en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Para las bases de datos de SAP HANA en Azure, consulte la sección de SAP HANA en Azure Virtual Machines.

Para lograr una alta disponibilidad de la carga de trabajo SAP en Azure, el documento de entrada es:

- [Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

El documento de entrada apunta a muchos otros documentos de arquitectura y escenario. En documentos posteriores del escenario, se proporcionan vínculos a documentos técnicos detallados que explican la implementación y configuración de los distintos métodos de alta disponibilidad. Los distintos documentos para establecer y configurar la alta disponibilidad para cargas de trabajo de SAP NetWeaver comprenden a los sistemas operativos Linux y Windows.


Para la integración entre Azure Active Directory (AAD), los servicios de SAP y el inicio de sesión único, los documentos son los siguientes:

- [Tutorial: Integración de Azure Active Directory con SAP Cloud for Customer](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP Cloud Platform Identity Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Su entorno de S/4HANA Fiori Launchpad SAML Single Sign-On con Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Para la integración de los servicios de Azure en los componentes de SAP, los documentos son los siguientes:

- [Uso de SAP HANA en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery y SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Uso del conector SAP BW en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory ofrece integración de datos de SAP HANA y Business Warehouse](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




