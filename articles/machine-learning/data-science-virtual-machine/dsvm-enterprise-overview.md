---
title: Introducción a entornos de equipos basados en Data Science Virtual Machine | Microsoft Docs
description: Patrones para implementar Data Science VM como entorno de equipos de empresa.
keywords: aprendizaje profundo, AI, herramientas de ciencia de datos, data science virtual machine, análisis geoespacial, proceso de ciencia de datos en equipo
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 8486b0be1fb5e1385da3c7ad55f6410a1059df93
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309255"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Entorno de análisis de equipo e IA basado en Data Science Virtual Machine 
[Data Science Virtual Machine](overview.md) (DSVM) ofrece un completo entorno en la plataforma Azure con software pregenerado de inteligencia artificial (IA) y análisis de datos. 

Tradicionalmente, DSVM se ha usado como un escritorio de análisis individuales. Los científicos de datos individuales obtienen productividad con esta noción compartida de su entorno de análisis creado previamente. Dado que los equipos de análisis grandes planean sus entornos de análisis para sus científicos de datos y desarrolladores de IA, uno de los temas recurrentes es una infraestructura de análisis compartida para desarrollo y experimentación. Esta infraestructura se administra en línea con las directivas de TI empresariales que también facilitan la colaboración y la coherencia entre los equipos de ciencia o análisis de datos. 

Una infraestructura compartida también permite al equipo de TI usar mejor el entorno de análisis. Algunas organizaciones denominan a la infraestructura de ciencia o análisis de datos basada en el equipo "espacio aislado de análisis". Permite a los científicos de datos acceder a distintos recursos de datos para entender los datos, ejecutar experimentos, validar hipótesis y generar modelos predictivos rápidamente, sin que ello afecte al entorno de producción. 

Dado que DSVM funciona en el nivel de infraestructura de Azure, los administradores de TI pueden configurar con facilidad DSVM para que funcione conforme a las directivas de TI de la empresa. DSVM ofrece total flexibilidad en la implementación de distintas arquitecturas de uso compartido con acceso a los recursos de datos corporativos de forma controlada. 

En esta sección se describen algunos patrones e instrucciones que pueden utilizarse para implementar la máquina virtual de ciencia de datos como una infraestructura de ciencia de datos basada en equipos. Los bloques de creación de estos patrones proceden de la infraestructura como servicio (IaaS) de Azure, por lo que se aplican a todas las máquinas virtuales de Azure. En esta serie de artículos, se pone de relieve la aplicación de estas capacidades de infraestructura de Azure estándar a Data Science VM. 

Algunos de los principales bloques de creación de un entorno de análisis de equipo empresarial son:

* [Grupo autoescalado de máquinas virtuales de Data Science Virtual Machine](dsvm-pools.md)
* [Identidad común y acceso a un área de trabajo desde cualquier máquina virtual DSVM del grupo](dsvm-common-identity.md)
* [Acceso seguro a orígenes de datos](dsvm-secure-access-keys.md)


En esta serie de artículos, se proporcionan instrucciones y punteros para cada uno de los elementos anteriores. No se tratan todas las consideraciones y necesidades de implementación de DSVM en configuraciones empresariales de gran tamaño. A continuación se incluye más documentación de Azure que puede usar para la implementación de instancias de DSVM de su empresa: 

* [Seguridad de las redes](https://docs.microsoft.com/azure/security/azure-network-security)
* [Supervisión](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) y [administración](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Registro y auditoría](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Control de acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Cumplimiento y configuración de directivas](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Cifrado](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Gobernanza y detección de datos](https://docs.microsoft.com/azure/data-catalog/)

El [centro de arquitectura de Azure](https://docs.microsoft.com/en-us/azure/architecture/) proporciona una arquitectura detallada de un extremo a otro y patrones para compilar y administrar la infraestructura de análisis basada en la nube. 
