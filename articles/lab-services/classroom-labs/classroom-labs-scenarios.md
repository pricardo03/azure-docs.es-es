---
title: Usar laboratorios educativos para aprendizajes - Azure Lab Services | Microsoft Docs
description: Aprenda a usar Azure DevTest Labs para escenarios de entrenamiento.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: spelluru
ms.openlocfilehash: 4d2ba11181977f1976b5ae933e8b93a92424fa96
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56809423"
---
# <a name="use-classroom-labs-for-trainings"></a>Usar laboratorios educativos para aprendizajes
Puede configurar un laboratorio para aprendizajes. Laboratorios de aula en Azure Lab Services permiten crear un laboratorio para el entrenamiento donde cada aprendiz usa entornos idénticos y aislados para el entrenamiento. Puede aplicar directivas para garantizar que los entornos de entrenamiento estarán disponibles para cada aprendiz solo cuando sea necesario y cuando contengan los suficientes recursos necesarios como, por ejemplo, máquinas virtuales, para el entrenamiento. 

![Laboratorio de clase](../media/classroom-labs-scenarios/classroom.png)

Laboratorios de clase cumple los siguientes requisitos necesarios para llevar a cabo el entrenamiento en cualquier entorno virtual: 

- Los aprendices pueden aprovisionar rápidamente sus entornos de entrenamiento
- Cada máquina de entrenamiento debe ser idéntica
- Los aprendices no pueden ver las máquinas virtuales creadas por otros aprendices
- Se pueden controlar los costos garantizando que los aprendices no obtengan más máquinas virtuales de las necesarias para el entrenamiento y también apagando estas cuando no estén en uso
- Se puede compartir fácilmente el laboratorio de entrenamiento con cada aprendiz
- El laboratorio de entrenamiento se puede volver a usar una y otra vez

En este artículo, obtendrá información sobre las distintas características de Azure Lab Services que se pueden usar para cumplir los requisitos de entrenamiento descritos anteriormente y los pasos detallados que puede seguir para configurar un laboratorio para entrenamiento.  

## <a name="create-the-lab-account-as-a-lab-account-administrator"></a>Crear la cuenta de laboratorio como un administrador de cuenta de laboratorio
El primer paso para usar Azure Lab Services es crear una cuenta de laboratorio en Azure portal. Después de que un administrador de cuenta de laboratorio crea la cuenta de laboratorio, el administrador agrega los usuarios que deseen crear laboratorios para el **creador de laboratorio** rol. Los instructores creación laboratorios con máquinas virtuales para los alumnos realizar los ejercicios del curso que incluyan la enseñanza. Para obtener más información, consulte [crear y administrar la cuenta de laboratorio](how-to-manage-lab-accounts.md).

## <a name="create-and-manage-classroom-labs"></a>Crear y administrar laboratorios educativos
Un instructor, que es un miembro del rol creador de laboratorio en una cuenta de laboratorio, puede crear uno o más laboratorios en la cuenta de laboratorio. Crear y configurar una plantilla de máquina virtual con todo el software necesario para realizar los ejercicios del curso. Tome una imagen de las imágenes disponibles para la creación de un laboratorio educativo listos para usar y, a continuación, personalizarla instalando el software necesario para el laboratorio. Para obtener más información, consulte [crear y administrar laboratorios educativos](how-to-manage-classroom-labs.md).

## <a name="configure-usage-settings-and-policies"></a>Configuración de los valores y directivas de uso
El creador de laboratorio puede agregar o quitar usuarios del laboratorio, obtenga el vínculo de registro para enviar a los usuarios del laboratorio, establecer directivas, como cuotas individuales de configuración por usuario, actualice el número de máquinas virtuales disponibles en el laboratorio y mucho más. Para obtener más información, consulte [configurar directivas y configuración de uso](how-to-configure-student-usage.md).

## <a name="create-and-manage-schedules"></a>Creación y administración de programaciones
Las programaciones le permiten configurar un laboratorio educativo de forma que las máquinas virtuales que contiene se inicien y apaguen automáticamente a una hora determinada. Puede definir una programación puntual o periódica. Para obtener más información, consulte [crear y administrar programaciones para laboratorios educativos](how-to-create-schedules.md).

## <a name="set-up-and-publish-a-template-vm"></a>Configurar y publicar una plantilla de máquina virtual
En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configurar la plantilla de máquina virtual para que se configura con exactamente lo que desea proporcionar a los asistentes de entrenamiento. Puede proporcionar un nombre y una descripción de la plantilla que verán los usuarios del laboratorio. Luego, publique la plantilla si desea que las instancias de la plantilla de máquina virtual estén disponibles para los usuarios del laboratorio. Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso es igual al número máximo de usuarios permitidos en el laboratorio, que se puede establecer en la política de uso del laboratorio. Todas las máquinas virtuales tienen la misma configuración que la plantilla. Para obtener más información, consulte [establecer configurar y publicar las máquinas virtuales de plantilla](how-to-create-manage-template.md). 

## <a name="use-vms-in-the-classroom-lab"></a>Uso de máquinas virtuales en el laboratorio de clase
Un estudiante o el Asistente de entrenamiento se registra en el laboratorio y se conecta a la máquina virtual para realizar los ejercicios del curso. Para obtener más información, consulte [cómo tener acceso a un laboratorio educativo](how-to-use-classroom-lab.md).

## <a name="next-steps"></a>Pasos siguientes
Empiece por crear una cuenta de laboratorio en laboratorios educativos siguiendo las instrucciones del artículo: [Tutorial: Configurar una cuenta de laboratorio con Azure Lab Services](tutorial-setup-lab-account.md).