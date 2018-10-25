---
title: Sincronizar la hora de las máquinas virtuales Linux en Azure | Microsoft Docs
description: Sincronice la hora de las máquinas virtuales Linux.
services: virtual-machines-linux
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: zarhoads
ms.openlocfilehash: aec36b8ea0f342b133e6844e5a96314c6348289b
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465481"
---
# <a name="time-sync-for-linux-vms-in-azure"></a>Sincronizar la hora en las máquinas virtuales de Linux en Azure

La sincronización de la hora es importante para la seguridad y la correlación de eventos. A veces se usa en la implementación de transacciones distribuidas. La precisión de hora entre varios sistemas de equipos se logra mediante la sincronización. Además, la sincronización puede verse afectada por varias cosas, incluyendo las operaciones de reinicio y el tráfico de red entre el origen de la hora y el equipo encargado de capturar la hora. 

Por ello, Azure cuenta con la infraestructura que ejecuta Windows Server 2016. Windows Server 2016 ha mejorado los algoritmos que se usan para corregir la hora y la condición del reloj local, y así poder sincronizarse con la hora UTC.  La característica de hora precisa de Windows Server 2016 ha mejorado en gran medida la manera en que el servicio VMICTimeSync rige las máquinas virtuales con el host para obtener la hora exacta. Las mejoras incluyen una hora inicial más precisa en el inicio o la restauración de la máquina virtual, así como la interrupción de la corrección de la latencia. 

