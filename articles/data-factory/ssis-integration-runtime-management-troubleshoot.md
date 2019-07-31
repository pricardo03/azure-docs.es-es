---
title: Solución de problemas con la administración de SSIS Integration Runtime en Azure Data Factory | Microsoft Docs
description: En este artículo se proporciona orientación para solucionar problemas de administración de SSIS Integration Runtime (SSIS IR)
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/08/2019
author: chinadragon0515
ms.author: dashe
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 17fe8d9ed02156b8fe9aafd7adca946531895883
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253014"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Solución de problemas con la administración de SSIS Integration Runtime en Azure Data Factory

En este documento se proporciona orientación para solucionar problemas de administración de SSIS Integration Runtime (SSIS IR).

## <a name="overview"></a>Información general

En caso de problema de aprovisionamiento o desaprovisionamiento en SSIS IR, en el portal de ADF aparecerá un mensaje de error o lo devolverá el cmdlet de PowerShell. El error siempre está en formato de código de error con un mensaje de error detallado.

Si el código de error es InternalServerError, significa que el servicio tiene problemas transitorios. Vuelva a intentarlo más tarde. Póngase en contacto con el equipo de soporte técnico de Azure Data Factory si el reintento no ayuda.

Hay tres dependencias externas principales que pueden causar errores: Si el código de error no es InternalServerError, puede ser el servidor de Azure SQL Database o Instancia administrada, el script de instalación personalizada o la configuración de la red virtual.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Problemas en el servidor de Azure SQL Database o Instancia administrada

Se requiere un servidor de Azure SQL Database o Instancia administrada si se aprovisiona SSIS IR con la base de datos de catálogo de SSIS. SSIS IR debe poder acceder al servidor de Azure SQL Database o Instancia administrada. La cuenta del servidor de Azure SQL Database o Instancia administrada debe tener el permiso para crear la base de datos de catálogo de SSIS (SSISDB). Si hay algún error, se mostrará un código de error con el mensaje de excepción de SQL detallado en el portal de ADF. Siga los pasos que se indican a continuación para solucionar los problemas de los códigos de error.

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

Puede ver este problema al aprovisionar una nueva instancia de SSIS IR o durante la ejecución de IR.

Si ve el error durante el aprovisionamiento de IR, puede deberse a lo siguiente y puede obtener el mensaje SqlException como mensaje de error.

* Problema de conexión de red. Compruebe si se puede acceder al nombre de host de SQL Server o Instancia administrada y que el acceso al servidor por parte de SSIS IR no esté bloqueado por firewall o un grupo de seguridad de red.
* Se ha producido un error de inicio de sesión y se ha usado la autenticación de SQL. Significa que la cuenta proporcionada no puede iniciar sesión en SQL Server. Asegúrese de que se ha proporcionado la cuenta de usuario correcta.
* Se ha producido un error de inicio de sesión y se ha usado la autenticación de AAD (Identidad administrada). Agregue la identidad administrada de su fábrica a un grupo de AAD y haga que la identidad administrada tenga permisos de acceso al servidor de la base de datos de catálogo.
* Tiempo de conexión agotado, siempre a causa de la configuración de seguridad. Se recomienda crear una máquina virtual, hacer que esta se una a la misma red virtual de IR (si IR está en una red virtual), instalar SSMS y comprobar el estado del servidor de Azure SQL Database o Instancia administrada.

Para otros problemas, consulte el mensaje de error de excepción de SQL detallado y corrija el problema que se muestra en mensaje de error. Póngase en contacto con el equipo de soporte técnico del servidor de Azure SQL Database o Instancia administrada si el problema continúa.

Es probable que haya algunos cambios en el firewall o grupo de seguridad de red si ve el error durante la ejecución de IR, lo que provoca que el nodo de trabajo de SSIS IR ya no pueda acceder al servidor de Azure SQL Database o Instancia administrada. Desbloquee el nodo de trabajo de SSIS IR para acceder al servidor de Azure SQL Database o Instancia administrada.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

