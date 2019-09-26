---
title: Introducción a SAP en máquinas virtuales de Azure | Microsoft Docs
description: Información sobre las soluciones SAP que se ejecutan en máquinas virtuales (VM) en Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/17/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dc1c6e4bf2e67d8945cf754aa47461b4ce8364a9
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71066821"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>Uso de Azure para hospedar y ejecutar escenarios de carga de trabajo de SAP

Cuando utiliza Microsoft Azure, puede ejecutar de forma confiable los escenarios y las cargas de trabajo de SAP críticas en una plataforma compatible, ampliable y de uso demostrado en la empresa. Consigue la escalabilidad, la flexibilidad y el ahorro de costos de Azure. Con la ampliación de la asociación entre Microsoft y SAP, puede ejecutar las aplicaciones SAP en escenarios de desarrollo, pruebas y producción en Azure y serán totalmente compatibles. De SAP NetWeaver a SAP S/4HANA, de BI de SAP en Linux a Windows y de SAP HANA a SQL, lo cubrimos todo.

Además de hospedar escenarios de SAP NetWeaver con los diferentes DBMS en Azure, puede hospedar otros escenarios de cargas de trabajo de SAP, como BI de SAP en Azure. 

La unicidad de Azure para SAP HANA es una oferta que distingue a Azure. Para habilitar el hospedaje de escenarios de SAP con más demanda de recursos de memoria y CPU que implican a SAP HANA, Azure ofrece el uso de hardware de reconstrucción completa dedicado para el cliente. Use esta solución para ejecutar las implementaciones de SAP HANA que requieren hasta 24 TB (120 TB de escalabilidad horizontal) de memoria para S/4HANA u otra carga de trabajo de SAP HANA. 

El hospedaje de escenarios de carga de trabajo de SAP en Azure también puede crear requisitos de integración de identidades y de inicio de sesión único. Esta situación puede producirse al usar Azure Active Directory (Azure AD) para conectar diferentes componentes y ofertas de software como servicio (SaaS) o plataforma como servicio (PaaS) de SAP. En la sección "Integración de identidades de AAD SAP e inicio de sesión único" se describe y documenta una lista de estos escenarios de integración e inicio de sesión único con entidades de Azure AD y SAP.

## <a name="changes-to-the-sap-workload-section"></a>Cambios en la sección de cargas de trabajo de SAP
Al final de este artículo se enumeran los cambios en los documentos de la sección sobre cargas de trabajo de SAP en Azure.


## <a name="sap-hana-on-azure-large-instances"></a>SAP HANA en Azure (instancias grandes)

Una serie de documentos le guían a través de SAP HANA en Azure (instancias grandes) o, para abreviar, las instancias grandes de HANA. Para obtener información acerca de las siguientes áreas de las instancias grandes de HANA, consulte:

