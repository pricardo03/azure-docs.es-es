---
title: TDE - Bring Your Own Key (BYOK) - Azure SQL Database| Microsoft Docs
description: Compatibilidad de Bring Your Own Key (BYOK) para el cifrado de datos transparente (TDE) con Azure Key Vault para SQL Database y Data Warehouse. Información general de TDE con BYOK, ventajas, funcionamiento, consideraciones y recomendaciones.
keywords: ''
services: sql-database
documentationcenter: ''
author: aliceku
manager: craigg
ms.prod: ''
ms.reviewer: vanto
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.custom: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 08/30/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: d87747e60c375f844681ed6cfd40dba84f46a9b2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963618"
---
# <a name="transparent-data-encryption-with-bring-your-own-key-support-for-azure-sql-database-and-data-warehouse"></a>Cifrado de datos transparente con compatibilidad con Bring Your Own Key para Azure SQL Database y Data Warehouse

La compatibilidad entre Bring Your Own Key (BYOK) para [cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) le permite cifrar la clave de cifrado de base de datos (DEK) con una clave asimétrica denominada protector de TDE.  El protector de TDE se almacena bajo su control en [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), el sistema de administración de claves externas basado en la nube de Azure. Azure Key Vault es el primer servicio de administración de claves con el que TDE ha integrado la compatibilidad con BYOK. La DEK de TDE, que se almacena en la página de inicio de una base de datos, se cifra y descifra por el protector de TDE. El protector de TDE se almacena en Azure Key Vault y nunca abandona el almacén de claves. Si se revoca el acceso del servidor al almacén de claves, una base de datos no se puede descifrar y leer en la memoria.  El protector de TDE se establece en el nivel de servidor lógico y se hereda por todas las bases de datos asociadas con dicho servidor. 

Gracias a la compatibilidad con BYOK, los usuarios ahora pueden controlar las tareas de administración de claves como las rotaciones de claves, los permisos del almacén de claves y la eliminación de claves, así como llevar a cabo auditorías o crear informes sobre todos los protectores de TDE mediante la funcionalidad de Azure Key Vault. Key Vault ofrece administración central de claves, aprovecha los módulos de seguridad de hardware (HSM) extremadamente supervisados y permite la separación de obligaciones entre la administración de claves y los datos para ayudar a cumplir las normativas.  


TDE con BYOK proporciona las siguientes ventajas:
- Mayor transparencia y control pormenorizado con la capacidad de administrar automáticamente el protector de TDE   
- Administración central de los protectores de TDE (junto con otras claves y secretos usados en otros servicios de Azure) hospedándolos en Key Vault
- Separación de las responsabilidades de administración de claves y datos dentro de la organización para facilitar la separación de obligaciones
- Mayor nivel de confianza de sus propios clientes, ya que Key Vault está diseñado para que Microsoft no vea ni extraiga ninguna clave de cifrado. 
- Compatibilidad con la rotación de claves

> [!IMPORTANT]
> Para aquellos usuarios de TDE administrado por el servicio a los que les gustaría empezar a usar Key Vault, TDE permanece habilitado durante el proceso de cambio a un protector de TDE en Key Vault. No hay ningún tiempo de inactividad ni nuevo cifrado de los archivos de la base de datos. El cambio de una clave administrada por el servicio a una clave de Key Vault solo requiere el nuevo cifrado de la clave de cifrado de base de datos (DEK), que es una operación rápida y en línea. 
>

## <a name="how-does-tde-with-byok-support-work"></a>¿Cómo funciona la compatibilidad de TDE con BYOK?
 
![Autenticación del servidor en Key Vault](./media/transparent-data-encryption-byok-azure-sql/tde-byok-server-authentication-flow.PNG)

Cuando TDE se configura por primera vez para usar un protector de TDE de Key Vault, el servidor envía la DEK de cada base de datos habilitada para TDE a Key Vault para una solicitud de encapsulado de clave. Key Vault devuelve la clave de cifrado de la base de datos cifrada, que se almacena en la base de datos del usuario.  

>[!IMPORTANT]
>Es importante tener en cuenta que **una vez que un protector de TDE se almacena en Azure Key Vault, nunca sale de Azure Key Vault**. El servidor lógico solo puede enviar solicitudes de operación de clave al material de clave del protector de TDE en Key Vault, y **nunca tiene acceso ni almacena en caché el protector de TDE**. El administrador de Key Vault puede revocar los permisos de Key Vault del servidor en cualquier momento, en cuyo caso todas las conexiones al servidor se cortan. 
>


## <a name="guidelines-for-configuring-tde-with-byok"></a>Directrices para configurar TDE con BYOK

