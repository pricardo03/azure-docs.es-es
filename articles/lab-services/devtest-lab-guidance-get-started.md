---
title: Escenarios populares para usar Azure DevTest Labs
description: Este artículo proporciona los escenarios principales para que usar Azure DevTest Labs y dos rutas de acceso generales para empezar a usar el servicio en su organización.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 8736ba4c24ac4c8f8d84345028d1cadfdef38697
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59272396"
---
# <a name="popular-scenarios-for-using-azure-devtest-labs"></a>Escenarios populares para usar Azure DevTest Labs
Según las necesidades de la empresa, DevTest Labs puede configurarse para satisfacer requisitos diferentes.  En este artículo se describe los escenarios populares. Cada escenario trata las ventajas gracias al uso de DevTest Labs y recursos para usar para implementar estos escenarios.  

- Equipos de escritorio del desarrollador
- Entornos de pruebas
- Sesiones de aprendizaje, laboratorios prácticos y hackathons
- Investigaciones en espacio aislado
- Laboratorios educativos

## <a name="developer-desktops"></a>Equipos de escritorio del desarrollador
Los desarrolladores a menudo tienen requisitos diferentes para las máquinas de desarrollo de proyectos diferentes. Con DevTest Labs, los desarrolladores pueden tener acceso a petición como en las máquinas que están configurados para que se adapte a sus escenarios más comunes. DevTest Labs ofrece las siguientes ventajas:

