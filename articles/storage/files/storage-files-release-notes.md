---
title: Notas de la versión del agente de Azure File Sync | Microsoft Docs
description: Notas de la versión del agente de Azure File Sync.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 12/10/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 5bea4c655e9a8970d8d0d946827cc3e46e7efa7a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255166"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Notas de la versión del agente de Azure File Sync
Azure File Sync le permite centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Las instalaciones de Windows Server se transforman en una memoria caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a los datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

En este artículo se tratan las notas de la versión de las versiones compatibles del agente de Azure File Sync.

## <a name="supported-versions"></a>Versiones compatibles
Las siguientes versiones son compatibles con Azure File Sync:

| Hito | Número de versión del agente | Fecha de lanzamiento | Status |
|----|----------------------|--------------|------------------|
| Paquete acumulativo de las actualizaciones de diciembre: [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10 de diciembre de 2018 | Compatible (versión recomendada) |
| Paquete acumulativo de actualizaciones de diciembre | 4.1.0.0 | 4 de diciembre de 2018 | Compatible |
| Versión V4 | 4.0.1.0 | 13 de noviembre de 2018 | Compatible |
| Paquete acumulativo de actualizaciones de septiembre | 3.3.0.0 | 24 de septiembre de 2018 | Compatible |
| Paquete acumulativo de actualizaciones de agosto | 3.2.0.0 | 15 de agosto de 2018 | Compatible |
| Disponibilidad general | 3.1.0.0 | 19 de julio de 2018 | Compatible |
| Paquete acumulativo de actualizaciones de junio | 3.0.13.0 | 29 de junio de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Actualización 2 | 3.0.12.0 | 22 de mayo de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Paquete acumulativo de actualizaciones de abril | 2.3.0.0 | 8 de mayo de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Paquete acumulativo de actualizaciones de marzo | 2.2.0.0 | 12 de marzo de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Paquete acumulativo de actualizaciones de febrero | 2.1.0.0 | 28 de febrero de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Actualización 1 | 2.0.11.0 | 8 de febrero de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Paquete acumulativo de actualizaciones de enero | 1.4.0.0 | 8 de enero de 2018 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Paquete acumulativo de actualizaciones de noviembre | 1.3.0.0 | 30 de noviembre de 2017 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Paquete acumulativo de actualizaciones de octubre | 1.2.0.0 | 31 de octubre de 2017 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |
| Versión preliminar inicial | 1.1.0.0 | 26 de septiembre de 2017 | No se admite: la versión del agente expiró el 1 de octubre de 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Directiva de actualización del agente de Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-4200"></a>Versión del agente 4.2.0.0
Las notas siguientes corresponden a la versión 4.2.0.0 del agente de Azure File Sync (publicada el 10 de diciembre de 2018). Estas notas se suman a las notas de la versión enumeradas para la versión 4.0.1.0.

Lista de problemas corregidos en esta versión:  
- Puede producirse un error de detención 0x3B o un error de detención 0x1E al crear una instantánea de VSS.  
- Puede producirse una pérdida de memoria al habilitarse la nube por niveles  

## <a name="agent-version-4100"></a>Versión del agente 4.1.0.0
Las notas siguientes corresponden a la versión 4.1.0.0 del agente de Azure File Sync (publicada el 4 de diciembre de 2018). Estas notas se suman a las notas de la versión enumeradas para la versión 4.0.1.0.

Lista de problemas corregidos en esta versión:  
- El servidor puede dejar de responder debido a una pérdida de memoria con la nube por niveles.  
- Se produce un error en la instalación del agente con el siguiente error: Error 1921. No se pudo detener el servicio "Agente de sincronización de almacenamiento" (FileSyncSvc).  Compruebe que tiene suficientes privilegios para detener servicios del sistema.  
- El servicio del agente de sincronización de almacenamiento (FileSyncSvc) puede bloquearse cuando el uso de memoria es alto.  
- Mejoras de confiabilidad varias para la sincronización y la nube por niveles.

## <a name="agent-version-4010"></a>Versión del agente 4.0.1.0
Las notas siguientes corresponden a la versión 4.0.1.0 del agente de Azure File Sync (publicada el 13 de noviembre de 2018).

### <a name="evaluation-tool"></a>Herramienta de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante la herramienta de evaluación de Azure File Sync. Esta herramienta es un cmdlet de AzureRM PowerShell que busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Para la instalación y las instrucciones de uso, consulte la sección [Herramienta de evaluación](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) en la Guía de planeación. 

### <a name="agent-installation-and-server-configuration"></a>Instalación del agente y configuración del servidor
Para más información sobre cómo instalar y configurar el agente de Azure File Sync con Windows Server, vea [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md) y [How to deploy Azure File Sync](storage-sync-files-deployment-guide.md) (Implementación de Azure Files Sync).

- El paquete de instalación del agente debe instalarse con permisos elevados (administrador).
- El agente no es compatible con las opciones de implementación de Windows Server Core ni Nano Server.
- El agente solo se admite en Windows Server 2016 y Windows Server 2012 R2.
- El agente requiere al menos 2 GB de memoria. Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina virtual debe configurarse con un mínimo de 2048 MiB de memoria.
- El servicio del agente de sincronización de Storage (FileSyncSvc) no admite puntos de conexión de servidor ubicados en un volumen que tiene el directorio de información del volumen del sistema (SVI) comprimido. Esta configuración producirá resultados inesperados.
- Puede producirse un error de detención 0x3B o un error de detención 0x1E al crear una instantánea de VSS.

### <a name="interoperability"></a>Interoperabilidad
- Las aplicaciones antivirus, de copia de seguridad y de otro tipo que accedan a archivos organizados en niveles pueden provocar recuperaciones no deseadas, salvo que respeten el atributo sin conexión y pasen por alto la lectura del contenido de esos archivos. Para más información, vea [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md).
- Los filtros de archivos del Administrador de recursos del servidor de archivos (FSRM) pueden provocar un gran número de errores de sincronización cuando los archivos están bloqueados debido al filtro de archivos.
- No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y puede provocar resultados inesperados. El agente de Azure File Sync se debe instalar después de implementar la imagen del servidor y completar la instalación mínima de la herramienta de preparación del sistema.
- En el mismo volumen no se puede desduplicar datos y ordenar en niveles en la nube.

### <a name="sync-limitations"></a>Limitaciones de la sincronización
Los siguientes elementos no se sincronizan, pero el resto del sistema funciona con normalidad:
- Archivos con caracteres no admitidos. Consulte la [Guía de solución de problemas](storage-sync-files-troubleshoot.md#handling-unsupported-characters) para obtener la lista de caracteres no admitidos.
- Archivos o directorios que terminan con un punto.
- Rutas de acceso de más de 2,048 caracteres.
- La parte de la lista de control de acceso discrecional (DACL) de un descriptor de seguridad, si es mayor de 2 KB. (Este problema aplica únicamente si tiene más de unas 40 entradas de control de acceso [ACE] en un solo elemento).
- La parte de la lista de control de acceso del sistema (SACL) de un descriptor de seguridad que se utiliza para la auditoría.
- Atributos extendidos.
- Flujos de datos alternativos.
- Puntos de repetición de análisis.
- Vínculos físicos.
- La compresión (si se establece en un archivo de servidor) no se conserva al sincronizarse los cambios con ese archivo desde otros puntos de conexión.
- Los archivos cifrados con EFS (u otros cifrados con modo de usuario) que evitan que el servicio lea los datos.

    > [!Note]  
    > Azure File Sync siempre cifra los datos en tránsito. Los datos siempre se cifran en reposo en Azure.
 
### <a name="server-endpoint"></a>Punto de conexión de servidor
- Un punto de conexión de servidor solo se puede crear en un volumen NTFS. En este momento Azure File Sync no admite ReFS, FAT, FAT32 ni otros sistemas de archivos.
- Los archivos en niveles se volverán inaccesibles si no se recuperan antes de eliminar el punto de conexión de servidor. Para restaurar el acceso a los archivos, vuelva a crear el punto de conexión de servidor. Si han pasado 30 días desde que se eliminó el punto de conexión de servidor o si se ha eliminado el punto de conexión en la nube, los archivos en niveles que no se hayan recuperado serán inutilizables.
- Los niveles de nube no se admiten en el volumen del sistema. Para crear un punto de conexión de servidor en el volumen del sistema, deshabilite los niveles de la nube al crear el punto de conexión de servidor.
- Los clústeres de conmutación por error solo son compatibles con discos en clúster, pero no con volúmenes compartidos de clúster (CSV).
- No se puede anidar un punto de conexión de servidor. Puede coexistir en el mismo volumen en paralelo con otro punto de conexión.
- No almacene un archivo de paginación de aplicación o sistema operativo en una ubicación de punto de conexión de servidor.
- Si se cambia el nombre del servidor, no se actualiza en el portal. Para actualizar el nombre del servidor en el portal, anule el registro y vuelva a registrar el servidor.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
- Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión de nube una vez cada 24 horas. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización.
- El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a un grupo de recursos o suscripción diferentes dentro del inquilino de Azure AD existente. Si se mueve la cuenta de almacenamiento, debe dar acceso al servicio File Sync híbrido a la cuenta de almacenamiento (consulte el apartado [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync no permite mover la suscripción a un inquilino de Azure AD distinto.

### <a name="cloud-tiering"></a>Niveles de nube
- La configuración de directiva de nube por niveles basada en fechas se utiliza para especificar los archivos que deben almacenarse en caché si se accede a ellos en un número de días especificado. Para obtener más información, consulte la [información general de nube por niveles](https://docs.microsoft.com/azure/storage/files/storage-sync-cloud-tiering#afs-force-tiering).
- Si un archivo en niveles se copia en otra ubicación mediante el uso de Robocopy, el archivo resultante no estará en niveles. El atributo sin conexión podría estar establecido porque Robocopy incluye incorrectamente dicho atributo en las operaciones de copia.
- Al copiar los archivos mediante Robocopy, utilice la opción /MIR para conservar las marcas de tiempo de archivo. Esto garantizará que los archivos antiguos se organicen por niveles antes que los archivos a los que se ha accedido recientemente.
- Al ver las propiedades de archivo desde un cliente de SMB, puede parecer que el atributo sin conexión se ha establecido incorrectamente debido a que SMB almacena en caché los metadatos del archivo.

## <a name="agent-version-3300"></a>Versión del agente 3.3.0.0
Las notas siguientes corresponden a la versión 3.3.0.0 del agente de Azure File Sync publicada el 24 de septiembre de 2018. Estas notas son adicionales a las notas de la versión enumeradas para la versión 3.1.0.0.

Lista de problemas corregidos en esta versión:
- El estado del servidor registrado es "Aparece sin conexión" después de que el agente de Azure File Sync se actualiza a la versión 3.1 o 3.2.
- El servicio del agente de sincronización del almacenamiento (FileSyncSvc) se bloquea debido a archivos que tienen rutas de acceso largas.
- En el registro del servidor se produce el error: no se pudo cargar el archivo o ensamblado Kailani.Afs.StorageSyncProtocol.V3.

## <a name="agent-version-3200"></a>Versión del agente 3.2.0.0
Las notas siguientes corresponden a la versión 3.2.0.0 del agente de Azure File Sync (publicada el 15 de agosto de 2018). Estas notas son adicionales a las notas de la versión enumeradas para la versión 3.1.0.0.

Esta versión incluye las siguientes correcciones:
- La sincronización produce un error de memoria insuficiente (0x8007000e) debido a una fuga de memoria.

## <a name="agent-version-3100"></a>Versión del agente 3.1.0.0
Las notas siguientes corresponden a la versión 3.1.0.0 del agente de Azure File Sync (publicada el 19 de junio de 2018).

### <a name="evaluation-tool"></a>Herramienta de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante la herramienta de evaluación de Azure File Sync. Esta herramienta es un cmdlet de AzureRM PowerShell que busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Para la instalación y las instrucciones de uso, consulte la sección [Herramienta de evaluación](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) en la Guía de planeación. 

### <a name="agent-installation-and-server-configuration"></a>Instalación del agente y configuración del servidor
Para más información sobre cómo instalar y configurar el agente de Azure File Sync con Windows Server, vea [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md) y [How to deploy Azure File Sync](storage-sync-files-deployment-guide.md) (Implementación de Azure Files Sync).

- El paquete de instalación del agente debe instalarse con permisos elevados (administrador).
- El agente no es compatible con las opciones de implementación de Windows Server Core ni Nano Server.
- El agente solo se admite en Windows Server 2016 y Windows Server 2012 R2.
- El agente requiere al menos 2 GB de memoria física.
- El servicio del agente de sincronización de Storage (FileSyncSvc) no admite puntos de conexión de servidor ubicados en un volumen que tiene el directorio de información del volumen del sistema (SVI) comprimido. Esta configuración producirá resultados inesperados.

### <a name="interoperability"></a>Interoperabilidad
- Las aplicaciones antivirus, de copia de seguridad y de otro tipo que accedan a archivos organizados en niveles pueden provocar recuperaciones no deseadas, salvo que respeten el atributo sin conexión y pasen por alto la lectura del contenido de esos archivos. Para más información, vea [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md).
- No utilice el Administrador de recursos del servidor de archivos (FSRM) ni otros filtros de archivos. Los filtros de archivos pueden provocar errores de sincronización sin fin cuando los archivos están bloqueados debido al filtro de archivos.
- No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y puede provocar resultados inesperados. La instalación del agente y el registro del servidor se deben realizar después de implementar la imagen del servidor y completar la instalación mínima de sysprep.
- En el mismo volumen no se puede desduplicar datos y ordenar en niveles en la nube.

### <a name="sync-limitations"></a>Limitaciones de la sincronización
Los siguientes elementos no se sincronizan, pero el resto del sistema funciona con normalidad:
- Rutas de acceso de más de 2,048 caracteres.
- La parte de la lista de control de acceso discrecional (DACL) de un descriptor de seguridad, si es mayor de 2 KB. (Este problema aplica únicamente si tiene más de unas 40 entradas de control de acceso [ACE] en un solo elemento).
- La parte de la lista de control de acceso del sistema (SACL) de un descriptor de seguridad que se utiliza para la auditoría.
- Atributos extendidos.
- Flujos de datos alternativos.
- Puntos de repetición de análisis.
- Vínculos físicos.
- La compresión (si se establece en un archivo de servidor) no se conserva al sincronizarse los cambios con ese archivo desde otros puntos de conexión.
- Los archivos cifrados con EFS (u otros cifrados con modo de usuario) que evitan que el servicio lea los datos.

    > [!Note]  
    > Azure File Sync siempre cifra los datos en tránsito. Los datos siempre se cifran en reposo en Azure.
 
### <a name="server-endpoint"></a>Punto de conexión de servidor
- Un punto de conexión de servidor solo se puede crear en un volumen NTFS. En este momento Azure File Sync no admite ReFS, FAT, FAT32 ni otros sistemas de archivos.
- Los archivos en niveles quedarán inservibles si los archivos no se vuelven a llamar antes de eliminar el punto de conexión de servidor.
- Los niveles de nube no se admiten en el volumen del sistema. Para crear un punto de conexión de servidor en el volumen del sistema, deshabilite los niveles de la nube al crear el punto de conexión de servidor.
- Los clústeres de conmutación por error solo son compatibles con discos en clúster, pero no con volúmenes compartidos de clúster (CSV).
- No se puede anidar un punto de conexión de servidor. Puede coexistir en el mismo volumen en paralelo con otro punto de conexión.
- No almacene un archivo de paginación de aplicación o sistema operativo en un punto de conexión de servidor.
- Si se cambia el nombre del servidor, no se actualiza en el portal. Para actualizar el nombre del servidor en el portal, anule el registro y vuelva a registrar el servidor.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
- Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión de nube una vez cada 24 horas. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización.
- El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a un grupo de recursos o suscripción diferentes dentro del inquilino de Azure AD existente. Si se mueve la cuenta de almacenamiento, debe dar acceso al servicio File Sync híbrido a la cuenta de almacenamiento (consulte el apartado [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync no permite mover la suscripción a un inquilino de Azure AD distinto.

### <a name="cloud-tiering"></a>Niveles de nube
- Si un archivo en niveles se copia en otra ubicación mediante el uso de Robocopy, el archivo resultante no estará en niveles. El atributo sin conexión podría estar establecido porque Robocopy incluye incorrectamente dicho atributo en las operaciones de copia.
- Al ver las propiedades de archivo desde un cliente de SMB, puede parecer que el atributo sin conexión se ha establecido incorrectamente debido a que SMB almacena en caché los metadatos del archivo.
