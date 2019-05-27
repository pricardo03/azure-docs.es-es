---
title: 'Solución de problemas: Azure Disk Encryption para VM de IaaS | Microsoft Docs'
description: En este artículo se ofrecen sugerencias para solucionar problemas de Microsoft Azure Disk Encryption para máquinas virtuales IaaS con Windows y Linux.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/12/2019
ms.custom: seodec18
ms.openlocfilehash: 35d494702673d59290a0073c55135138f533b8bf
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956698"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Guía de solución de problemas de Azure Disk Encryption

Esta guía está destinada a profesionales de tecnologías de la información (TI), analistas de seguridad de la información y administradores de la nube cuyas organizaciones utilizan Azure Disk Encryption. En este artículo se ofrece ayuda para solucionar problemas relacionados con el cifrado de disco.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="troubleshooting-linux-os-disk-encryption"></a>Solución de problemas del cifrado de discos con el SO Linux

El cifrado de discos con el sistema operativo Linux debe desmontar la unidad del sistema operativo antes de pasar por el proceso de cifrado de disco completo. Si no puede desmontar la unidad, es probable que aparezca el mensaje de error "no se pudo desmontar después de...".

Este error suele ocurrir cuando se intenta el cifrado del disco del sistema operativo en un entorno de máquina virtual de destino que se ha modificado desde su imagen de la galería admitida. Las desviaciones de la imagen admitida pueden interferir con la capacidad de la extensión para desmontar la unidad del sistema operativo. Algunos ejemplos de desviaciones pueden incluir los siguientes elementos:
- Imágenes personalizadas que ya no coinciden con un sistema de archivos o esquema de particiones compatibles.
- Las aplicaciones grandes, como SAP, MongoDB, Apache Cassandra y Docker, no se admiten cuando están instaladas y se ejecutan en el sistema operativo antes de que se realice el cifrado. Azure Disk Encryption no puede apagar con seguridad estos procesos tal como se requiere en la preparación de la unidad del sistema operativo para el cifrado de disco. Si hay procesos que siguen activos y que contienen identificadores de archivos abiertos en la unidad del sistema operativo, dicha unidad no se podrá desmontar y se producirá un error al cifrarla. 
- Se ejecutan scripts personalizados casi al mismo tiempo que se deshabilita el cifrado, o si se hace algún otro cambio en la máquina virtual durante el proceso de cifrado. Este conflicto puede ocurrir cuando una plantilla de Azure Resource Manager define varias extensiones para ejecutarse al mismo tiempo o cuando una extensión de script personalizada u otra acción se ejecutan simultáneamente que el cifrado del disco. La serialización y el aislamiento de estos pasos podría resolver el problema.
- Security Enhanced Linux (SELinux) no se ha deshabilitado antes de habilitar el cifrado, por lo que se ha producido un error en el paso de desmontaje. SELinux puede habilitarse de nuevo después de completarse el cifrado.
- El disco del sistema operativo utiliza un esquema de administrador de volúmenes lógicos (LVM). Aunque hay compatibilidad limitada con los discos de datos LVM, no se admite el disco del sistema operativo LVM.
- No se cumplen los requisitos mínimos de memoria (se recomiendan 7 GB para el cifrado del disco de sistema operativo).
- Las unidades de datos se han montado recursivamente en el directorio /mnt/ o entre sí (por ejemplo, /mnt/datos1, /mnt/datos2, /datos3 + /datos3/datos4).
- No se cumplen otros [requisitos previos](azure-security-disk-encryption-prerequisites.md) de Azure Disk Encryption para Linux.

## <a name="bkmk_Ubuntu14"></a> Actualización del kernel predeterminado para Ubuntu 14.04 LTS

