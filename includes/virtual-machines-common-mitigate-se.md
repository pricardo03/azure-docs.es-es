---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 4c5b4c5eacd4be751004af551e3753a61873c7a7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551536"
---
**Última actualización del documento**: 14 de agosto de 2018 10:00 A.M. hora del Pacífico.

La divulgación de una [nueva clase de vulnerabilidades de CPU](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), conocidas como ataques de canal lateral de ejecución especulativa, han generado preguntas de los clientes que buscan mayor claridad.  

Microsoft ha implementado mitigaciones en todos nuestros servicios en la nube. La infraestructura que ejecuta Azure y aísla las cargas de trabajo de los clientes entre sí está protegida. Esto significa que un atacante potencial que use la misma infraestructura no puede atacar su aplicación mediante estas vulnerabilidades.

Azure utiliza un [mantenimiento con conservación de memoria](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#maintenance-not-requiring-a-reboot) siempre que es posible para minimizar el impacto en el cliente y eliminar la necesidad de reinicios. Azure seguirá utilizando estos métodos al realizar actualizaciones del host en todo el sistema y proteger a los clientes.

Encontrará más información acerca de cómo integrar la seguridad en todos los aspectos de Azure en el sitio de [documentación de Azure Security Center](https://docs.microsoft.com/azure/security/). 

> [!NOTE] 
> Desde que este documento se publicó por primera vez, se han divulgado varias variantes de esta clase de vulnerabilidad. Microsoft continúa realizando grandes esfuerzos por proteger a nuestros clientes y proporcionarles orientación. Esta página se actualizará a medida que se publiquen más correcciones. 
> 
> El 14 de agosto de 2018, el sector reveló una nueva vulnerabilidad de canal lateral de ejecución especulativa conocida como [L1 Terminal Fault](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018) (L1TF), a la que se han asignado varios identificadores ([CVE-2018-3615, CVE-2018-3620 y CVE-2018-3646](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00161.html)). Esta vulnerabilidad afecta a los procesadores Intel® Core® y los procesadores Intel® Xeon®. Microsoft ha implementado elementos de mitigación en nuestros servicios en la nube que refuerzan el aislamiento entre clientes. A continuación encontrará orientación adicional para protegerse frente a L1TF y vulnerabilidades anteriores ([Spectre variante 2 CVE-2017-5715 y Meltdown variante 3 CVE-2017-5754](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution)).
>  






## <a name="keeping-your-operating-systems-up-to-date"></a>Mantener actualizados los sistemas operativos

Aunque no se necesita una actualización del sistema operativo para aislar de otros clientes las aplicaciones que se ejecutan en Azure, mantener el software actualizado siempre es un procedimiento recomendado. Los últimos paquetes acumulativos de actualizaciones de seguridad para Windows contienen mitigaciones para varias vulnerabilidades de canal lateral de ejecución especulativa. De forma similar, las distribuciones de Linux han publicado varias actualizaciones para corregir estas vulnerabilidades. Estas son las acciones recomendadas para actualizar el sistema operativo:

| Oferta | Acción recomendada  |
|----------|---------------------|
| Azure Cloud Services  | Habilite las [actualizaciones automáticas](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal) o asegúrese de que está ejecutando la versión más reciente del sistema operativo invitado. |
| Máquinas virtuales Linux en Azure | Instale las actualizaciones de su proveedor de sistema operativo. Para más información, consulte [Linux](#linux) más adelante en este documento. |
| Máquinas virtuales Windows en Azure  | Instale las últimas actualizaciones acumulativas de seguridad.
| Otros servicios PaaS de Azure | No es necesaria ninguna acción para los clientes que usan estos servicios. Azure mantiene actualizadas las versiones de sistema operativo de forma automática. |

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Instrucciones adicionales si está ejecutando código que no es de confianza 

Los clientes que permiten a los usuarios que no son de confianza ejecutar código arbitrario tal vez deseen implementar algunas características de seguridad adicionales en las máquinas virtuales o en los servicios en la nube de Azure. Estas características protegen frente a los vectores de divulgación entre procesos que describen varias vulnerabilidades de ejecución especulativa.

Escenarios de ejemplo donde se recomiendan las características de seguridad adicionales:

- Permite la ejecución de código que no es de confianza en la máquina virtual.  
    - *Por ejemplo, permite que uno de sus clientes cargue un archivo binario o un script que, a continuación, se ejecutará en la aplicación*. 
- Permite iniciar sesión en la máquina virtual con cuentas con privilegios reducidos a usuarios que no son de confianza.   
    - *Por ejemplo, permite iniciar sesión en una de las máquinas virtuales mediante escritorio remoto o SSH a un usuario con privilegios reducidos*.  
- Permite acceder a las máquinas virtuales implementadas mediante virtualización anidada a usuarios que no son de confianza.  
    - *Por ejemplo, controla el host de Hyper-V, pero asigna las máquinas virtuales a usuarios que no son de confianza*. 

Los clientes que no implementan un escenario relacionado con código que no es de confianza no necesitan habilitar estas características de seguridad adicionales. 

## <a name="enabling-additional-security"></a>Habilitación de la seguridad adicional 

Puede habilitar las características de seguridad adicionales en la máquina virtual o en el servicio en la nube.

### <a name="windows"></a> Windows 

El sistema operativo de destino debe estar actualizado para habilitar estas características de seguridad adicionales. Aunque hay habilitadas numerosas mitigaciones del canal lateral de ejecución especulativa de forma predeterminada, las características adicionales que se describen aquí deben habilitarse manualmente y pueden afectar al rendimiento. 

**Paso 1**: [Póngase en contacto con soporte técnico de Azure](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) exponen actualizar firmware (habilitación del microcódigo) en las máquinas virtuales. 

**Paso 2**: Habilitar la compatibilidad de sombreado de dirección Virtual (KVAS) de Kernel y el sistema operativo de inyección de código de destino de rama (BTI). Siga las instrucciones descritas en el artículo [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) para habilitar las protecciones mediante las claves del Registro `Session Manager`. Es necesario reiniciar. 

**Paso 3**: Para las implementaciones que usan [virtualización anidada](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (D3 y E3 solo): Estas instrucciones se aplican dentro de la máquina virtual que se va a usar como un host de Hyper-V. 

1. Siga las instrucciones descritas en el artículo [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) para habilitar las protecciones mediante las claves del Registro `MinVmVersionForCpuBasedMitigations`.  
 
1. Establezca el tipo de programador del hipervisor en **Core** con las instrucciones descritas [aquí](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types). 

**Paso 4**: Siga las instrucciones de [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) para comprobar las protecciones se habilitan mediante la [SpeculationControl](https://aka.ms/SpeculationControlPS) módulo de PowerShell. 

> [!NOTE]
> Si descargó este módulo anteriormente, deberá instalar la versión más reciente.
>

Todas las máquinas virtuales deben mostrar:

```
branch target injection mitigation is enabled: True

kernel VA shadow is enabled: True  

L1TFWindowsSupportEnabled: True
```


### <a name="linux"></a>Linux

<a name="linux"></a>Se requiere que el sistema operativo de destino esté completamente actualizado para habilitar el conjunto de características de seguridad adicionales. Algunas de las mitigaciones estarán habilitadas de forma predeterminada. La siguiente sección describe las características que están desactivadas de forma predeterminada y sujetas a la compatibilidad de hardware (microcódigo). La habilitación de estas características puede afectar al rendimiento. Consulte la documentación del proveedor de su sistema operativo para más instrucciones
 
**Paso 1**: [Póngase en contacto con soporte técnico de Azure](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) exponen actualizar firmware (habilitación del microcódigo) en las máquinas virtuales.
 
**Paso 2**: Habilitar la compatibilidad de inyección de código de destino de rama (BTI) del sistema operativo mitigar CVE-2017-5715 (variante 2 de Spectre) siguiendo la documentación del proveedor de su sistema operativo. 
 
**Paso 3**: Habilitar el aislamiento de tabla página de Kernel (KPTI) para mitigar CVE-2017-5754 (Meltdown Variant 3) siguiendo la documentación del proveedor de su sistema operativo. 
 
El proveedor de su sistema operativo dispone de más información:  
 
- [RedHat y CentOS](https://access.redhat.com/security/vulnerabilities/speculativeexecution) 
- [Suse](https://www.suse.com/support/kb/doc/?id=7022512) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/SpectreAndMeltdown) 


## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [Securing Azure customers from CPU vulnerability](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) (Proteger a los clientes de Azure de las vulnerabilidades de CPU).
