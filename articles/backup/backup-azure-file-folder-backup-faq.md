---
title: Preguntas frecuentes para la copia de seguridad de archivos y carpetas con Azure Backup
description: Responde a preguntas comunes sobre cómo realizar copias de seguridad de archivos y carpetas con Azure Backup.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: dacurwin
ms.openlocfilehash: 1ac3cdecc79cafb9ea2697cca3c87b2ebe083d40
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254861"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Preguntas comunes sobre cómo realizar copias de seguridad de archivos y carpetas 

Este artículo contiene respuestas a preguntas habituales abundan las copias de archivos y carpetas con el agente de Microsoft Azure Recovery Services (MARS) en el [Azure Backup](backup-overview.md) service.

## <a name="general"></a>General

### <a name="why-does-the-mars-agent-need-net-framework-452-or-higher"></a>¿Por qué se necesita .NET framework 4.5.2 en el agente de MARS o superior?

Nueva funcionalidad disponible en [la restauración instantánea](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) necesita .NET Framework 4.5.2 o superior.

## <a name="configure-backups"></a>Configurar copias de seguridad

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>¿Dónde puedo descargar la versión más reciente del agente de MARS? 
El agente de MARS más reciente que se usan cuando la copia de seguridad de Microsoft Azure Backup server, System Center DPM y máquinas de Windows Server está disponible para [descargar](https://aka.ms/azurebackup_agent). 

### <a name="how-long-are-vault-credentials-valid"></a>¿Cuánto tiempo son las credenciales de almacén válido?
Las credenciales de almacén expiran a las 48 horas. Si expira el archivo de credenciales, descargue el archivo de nuevo desde el portal de Azure.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>¿De qué unidades puedo hacer copias de seguridad de archivos y carpetas? 

No se puede realizar copias de seguridad de los siguientes tipos de volúmenes y unidades:

* Medios extraíbles: todos los orígenes de elementos de copia de seguridad deben notificarse como corregidos.
* Volúmenes de solo lectura: el volumen debe ser grabable para que el servicio de copia de instantáneas de volumen (VSS) funcione.
* Volúmenes sin conexión: el volumen debe estar en línea para que VSS funcione.
* Recursos compartidos de red: el volumen debe ser local en el servidor para que la copia de seguridad se realice en línea.
* Volúmenes protegidos por BitLocker: el volumen debe desbloquearse antes de que se pueda crear la copia de seguridad.
* Identificación del sistema de archivos: NTFS es el único sistema de archivos admitido.

### <a name="what-file-and-folder-types-are-supported"></a>¿Se admiten los tipos de archivos y carpetas?

Se admiten los siguientes tipos:

* Cifrados
* Comprimidos
* Dispersos
* Comprimidos + dispersos
* Vínculos físicos: no compatibles, se omiten
* Punto de repetición de análisis: no compatibles, se omiten
* Cifrados + dispersos: no compatibles, se omiten
* Flujo comprimido: no compatibles, se omiten
* Puntos de análisis, incluyendo vínculos DFS y puntos de unión


### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>¿Puedo usar al agente de MARS para realizar una copia de seguridad de archivos y carpetas en una máquina virtual de Azure?  
Sí. Azure Backup proporciona copia de seguridad de nivel de máquina virtual para máquinas virtuales de Azure con la extensión de máquina virtual para el agente de máquina virtual de Azure. Si desea realizar una copia de seguridad de archivos y carpetas en el sistema de operativo de Windows en la máquina virtual, puede instalar el agente de MARS para hacerlo. 

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>¿Puedo usar al agente de MARS para realizar una copia de seguridad de archivos y carpetas en un almacenamiento temporal para la máquina virtual de Azure? 
Sí. Instale al agente de MARS y realizar una copia de seguridad de archivos y carpetas en el sistema operativo Windows en un almacenamiento temporal. -Se producirá un error en trabajos copia de seguridad sobre el almacenamiento temporal que se borren los datos.
- Si se eliminan los datos de almacenamiento temporal, solo se puede restaurar un almacenamiento no volátil.

### <a name="how-do-i-register-a-server-to-another-region"></a>¿Cómo se puede registrar un servidor en otra región?

Datos de copia de seguridad se envían al centro de datos del almacén en el que el servidor está registrado. La manera más fácil de cambiar el centro de datos consiste en desinstalar y reinstalar el agente y, a continuación, registrar la máquina en un nuevo almacén en la región que necesita

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>¿La compatibilidad con Windows Server 2012 el agente MARS desduplicación se realiza?
Sí. El agente de MARS convierte los datos desduplicados en datos normales cuando prepara la operación de copia de seguridad. Luego optimiza los datos de copia de seguridad, cifra los datos y, a continuación, envía los datos cifrados en el almacén.

## <a name="manage-backups"></a>Administración de copias de seguridad

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>¿Qué ocurre si cambio el nombre de un equipo de Windows configurado para copia de seguridad?

Al cambiar el nombre de una máquina de Windows, se detienen todas las copias de seguridad configuradas actualmente.

- Deberá registrar el nuevo nombre del equipo con el almacén de copia de seguridad.
- Al registrar el nuevo nombre en el almacén, la primera operación es un *completa* copia de seguridad.
- Si necesita recuperar datos de copia de seguridad en el almacén con el nombre antiguo del servidor, utilice la opción para restaurar en una ubicación alternativa en el Asistente para recuperar datos. [Más información](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine). 

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>¿Qué es la longitud de ruta de acceso de archivo máximo para copia de seguridad?
El agente de MARS se basa en NTFS y usa la especificación de longitud filepath limitada por la [Windows API](/windows/desktop/FileIO/naming-a-file#fully_qualified_vs._relative_paths). Si los archivos que desea proteger se supera el valor permitido, copia de seguridad de la carpeta primaria o la unidad de disco.  

### <a name="what-characters-are-allowed-in-file-paths"></a>¿Qué caracteres se permiten en las rutas de acceso de archivo?

El agente de MARS se basa en NTFS y permite [admite caracteres](/windows/desktop/FileIO/naming-a-file#naming_conventions) en nombres o rutas de acceso del archivo.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>Aparece la advertencia "Copias de seguridad de Azure no hayan sido configurados para este servidor".
Esta advertencia puede aparecer incluso si ha configurado una directiva de copia de seguridad, cuando la configuración de programación de copia de seguridad almacenada en el servidor local no es igual que la configuración almacenada en el almacén de copia de seguridad.
- Cuando se recuperan del servidor o la configuración a un estado válido conocido, las programaciones de copia de seguridad pueden perder la sincronización.
- Si aparece esta advertencia, [configurar](backup-azure-manage-windows-server.md) la directiva de copia de seguridad nuevo y, a continuación, copia de seguridad de ejecución y a petición para sincronizar el servidor local con Azure.


## <a name="manage-the-backup-cache-folder"></a>Administrar la carpeta de caché de copia de seguridad

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>¿Cuál es el requisito de tamaño mínimo para la carpeta de caché?
El tamaño de la carpeta de caché determina la cantidad de datos de los que se realiza la copia de seguridad.
- Los volúmenes de la carpeta de caché deben tener espacio libre que es igual al menos 5-10% del tamaño total de datos de copia de seguridad.
- Si el volumen tiene menos del 5% de espacio libre, aumente el tamaño del volumen o mueva la carpeta de caché a un volumen con espacio suficiente.
- Si la copia de seguridad del estado del sistema Windows, necesitaría 30-35 GB adicionales de espacio libre en el volumen que contiene la carpeta de caché
### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>¿Cómo se puede cambiar la ubicación de caché para el agente de MARS?


1. Ejecute este comando en un símbolo del sistema con privilegios elevados para detener el motor de copia de seguridad:

    ```PS C:\> Net stop obengine```

2. No mueva los archivos. En su lugar, copie la carpeta de caché de espacio en una unidad diferente que tenga espacio suficiente.
3. Actualice las siguientes entradas del registro con la ruta de acceso de la nueva carpeta de caché.<br/>

    | Ruta de acceso del Registro | Clave del Registro | Valor |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nueva ubicación de la carpeta de la memoria caché* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nueva ubicación de la carpeta de la memoria caché* |

4. Reinicie el motor de copia de seguridad en un símbolo del sistema con privilegios elevados:

    ```PS C:\> Net start obengine```

5. Al finalizar la copia de seguridad correctamente con la nueva ubicación, puede quitar la carpeta de caché original.


### <a name="where-should-the-cache-folder-be-located"></a>¿Dónde debería estar la carpeta de caché?

No se recomiendan las siguientes ubicaciones para la carpeta de caché:

* Medios extraíbles o recurso compartido de red: la carpeta de caché debe ser local para el servidor que necesita realizar copias de seguridad mediante la copia de seguridad en línea. No se admiten ubicaciones de red ni medios extraíbles como unidades USB.
* Volúmenes sin conexión: la carpeta de caché debe estar en línea para la copia de seguridad esperada con el agente de Azure Backup.

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>¿Hay algún atributo de la carpeta de caché que no es compatibles?
No se admiten los siguientes atributos ni sus combinaciones para la carpeta de caché:

* Cifrados
* Desduplicados
* Comprimidos
* Dispersos
* Punto de reanálisis

Ni la carpeta de caché ni los metadatos del disco duro virtual tienen los atributos necesarios para el agente de Azure Backup.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>¿Hay alguna forma de ajustar la cantidad de ancho de banda utilizado para copia de seguridad?
 
Sí, puede usar el **cambiar propiedades** opción en el agente de MARS para ajustar el ancho de banda y el control de tiempo. [Obtenga más información](backup-configure-vault.md#enable-network-throttling)**.

## <a name="restore"></a>Restauración

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>¿Qué ocurre si se cancela un trabajo de restauración en curso?

Si se cancela un trabajo de restauración en curso, se detiene el proceso de restauración. Todos los archivos restaurados antes de la cancelación permanecen en destino configurado (ubicación original o alternativa), sin las reversiones.


## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre](tutorial-backup-windows-server-to-azure.md) cómo realizar copias de seguridad de una máquina de Windows.
