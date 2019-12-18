---
title: Acerca del agente de MARS
description: Obtenga información sobre cómo el agente de MARS admite los escenarios de copia de seguridad
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: c036d93c09195c0c330cfe86f307d28866131d9f
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896878"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Acerca del agente de Microsoft Azure Recovery Services (MARS)

En este artículo se describe de qué manera el servicio Azure Backup usa el agente de Microsoft Azure Recovery Services (MARS) para hacer copias de seguridad y restaurar archivos/carpetas, un volumen o el estado del sistema desde un equipo local a Azure.

El agente de MARS admite los escenarios de copia de seguridad siguientes:

![panel del almacén de Servicios de recuperación](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Archivos y carpetas**: Proteja selectivamente archivos y carpetas de Windows.
- **Nivel de volumen**: Proteja todo el volumen de Windows de la máquina.
- **Nivel del sistema**: Proteja todo el estado del sistema Windows.

El agente de MARS admite los siguientes escenarios de restauración:

![panel del almacén de Servicios de recuperación](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

-   **Mismo servidor**: Mismo servidor en el que se creó originalmente la copia de seguridad.
    -    **Archivos y carpetas**: Puede examinar y elegir archivos y carpetas individuales que desee restaurar.
    -    **Nivel de volumen**: Puede elegir el volumen y el punto de recuperación que desea restaurar, y restaurarlo en la misma ubicación o en una ubicación alternativa en la misma máquina.  Puede crear una copia de los archivos existentes, sobrescribir los archivos existentes u omitir la recuperación de los archivos existentes.
    -    **Nivel del sistema**: Puede elegir el estado del sistema y el punto de recuperación para restaurar en la misma máquina en una ubicación especificada.


-   **Servidor alternativo**: Otro servidor, es decir, no el mismo servidor donde se hizo la copia de seguridad.
    -    **Archivos y carpetas**: Puede examinar y elegir archivos y carpetas individuales que desee restaurar del punto de recuperación en una máquina de destino.
    -    **Nivel de volumen**: Para elegir el volumen y el punto de recuperación que desee restaurar en una ubicación alternativa; para ello, puede crear una copia de los archivos existentes, sobrescribir los archivos existentes u omitir la recuperación de los archivos existentes.
    -    **Nivel del sistema**: Puede elegir el estado del sistema y el punto de recuperación para restaurar como archivo de estado del sistema en una máquina alternativa.

## <a name="backup-process"></a>Proceso de copia de seguridad

1.  En Azure Portal, cree un [almacén de Recovery Services](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-recovery-services-vault) y elija los archivos y las carpetas o el estado del sistema de los objetivos de copia de seguridad.
2.  [Descargue](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) las credenciales del almacén de Recovery Services y el instalador del agente en una máquina local. Para proteger la máquina local seleccionando la opción de copia de seguridad, elija archivos y carpetas y estado del sistema y descargue el agente de MARS.
3.  Preparación de la infraestructura:

    a.    Ejecute el instalador para [instalar](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent) el agente.

    b.  Use las credenciales del almacén descargadas para registrar la máquina en el almacén de Recovery Services.
4.  En la consola del agente del cliente, use [programar copia de seguridad](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-backup-policy) para configurar la copia de seguridad. Especifique la directiva de retención de los datos de copia de seguridad y empiece a proteger.

![panel del almacén de Servicios de recuperación](./media/backup-try-azure-backup-in-10-mins/backup-process.png)


### <a name="additional-scenarios"></a>Otros escenarios
-   **Hacer una copia de seguridad de archivos y carpetas específicos dentro de la VM de Azure**: El método principal para hacer copias de seguridad de máquinas virtuales (VM) de Azure es mediante una extensión de Azure Backup en la VM. De esta manera, se realiza la copia de seguridad de toda la máquina virtual. Si quiere hacer una copia de seguridad de archivos y carpetas específicos dentro de una VM, puede instalar el agente de MARS en VM de Azure. [Más información](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup).

-   **Propagación sin conexión**: Las copias de seguridad iniciales completas de datos a Azure transfieren grandes cantidades de datos y requieren un mayor ancho de banda de red en comparación con las copias de seguridad posteriores que solo transfieren los cambios diferenciales e incrementales. Azure Backup permite comprimir las copias de seguridad iniciales. A través del proceso de propagación sin conexión, Azure Backup puede usar discos para cargar los datos comprimidos iniciales de copia de seguridad sin conexión en Azure. [Más información](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-).


## <a name="next-steps"></a>Pasos siguientes
[Matriz de compatibilidad del agente de MARS](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[Preguntas frecuentes del agente de MARS](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