La imagen de Ubuntu 14.04 LTS se suministra con una versión de kernel predeterminada de 4.4. Esta versión de kernel tiene un problema conocido por el cual el cancelador de procesos por memoria insuficiente (OOM Killer) finaliza incorrectamente el comando dd durante el proceso de cifrado del sistema operativo. Este error se ha corregido en el kernel Linux más reciente ajustado para Azure. Para evitar este error, antes de habilitar el cifrado en la imagen, actualice al [kernel optimizado para Azure 4.15](https://packages.ubuntu.com/trusty/linux-azure) o posterior con los siguientes comandos:

```
sudo apt-get update
sudo apt-get install linux-azure
sudo reboot
```

Una vez la VM se haya reiniciado en el nuevo kernel, la nueva versión del kernel se puede confirmar con:

```
uname -a
```

## <a name="update-the-azure-virtual-machine-agent-and-extension-versions"></a>Actualizar el agente de máquina Virtual de Azure y las versiones de extensión

Operaciones de Azure Disk Encryption pueden producir un error en las imágenes de máquina virtual con las versiones no compatibles del agente de máquina Virtual de Azure. Las imágenes de Linux con versiones anteriores a 2.2.38 del agente deben actualizarse antes de habilitar el cifrado. Para obtener más información, consulte [cómo actualizar el agente de Linux de Azure en una máquina virtual](../virtual-machines/extensions/update-linux-agent.md) y [soporte de versión mínima para los agentes de máquina virtual en Azure](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

La versión correcta de la extensión del agente invitado Microsoft.Azure.Security.AzureDiskEncryption o Microsoft.Azure.Security.AzureDiskEncryptionForLinux también es necesaria. Las versiones de extensión se mantienen y actualizan automáticamente por la plataforma cuando se cumplen los requisitos previos del agente de máquina Virtual de Azure y se usa una versión compatible del agente de máquina virtual.

La extensión Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux está desusada y ya no se admite.  

## <a name="unable-to-encrypt-linux-disks"></a>No se pueden cifrar los discos de Linux

En algunos casos, parece que el cifrado del disco de Linux se atasca en el mensaje que indica que se inició el cifrado de disco del sistema operativo, y SSH se deshabilita. El proceso de cifrado puede tardar entre 3 y 16 horas en completarse en una imagen de la galería de existencias. Si se agregan discos de datos de varios terabytes, el proceso puede tardar días.

La secuencia de cifrado del disco del sistema operativo Linux desmonta la unidad del sistema operativo temporalmente. A continuación, realiza el cifrado de bloque a bloque de todo el disco del sistema operativo, antes de que vuelva a montarse en su estado cifrado. A diferencia de Azure Disk Encryption en Windows, el cifrado del disco de Linux no permite el uso simultáneo de la máquina virtual mientras el cifrado está en curso. Las características de rendimiento de la máquina virtual pueden suponer una diferencia significativa en el tiempo necesario para el cifrado completo. Estas características incluyen el tamaño del disco y si la cuenta de almacenamiento es almacenamiento premium (SSD) o estándar.

Para comprobar el estado de cifrado, sondee el **ProgressMessage** los campos devueltos desde el [Get AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) comando. Mientras se cifra la unidad del sistema operativo, la máquina virtual entra en estado de mantenimiento y deshabilita SSH para evitar cualquier interrupción en el proceso en curso. El mensaje **EncryptionInProgress** aparece la mayoría del tiempo mientras el cifrado está en curso. Varias horas después, se muestra un mensaje **VMRestartPending** que le pide que reinicie la máquina virtual. Por ejemplo:


```azurepowershell
PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Tras reiniciar la máquina virtual cuando se le pida, debe esperar de dos a tres minutos para el reinicio y para que se realicen los pasos finales en el destino. El mensaje de estado cambia cuando el cifrado se completa finalmente. Cuando este mensaje esté disponible, es de esperar que la unidad de sistema operativo cifrada y la máquina virtual estén listas para usarse de nuevo.

En los casos siguientes, se recomienda que restaure la máquina virtual con la instantánea o la copia de seguridad realizada inmediatamente antes del cifrado:
   - Si la secuencia de reinicio descrita anteriormente no se produce.
   - Si la información de inicio, el mensaje de progreso u otro indicador informa de que se ha producido un error en el cifrado del sistema operativo en mitad de este proceso. Un ejemplo de mensaje es el error "no se pudo desmontar" que se describe en esta guía.

Antes de volver a intentarlo, vuelva a evaluar las características de la máquina virtual y asegúrese de que se cumplen todos los requisitos previos.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Solución de problemas de Azure Disk Encryption detrás de un firewall
Cuando la conectividad está limitada por un firewall, la configuración del grupo de seguridad de red (NSG) o el requisito de proxy, podría perderse la capacidad de la extensión para realizar las tareas necesarias. Esto puede dar lugar a mensajes de estado similares a "El estado de extensión no está disponible en la máquina virtual". En los escenarios esperados, el cifrado no puede finalizar. En las secciones siguientes se enumeran algunos problemas comunes del firewall que podría investigar.

### <a name="network-security-groups"></a>Grupos de seguridad de red
Parte de la configuración del grupo de seguridad de red que se aplica debe permitir todavía que el punto de conexión cumpla con los [requisitos previos](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) documentados de la configuración de red para el cifrado del disco.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Key Vault detrás de un firewall

Cuando se está habilitando el cifrado con [credenciales de Azure AD](azure-security-disk-encryption-prerequisites-aad.md), la máquina virtual de destino debe permitir la conectividad a los puntos de conexión de Azure Active Directory y Key Vault. Los puntos de conexión de autenticación actuales de Azure Active Directory se mantienen en las secciones 56 y 59 de la documentación sobre los [intervalos de direcciones IP y direcciones URL de Office 365](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges). En la documentación sobre cómo [acceder a Azure Key Vault detrás de un firewall](../key-vault/key-vault-access-behind-firewall.md) se proporcionan instrucciones de Key Vault.

### <a name="azure-instance-metadata-service"></a>Servicio de metadatos de instancia de Azure 
La máquina virtual debe poder acceder al punto de conexión de [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) que utiliza una dirección IP no enrutable conocida (`169.254.169.254`) a la que solo se puede acceder desde la máquina virtual.  No se admiten las configuraciones de proxy que modifican el tráfico HTTP local a esta dirección (por ejemplo, agregando un encabezado X-Forwarded-For).

### <a name="linux-package-management-behind-a-firewall"></a>Administración de paquetes de Linux detrás de un firewall

En el entorno de ejecución, Azure Disk Encryption para Linux se basa en el sistema de administración de paquetes de la distribución de destino para instalar los componentes necesarios conforme a los requisitos previos antes de habilitar el cifrado. Si la configuración de firewall impide que la máquina virtual pueda descargar e instalar estos componentes, es previsible que se produzcan errores. Los pasos para configurar este sistema de administración de paquetes pueden variar según la distribución. En Red Hat, cuando se necesite un proxy, debe asegurarse de que el administrador de la suscripción y yum están configurados correctamente. Para obtener más información, consulte [cómo solucionar problemas de administrador de suscripción y yum](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Solución de problemas de Windows Server 2016 Server Core

En Windows Server 2016 Server Core, el componente bdehdcfg no está disponible de forma predeterminada. y Azure Disk Encryption necesita dicho componente. Se utiliza para dividir el volumen del sistema del volumen del sistema operativo, lo que se realiza una única vez en toda la duración de la máquina virtual. Estos archivos binarios no son necesarios durante las operaciones de cifrado posteriores.

Para solucionar este problema, copie los cuatro archivos siguientes de una máquina virtual del centro de datos de Windows Server 2016 en la misma ubicación de Server Core:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

1. Escriba el comando siguiente:

   ```
   bdehdcfg.exe -target default
   ```

1. Este comando crea una partición de sistema de 550 MB. Reinicie el sistema.

1. Use DiskPart para comprobar los volúmenes y, a continuación, siga.  

Por ejemplo:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="troubleshooting-encryption-status"></a>Solución de problemas de estado del cifrado 

El portal es posible que muestre un disco cifrado incluso después de haber sido sin cifrar en la máquina virtual.  Esto puede ocurrir cuando se usan comandos de bajo nivel para descifrar directamente el disco de la máquina virtual, en lugar de usar los comandos de administración de Azure Disk Encryption de nivel superior.  Los comandos del nivel superior no solo descifrar el disco de la máquina virtual, pero fuera de la máquina virtual también actualizan configuración de cifrado de nivel de plataforma importante y configuración de la extensión asociada con la máquina virtual.  Si estos no se mantienen en la alineación, la plataforma no podrá informar del estado de cifrado ni aprovisionar la máquina virtual correctamente.   

Para deshabilitar el cifrado de disco de Azure con PowerShell, use [Disable AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) seguido [Remove-AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension). Ejecute Remove-AzVMDiskEncryptionExtension antes de deshabilita el cifrado se producirá un error.

Para deshabilitar Azure Disk Encryption con la CLI, utilice [deshabilitar el cifrado de máquinas virtuales de az](/cli/azure/vm/encryption). 

## <a name="next-steps"></a>Pasos siguientes

En este documento, aprendió más acerca de algunos problemas comunes de Azure Disk Encryption y cómo solucionarlos. Para más información acerca de este servicio y su funcionalidad, consulte los artículos siguientes:

- [Aplicación de cifrado de discos en Azure Security Center](../security-center/security-center-apply-disk-encryption.md)
- [Cifrado de datos en reposo de Azure](azure-security-encryption-atrest.md)
