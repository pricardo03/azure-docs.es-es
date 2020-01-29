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
ms.date: 11/05/2019
ms.author: spelluru
ms.openlocfilehash: 3bc58e9545f38508a9e08e9ae1aa9cf8713cc520
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264753"
---
# <a name="classroom-labs-in-azure-lab-services--frequently-asked-questions-faq"></a>Laboratorios de clase en Azure Lab Services: preguntas frecuentes
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

### <a name="what-port-ranges-should-i-open-on-my-organizations-firewall-setting-to-connect-to-lab-virtual-machines-via-rdpssh"></a>¿Qué intervalos de puertos debo abrir en la configuración del firewall de mi organización para conectarme a las máquinas virtuales de laboratorio a través de RDP/SSH?

Los puertos son: 49152–65535. Los laboratorios de clase se encuentran detrás de un equilibrador de carga, por lo que todas las máquinas virtuales de un laboratorio tienen una dirección IP única y cada máquina virtual del laboratorio tiene un puerto único. Los números de puerto y la dirección IP pública pueden cambiar cada vez que se vuelve a publicar el laboratorio.

### <a name="what-public-ip-address-range-should-i-open-on-my-organizations-firewall-settings-to-connect-to-lab-virtual-machines-via-rdpssh"></a>¿Qué intervalos de direcciones IP públicas debo abrir en la configuración del firewall de mi organización para conectarme a las máquinas virtuales de laboratorio a través de RDP/SSH?
Consulte [Intervalos IP de Azure y etiquetas de servicio: nube pública](https://www.microsoft.com/download/details.aspx?id=56519), que proporciona el intervalo de direcciones IP públicas para centros de datos en Azure. Puede abrir las direcciones IP de las regiones en las que se encuentran las cuentas de laboratorio.

## <a name="users"></a>Usuarios

### <a name="how-many-users-can-be-in-a-classroom-lab"></a>¿Cuántos usuarios pueden estar en un laboratorio educativo?
Puede agregar hasta 400 usuarios a un laboratorio educativo. 

## <a name="blog-post"></a>Entrada de blog
Suscríbase al [blog de Azure Lab Services](https://azure.microsoft.com/blog/tag/azure-lab-services/).

## <a name="update-notifications"></a>Notificaciones de actualización
Suscríbase a las [actualizaciones de Lab Services](https://azure.microsoft.com/updates/?product=lab-services) para mantenerse informado sobre las nuevas características de Lab Services.

## <a name="general"></a>General
### <a name="what-if-my-question-isnt-answered-here"></a>Mi pregunta no está respondida aquí. ¿Qué debo hacer?
Si su pregunta no aparece aquí, háganoslo saber para que podamos ayudarlo a encontrar una respuesta.

- Publique una pregunta al final de este documento. 
- Para llegar a un público más amplio, publique una pregunta en el [Foro Stack Overflow de Azure Lab Services](https://stackoverflow.com/questions/tagged/azure-lab-services). 
- Para solicitudes de características, envíe sus solicitudes e ideas al sitio [User Voice de Azure Lab Services](https://feedback.azure.com/forums/320373-lab-services?category_id=352774).