### <a name="general-guidelines"></a>Directrices generales
- Asegúrese de que Azure Key Vault y Azure SQL Database van a estar en el mismo inquilino.  No se admiten las **interacciones de servidor y almacén de claves entre inquilinos**.
- Decida qué suscripciones se van a usar para los recursos necesarios: el traslado del servidor de una suscripción a otra más adelante requerirá una nueva configuración de TDE con BYOK. Obtenga más información sobre [el movimiento de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- Al configurar TDE con BYOK, es importante tener en cuenta la carga que han puesto en el almacén de claves las repetidas operaciones de encapsular/desencapsular. Por ejemplo, dado que todas las bases de datos asociadas a un servidor lógico usan el mismo protector de TDE, una conmutación por error de ese servidor desencadenará tantas operaciones de clave en el almacén como bases de datos hay en el servidor. De acuerdo con nuestra experiencia y los [límites de servicio de almacén de claves](https://docs.microsoft.com/azure/key-vault/key-vault-service-limits) documentados, se recomienda asociar como máximo 500 bases de datos estándar o de uso general o 200 bases de datos premium o críticas para la empresa a una instancia de Azure Key Vault en una sola suscripción para asegurarse una disponibilidad alta uniforme al acceder al protector de TDE en el almacén. 
- Recomendación: mantenga una copia del protector de TDE en el entorno local.  Esto requiere un dispositivo HSM para crear un protector de TDE localmente y un sistema de custodia de claves a fin de almacenar una copia local del protector de TDE.  Obtenga información sobre [cómo transferir una clave desde un HSM local a Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys).


### <a name="guidelines-for-configuring-azure-key-vault"></a>Directrices para configurar Azure Key Vault

- Cree un almacén de claves con la [eliminación temporal](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) habilitada para protegerlo contra la eliminación de datos en caso de que se eliminen las claves o el almacén de claves de manera involuntaria.  Debe usar [PowerShell para habilitar la propiedad de "eliminación temporal"](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) en el almacén de claves (esta opción no está disponible aún en el portal de AKV, pero es obligatoria para SQL):  
  - Los recursos eliminados temporalmente se conservan durante un período de tiempo establecido, 90 días a menos que se recuperen o purguen.
  - Las acciones de **recuperación** y **purga** tienen sus propios permisos asociados en una directiva de acceso del almacén de claves. 
- Configure un bloqueo de recursos en el almacén de claves para controlar quién puede eliminar este recurso crítico y ayudar a evitar la eliminación accidental o no autorizada.  [Más información acerca de los bloqueos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)

- Conceda al servidor lógico acceso al almacén de claves usando su identidad de Azure Active Directory (Azure AD).  Cuando se usa la interfaz de usuario del portal, la identidad de Azure AD se crea automáticamente y se conceden los permisos de acceso del almacén de claves al servidor.  Al usar PowerShell para configurar TDE con BYOK, se debe crear la identidad de Azure AD y se debe comprobar la finalización. Consulte [Configure TDE with BYOK](transparent-data-encryption-byok-azure-sql-configure.md) (Configuración de TDE con BYOK) para obtener instrucciones detalladas paso a paso cuando se usa PowerShell.

  >[!NOTE]
  >Si la identidad de Azure AD **se elimina accidentalmente o se revocan los permisos del servidor** con la directiva de acceso del almacén de claves, el servidor pierde el acceso al almacén de claves y las bases de datos cifradas con TDE se eliminan en 24 horas.
  >

- Configure Azure Key Vault sin una red virtual o un firewall.  Si SQL pierde el acceso al almacén de claves, las bases de datos cifradas con TDE se eliminan en 24 horas.
- Habilite la auditoría y la generación de informes en todas las claves de cifrado: Key Vault proporciona registros que son fáciles de insertar en otras herramientas de administración de eventos e información de seguridad (SIEM). [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault) de Operations Management Suite (OMS) es un ejemplo de un servicio que ya está integrado.
- Para garantizar una alta disponibilidad de bases de datos cifradas, configure cada servidor lógico con dos almacenes de Azure Key Vault que residan en regiones diferentes.


### <a name="guidelines-for-configuring-the-tde-protector-asymmetric-key-stored-in-azure-key-vault"></a>Directrices para configurar el protector de TDE (clave asimétrica) almacenado en Azure Key Vault

- Cree la clave de cifrado localmente en un dispositivo HSM local. Asegúrese de que sea una clave asimétrica RSA 2048 para que se pueda almacenar en Azure Key Vault.
- Custodie la clave en un sistema de custodia de claves.  
- Importe el archivo de clave de cifrado (.pfx, .byok o .backup) a Azure Key Vault. 
    

>[!NOTE] 
    >Con fines de prueba, es posible crear una clave con Azure Key Vault; sin embargo, esta clave no se puede custodiar, porque la clave privada nunca puede abandonar el almacén de claves.  Haga siempre copias de seguridad de las claves utilizadas para cifrar los datos de producción y custódielas, ya que la pérdida de la clave (eliminación accidental en el almacén de claves, vencimiento, etc.) da como resultado la pérdida permanente de datos.
    >
    
- Use una clave sin fecha de expiración y no configure nunca una fecha de expiración en una clave que ya se está usando: **una vez que expire la clave, las bases de datos cifradas pierden el acceso a su protector de TDE y se eliminan en 24 horas**.
- Asegúrese de que la clave está habilitada y tiene permisos para realizar operaciones de *obtención*, *encapsulado de clave* y *desencapsulado de clave*.
- Cree una copia de seguridad de clave de Azure Key Vault antes de usar la clave en Azure Key Vault por primera vez. Obtenga más información sobre el comando [Backup-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1).
- Cree una nueva copia de seguridad cada vez que se realicen cambios en la clave (por ejemplo, agregar ACL, agregar etiquetas, agregar atributos de clave).
- **Mantenga versiones anteriores** de la clave en el almacén de claves al rotar las claves, de manera que se puedan restaurar las copias de seguridad de bases de datos más antiguas. Cuando se cambia el protector de TDE para una base de datos, las copias de seguridad antiguas de la base de datos **no se actualizan** para usar el protector de TDE más reciente.  Cada copia de seguridad necesita el protector de TDE que se creó durante la restauración. Las rotaciones de claves se pueden realizar según las instrucciones de [Rotate the Transparent Data Encryption Protector Using PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md) (Rotación del protector de cifrado de datos transparente mediante PowerShell).
- Mantenga todas las claves usadas anteriormente en Azure Key Vault después de cambiar de nuevo a claves administradas por el servicio.  Esto garantiza que las copias de seguridad de las bases de datos se puedan restaurar con los protectores de TDE almacenados en Azure Key Vault.  Los protectores de TDE creados con Azure Key Vault deben conservarse hasta que se hayan creado todas las copias de seguridad almacenadas con claves administradas por el servicio.  
- Realice copias de seguridad recuperables de estas claves mediante [Backup-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1).
- Para quitar una clave potencialmente en peligro durante un incidente de seguridad sin el riesgo de pérdida de datos, siga los pasos descritos en [Remove a potentially compromised key](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) (Eliminación de una clave potencialmente en peligro).


## <a name="high-availability-geo-replication-and-backup--restore"></a>Alta disponibilidad, replicación geográfica y copia de seguridad y restauración

### <a name="high-availability-and-disaster-recovery"></a>Alta disponibilidad y recuperación ante desastres

La configuración de la alta disponibilidad con Azure Key Vault depende de la configuración de la base de datos y el servidor lógico, y estas son las configuraciones recomendadas para dos casos distintos.  El primer caso es una base de datos independiente o un servidor lógico sin ninguna redundancia geográfica configurada.  El segundo caso es una base de datos o un servidor lógico configurados con grupos de conmutación por error o redundancia geográfica, donde debe garantizarse que cada copia con redundancia geográfica tenga un almacén local de Azure Key Vault dentro del grupo de conmutación por error para garantizar que funcionen las conmutaciones por error geográficas. En el primer caso, si necesita alta disponibilidad de una base de datos y un servidor lógico sin ninguna redundancia geográfica configurada, se recomienda encarecidamente configurar el servidor para usar dos almacenes de claves distintos en dos regiones diferentes con el mismo material de clave.  Esto puede realizarse mediante la creación de un protector de TDE que utilice el almacén de Key Vault ubicado en la misma región que el servidor lógico y la clonación de la clave en un almacén de claves de otra región de Azure, para que el servidor tenga acceso a un segundo almacén de claves por si el almacén de claves principal experimenta una interrupción mientras la base de datos se está ejecutando. Use el cmdlet Backup-AzureKeyVaultKey para recuperar la clave en formato cifrado desde el almacén de claves principal y, a continuación, use el cmdlet Restore-AzureKeyVaultKey y especifique un almacén de claves en la segunda región.


![Alta disponibilidad de servidor único sin recuperación ante desastres con localización geográfica](./media/transparent-data-encryption-byok-azure-sql/SingleServer_HA_Config.PNG)

## <a name="how-to-configure-geo-dr-with-azure-key-vault"></a>Configuración de la recuperación ante desastres con localización geográfica con Azure Key Vault

Para mantener la alta disponibilidad de los protectores de TDE para las bases de datos cifradas, es necesario configurar almacenes de Azure Key Vault redundantes basados en los grupos de conmutación por error de SQL Database existentes o deseados o las instancias de replicación geográfica activas.  Cada servidor de replicación geográfica requiere un almacén de claves independiente, que debe estar ubicado con el servidor en la misma región de Azure. En caso de que una base de datos principal deje de estar accesible debido a una interrupción en una región y se desencadene una conmutación por error, la base de datos secundaria es capaz de tomar el control usando el almacén de claves secundario. 
 
Para las instancias de Azure SQL Database con replicación geográfica, se requiere la siguiente configuración de Azure Key Vault:
- Una base de datos principal con un almacén de claves en la región y una base de datos secundaria con un almacén de claves en la región. 
- Se necesita al menos una réplica secundaria, y se admite hasta un máximo de cuatro. 
- No se admiten bases de datos secundarias de secundarias (encadenamiento).

En la sección siguiente se recorren los pasos de instalación y configuración con más detalle. 

### <a name="azure-key-vault-configuration-steps"></a>Pasos de configuración de Azure Key Vault

- Instale [PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0). 
- Cree dos almacenes de Azure Key Vault en dos regiones diferentes mediante [PowerShell para habilitar la propiedad de "eliminación temporal"](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) en los almacenes de claves (esta opción no está disponible aún en el portal de AKV, pero es obligatoria para SQL).
- Ambos almacenes de Azure Key Vault deben encontrarse en las dos regiones disponibles en la misma zona geográfica de Azure para que la copia de seguridad y la restauración de claves funcione.  Si necesita que los dos almacenes de claves se encuentren en diferentes zonas geográficas para satisfacer los requisitos de recuperación ante desastres con localización geográfica de SQL, siga el [proceso BYOK](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) que permite la importación de claves desde un HSM local.
- Cree una nueva clave en el primer almacén de claves:  
  - Clave RSA/RSA-HSA 2048 
  - Sin fecha de expiración 
  - La clave está habilitada y tiene permisos para realizar operaciones de obtención, encapsulado de clave y desencapsulado de clave 
- Haga una copia de seguridad de la clave principal y restaure la clave en el segundo almacén de claves.  Consulte [BackupAzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1) y [Restore-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-5.5.0). 

### <a name="azure-sql-database-configuration-steps"></a>Pasos de configuración de Azure SQL Database

Los siguientes pasos de configuración son diferentes si se empieza con una nueva implementación de SQL o si trabaja con una implementación de recuperación ante desastres con localización geográfica de SQL ya existente.  Primero se describen los pasos de configuración de una nueva implementación y, a continuación, se explica cómo asignar protectores de TDE almacenados en Azure Key Vault a una implementación existente que ya tiene un vínculo de recuperación ante desastres con localización geográfica establecido. 

Pasos para una nueva implementación:
- Cree los dos servidores lógicos de SQL en las dos mismas regiones que los almacenes de claves creados anteriormente. 
- Seleccione el panel de TDE del servidor lógico, y para cada servidor SQL lógico:  
   - Seleccione el AKV en la misma región. 
   - Seleccione la clave que se usará como protector del TDE: cada servidor usará la copia local del protector de TDE. 
   - Si se hace en el portal, se creará un valor [AppID](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) para el servidor SQL lógico, que se usa para asignar los permisos lógicos de SQL Server para acceder al almacén de claves; no elimine esta identidad. El acceso se puede revocar eliminando los permisos en Azure Key Vault en su lugar para el servidor SQL lógico, que se usa para asignar los permisos lógicos de SQL Server para acceder al almacén de claves.
- Cree la base de datos principal. 
- Siga las [instrucciones de replicación geográfica activa](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) para completar el escenario; con este paso se creará la base de datos secundaria.

![Grupos de conmutación por error y recuperación ante desastres con localización geográfica](./media/transparent-data-encryption-byok-azure-sql/Geo_DR_Config.PNG)

>[!NOTE]
>Es importante asegurarse de que los mismos protectores de TDE estén presentes en ambos almacenes de claves, antes de continuar para establecer el vínculo de replicación geográfica entre las bases de datos.
>

Pasos para una instancia de SQL Database existente con la implementación de recuperación ante desastres con localización geográfica:

Puesto que ya existen los servidores lógicos de SQL y las bases de datos principal y secundaria ya están asignadas, se deben realizar los pasos para configurar Azure Key Vault en el orden siguiente: 
- Comience con el servidor SQL Server lógico que hospeda la base de datos secundaria: 
   - Asigne el almacén de claves ubicado en la misma región. 
   - Asigne el protector de TDE. 
- Ahora, vaya al servidor SQL Server lógico que hospeda la base de datos principal: 
   - Seleccione el mismo protector de TDE que se usó para la base de datos secundaria.
   
![Grupos de conmutación por error y recuperación ante desastres con localización geográfica](./media/transparent-data-encryption-byok-azure-sql/geo_DR_ex_config.PNG)

>[!NOTE]
>Al asignar el almacén de claves al servidor, es importante empezar con el servidor secundario.  En el segundo paso, asigne el almacén de claves al servidor principal y actualice el protector de TDE, el vínculo de recuperación ante desastres con localización geográfica seguirá funcionando porque en ese momento el protector de TDE usado por la base de datos replicada está disponible para ambos servidores.
>

Antes de habilitar TDE con las claves administradas por el cliente en Azure Key Vault para un escenario de recuperación ante desastres con localización geográfica de SQL Database, es importante crear y mantener dos almacenes de Azure Key Vault con un contenido idéntico en las mismas regiones que se usarán para la replicación geográfica de SQL Database.  En concreto, "contenido idéntico" significa que ambos almacenes de claves deben contener copias de los mismos protectores de TDE para que ambos servidores tengan acceso a los protectores de TDE que usan todas las bases de datos.  A partir de ahora, es necesario mantener ambos almacenes de claves sincronizados, lo que significa que deben contener las mismas copias de los protectores de TDE después de la rotación de claves, mantener las versiones anteriores de las claves usadas para los archivos de registro o las copias de seguridad, los protectores de TDE deben mantener las mismas propiedades de clave y los almacenes de claves deben mantener los mismos permisos de acceso de SQL.  
 
Siga los pasos de [Información general: grupos de conmutación por error automática y replicación geográfica activa](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) para probar y desencadenar una conmutación por error, que debe realizarse de forma periódica para confirmar que se han mantenido los permisos de acceso de SQL para ambos almacenes de claves. 


### <a name="backup-and-restore"></a>Copia de seguridad y restauración

Una vez que una base de datos se haya cifrado con TDE mediante una clave de Key Vault, las copias de seguridad generadas también se cifran con el mismo protector de TDE.

Para restaurar una copia de seguridad cifrada con un protector de TDE de Key Vault, asegúrese de que el material de clave aún está en el almacén original bajo el nombre de la clave original. Cuando se cambia el protector de TDE para una base de datos, las copias de seguridad antiguas de la base de datos **no se actualizan** para usar el protector de TDE más reciente. Por lo tanto, se recomienda mantener todas las versiones antiguas del protector de TDE en Key Vault, de manera que se puedan restaurar las copias de seguridad de la base de datos. 

Si una clave que podría ser necesaria para restaurar una copia de seguridad ya no se encuentra en su almacén de claves original, se devuelve el siguiente mensaje de error: "Target server <Servername> does not have access to all AKV Uris created between <Timestamp #1> and <Timestamp #2>. Please retry operation after restoring all AKV Uris" (El servidor destino no tiene acceso a todos los URI de AKV creados entre <marca de tiempo #1> y <marca de tiempo #2>. Vuelva a intentar la operación después de restaurar a todos los URI de AKV").

Para evitar este problema, ejecute el cmdlet [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) para devolver la lista de claves de Key Vault que se han agregado al servidor (a menos que se hayan eliminado por el usuario). Para asegurarse de que se pueden restaurar las copias de seguridad, asegúrese de que el servidor de destino para la copia de seguridad tiene acceso a todas estas claves.

   ```powershell
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
Para obtener más información acerca de la recuperación de copia de seguridad de SQL Database, consulte la información sobre la [recuperación de una base de datos Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups). Para obtener más información acerca de la recuperación de copia de seguridad de SQL Data Warehouse, consulte la información sobre la [recuperación de una instancia de Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-overview).


Una consideración adicional para copias de seguridad de archivos de registro: las copias de seguridad de archivos de registro permanecen cifradas con el sistema de cifrado de TDE, incluso si el protector de TDE se ha rotado y la base de datos usa ahora un nuevo protector de TDE.  Durante la restauración, se necesitarán ambas claves para restaurar la base de datos.  Si el archivo de registro está usando un protector de TDE almacenado en Azure Key Vault, se necesitará esta clave durante la restauración, incluso si la base de datos se ha cambiado para usar TDE administrado por el servicio durante el proceso.