- [Introducción de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Infraestructura y conectividad con SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [Instalación de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Supervisión de SAP HANA en Azure (Instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Pasos siguientes:

- Consulte la [Introducción y arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA en máquinas virtuales de Azure
Esta sección de la documentación trata diferentes aspectos de SAP HANA. Antes debe estar familiarizado con los principales servicios de Azure que proporcionan servicios básicos de IaaS de Azure. Por lo tanto, necesita conocer el proceso, almacenamiento y redes de Azure. Muchos de estos temas se tratan en la [guía de planeamiento de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) relacionada con SAP NetWeaver. 

Para obtener información sobre HANA en Azure, consulte los siguientes artículos y sus artículos secundarios:

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

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver implementado en máquinas virtuales de Azure
En esta sección se incluye documentación sobre la planeación e implementación para SAP NetWeaver y Business One en Azure. La documentación se centra en los conceptos básicos y el uso de las bases de datos no perteneciente a HANA con una carga de trabajo de SAP en Azure. Los documentos y los artículos sobre alta disponibilidad también son la base para la alta disponibilidad de HANA en Azure como, por ejemplo:

- [SAP Business One en Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Implementación de SAP IDES EHP7 SP3 para SAP ERP 6.0 en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [Ejecución de SAP NetWeaver en máquinas virtuales de SUSE Linux de Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Implementación y planeamiento de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Implementación de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Protección de la implementación de una aplicación SAP NetWeaver de niveles múltiples mediante Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Conector de SAP LaMa para Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Para obtener información sobre las bases de datos no pertenecientes a HANA bajo una carga de trabajo de SAP en Azure, consulte:

- [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [Implementación de DBMS de Azure Virtual Machines de SQL Server para la carga de trabajo de SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [Implementación de DBMS de Azure Virtual Machines de Oracle para una carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [Implementación de DBMS de Azure Virtual Machines de IBM DB2 para una carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [Implementación de DBMS de Azure Virtual Machines de SAP ASE para una carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Implementación de SAP MaxDB, liveCache y del servidor de contenido en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Para obtener información sobre las bases de datos de SAP HANA en Azure, consulte la sección "SAP HANA en máquinas virtuales de Azure".

Para obtener información sobre la alta disponibilidad de una carga de trabajo de SAP en Azure, consulte:

- [Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Este documento apunta a muchos otros documentos de arquitectura y escenario. En documentos posteriores del escenario, se proporcionan vínculos a documentos técnicos detallados que explican la implementación y configuración de los distintos métodos de alta disponibilidad. Los distintos documentos que muestran cómo establecer y configurar la alta disponibilidad para cargas de trabajo de SAP NetWeaver comprenden a los sistemas operativos Linux y Windows.


Para obtener información sobre la integración entre Azure Active Directory (AAD), los servicios de SAP y el inicio de sesión único, consulte:

- [Tutorial: Integración de Azure Active Directory con SAP Cloud for Customer](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP Cloud Platform Identity Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Integración de Azure Active Directory con SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Your S/4HANA environment: Fiori Launchpad SAML single sign-on with Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/) (Entorno S/4HANA: inicio de sesión único basado en SAML para Fiori Launchpad con Azure AD)

Para obtener información sobre la integración de servicios de Azure en los componentes de SAP, consulte:

- [Uso de SAP HANA en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery y SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Uso del conector SAP BW en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory ofrece integración de datos de SAP HANA y Business Warehouse](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Registro de cambios
- 17/09/2019: Cambio en la guía de planeación e implementación de NetWeaver para unificar los términos en torno a la extensión de máquina virtual para SAP
- 22/08/2019: Cambios en [Configuración de Pacemaker en SUSE Linux Enterprise Server en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) para actualizar las direcciones URL para la creación de roles personalizados.  
- 16/08/2019: Cambios en [Configuración de Pacemaker en Red Hat Enterprise Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) para recordar a los clientes que si se actualizan a la nueva versión del agente de barrera de Azure, actualicen las acciones en el rol personalizado.  
- 15/08/2019: Cambios en [Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) para reflejar la disponibilidad general de discos Ultra (anteriormente SSD Ultra)
- 01/08/2019: Cambios en la [Configuración de Pacemaker en SUSE Linux Enterprise Server en Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) para integrar los cambios específicamente para SLES 15 
- 23/07/2019: Cambios en [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un recurso compartido de archivos en Azure](sap-high-availability-guide-wsfc-file-share.md) para reflejar la compatibilidad de Espacio de almacenamiento directo con Azure Site Recovery Services
- 14/07/2019: Versión de [Grupos de selección de ubicación de proximidad de Azure para una latencia de red óptima con aplicaciones SAP](sap-proximity-placement-scenarios.md)
- 11/07/2019: Cambios en varios documentos que tratan HANA Instancias grandes para cubrir la revisión 4 de HANA Instancias grandes
- 09/07/2019: Versión de la nueva guía para [IBM Db2 HADR en Red Hat Enterprise Server](high-availability-guide-rhel-ibm-db2-luw.md)
- 13/06/2019: Versión de [Alta disponibilidad para SAP NetWeaver en Red Hat Enterprise Linux con Azure NetApp Files para aplicaciones SAP](high-availability-guide-rhel-netapp-files.md)


