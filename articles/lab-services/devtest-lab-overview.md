---
title: Acerca de Azure DevTest Labs | Microsoft Docs
description: Aprenda cómo DevTest Labs puede facilitar la creación, la administración y la supervisión de máquinas virtuales de Azure
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: b7cd6bb1fd0377ca1440d9c667453df922aacbd4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561462"
---
# <a name="about-azure-devtest-labs"></a>Acerca de Azure DevTest Labs
Azure DevTest Labs permite a los desarrolladores en los equipos a administrar eficazmente automáticamente las máquinas virtuales (VM) y los recursos de PaaS sin esperar a las aprobaciones.

DevTest Labs, crean laboratorios que consta de bases de datos configuradas previamente o plantillas de Azure Resource Manager. Estos tienen todas las herramientas necesarias y software que puede usar para crear entornos. Puede crear entornos en unos minutos, en lugar de horas o días.

Mediante el uso de DevTest Labs, puede probar las versiones más recientes de las aplicaciones, realice las siguientes tareas:

- Aprovisionar rápidamente entornos Windows y Linux mediante plantillas y artefactos reutilizables.
- Integre fácilmente la canalización de la implementación con DevTest Labs para aprovisionar entornos a petición.
- Escalar verticalmente su prueba mediante el aprovisionamiento de varios agentes de prueba de carga y creación de entornos aprovisionados previamente para capacitaciones y demostraciones.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]

## <a name="capabilities"></a>Capacidades
DevTest Labs proporciona las siguientes funcionalidades a los desarrolladores que trabajan con máquinas virtuales:

- Crear rápidamente máquinas virtuales con menos de cinco pasos simples.
- Elegir entre una lista exclusiva de las bases de máquina virtual que se configuran, aprobado y autorizado por el responsable de equipo o central de TI.
- Crear máquinas virtuales desde imágenes personalizadas creadas previamente que tienen todo el software y herramientas que ya está instaladas. 
- Crear máquinas virtuales de las fórmulas que son imágenes personalizadas esencialmente combinadas con las compilaciones más recientes del software que se instala cuando se crean las máquinas virtuales. 
- Instalar artefactos que son las extensiones implementadas en máquinas virtuales después de que se están Aprovisionando.
- Establecer el apagado automático y las programaciones en máquinas virtuales de inicio a automático.
- Notificación sin pasar por el proceso de creación de una máquina virtual creada previamente.

DevTest Labs proporciona las siguientes funcionalidades a los desarrolladores que trabajan con entornos de PaaS:

- Uso de Resource Manager para crear rápidamente entornos de PaaS con menos de tres pasos simples.
- Elegir entre una lista exclusiva de las plantillas de Resource Manager, que se configuran y autorizado por el responsable de equipo o centro de TI.
- Ponga en marcha un grupo de recursos vacío (sandbox) mediante una plantilla de Resource Manager para explorar Azure dentro del contexto de un laboratorio.

DevTest Labs también permite central TI para controlar los residuos, optimizar los costos en recursos y mantenerse dentro de los presupuestos, realice las siguientes tareas:  

- Configuración de las programaciones de apagado e inicio automático en las máquinas virtuales.
- Establecimiento de directivas en el número de máquinas virtuales que los usuarios pueden crear.
- Establecimiento de directivas en los tamaños y las imágenes de la galería que los usuarios elegir las máquinas virtuales.
- Seguimiento de los costos y el establecimiento de destinos en laboratorios.
- Permite recibir una notificación en altos costos proyectados para laboratorios para que pueda emprender las acciones necesarias.

DevTest Labs proporciona las siguientes ventajas en la creación, configuración y administración de entornos en la nube.

## <a name="cost-control-and-governance"></a>Gobierno y control de costos
DevTest Labs facilita el control de los costos, ya que permite realizar las tareas siguientes:

- [Establecer directivas en sus laboratorios](devtest-lab-get-started-with-lab-policies.md), como el número de máquinas virtuales por usuario o por laboratorio. 
- Crear [directivas para apagar automáticamente](devtest-lab-set-lab-policy.md) e iniciar las máquinas virtuales.
- Realizar un seguimiento de los costos en recursos de PaaS y máquinas virtuales hilado copia dentro de laboratorios para mantenerse dentro de [su presupuesto](devtest-lab-configure-cost-management.md).
- Permanezca dentro del contexto de los laboratorios, por lo que no pierda los recursos fuera de ella.

## <a name="quickly-get-to-ready-to-test"></a>Obtenga acceso rápidamente al modo "Listo para probar"
DevTest Labs le permite crear entornos aprovisionados previamente equipados con todo que su equipo necesita para desarrollar y probar aplicaciones. Simplemente [notificar los entornos](devtest-lab-add-claimable-vm.md) donde está instalada la última compilación correcta de la aplicación y el trabajo de inicio. O usar contenedores para la creación del entorno aún más rápido, más ligeros.

## <a name="create-once-use-everywhere"></a>Se crean una vez y se utilizan en todas partes
Capture y comparta PaaS [plantillas de entorno](devtest-lab-create-environment-from-arm.md) y [artefactos](add-artifact-repository.md) dentro de su equipo u organización, todo en el control de código fuente, para crear para desarrolladores y fácilmente entornos de prueba.

## <a name="worry-free-self-service"></a>Autoservicio sin preocupaciones
DevTest Labs permite a los desarrolladores y evaluadores a rápida y fácilmente [crear máquinas virtuales de IaaS](devtest-lab-add-vm.md) y [los recursos de PaaS](devtest-lab-create-environment-from-arm.md) mediante el uso de un conjunto de recursos configurados previamente.

## <a name="use-iaas-and-paas-resources"></a>Usar recursos de IaaS y PaaS 
Los desarrolladores pueden también poner en marcha los recursos de PaaS, como clústeres de Azure Service Fabric, la característica Web Apps de Azure App Service y granjas de SharePoint, mediante el uso de plantillas de Resource Manager. Para empezar a trabajar en PaaS en laboratorios, use las plantillas desde el [repositorio público del entorno](devtest-lab-configure-use-public-environments.md) o [conectar el laboratorio a su propio repositorio de Git](devtest-lab-create-environment-from-arm.md#configure-your-own-template-repositories). También puede realizar un seguimiento de los costos en estos recursos para mantenerse dentro de su presupuesto.

## <a name="integrate-with-your-existing-toolchain"></a>Integrar con la cadena de herramientas existente
Use prefabricadas complementos o la API para aprovisionar entornos de desarrollo y pruebas directamente desde su preferido [herramienta de integración continua (CI)](devtest-lab-integrate-ci-cd-vsts.md), entorno de desarrollo (IDE) integrado o canalización de versiones automatizada. También puede usar la herramienta de línea de comandos completa.

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- Para obtener más información sobre DevTest Labs, consulte [conceptos de DevTest Labs](devtest-lab-concepts.md).
- Para ver un tutorial con instrucciones paso a paso, consulte [Tutorial: Configurar un laboratorio con Azure DevTest Labs](tutorial-create-custom-lab.md).