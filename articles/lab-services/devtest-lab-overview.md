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
ms.openlocfilehash: 599b4325e0eb9aa7f6ccca2626c6d4b14f38149d
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339336"
---
# <a name="about-azure-devtest-labs"></a>Acerca de Azure DevTest Labs
Azure DevTest Labs es un servicio que permite a los desarrolladores en un equipo para las máquinas virtuales de forma eficaz autoservicio o los recursos de PaaS que necesitan para desarrollar, probar, formación y demostraciones, etc. sin tener que esperar para aprobaciones constantes en las herramientas que necesitan. 

El laboratorio ya consta de bases de datos configuradas previamente o plantillas de Resource Manager con todas las herramientas necesarias y software que los desarrolladores pueden usar para crear entornos. Los desarrolladores pueden crear sus entornos en unos minutos en lugar de horas o días. 

Uso de DevTest Labs, puede probar la versión más reciente de la aplicación, realice las siguientes tareas:

- Aprovisionar rápidamente entornos Windows y Linux con artefactos y plantillas reutilizables
- Integre fácilmente la canalización de implementación con DevTest Labs para aprovisionar entornos bajo demanda
- Escalar verticalmente su prueba mediante el aprovisionamiento de varios agentes de prueba de carga y creación de entornos aprovisionados previamente para capacitaciones y demostraciones.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Capacidades
DevTest Labs proporciona las siguientes funcionalidades a los desarrolladores que trabajan con máquinas virtuales:

- Crear una máquina virtual rápidamente con menos de cinco pasos simple.
- Elegir entre una lista exclusiva de las bases de máquina virtual que se configuran, aprobado y autorizado por el responsable de equipo o central de TI.
- Cree las máquinas de imágenes personalizadas creadas previamente que tienen todo el software y herramientas instaladas en la imagen. 
- Cree las máquinas de fórmulas que son básicamente las imágenes personalizadas y una compilación más reciente del software que está instalado en el momento de creación de la máquina virtual.
- Instalar artefactos que son las extensiones implementadas en la máquina virtual después de su aprovisionamiento.
- Configuración de cierre automático y auto programaciones de inicio en los equipos para tener cierre al final del día y, a continuación, hasta y ejecutar por la mañana siguiente.
- Simplemente puede reclamar una máquina virtual creada previamente sin tener que pasar por el proceso de creación de la máquina. 

DevTest Labs proporciona las siguientes funcionalidades a los desarrolladores que trabajan con entornos de PaaS:

- Cree entornos de PaaS de Azure Resource Manager rápidamente siguiendo menos de tres pasos sencillos.
- Elija una en la lista de plantillas de Resource Manager, que están configurados, aprobado y autorizado por el responsable de equipo o central de TI.
- Ponga en marcha un grupo de recursos vacío (Sandbox) mediante una plantilla de Resource Manager para explorar todo Azure manteniéndose dentro del contexto del laboratorio todavía.
- DevTest Labs proporciona las siguientes ventajas en la creación, la configuración y la administración de entornos de desarrollador y prueba en la nube

Además de un modelo de autoservicio para desarrolladores en un equipo, el servicio permite central TI para controlar residuos, optimizar los costos en recursos y mantenerse dentro de los presupuestos, realice las siguientes tareas: 

- Auto y cierre automático de configuración inician programaciones en máquinas virtuales.
- Puede crear la configuración de directivas en el número de usuarios de las máquinas virtuales.
- Establecimiento de directivas en la galería y tamaños de máquinas virtuales a los usuarios de imágenes puede elegir entre.
- Seguimiento de los costos y establecimiento de destinos en el laboratorio.
- Permite recibir una notificación en altos costos proyectados para el laboratorio de modo que pueda tomar las acciones necesarias. 

DevTest Labs proporciona las siguientes ventajas en la creación, configuración y administración de entornos en la nube:

## <a name="cost-control-and-governance"></a>Gobierno y control de costos
DevTest Labs facilita el control de los costos, ya que permite realizar las tareas siguientes:

- Establecer directivas en su laboratorio - como número de máquinas virtuales (VM) por usuario y el número de máquinas virtuales por laboratorio. 
- Crear directivas para apagar e iniciar las máquinas virtuales automáticamente.
- Permite realizar un seguimiento de los costos en las máquinas virtuales y recursos de PaaS marcha dentro del laboratorio, por lo que puede mantenerse dentro del presupuesto predefinido. 
- Permite a los desarrolladores permanezca dentro del contexto del laboratorio de modo que no terminan de poner en marcha cualquier recurso fuera de ella en la suscripción o grupo de recursos subyacente.

## <a name="quickly-get-to-ready-to-test"></a>Obtenga acceso rápidamente al modo "Listo para probar"
DevTest Labs le permite crear entornos ya aprovisionados con todo lo que su equipo necesita para comenzar a desarrollar y probar aplicaciones. Notificación simplemente los entornos donde se instala la última compilación correcta de la aplicación y empezar a trabajar de inmediato. O bien, utilice contenedores para agilizar y simplificar aún más la creación de entornos.

## <a name="create-once-use-everywhere"></a>Se crean una vez y se utilizan en todas partes
Capture y comparta plantillas de entorno de PaaS y artefactos dentro de su equipo u organización, todo con control de código fuente - para crear el desarrollador y entornos de prueba con facilidad.

## <a name="worry-free-self-service"></a>Autoservicio sin preocupaciones
DevTest Labs permite a los desarrolladores y evaluadores de forma rápida y fácilmente crea máquinas virtuales (IaaS) y los recursos de PaaS dentro de unos pocos clics mediante un conjunto de recursos configurados previamente para ellos.

## <a name="use-iaas-and-paas-resources"></a>Usar recursos de IaaS y PaaS 
DevTest Labs también permite a los desarrolladores poner en marcha los recursos de PaaS, como Web Apps, clústeres de Service Fabric, granjas de SharePoint y así sucesivamente. dentro del laboratorio mediante una plantilla de Resource Manager. Usar las plantillas de Resource Manager desde nuestro repositorio público del entorno o conectar el laboratorio a su propio repositorio de Git para empezar a trabajar en PaaS en laboratorios. También puede realizar un seguimiento de los costos en estos recursos para mantenerse dentro de los presupuestos predefinidos. 

## <a name="integrate-with-your-existing-toolchain"></a>Integrar con la cadena de herramientas existente
Use complementos ya creados o nuestra API para aprovisionar entornos de desarrollo y pruebas directamente desde la herramienta preferida de integración continua (CI), el entorno de desarrollo (IDE) integrado o la canalización de versiones. También puede utilizar nuestra completa herramienta de línea de comandos.

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 

- Para obtener más información sobre DevTest Labs, consulte [conceptos de DevTest Labs](devtest-lab-concepts.md)
- Para ver un tutorial con instrucciones paso a paso, consulte [Tutorial: Configurar un laboratorio con Azure DevTest Labs](tutorial-create-custom-lab.md)


