---
title: Preguntas habituales cuando se realizan copias de seguridad de archivos y carpetas con Azure Backup
description: Responde las preguntas habituales acerca de la realización de copias de seguridad de archivos y carpetas con Azure Backup.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: dacurwin
ms.openlocfilehash: dd800c0eeb18fe45b44a72aeb58b500623b2b366
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705091"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Preguntas comunes acerca de la realización de copias de seguridad de archivos y carpetas

Este artículo contiene respuestas a preguntas habituales acerca de la copia de seguridad de archivos y carpetas con el agente de Microsoft Azure Recovery Services (MARS) en el servicio [Azure Backup](backup-overview.md).

## <a name="general"></a>General

## <a name="configure-backups"></a>Configuración de copias de seguridad

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>¿Dónde se puede descargar la versión más reciente del agente de MARS?
La versión más reciente del agente de MARS que se usa al hacer copias de seguridad de equipos con Windows Server machines, del DPM de System Center y del servidor de Microsoft Azure Backup esta disponible para [descargarla](https://aka.ms/azurebackup_agent).

### <a name="how-long-are-vault-credentials-valid"></a>¿Cuál es el tiempo de validez de las credenciales de un almacén?
Las credenciales de almacén expiran a las 48 horas. Si el archivo de credenciales expira, vuelva a descargarlo de Azure Portal.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>¿De qué unidades se pueden hacer copias de seguridad de archivos y carpetas?

No se pueden hacer copias de seguridad de los siguientes unidades y volúmenes:

* Soportes físicos extraíbles: todos los orígenes de elementos de copia de seguridad deben notificarse como corregidos.
* Volúmenes de solo lectura: el volumen debe ser grabable para que el servicio de copia de instantáneas de volumen (VSS) funcione.
* Volúmenes sin conexión: el volumen debe estar en línea para que VSS funcione.
* Recursos compartidos de red: el volumen debe ser local en el servidor para que la copia de seguridad se realice en línea.
* Volúmenes protegidos por BitLocker: el volumen debe desbloquearse antes de que se pueda crear la copia de seguridad.
* Identificación del sistema de archivos: NTFS es el único sistema de archivos admitido.

### <a name="what-file-and-folder-types-are-supported"></a>¿Qué tipos de archivos y carpetas se admiten?

[Más información](backup-support-matrix-mars-agent.md#supported-file-types-for-backup) sobre los tipos de archivos y carpetas que admite la copia de seguridad.

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>¿Puedo usar al agente de MARS para realizar copias de seguridad de archivos y carpetas en una máquina virtual de Azure?  
Sí. Azure Backup proporciona copia de seguridad de nivel de máquina virtual para las máquinas virtuales de Azure Virtual Machines que usen la extensión de máquina virtual para el agente de máquina virtual de Azure. Si desea realizar copias de seguridad de archivos y carpetas del sistema de operativo Windows en la máquina virtual, puede instalar el agente de MARS para hacerlo.

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>¿Se puede usar al agente de MARS para realizar copias de seguridad de archivos y carpetas que se encuentren en un almacenamiento temporal para la máquina virtual de Azure?
Sí. Instale el agente de MARS y realice una copia de seguridad de los archivos y las carpetas del sistema operativo Windows invitado en un almacenamiento temporal.

- Las copias de seguridad de los trabajos dejarán de funcionar cuando se borren los datos del almacenamiento temporal.
- Si los datos del almacenamiento temporal se eliminan, la restauración solo se podrá realizar en un almacenamiento no volátil.

### <a name="how-do-i-register-a-server-to-another-region"></a>¿Cómo se registra un servidor en otra región?

Los datos de la copia de seguridad se envían al centro de datos del almacén en el que está registrado el servidor. La forma más sencilla de cambiar el centro de datos es desinstalar el agente y volver a instalarlo y, después, registrar la máquina en un nuevo almacén de la región que se necesite.

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>¿Admite el agente de MARS la desduplicación de Windows Server 2012?
Sí. El agente de MARS convierte los datos desduplicados en datos normales cuando prepara la operación de copia de seguridad. Luego optimiza los datos para la copia de seguridad, los cifra y los envía al almacén.

## <a name="manage-backups"></a>Administración de copias de seguridad

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>¿Qué pasa si se cambia el nombre de un equipo Windows configurado para copia de seguridad?

Al cambiar el nombre de un equipo Windows, se detienen todas las copias de seguridad que estén configuradas en ese momento.

- Es preciso registrar el nuevo nombre del equipo en el almacén de Backup.
- Cuando se registra el nuevo nombre en el almacén, la primera operación es una *copia de seguridad* completa.
- Si necesita recuperar los datos de una copia de seguridad en el almacén con el nombre de servidor antiguo, utilice la opción de restaurar en una ubicación alternativa en el Asistente para recuperar datos. [Más información](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>¿Cuál es la longitud máxima de la ruta de acceso de un archivo del que se va a realizar una copia de seguridad?
El agente de MARS usa NTFS y utiliza la especificación de longitud de la ruta de acceso a los archivos limitada por la [API de Windows](/windows/desktop/FileIO/naming-a-file#fully-qualified-vs-relative-paths). Si el tamaño de los archivos que desea proteger supera el valor permitido, realice una copia de seguridad de la carpeta primaria o de la unidad de disco.  

### <a name="what-characters-are-allowed-in-file-paths"></a>¿Qué caracteres se permiten en las rutas de acceso de archivo?

El agente de MARS usa NTFS y permite los [caracteres compatibles](/windows/desktop/FileIO/naming-a-file#naming-conventions) en los nombres de archivo o las rutas de acceso.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>Aparece la advertencia "Azure Backups have not been configured for this server" (No se han copiado instancias de Azure Backup para este servidor).
Esta advertencia puede aparecer aunque se haya configurado una directiva de copia de seguridad cuando la configuración de programación de la copia de seguridad almacenada en el servidor local no es la misma que la configuración del almacén de copia de seguridad.
- Cuando el servidor o la configuración se recuperan a un estado válido conocido, las programaciones de copia de seguridad pueden perder la sincronización.
- Si recibe esta advertencia, vuelva a [configurar](backup-azure-manage-windows-server.md) la directiva de copia de seguridad y ejecute una copia de seguridad a petición para volver a sincronizar el servidor local con Azure.


## <a name="manage-the-backup-cache-folder"></a>Administración de la carpeta de caché de las copias de seguridad

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>¿Cuál es el requisito de tamaño mínimo para la carpeta de caché?
El tamaño de la carpeta de caché determina la cantidad de datos de los que se realiza la copia de seguridad.
- Los volúmenes de la carpeta de caché deben tener un espacio libre equivalente, al menos, al 5-10 % del tamaño total de datos de copia de seguridad.
- Si el volumen tiene menos del 5 % de espacio libre, aumente el tamaño del volumen o mueva la carpeta de caché a un volumen que tenga suficiente espacio libre.
- Si se realiza una copia de seguridad del estado del sistema Windows, se necesitan 30-35 GB adicionales de espacio libre en el volumen que contiene la carpeta de caché.

### <a name="how-to-check-if-scratch-folder-is-valid-and-accessible"></a>¿Cómo se puede comprobar si la carpeta temporal es válida y accesible?

1. De forma predeterminada, la carpeta temporal se encuentra en `\Program Files\Microsoft Azure Recovery Services Agent\Scratch`.
2. Asegúrese de que la ruta de acceso de la ubicación de la carpeta temporal coincide con los valores de las entradas de clave del Registro que se muestran a continuación:

  | Ruta de acceso del Registro | Clave del Registro | Valor |
  | --- | --- | --- |
  | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nueva ubicación de la carpeta de la memoria caché* |
  | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nueva ubicación de la carpeta de la memoria caché* |

### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>¿Cómo se cambia la ubicación de la caché del agente de MARS?

1. Ejecute este comando en un símbolo del sistema con privilegios elevados para detener el motor de Azure Backup:

    ```PS C:\> Net stop obengine```

2. No mueva los archivos. En su lugar, copie la carpeta de espacio en caché en otra unidad que tenga suficiente espacio.
3. Actualice las siguientes entradas del Registro con la ruta de acceso de la nueva carpeta de la caché.<br/>

    | Ruta de acceso del Registro | Clave del Registro | Valor |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nueva ubicación de la carpeta de la memoria caché* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nueva ubicación de la carpeta de la memoria caché* |

4. Reinicie el motor de Azure Backup en una ventana del símbolo del sistema con privilegios elevados:

    ```PS C:\> Net start obengine```

5. Si la copia de seguridad finaliza correctamente con la nueva ubicación, puede quitar la carpeta de caché original.


### <a name="where-should-the-cache-folder-be-located"></a>¿Dónde debería estar la carpeta de caché?

No se recomiendan las siguientes ubicaciones para la carpeta de caché:

* Recurso compartido de red o soporte físico extraíble: la carpeta de caché debe ser local para el servidor que necesita realizar copias de seguridad mediante la copia de seguridad en línea. No se admiten ubicaciones de red ni medios extraíbles como unidades USB.
* Volúmenes sin conexión: la carpeta de caché debe estar en línea para la copia de seguridad esperada con el agente de Azure Backup.

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>¿Hay algún atributo de la carpeta de caché que no se admita?
No se admiten los siguientes atributos ni sus combinaciones para la carpeta de caché:

* Cifrados
* Desduplicados
* Comprimidos
* Dispersos
* Punto de reanálisis

Ni la carpeta de caché ni los metadatos del disco duro virtual tienen los atributos necesarios para el agente de Azure Backup.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>¿Hay alguna forma de ajustar la cantidad de ancho de banda que se utiliza para la copia de seguridad?

Sí, puede usar la opción **Cambiar propiedades** del agente de MARS para ajustar el ancho de banda y el tiempo. [Más información](backup-configure-vault.md#enable-network-throttling).

## <a name="restore"></a>Restauración

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>¿Qué ocurre si se cancela un trabajo de restauración en curso?

Si se cancela un trabajo de restauración en curso, se detiene el proceso de restauración. Todos los archivos restaurados antes de la cancelación permanecen en el destino configurado (ubicación original o alternativa) sin las reversiones.


## <a name="next-steps"></a>Pasos siguientes

[Aprenda a](tutorial-backup-windows-server-to-azure.md) hacer copias de seguridad de un equipo Windows.
