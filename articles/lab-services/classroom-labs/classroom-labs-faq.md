---
title: 'Laboratorios de clase en Azure Lab Services: preguntas frecuentes| Microsoft Docs'
description: Encuentre respuestas a preguntas habituales sobre los laboratorios de clase en Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: spelluru
ms.openlocfilehash: 261f337ac72a28bc2e1c74e23731242cc3bd0075
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571296"
---
# <a name="classroom-labs-in-azure-lab-services---frequently-asked-questions-faq"></a>Laboratorios de clase en Azure Lab Services: preguntas frecuentes
Obtenga respuestas a algunas de las preguntas más habituales sobre los laboratorios de clase en Azure Lab Services. 

## <a name="quotas"></a>Cuotas

### <a name="is-the-quota-per-user-or-per-week-or-per-entire-duration-of-the-lab"></a>¿La cuota es por usuario, por semana o por toda la duración del laboratorio? 
La cuota que establezca para un laboratorio es para cada alumno por toda la duración del laboratorio. Además, la [hora de ejecución programada de las máquinas virtuales](how-to-create-schedules.md) no cuenta en la cuota asignada a un usuario. La cuota es el tiempo fuera del horario programado que un alumno pasa en las máquinas virtuales.  Para más información sobre las cuotas, vea [Establecimiento de cuotas para los usuarios](how-to-configure-student-usage.md#set-quotas-for-users).

## <a name="schedules"></a>Programaciones

### <a name="do-all-vms-in-the-lab-start-automatically-when-a-schedule-is-set"></a>¿Se inician automáticamente todas las máquinas virtuales del laboratorio cuando se establece una programación? 
No. No todas las máquinas virtuales. Solo las máquinas virtuales que se asignan a los usuarios según una programación. Las máquinas virtuales que no están asignadas a un usuario no se inician automáticamente. Es por diseño. 

## <a name="lab-accounts"></a>Cuentas de laboratorio

### <a name="why-am-i-not-able-to-create-a-lab-because-of-unavailability-of-the-address-range"></a>¿Por qué no puedo crear un laboratorio debido a la falta de disponibilidad del intervalo de direcciones? 
Los laboratorios de clase pueden crear máquinas virtuales de laboratorio dentro de un intervalo de direcciones IP que se especifique al crear la cuenta de laboratorio en Azure Portal. Cuando se proporciona un intervalo de direcciones, cada laboratorio que se crea después de haber asignado 512 direcciones IP para las máquinas virtuales del laboratorio. El intervalo de direcciones de la cuenta de laboratorio debe ser lo suficientemente grande como para dar cabida a todos los laboratorios que se van a crear en la cuenta de laboratorio. 

Por ejemplo, si tiene un bloque de /19 - 10.0.0.0/19, este intervalo de direcciones abarca 8192 direcciones IP y 16 laboratorios (8192/512 = 16 laboratorios). En este caso, la creación del laboratorio 17 genera un error.

## <a name="blog-post"></a>Entrada de blog
Suscríbase al [blog de Azure Lab Services](https://azure.microsoft.com/blog/tag/azure-lab-services/).

## <a name="update-notifications"></a>Notificaciones de actualización
Suscríbase a las [actualizaciones de Lab Services](https://azure.microsoft.com/updates/?product=lab-services) para mantenerse informado sobre las nuevas características de DevTest Labs.

## <a name="general"></a>General
### <a name="what-if-my-question-isnt-answered-here"></a>Mi pregunta no está respondida aquí. ¿Qué debo hacer?
Si su pregunta no aparece aquí, háganoslo saber para que podamos ayudarlo a encontrar una respuesta.

- Publique una pregunta al final de este documento. 
- Para llegar a un público más amplio, publique una pregunta en el [foro de MSDN de Azure DevTest Labs](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). 
- Para solicitudes de características, envíe sus solicitudes e ideas al sitio [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

