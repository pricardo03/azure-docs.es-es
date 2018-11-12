---
title: Habilitación de la copia de seguridad de Azure Stack desde el portal de administración | Microsoft Docs
description: Habilite el servicio Infrastructure Backup con el portal de administración para que Azure Stack se pueda restaurar si se produce un error.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: d7d47b61c926c6704a06dacc55f00d77a1266988
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038372"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Habilitación de la copia de seguridad de Azure Stack desde el portal de administración
Habilite el servicio Infrastructure Backup con el portal de administración para que Azure Stack pueda generar copias de seguridad. Puede utilizar estas copias de seguridad para restaurar el entorno mediante recuperación en la nube si se produce un [error catastrófico](.\azure-stack-backup-recover-data.md). La finalidad de la recuperación en la nube es garantizar que los operadores y usuarios puedan volver a iniciar sesión en el portal una vez que se complete la recuperación. Los usuarios tendrán sus suscripciones restauradas, incluidos los permisos de acceso basado en roles y los roles, los planes originales, las ofertas, así como el proceso, el almacenamiento y las cuotas de red definidos previamente.

Sin embargo, el servicio Copia de seguridad de infraestructura no realiza copias de seguridad de las máquinas virtuales de IaaS, las configuraciones de red y los recursos de almacenamiento, como las cuentas de almacenamiento, los blobs, las tablas, etc., por lo que los usuarios que inicien sesión después de la recuperación en la nube no verán ninguno de sus recursos previamente existentes. El servicio también hace una copia de seguridad de los recursos y los datos de Plataforma como servicio (PaaS). 

Los administradores y los usuarios son los responsables de realizar copias de seguridad de los recursos de IaaS y PaaS y restaurar dichos recursos por separado a partir de los procesos de copia de seguridad de la infraestructura. Para información sobre la copia de seguridad de los recursos de IaaS y PaaS, vea los siguientes vínculos:

- [Máquinas virtuales](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


## <a name="enable-or-reconfigure-backup"></a>Habilitación o reconfiguración de la copia de seguridad

1. Abra [el portal de administración de Azure Stack](azure-stack-manage-portals.md).
2. Seleccione **Todos los servicios** y, a continuación, en la categoría **ADMINISTRACIÓN**, seleccione **Copia de seguridad de infraestructura**. Elija **Configuración** en la hoja **Copia de seguridad de infraestructura**.
3. Escriba la ruta de acceso a la **ubicación de almacenamiento de la copia de seguridad**. Utilice una cadena de convención de nomenclatura universal (UNC) para la ruta de acceso de un recurso compartido de archivos hospedado en un dispositivo independiente. Una cadena UNC especifica la ubicación de recursos como archivos compartidos o dispositivos. Para el servicio, puede usar una dirección IP. Para garantizar la disponibilidad de los datos de copia de seguridad después de un desastre, el dispositivo debe estar en una ubicación independiente.

    > [!Note]  
    > Si el entorno admite la resolución de nombres de la red de infraestructura de Azure Stack para su entorno empresarial, puede usar un nombre de dominio completo en lugar de la dirección IP.
    
4. Escriba el **nombre de usuario** con el dominio y el nombre de usuario con acceso suficiente para leer y escribir archivos. Por ejemplo, `Contoso\backupshareuser`.
5. Escriba la **Contraseña** del usuario.
6. Escriba la contraseña de nuevo para **Confirmar la contraseña**.
7. La **frecuencia en horas** determina con qué frecuencia se crean las copias de seguridad. El valor predeterminado es 12. Scheduler admite un máximo de 12 y un mínimo de 4. 
8. El **período de retención en días** determina cuántos días de copias de seguridad se conservan en la ubicación externa. El valor predeterminado es 7. Scheduler admite un máximo de 14 y un mínimo de 2. Las copias de seguridad anteriores al período de retención se eliminan automáticamente de la ubicación externa.

    > [!Note]  
    > Si desea archivar las copias de seguridad anteriores al período de retención, asegúrese de hacer una copia de seguridad de los archivos antes de que Scheduler las elimine. Si reduce el período de retención de copia de seguridad (por ejemplo, de 7 a 5 días), Scheduler eliminará todas las copias de seguridad anteriores al período de retención nuevo. Asegúrese de que desea que las copias de seguridad se eliminen antes de actualizar este valor. 

9. Proporcione una clave precompartida en la casilla **Clave de cifrado**. Los archivos de copia de seguridad se cifran mediante esta clave. Asegúrese de almacenar esta clave en una ubicación segura. Una vez que configure esta clave por primera vez o la rote en el futuro, no podrá verla desde esta interfaz. Para crear la clave, ejecute los siguientes comandos de PowerShell de Azure Stack:
    ```powershell
    New-AzsEncryptionKeyBase64
    ```
10. Seleccione **Aceptar** para guardar la configuración del controlador de copia de seguridad.

    ![Azure Stack: configuración del controlador de copia de seguridad](media\azure-stack-backup\backup-controller-settings.png)

## <a name="start-backup"></a>Inicio de la copia de seguridad
Para iniciar una copia de seguridad, haga clic en **Backup now** (Hacer copia de seguridad ahora) y comience la copia de seguridad a petición. Una copia de seguridad a petición no modificará la hora de la siguiente copia de seguridad programada. Una vez completada la tarea, puede confirmar la configuración en **Información esencial**:

![Azure Stack: copia de seguridad a petición](media\azure-stack-backup\scheduled-backup.png)

También puede ejecutar el cmdlet de PowerShell **Start-AzsBackup** en el equipo de administración de Azure Stack. Para obtener más información, consulte [Copia de seguridad de Azure Stack](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>Habilitación o deshabilitación de las copias de seguridad automáticas
Las copias de seguridad se programan automáticamente cuando se habilita la copia de seguridad. Puede comprobar el momento en que se realizará la próxima copia de seguridad en **Información esencial**. 

![Azure Stack: copia de seguridad a petición](media\azure-stack-backup\on-demand-backup.png)

Si necesita deshabilitar futuras copias de seguridad programadas, haga clic en **Deshabilitar copias de seguridad automáticas**. Al deshabilitar las copias de seguridad automáticas, se mantendrá la configuración y la programación de las copias de seguridad. De este modo, simplemente indica al programador que omita las futuras copias de seguridad. 

![Azure Stack: deshabilitar las copias de seguridad programadas](media\azure-stack-backup\disable-auto-backup.png)

Confirme en **Información esencial** que se han deshabilitado las copias de seguridad programadas futuras:

![Azure Stack: confirmar que se han deshabilitado las copias de seguridad](media\azure-stack-backup\confirm-disable.png)

Haga clic en **Habilitar copias de seguridad automáticas** para informar a Scheduler de que deben iniciarse las futuras copias de seguridad en el momento programado. 

![Azure Stack: habilitar las copias de seguridad programadas](media\azure-stack-backup\enable-auto-backup.png)


> [!Note]  
> Si ha configurado la copia de seguridad de la infraestructura antes de actualizar a la versión 1807, se deshabilitarán las copias de seguridad automáticas. De este modo, las copias de seguridad iniciadas por Azure Stack no entran en conflicto con las iniciadas por un motor de programación de tareas externo. Una vez que deshabilite los programadores de tareas externos, haga clic en **Habilitar copias de seguridad automáticas**.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a ejecutar una copia de seguridad. Consulte [Copia de seguridad de Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Aprenda a comprobar que la copia de seguridad se ha ejecutado. Consulte [Confirmación de la copia de seguridad realizada en el portal de administración](azure-stack-backup-back-up-azure-stack.md).