- Las organizaciones pueden proporcionar asegura la consistencia de los equipos de los equipos de desarrollo comunes.
- Los desarrolladores pueden aprovisionar rápidamente las máquinas de desarrollo a petición o [reclamar una máquina preconfigurada existente](devtest-lab-add-claimable-vm.md).
- Los desarrolladores pueden aprovisionar recursos de una manera de autoservicio sin necesidad de tener permisos de nivel de suscripción.
- TI o los administradores pueden [predefinir la topología de red](devtest-lab-configure-vnet.md) y los desarrolladores pueden directamente usarlo en un método sencillo e intuitivo sin necesidad de ningún acceso especial.
- Los desarrolladores pueden fácilmente [personalizar](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) su desarrollo de equipos según sea necesario.
- Los administradores pueden controlar los costos garantizando que:
    - Los desarrolladores [no se puede obtener más máquinas virtuales](devtest-lab-set-lab-policy.md#set-virtual-machines-per-user) que necesitan para el desarrollo
    - [Máquinas virtuales se apagan](devtest-lab-set-lab-policy.md#set-auto-shutdown) cuando no esté en uso
    - Solo [lo que permite un subconjunto de tamaños de instancia de máquina virtual](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes) para los laboratorios específicos
    - [Administrar los objetivos de costos y notificaciones](devtest-lab-configure-cost-management.md) para cada uno de ellos.

Para obtener más información, consulte [usar Azure DevTest Labs para desarrolladores](devtest-lab-developer-lab.md). 

## <a name="test-environments"></a>Entornos de pruebas
Crear y administrar entornos de prueba en una empresa puede requerir un esfuerzo considerable. Con DevTest Labs, entornos de prueba pueden ser fácilmente crea, actualiza o está duplicados. Permite que los equipos tener acceso a un entorno completamente configurado cuando sea necesario. En este escenario, DevTest Labs ofrece las siguientes ventajas:

- Las organizaciones pueden proporcionar entornos de pruebas comunes asegura la consistencia de los equipos.
- Los evaluadores pueden probar la versión más reciente de su aplicación Aprovisionando rápidamente entornos Windows y Linux mediante el uso de plantillas reutilizables.
- Los administradores pueden conectarse el laboratorio para DevOps de Azure para habilitar escenarios de DevOps
- Los propietarios de laboratorio pueden controlar los costos garantizando:
    - [Se apagan las máquinas virtuales en entornos](devtest-lab-set-lab-policy.md#set-auto-shutdown) cuando no esté en uso
    - Solo [lo que permite un subconjunto de tamaños de instancia de máquina virtual para](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes) los laboratorios específicos
    - [Administrar los objetivos de costos y notificaciones](devtest-lab-configure-cost-management.md) para cada uno de ellos.

Para obtener más información, consulte [entornos de prueba de usar Azure DevTest Labs para máquinas virtuales y PaaS](devtest-lab-test-env.md).

## <a name="sandboxed-investigations"></a>Investigaciones en espacio aislado
Los desarrolladores a menudo investigue diferentes tecnologías o diseño de la infraestructura. De forma predeterminada, se crean todos los entornos creados con DevTest Labs en su propio grupo de recursos. El usuario de DevTest Labs obtiene acceso de solo lectura a esos recursos. Sin embargo, para los desarrolladores que necesitan más control, una configuración de todo el laboratorio se puede actualizar para dar [derechos de colaborador](https://azure.microsoft.com/updates/azure-devtest-labs-view-and-set-access-rights-to-an-environment-rg/) al usuario de DevTest Labs original para todos los entornos que creen.  Con DevTest Labs, los desarrolladores se pueden dar permiso de colaborador automáticamente a los entornos que crean en el laboratorio.  Este escenario permite a los desarrolladores agregar o cambiar los recursos de Azure que necesitan para sus entornos de desarrollo o prueba. El [coste por recurso](devtest-lab-configure-cost-management.md#view-cost-by-resource) página permite a los propietarios de laboratorio realizar un seguimiento el costo de cada entorno que se usa para las investigaciones.

Para obtener más información, consulte [establecer derechos de acceso a un grupo de recursos del entorno](https://aka.ms/dtl-sandbox).

## <a name="trainings-hands-on-labs-and-hackathons"></a>Aprendizajes, laboratorios prácticos y hackathons 
Un laboratorio en Azure DevTest Labs actúa como un contenedor para actividades transitorios como talleres, laboratorios prácticos, aprendizajes o hackathons excelente.  El servicio permite crear un laboratorio en el que puede proporcionar plantillas personalizadas que cada aprendiz puede usar para crear entornos idénticos y aislados con fines de entrenamiento. En este escenario, DevTest Labs ofrece las siguientes ventajas:

- [Las directivas](devtest-lab-set-lab-policy.md) asegurarse de que los aprendices solo obtención el número de recursos, como máquinas virtuales, que necesitan.
- Son creadas y configuradas previamente máquinas [reclamado](devtest-lab-add-claimable-vm.md) con una acción única de aprendiz.
- Los laboratorios se comparten con los aprendices accediendo [dirección URL para el laboratorio](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab).
- [Las fechas de caducidad](devtest-lab-add-vm.md#steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs) en máquinas virtuales, asegúrese de que las máquinas se eliminan después de que ya no son necesarios.
- Es fácil [eliminar un laboratorio](devtest-lab-delete-lab-vm.md#delete-a-lab) y todos los [recursos relacionados](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) cuando está el entrenamiento sobre.

Para obtener más información, consulte [usar Azure DevTest Labs para entrenamiento](devtest-lab-training-lab.md).  

## <a name="proof-of-concept-vs-scaled-deployment"></a>Prueba de concepto frente a la implementación escalada
Una vez que decide explorar DevTest Labs, hay dos rutas generales hacia delante: Prueba de concepto frente a implementación de escalado.  

Una **implementación a escala** conlleva semanas o meses de revisión y planeamiento con la intención de implementar DevTest Labs en toda la empresa con cientos o miles de desarrolladores.

Un **prueba de concepto** implementación se centra en un esfuerzo concentrado desde un único equipo para establecer el valor de la organización. Aunque puede ser tentador pensar en una implementación con escala, el enfoque tiende a recaer con más frecuencia en la opción de la prueba de concepto. Por lo tanto, se recomienda que empiece poco a poco, que aprenda con el primer equipo, que repita el mismo enfoque con dos o tres equipos más y que luego planee una implementación a escala basándose en los conocimientos adquiridos. Para una correcta prueba de concepto, se recomienda que elija uno o dos equipos e identifique sus escenarios (entornos de pruebas frente a entornos de desarrollo), documente sus casos de uso actuales e implemente DevTest Labs.

## <a name="next-steps"></a>Pasos siguientes
Lea los artículos siguientes:

- [Conceptos de DevTest Labs](devtest-lab-concepts.md)
- [Preguntas más frecuentes sobre DevTest Labs](devtest-lab-faq.md)

