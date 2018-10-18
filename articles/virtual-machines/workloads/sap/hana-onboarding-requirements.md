---
title: Requisitos de incorporación de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Requisitos de incorporación de SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d287f79f4161c509f96b679d4b794c2906f2e020
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44030660"
---
# <a name="onboarding-requirements"></a>Requisitos de incorporación

En esta lista se recopilan los requisitos para ejecutar SAP HANA en Azure (instancias grandes).

**Microsoft Azure**

- Una suscripción de Azure que se pueda vincular a SAP HANA en Azure (Instancias grandes).
- Contrato de soporte técnico Premier de Microsoft. Consulte [SAP Support Note #2015553 – SAP on Microsoft Azure: Support Prerequisites](https://launchpad.support.sap.com/#/notes/2015553) (Nota de SAP sobre soporte 2015553: Requisitos previos de soporte técnico de SAP en Microsoft Azure) para información específica relacionada con la ejecución de SAP en Azure. Si usa unidades de HANA (instancias grandes) con 384 o más CPU, también necesita extender el contrato de soporte técnico Premier para incluir Respuesta rápida de Azure.
- Conocimiento de las SKU de HANA (instancias grandes) que necesita después de realizar un ajuste de tamaño con SAP.

**Conectividad de red**

- ExpressRoute entre la infraestructura local y la de Azure: asegúrese de pedir a su ISP una conexión de 1 Gbps como mínimo para conectar el centro de datos local a Azure. 

**Sistema operativo**

- Licencias para SUSE Linux Enterprise Server 12 for SAP Applications.

   > [!NOTE] 
   > El sistema operativo entregado por Microsoft no está registrado con SUSE. No está conectado a una instancia de la herramienta de administración de suscripciones.

- SUSE Linux Subscription Management Tool implementada en Azure en una máquina virtual. Esta herramienta proporciona la funcionalidad para que SAP HANA en Azure (instancias grandes) sea registrado y actualizado respectivamente por SUSE. (No hay ningún acceso a internet en el centro de datos de HANA [instancias grandes]). 
- Licencias para Red Hat Enterprise Linux 6.7 o 7.x para SAP HANA.

   > [!NOTE]
   > El sistema operativo entregado por Microsoft no está registrado con Red Hat. No está conectado a una instancia del administrador de suscripciones de Red Hat.

- Red Hat Subscription Manager implementado en Azure en una máquina virtual. Red Hat Subscription Manager proporciona la funcionalidad para que SAP HANA en Azure (instancias grandes) sea registrado y actualizado respectivamente por Red Hat. (No hay ningún acceso directo a Internet desde el inquilino implementado en la demarcación de instancias grandes de Azure).
- SAP requiere que también disponga de un contrato de soporte con el proveedor de Linux. Este requisito no se elimina con la solución de HANA (instancias grandes) ni por el hecho de ejecutar Linux en Azure. A diferencia de lo que ocurre con algunas de las imágenes de la galería de Azure para Linux, la tarifa del servicio *no* se incluye en la oferta de la solución HANA (instancias grandes). Es su responsabilidad cumplir los requisitos de SAP en relación con los contratos de soporte técnico con el distribuidor de Linux. 
   - Si se trata de SUSE Linux, busque los requisitos del contrato de soporte técnico en [SAP Note #1984787 - SUSE Linux Enterprise Server 12: Installation notes](https://launchpad.support.sap.com/#/notes/1984787) (Nota de SAP 1984787: SUSE Linux Enterprise Server 12: notas de instalación) y [SAP Note #1056161 - SUSE Priority Support for SAP applications](https://launchpad.support.sap.com/#/notes/1056161) (Nota de SAP 1056161: Soporte con prioridad de SUSE para aplicaciones SAP).
   - Para Red Hat Linux, necesita tener los niveles de suscripción adecuados que incluyan soporte técnico y actualizaciones del servicio de los sistemas operativos de HANA (instancias grandes). Red Hat recomienda la suscripción a Red Hat Enterprise Linux para soluciones de SAP. Consulte https://access.redhat.com/solutions/3082481. 

Para la matriz de compatibilidad de las distintas versiones de SAP HANA con las diferentes versiones de Linux, consulte la [Nota de SAP 2235581](https://launchpad.support.sap.com/#/notes/2235581).

Para la matriz de compatibilidad del sistema operativo y de las versiones de firmware HLI o de controlador, consulte [OS Upgrade for HLI](os-upgrade-hana-large-instance.md) (Actualización del sistema operativo para HLI).


> [!IMPORTANT] 
> Para las unidades de tipo II, en este momento solo se admite la versión del sistema operativo SLES 12 SP2. 


**Base de datos**

- Las licencias y los componentes de instalación de software para SAP HANA (Platform Edition o Enterprise Edition).

**Applications**

- Licencias y componentes de instalación de software para todas las aplicaciones de SAP que se conecten a SAP HANA y contratos de soporte de SAP relacionados.
- Las licencias y componentes de instalación de software para aplicaciones que no sean de SAP utilizadas en relación con el entorno de SAP HANA en Azure (instancias grandes) y los contratos de soporte relacionados.

**Habilidades**

- Experiencia y conocimientos sobre IaaS de Azure y sus componentes.
- Experiencia y conocimientos sobre cómo implementar una carga de trabajo SAP en Azure.
- Personal certificado para la instalación de SAP HANA.
- Conocimientos de la arquitectura de SAP para diseñar la alta disponibilidad y la recuperación ante desastres de SAP HANA.

**SAP**

- Se espera que sea un cliente de SAP y que tenga un contrato de soporte técnico con SAP.
- Especialmente para las implementaciones de las SKU de clase de tipo II de HANA (instancias grandes), consulte con SAP las versiones de SAP HANA y las configuraciones posibles en hardware de escalado vertical de tamaño grande.

**Pasos siguientes**
- Consulte [Arquitectura de SAP HANA (instancias grandes) en Azure](hana-architecture.md).