El mensaje de error se parece a "La base de datos 'SSISDB' ha alcanzado su cuota de tamaño. Cree particiones o elimine datos, quite índices o consulte la documentación para obtener soluciones posibles". Las posibles soluciones son:
* Aumente la cuota de tamaño de la SSISDB.
* Cambie las configuraciones de SSISDB para reducir el tamaño, por ejemplo:
   * Al reducir el período de retención y el número de versiones del proyecto.
   * Al reducir el período de retención del registro.
   * Al cambiar el nivel predeterminado del registro, etc.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Este error significa que el servidor de Azure SQL Database o Instancia administrada ya tiene una SSISDB creada y utilizada por otra instancia de IR. Debe proporcionar un servidor de Azure SQL Database o Instancia administrada distintos o eliminar la SSISDB existente y reiniciar la nueva instancia de IR.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Este error puede deberse a los siguientes motivos:

* La cuenta de usuario que está configurada para la instancia de SSIS IR no tiene permiso para crear la base de datos. Puede conceder permiso al usuario para crear la base de datos.
* Tiempo de espera de creación de base de datos agotado, por ejemplo, tiempo de espera de ejecución agotado, tiempo de espera de operación de base de datos agotado, etc. Puede volver a intentarlo más tarde para ver si se ha resuelto el problema. Póngase en contacto con el equipo de soporte técnico del servidor de Azure SQL Database o Instancia administrada si el reintento no funciona.

Para otros problemas, compruebe el mensaje de error de excepción de SQL y corrija el problema que en él se menciona. Póngase en contacto con el equipo de soporte técnico del servidor de Azure SQL Database o Instancia administrada si el problema continúa.

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

El mensaje de error es de tipo "Nombre de objeto 'catalog.catalog_properties' no válido."; significa que ya tiene una base de datos denominada SSISDB pero que no la ha creado SSIS IR, o bien, que la base de datos está en un estado no válido debido a errores en el último aprovisionamiento de SSIS IR. Puede quitar la base de datos existente con el nombre SSISDB o configurar un nuevo servidor de Azure SQL Database o Instancia administrada para IR.

## <a name="custom-setup"></a>Instalación personalizada