>[!NOTE]
>Para obtener información general rápida del servicio Horario de Windows, eche un vistazo a este [vídeo de información general de alto nivel](https://aka.ms/WS2016TimeVideo).
>
> Para obtener más información, consulte el artículo [Hora de Windows Server 2016 precisa](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Información general

La precisión del reloj de un equipo se mide en función de lo cerca que esté ese reloj de la hora universal coordinada (UTC) estándar. La UTC se define mediante una muestra internacional de relojes atómicos precisos que solo se retrasan un segundo en 300 años. Aún así, es necesario tener hardware especializado para leer la UTC directamente. En cambio, los servidores horarios se sincronizan con la UTC y se accede a ellos desde otros equipos para proporcionar escalabilidad y robustez. Cada equipo tiene un servicio de sincronización de hora en ejecución que sabe qué servidores horarios debe usar y que comprueba periódicamente si el reloj del equipo necesita ser corregido ajustándolo si fuera necesario. 

Los hosts de Azure se sincronizan con los servidores horarios internos de Microsoft que ajustan su hora a partir de los dispositivos Stratum 1 que pertenecen a Microsoft, y que cuentan con antenas GPS. Las máquinas virtuales en Azure pueden depender de su host para proporcionar la hora exacta (*hora del host*) a la máquina virtual, o la máquina virtual puede obtener directamente la hora de un servidor horario, o de una combinación de ambas opciones. 

En el hardware independiente, el sistema operativo Linux solo lee el reloj del hardware del host en el arranque. Después de eso, el reloj se mantiene gracias al temporizador de interrupción en el kernel de Linux. En esta configuración, el reloj dejará de ser preciso con el tiempo. En las nuevas distribuciones de Linux en Azure, las máquinas virtuales pueden usar el proveedor VMICTimeSync que está incluido en los servicios de integración de Linux (LIS), para consultar con más frecuencia actualizaciones del reloj desde el host.

Las interacciones de la máquina virtual con el host también pueden afectar al reloj. Durante [el mantenimiento de conservación de la memoria](maintenance-and-updates.md#memory-preserving-maintenance) las máquinas virtuales se detienen hasta 30 segundos. Por ejemplo, antes de que comience el proceso de mantenimiento que dura 28 segundos, el reloj de la máquina virtual muestra las 10:00:00 A.M. Cuando se reanuda la máquina virtual, el reloj de la misma todavía muestra las 10:00:00 A.M., por lo que tendría 28 segundos de retraso. Para corregir esto, el servicio VMICTimeSync supervisa lo que sucede en el host y solicita que se realicen cambios en las máquinas virtuales para compensar este retraso.

Por supuesto, si la sincronización de la hora no funciona, el reloj de la máquina virtual solo acumularía errores. Cuando solo hay una máquina virtual, este efecto puede no ser significativo a menos que la carga de trabajo requiera un cronometraje altamente preciso. Aún así, en la mayoría de los casos hay varias máquinas virtuales conectadas entre sí que usan la hora para realizar el seguimiento de las transacciones, por lo que esta hora debe ser consistente en toda la implementación. Cuando la hora de las máquinas virtuales es diferente, puede ver los siguientes efectos:

- Se produce un error en la autenticación. Los protocolos de seguridad como Kerberos o la tecnología dependiente de los certificados confían en que la hora sea consistente en todos los sistemas.
- Es muy difícil averiguar qué ha ocurrido en un sistema si los registros (u otros datos) tienen horas diferentes. Parecería que el mismo evento ocurrió en diferentes momentos, lo que dificulta la correlación.
- Si el reloj está atrasado, es posible que la facturación no se calcule correctamente.



## <a name="configuration-options"></a>Opciones de configuración

En general, existen tres formas de configurar la sincronización horaria de las máquinas virtuales de Linux hospedadas en Azure:

- La configuración predeterminada para las imágenes de Azure Marketplace usa tanto la hora del servidor NTP como la del host de VMICTimeSync. 
- Solo con el host mediante VMICTimeSync.
- Use otro servidor horario externo con o sin usar la hora del host de VMICTimeSync.


### <a name="use-the-default"></a>Usar el valor predeterminado

De forma predeterminada, la mayoría de las imágenes de Azure Marketplace para Linux están configuradas para sincronizarse desde dos orígenes: 

- NTP como origen principal, que obtiene la hora de un servidor NTP. Por ejemplo, las imágenes de Ubuntu 16.04 LTS Marketplace usan **ntp.ubuntu.com**.
- El servicio VMICTimeSync como origen secundario, que se usa para comunicar la hora del host a las máquinas virtuales y hacer correcciones después de pausar la máquina virtual para el proceso de mantenimiento. Los hosts de Azure usan dispositivos Stratum 1 de Microsoft para mantener la exactitud de la hora.

En las últimas distribuciones de Linux, el servicio VMICTimeSync usa el protocolo de tiempo de precisión (PTP), pero es posible que las distribuciones anteriores no sean compatibles con PTP, por lo que recurrirán a NTP para obtener la hora del host.

Para confirmar que NTP se está sincronizando correctamente, ejecute el comando `ntpq -p`.

### <a name="host-only"></a>Solo para el host 

Debido a que los servidores NTP como time.windows.com y ntp.ubuntu.com son públicos, es necesario enviar tráfico a través de Internet para sincronizar la hora con ellos. Los retrasos varios que pueden sufrir los paquetes pueden afectar negativamente a la calidad de la sincronización horaria. Si elimina NTP y cambia a la sincronización solo para el host, puede mejorar los resultados de la sincronización de hora.

Tenga en cuenta que solo merece la pena cambiar a la sincronización de hora solo para el host si tiene problemas al sincronizar la hora con la configuración predeterminada. Pruebe la sincronización solo para el host y compruebe si así mejora la sincronización de hora en la máquina virtual. 

### <a name="external-time-server"></a>Servidor de hora externo

Si tiene requisitos de sincronización horaria específica, también hay una opción que le permitirá usar servidores horarios externos. Los servidores horarios externos pueden proporcionar una hora específica, que puede ser útil para escenarios de prueba; gracias a ello, asegurará la uniformidad de la hora de las máquinas hospedadas en centros de datos que no sean de Microsoft, o podrá controlar los segundos intercalares de una manera especial.

Puede combinar servidores horarios externos con el servicio VMICTimeSync para proporcionar resultados similares a los de la configuración predeterminada. La combinación de un servidor horario externo con VMICTimeSync es la mejor opción para tratar los problemas que se pueden originar cuando las máquinas virtuales están en pausa para el mantenimiento. 

## <a name="tools-and-resources"></a>Herramientas y recursos

Hay algunos comandos básicos para comprobar la configuración de la sincronización horaria. En la documentación de la distribución de Linux encontrará más detalles sobre la mejor manera de configurar la sincronización horaria para esa distribución en cuestión.

### <a name="integration-services"></a>Servicios de integración

Compruebe si el servicio de integración (hv_utils) está cargado.

```bash
lsmod | grep hv_utils
```
Verá algo parecido a esto:

```
hv_utils               24418  0
hv_vmbus              397185  7 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
```

Comprobar si el demonio de servicios de integración de Hyper-V se está ejecutando.

```bash
ps -ef | grep hv
```

Verá algo parecido a esto:

```
root        229      2  0 17:52 ?        00:00:00 [hv_vmbus_con]
root        391      2  0 17:52 ?        00:00:00 [hv_balloon]
```


### <a name="check-for-ptp"></a>Buscar un origen de PTP

Con las versiones más recientes de Linux, un origen del reloj del protocolo de tiempo de precisión (PTP) está disponible como parte del proveedor VMICTimeSync. En versiones anteriores de Red Hat Enterprise Linux o CentOS 7.x, los [servicios de integración de Linux](https://github.com/LIS/lis-next) se pueden descargar y usar para instalar el controlador actualizado. Cuando se usa el PTP, el dispositivo Linux tendrá el formato /dev/ptp*x*. 

Consulte los orígenes de reloj del PTP que están disponibles.

```bash
ls /sys/class/ptp
```

En este ejemplo, el valor devuelto es *ptp0*, así que lo usamos para comprobar el nombre del reloj. Para comprobar el dispositivo, compruebe también el nombre del reloj.

```bash
cat /sys/class/ptp/ptp0/clock_name
```

Esto debería devolver el valor **hyperv**.

### <a name="chrony"></a>chrony

En Red Hat Enterprise Linux y CentOS 7.x, se ha configurado [chrony](https://chrony.tuxfamily.org/) para usar un reloj de origen del PTP. El demonio del protocolo de tiempo de redes (ntpd) no admite orígenes de PTP, por lo que se recomienda usar **chronyd**. Para habilitar el PTP, actualice **chrony.conf**.

```bash
refclock PHC /dev/ptp0 poll 3 dpoll -2 offset 0
```

Para obtener más información sobre Red Hat y NTP, consulte [Configure NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-configure_ntp) (Configurar NTP). 

Para obtener más información sobre chrony, consulte [Using chrony](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-using_chrony) (Usar chrony).

Si los orígenes de chrony y TimeSync están habilitados simultáneamente, puede marcar el que **prefiera**; al hacer esto, el otro origen se establecerá como una copia de seguridad. Como los servicios de NTP no actualizan el reloj para grandes distorsiones, excepto después de un largo período, VMICTimeSync recuperará el reloj de los eventos de la máquina virtual en pausa mucho más rápido que las herramientas basadas en NTP.


### <a name="systemd"></a>systemd 

En Ubuntu y SUSE, la sincronización de hora se configura con [systemd](https://www.freedesktop.org/wiki/Software/systemd/). Para obtener más información sobre Ubuntu, consulte [Time Synchronization](https://help.ubuntu.com/lts/serverguide/NTP.html) (Sincronización de hora). Para obtener más información sobre SUSE, consulte la sección 4.5.8 en [SUSE Linux Enterprise Server 12 SP3 Release Notes](https://www.suse.com/releasenotes/x86_64/SUSE-SLES/12-SP3/#InfraPackArch.ArchIndependent.SystemsManagement) (Notas de la versión de SUSE Linux Enterprise Server 12 SP3).



## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte el artículo [Hora de Windows Server 2016 precisa](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time).


