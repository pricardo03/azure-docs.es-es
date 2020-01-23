---
title: Solución de problemas de replicación para máquinas virtuales de Azure con Azure Site Recovery
description: Solución de problemas de replicación en recuperación ante desastres de VM de Azure con Azure Site Recovery
author: sideeksh
manager: rochakm
ms.topic: troubleshooting
ms.date: 8/2/2019
ms.openlocfilehash: b8afdd0f2dd98260a628116fa7402e05cd39e06b
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "75965851"
---
# <a name="troubleshoot-replication-in-azure-vm-disaster-recovery"></a>Solución de problemas de replicación en recuperación ante desastres de VM de Azure

En este artículo se describen los problemas comunes en Azure Site Recovery cuando se van a replicar y recuperar máquinas virtuales de Azure de una región a otra. También se explica cómo solucionarlos. Para más información sobre las configuraciones admitidas, consulte la [matriz de compatibilidad para la replicación de máquinas virtuales de Azure](site-recovery-support-matrix-azure-to-azure.md).

Azure Site Recovery replica los datos de manera coherente de la región de origen a la región de recuperación ante desastres y crea puntos coherentes de recuperación por bloqueo cada 5 minutos. Si Site Recovery no se puede crear puntos de recuperación durante 60 minutos, le avisa con esta información:

Mensaje de error: "No crash consistent recovery point available for the VM in the last 60 minutes" (No se ha producido ningún punto de recuperación coherente con bloqueo de la máquina virtual en los últimos 60 minutos).</br>
Id. del error: 153007 </br>

En las secciones siguientes se describen las causas y las soluciones.

## <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Velocidad de cambio elevada en los datos de la máquina virtual de origen
Azure Site Recovery desencadena un evento si la velocidad de cambio en la máquina virtual de origen supera los límites admitidos. Para comprobar si el problema se debe al alto grado de renovación, vaya a **Elementos replicados** > **Máquina virtual** > **Events -last 72 hours** (Eventos: últimas 72 horas).
Debería ver el evento "Frecuencia de cambio de datos superior a los límites admitidos":

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Si selecciona el evento, debería ver la información exacta del disco:

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


### <a name="azure-site-recovery-limits"></a>Límites de Azure Site Recovery
En la tabla siguiente se proporcionan los límites de Azure Site Recovery. Estos límites se basan en nuestras pruebas, pero no pueden cubrir todas las combinaciones de E/S posibles de la aplicación. Los resultados reales pueden variar en función de la combinación de E/S de la aplicación.

Hay dos límites que se deben tener en cuenta: la actividad de datos por disco y la actividad de datos por máquina virtual. Por ejemplo, veamos el disco Premium P20 en la tabla siguiente. Site Recovery puede asumir 5 MB/s de la actividad por disco con un máximo de cinco estos discos por máquina virtual, debido al límite de 25 MB/s de la actividad total por máquina virtual.

**Destino de almacenamiento de la replicación** | **Tamaño medio de E/S de disco de origen** |**Actividad de datos media de disco de origen** | **Actividad de datos total por día de disco de datos de origen**
---|---|---|---
Standard Storage | 8 KB | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 8 KB  | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 16 KB | 4 MB/s |  336 GB por disco
Disco Premium P10 o P15 | 32 KB, o más | 8 MB/s | 672 GB por disco
Disco Premium P20, P30, P40 o P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20, P30, P40 o P50 | 16 KB, o más |10 MB/s | 842 GB por disco

### <a name="solution"></a>Solución
Azure Site Recovery tiene límites de velocidad de cambio de datos en función del tipo de disco. Para saber si este problema es periódico o momentáneo, busque la velocidad de cambio en la máquina virtual afectada. Vaya a la máquina virtual de origen. busque las métricas en **Supervisión** y agregue las métricas como se muestra en esta captura de pantalla:

![Proceso de tres pasos para buscar la velocidad de cambio de datos](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Seleccione **Agregar métrica** y agregue **Bytes de escritura de discos del SO/s** y **Bytes de escritura de discos de datos/s**.
2. Supervise el aumento como se muestra en la captura de pantalla.
3. Vea el total de todas las operaciones de escritura que se producen en el disco del sistema operativo y en todos los discos de datos. Estas métricas podrían no proporcionar información en el nivel de disco, pero indican el patrón de la actividad de datos.

Si un pico procede de una ráfaga de datos ocasional y la velocidad de cambio de los datos es superior a 10 MBps (para premium) y 2 MBps (para estándar) durante algún tiempo y desciende, la replicación mantendrá el ritmo. Sin embargo, si la renovación supera con creces el límite admitido la mayoría del tiempo, considere una de las siguientes opciones (si es posible):

* **Excluya el disco que provoca la alta velocidad de cambio en los datos**: Puede excluir el disco mediante el uso de [PowerShell](./azure-to-azure-exclude-disks.md). Para excluir el disco debe deshabilitar la replicación en primer lugar.
* **Cambie el nivel de disco de almacenamiento de recuperación ante desastres**: Esta opción solo es posible si la renovación de datos de disco tiene una velocidad inferior a 20 MB/s. Digamos que una máquina virtual con un disco P10 renueva datos a una velocidad mayor que 8 MB/s pero menor que 10 MB/s. Si el cliente puede usar un disco P30 de almacenamiento de destino durante la protección, el problema se puede resolver. Tenga en cuenta que esta solución solo es posible para las máquinas que usan Managed Disks Premium. Siga los pasos siguientes:
    - Vaya a la hoja Discos de la máquina replicada afectada y copie el nombre del disco de réplica.
    - Vaya a este disco administrado de réplica.
    - Es posible que vea un mensaje emergente en la hoja Información general que indica que se ha generado una dirección URL de SAS. Haga clic en este mensaje emergente y cancele la exportación. Omita este paso si no aparece el mensaje emergente.
    - En cuanto se revoque la dirección URL de SAS, vaya a la hoja Configuración del disco administrado y aumente el tamaño para que Site Recovery admita la tasa de renovación observada en el disco de origen.

## <a name="Network-connectivity-problem"></a>Problemas de conectividad de red

### <a name="network-latency-to-a-cache-storage-account"></a>Latencia de red en la cuenta de almacenamiento en caché
Site Recovery envía los datos replicados a la cuenta de almacenamiento en caché. Es posible que vea la latencia de red si la carga los datos de una máquina virtual a la cuenta de almacenamiento en caché es más lenta que 4 MB en 3 segundos.

Para comprobar si hay algún problema relacionado con la latencia, utilice [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) para cargar datos desde la máquina virtual en la cuenta de almacenamiento en caché. Si la latencia es alta, compruebe si está utilizando aplicaciones virtuales de red (NVA) para controlar el tráfico que sale de las máquinas virtuales. Es posible que se genere un cuello de botella si todo el tráfico de replicación pasa por la aplicación virtual de red.

Se recomienda crear un punto de conexión de servicio de red en la red virtual de "Storage" para que el tráfico de replicación no se dirija a la NVA. Para obtener más información, consulte [Configuración de la aplicación virtual de red](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

### <a name="network-connectivity"></a>Conectividad de red
Para que la replicación de Site Recovery funcione, la máquina virtual debe disponer de conectividad saliente a direcciones URL o intervalos IP específicos. Si la máquina virtual está detrás de un firewall o usa reglas de grupo de seguridad de red (NSG) para controlar la conectividad saliente, puede encontrarse alguno de estos problemas. Consulte [Conectividad de salida para las direcciones URL de Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) para comprobar que todas las direcciones URL están conectadas.

## <a name="error-id-153006---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Error ID 153006: No se ha producido ningún punto de recuperación coherente con la aplicación de la máquina virtual en los últimos "XXX" minutos

A continuación se enumeran algunos de los problemas más comunes

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>Causa 1: Incidencia conocida en SQL Server 2008/2008 R2
**Solución:** Existe una incidencia conocida en SQL Server 2008/2008 R2. Consulte este artículo de KB [Copia de seguridad de ASR Agent u otros VSS no componente falla en un servidor que aloja SQL Server 2008 R2](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-auto_close-dbs"></a>Causa 2: Se producen errores en los trabajos de Azure Site Recovery en los servidores que alojan cualquier versión de las instancias de SQL Server con bases de datos AUTO_CLOSE
**Solución:** Consulte el [artículo](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser) de KB


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>Causa 3: Incidencia conocida en SQL Server 2016 y 2017
**Solución:** Consulte el [artículo](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component) de KB

#### <a name="cause-4-you-are-using-storage-spaces-direct-configuration"></a>Causa 4: Está usando la configuración de Espacios de almacenamiento directo
**Solución:** Azure Site Recovery no puede crear un punto de recuperación coherente con la aplicación para la configuración de Espacios de almacenamiento directo. Consulte el artículo para [configurar correctamente la directiva de replicación](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication-s2d-vms)

### <a name="more-causes-due-to-vss-related-issues"></a>Más causas debidas a problemas relacionados con VSS:

Para seguir solucionando el problema, compruebe los archivos en la máquina de origen para obtener el código de error exacto:

    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

¿Cómo buscar los errores en el archivo?
Busque la cadena "vacpError" abriendo el archivo vacp.log en un editor

    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

En el ejemplo anterior, **2147754994** es el código de error que indica el error como se muestra a continuación

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS Writer no está instalado - Error 2147221164

*Solución:* Para generar la etiqueta de coherencia de la aplicación, Azure Site Recovery usa Microsoft Volume Shadow Copy Service (VSS). Instala un proveedor de VSS para su funcionamiento y para realizar instantáneas de la coherencia de las aplicaciones. Este proveedor de VSS se instala como un servicio. En caso de que el servicio del proveedor de VSS no esté instalado, la creación de instantáneas de coherencia de las aplicaciones genera el id. de error 0x80040154 "Clase no registrada". </br>
Consulte el [artículo para solucionar problemas con la instalación de VSS Writer](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) 

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS Writer está deshabilitado - Error 2147943458

**Solución:** Para generar la etiqueta de coherencia de la aplicación, Azure Site Recovery usa Microsoft Volume Shadow Copy Service (VSS). Instala un proveedor de VSS para su funcionamiento y para realizar instantáneas de la coherencia de las aplicaciones. Este proveedor de VSS se instala como un servicio. En caso de que el servicio del proveedor de VSS esté deshabilitado, la creación de instantáneas de coherencia de las aplicaciones generará el id. de error "El servicio especificado está deshabilitado y no se puede iniciar (0x80070422)". </br>

- Si VSS está deshabilitado:
    - Compruebe que el tipo de inicio del servicio de proveedor de VSS está establecido en **Automático**.
    - Reinicie los servicios siguientes:
        - Servicio VSS
        - Proveedor VSS de Azure Site Recovery
        - Servicio VDS

####  <a name="vss-provider-not_registered---error-2147754756"></a>VSS PROVIDER NOT_REGISTERED - Error 2147754756

**Solución:** Para generar la etiqueta de coherencia de la aplicación, Azure Site Recovery usa Microsoft Volume Shadow Copy Service (VSS).
Compruebe si el servicio de proveedor de VSS de Azure Site Recovery está instalado o no. </br>

- Vuelva a intentar la instalación del proveedor con los comandos siguientes:
- Desinstale el proveedor existente: C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd
- Vuelva a instalar: C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd

Compruebe que el tipo de inicio del servicio de proveedor de VSS está establecido en **Automático**.
    - Reinicie los servicios siguientes:
        - Servicio VSS
        - Proveedor VSS de Azure Site Recovery
        - Servicio VDS
