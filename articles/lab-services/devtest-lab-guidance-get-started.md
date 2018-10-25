---
title: Introducción al uso de Azure DevTest Labs
description: Este artículo proporciona los escenarios principales de uso de Azure DevTest Labs y dos rutas generales para empezar a usar el servicio en su organización.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: spelluru
ms.openlocfilehash: 1d00dbe23fbd724893ba2b964fc54a3ea9066dcb
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48250831"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Introducción al uso de Azure DevTest Labs
Una vez que decide explorar DevTest Labs, hay dos rutas generales: prueba de concepto frente a implementación a escala. 

Una **implementación a escala** conlleva semanas o meses de revisión y planeamiento con la intención de implementar DevTest Labs en toda la empresa con cientos o miles de desarrolladores. 

Una **implementación de prueba de concepto** se centra en un esfuerzo concentrado de un único equipo para establecer el valor de la organización. Aunque puede ser tentador pensar en una implementación con escala, el enfoque tiende a recaer con más frecuencia en la opción de la prueba de concepto. Por lo tanto, se recomienda que empiece poco a poco, que aprenda con el primer equipo, que repita el mismo enfoque con dos o tres equipos más y que luego planee una implementación a escala basándose en los conocimientos adquiridos. Para una correcta prueba de concepto, se recomienda que elija uno o dos equipos e identifique sus escenarios (entornos de pruebas frente a entornos de desarrollo), documente sus casos de uso actuales e implemente DevTest Labs. 

## <a name="scenarios"></a>Escenarios
Hay tres escenarios principales para una implementación de DevTest Labs: 

- Equipos de escritorio del desarrollador
- Entornos de pruebas
- Laboratorios, cursos y hackathon

## <a name="developer-desktops"></a>Equipos de escritorio del desarrollador
Los desarrolladores a menudo tienen requisitos diferentes para las máquinas de desarrollo de proyectos diferentes. Con DevTest Labs, los desarrolladores pueden tener acceso a máquinas virtuales a petición configuradas previamente para que se adapten a sus escenarios más comunes. DevTest Labs ofrece las siguientes ventajas:

- Las organizaciones pueden proporcionar entornos comunes de desarrollo y pruebas, para garantizar la coherencia entre los equipos.
- Los desarrolladores pueden aprovisionar rápidamente las máquinas de desarrollo a petición.
- Los desarrolladores pueden aprovisionar recursos mediante autoservicio sin necesidad de tener permisos a nivel de suscripción.
- El departamento de TI o los administradores pueden definir previamente la topología de red, y los desarrolladores pueden usarla directamente de una forma fácil e intuitiva sin requerir ningún acceso especial.
- Los desarrolladores pueden personalizar fácilmente las máquinas de desarrollo, según proceda.
- Los administradores pueden controlar los costos garantizando que:
    - Los desarrolladores no puedan obtener más máquinas virtuales de las que necesitan para el desarrollo.
    - Las máquinas virtuales se apagan cuando no están en uso.
    - Se admita solo un subconjunto de tamaños de instancia de máquina virtual para cargas de trabajo específicas.

## <a name="test-environments"></a>Entornos de pruebas
Crear y administrar entornos de prueba en una empresa puede requerir un esfuerzo considerable. Con DevTest Labs, los entornos de prueba se pueden crear, actualizar o duplicar con facilidad, lo que permite a los equipos tener acceso a un entorno completamente configurado cuando sea necesario. En este escenario, DevTest Labs ofrece las siguientes ventajas:

- Los evaluadores pueden probar la última versión de la aplicación aprovisionando rápidamente entornos de Windows y Linux mediante plantillas y artefactos reutilizables.
- Los evaluadores pueden escalar verticalmente la prueba de carga aprovisionando varios agentes de prueba.
- Los administradores pueden conectar el laboratorio a VSTS para habilitar escenarios de DevOps.
- Los administradores pueden controlar los costos garantizando que:
    - Los evaluadores no puedan obtener más máquinas virtuales de las que necesitan.
    - Las máquinas virtuales se apagan cuando no están en uso.
    - Se admita solo un subconjunto de tamaños de instancia de máquina virtual para cargas de trabajo específicas.

## <a name="labs-workshops-trainings-and-hackathons"></a>Laboratorios, talleres, cursos y hackathons  
Un laboratorio de Azure DevTest Labs actúa como un magnífico contenedor para actividades transitorias como talleres, laboratorios prácticos, cursos o hackathons. El servicio permite crear un laboratorio en el que puede proporcionar plantillas personalizadas que cada aprendiz puede usar para crear entornos idénticos y aislados con fines de entrenamiento. Puede aplicar directivas para garantizar que los entornos de entrenamiento estarán disponibles para cada aprendiz solo cuando sea necesario y cuando contengan los suficientes recursos necesarios como, por ejemplo, máquinas virtuales, para el entrenamiento. Por último, puede compartir fácilmente el laboratorio con los aprendices y estos podrán acceder fácilmente a él con un solo clic. Una vez concluida la clase, es fácil eliminar el laboratorio y todos los recursos relacionados.


## <a name="next-steps"></a>Pasos siguientes
Consulte el siguiente artículo de esta serie: [Escalado vertical de la implementación de DevTest Labs](devtest-lab-guidance-scale.md).