La instalación personalizada proporciona una interfaz para agregar pasos de instalación propios durante el aprovisionamiento o la reconfiguración de SSIS IR. Para más información, consulte [Instalación personalizada de Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Asegúrese de que el contenedor incluya solo los archivos de instalación personalizados necesarios, ya que se descargarán en el nodo de trabajo de SSIS IR todos los archivos del contenedor. Se recomienda probar el script de instalación personalizada en una máquina local para corregir cualquier problema de ejecución antes de ejecutarlo en SSIS IR.

El contenedor del script de instalación personalizada también se comprobará durante la ejecución de IR, ya que SSIS IR se actualiza normalmente y necesita acceder al contenedor de nuevo para descargar el script de instalación personalizada e instalarlo de nuevo. La comprobación incluirá si el contenedor es accesible y si existe el archivo main.cmd.

Si se produce un error en la instalación personalizada, verá un error con el código CustomSetupScriptFailure; compruebe el mensaje de error con un subcódigo de error.  Siga los pasos que se indican a continuación para solucionar los problemas con los subcódigos de error.  

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

Significa que SSIS IR no puede acceder al contenedor de blobs de Azure para la instalación personalizada. Compruebe que el URI de SAS del contenedor es accesible y que no ha expirado.

Primero, detenga IR si se está ejecutando, vuelva a configurarlo con el nuevo URI de SAS del contenedor de instalación personalizada y vuelva a iniciarlo.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Significa que SSIS IR no puede encontrar el script de instalación personalizada (main.cmd) en el contenedor de blobs. Asegúrese de que main.cmd existe en el contenedor, que es el punto de entrada para la instalación personalizada.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Significa que se ha producido un error en la ejecución del script de instalación personalizada (main.cmd). Puede probar el script primero en la máquina local o comprobar los registros de ejecución de la instalación personalizada en el contenedor de blobs.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Significa que se va a ejecutar el tiempo de espera del script de instalación personalizada. Asegúrese de que el contenedor de blobs contiene solo los archivos de instalación personalizada necesarios. También puede comprobar los registros de ejecución de la instalación personalizada en el contenedor de blobs. El período máximo para la instalación personalizada está establecido en 45 minutos antes de que se agote el tiempo y el período máximo incluye el tiempo necesario para descargar todos los archivos del contenedor e instalarlos en SSIS IR. Si es necesario un período más largo, genere una incidencia de soporte técnico.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Significa que la carga de los registros de ejecución de la instalación personalizada en el contenedor de blobs produjo un error, ya sea porque SSIS IR no tiene permiso de escritura en el contenedor de blobs o por problemas de almacenamiento o de red. Si la instalación personalizada se realiza correctamente, este error no afecta a ninguna función de SSIS, pero faltan registros. Si se produce otro error en la instalación personalizada y no se puede cargar el registro, este error se notificará en primer lugar para que el registro pueda cargarse para el análisis y, una vez resuelto este problema, se notificará el otro problema más específico. Si el problema persiste tras un reintento, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.

## <a name="virtual-network-configuration"></a>Configuración de redes virtuales

Al unir SSIS IR a una red virtual (VNet), se usa esta en la suscripción de usuario. Para más información, consulte [Unión de Azure-SSIS Integration Runtime a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Cuando haya un problema relacionado con la red virtual, verá el siguiente error

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

Podría deberse a varios motivos. Siga los pasos que se indican a continuación para solucionar los problemas con los subcódigos de error.

### <a name="forbidden"></a>Prohibido

El mensaje de error es de tipo "El identificador de subred no está habilitado para la cuenta actual. El proveedor de recursos Microsoft.Batch no está registrado en la misma suscripción que la red virtual".

Significa que Azure Batch no puede acceder a la red virtual. Registre el proveedor de recursos Microsoft.Batch en la misma suscripción que la red virtual.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

El mensaje de error es de tipo "O bien, la red virtual especificada no existe, o bien, el servicio Batch no tiene acceso a ella" o "La subred especificada XXX no existe".

Significa que, o bien, la red virtual no existe o que el servicio Azure Batch no puede acceder a ella, o bien, no existe la subred proporcionada. Asegúrese de que la red virtual y la subred existen y de que Azure Batch puede acceder a ellas.

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

El mensaje es de tipo "No se pudo aprovisionar Integration Runtime en la red virtual. Si están configurados el servidor DNS o el grupo de seguridad de red, asegúrese de que el primero sea accesible y de que el segundo esté configurado correctamente".

Es probable que tenga alguna configuración personalizada del servidor DNS o del grupo de seguridad de red, lo cual haga que SSIS IR no pueda resolver el nombre del servidor de Azure o que no se pueda acceder a este. Para más información, consulte el documento de [configuración de la red virtual de SSIS IR](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Si el problema persiste, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR se actualizará automáticamente de forma periódica. Durante la actualización se creará un grupo de Azure Batch y se eliminará el grupo de Azure Batch anterior, se eliminarán los recursos relacionados con la red virtual del grupo anterior y se creará un recurso relacionado con la red virtual en la suscripción. Este error significa que se ha producido un error al eliminar el recurso relacionado con la red virtual del grupo anterior a causa de un bloqueo de eliminación a nivel de la suscripción o del grupo de recursos. Elimine el bloqueo de eliminación.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

Se puede producir un error en el aprovisionamiento de SSIS IR por distintos motivos. Si se produce un error, se eliminarán todos los recursos creados. Sin embargo, los recursos de red virtual no se pueden eliminar, ya que hay un bloqueo de eliminación en ellos a nivel de suscripción o de grupo de recursos. Quite el bloqueo de eliminación y reinicie la instancia de IR.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

Al detener SSIS IR, se eliminarán todos los recursos relacionados con la red virtual, pero se ha producido un error en la eliminación debido a que hay un bloqueo de eliminación de recursos en el nivel de suscripción o de grupo de recursos. Quite el bloqueo de eliminación y vuelva a intentarlo de nuevo.

### <a name="nodeunavailable"></a>NodeUnavailable

Este error se produce durante la ejecución de IR, lo que significa que el estado de IR era correcto pero ya no. Siempre se debe a un cambio de configuración en el servidor DNS o el grupo de seguridad de red y hace que SSIS IR no se pueda conectar al servicio del que depende. Es necesario resolver estos problemas de configuración. Para más información, consulte el artículo de [configuración de la red virtual de SSIS IR](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Si el problema persiste, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.
