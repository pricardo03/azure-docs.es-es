---
ms.assetid: ''
title: Punto de conexión de servicio de VNET para Azure Key Vault | Microsoft Docs
description: Introducción a los puntos de conexión de servicio de VNET para Key Vault
services: key-vault
author: amitbapat
ms.author: ambapat
manager: mbaldwin
ms.date: 08/31/2018
ms.service: key-vault
ms.workload: identity
ms.topic: conceptual
ms.openlocfilehash: 9b0600a7afb07600116440461037c7abcb9236de
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404331"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Puntos de conexión de servicio de VNET para Azure Key Vault

Los puntos de conexión de servicio de red virtual para Key Vault le permiten restringir el acceso a la red virtual especificada y a una lista de rangos de direcciones IPv4 (versión 4 del protocolo de Internet). Si el autor de la llamada se conecta a su almacén de claves desde fuera de esas fuentes, se le negará el acceso. Si el cliente optó por permitir "servicios de confianza de Microsoft" como Office 365 Exchange Online, Office 365 SharePoint Online, Azure, Azure Resource Manager, y Azure Backup, las conexiones de esos servicios se permitirán a través el firewall. Por supuesto, tales autores de llamadas deben presentar un token de AAD válido, y deben tener permisos (configurados como directivas de acceso) para realizar la operación solicitada. Obtenga más información técnica sobre los [puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Escenarios de uso

Puede configurar los [cortafuegos y redes virtuales de Key Vault](key-vault-network-security.md) para denegar el acceso al tráfico de todas las redes (incluido el tráfico de Internet) de forma predeterminada. Puede conceder acceso al tráfico desde redes virtuales específicas de Azure y rangos de direcciones IP públicas de Internet, lo que le permite generar un límite de red seguro para las aplicaciones.

> [!NOTE]
> Los firewalls de Key Vault y las reglas de red virtual SOLO se aplican al [plan de datos](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) del almacén de claves. Las operaciones del plano de control de Key Vault (como crear, eliminar, modificar operaciones, configurar directivas de acceso, configurar firewalls y reglas de red virtual para el almacén de claves) no se ven afectadas por los firewalls y las reglas de red virtual.

Por ejemplo,
* Si utiliza Key Vault para almacenar claves de cifrado, secretos de aplicación o certificados y quiere bloquear el acceso al almacén de claves de internet pública.
* Si quiere bloquear el acceso al almacén de claves para que solo la aplicación o una lista breve de los hosts designados puedan conectarse al almacén de claves.
* Tiene una aplicación que se ejecuta en su red virtual de Azure (VNET) y esta red virtual está bloqueada para todo el tráfico entrante y saliente. Su aplicación aún necesita conectarse al almacén de claves para buscar secretos o certificados, o usar claves criptográficas.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Configuración de firewalls y redes virtuales de Key Vault

Estos son los pasos necesarios para configurar firewalls y redes virtuales. Estos pasos siguen siendo iguales con independencia de qué interfaz (PowerShell, CLI, Azure Portal) vaya a usar para configurar las reglas del firewall y la red virtual.
1. Opcional pero recomendable: permita el [registro del almacén de claves](key-vault-logging.md) para ver registros de acceso detallados. Esto le ayudará a realizar diagnósticos cuando las reglas de los firewalls y la red virtual impiden el acceso a un almacén de claves.
2. Habilite los "puntos de conexión de servicio para el almacén de claves" de las redes y subredes virtuales de destino.
3. Establezca reglas de firewall y red virtual para un almacén de claves, y así limitar el acceso a ese almacén de claves desde redes virtuales, subredes y rangos de direcciones IPv4 específicos.
4. Si este almacén de claves debe ser accesible para los servicios de confianza de Microsoft, debe habilitar la opción para permitir que los "servicios de confianza de Azure" se conecten al almacén de claves.

Consulte [Configure Azure Key Vault Firewalls and Virtual Networks](key-vault-network-security.md) (Configurar los firewalls y las redes virtuales de Azule Key Vault) para obtener instrucciones detalladas paso a paso.

> [!IMPORTANT]
> Una vez que las reglas del firewall están en vigor, todas las operaciones del [plano de datos](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) de Key Vault SOLAMENTE se pueden realizar cuando las solicitudes del autor de la llamada se originan desde redes virtuales o rangos de direcciones IPV4 permitidos. Esto también se aplica al acceso al almacén de claves desde Azure Portal. Aunque un usuario puede ir a un almacén de claves desde Azure Portal, es posible que no pueda enumerar las claves, secretos o certificados si su máquina cliente no está en la lista de dispositivos permitidos. Esto también afecta al "selector de Key Vault" de otros servicios de Azure. Los usuarios pueden ver la lista de almacenes de claves, pero no pueden enumerar las claves si las reglas del firewall limitan su máquina cliente.


> [!NOTE]
> * Se permiten un máximo de 127 reglas de VNET y 127 reglas de IPv4. 
> * Los intervalos de dirección pequeños con tamaños de prefijos "/31" o "/32" no son compatibles. Estos intervalos se deberían configurar utilizando reglas de direcciones IP individuales.
> * Las reglas de red IP solo se permiten para direcciones IP públicas. No se permiten intervalos de direcciones IP reservados para redes privadas (tal y como se define en RFC 1918) en las reglas IP. Las redes privadas incluyen direcciones que comienzan por *10.*\*, *172.16.*\* y *192.168.*\*. 
> * Solo se admiten direcciones IPV4 en este momento.

## <a name="trusted-services"></a>Servicios de confianza
Esta es una lista de servicios de confianza que tienen permiso para acceder a un almacén de claves si está habilitada la opción "Permitir servicios de confianza".

|Servicio de confianza|Escenarios de uso|
| --- | --- |
|Servicio de implementación de Azure Virtual Machines|[Implementar certificados en máquinas virtuales desde el almacén de claves administrado por el cliente](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)|
|Servicio de implementación de plantillas de Azure Resource Manager|[Paso de valores seguros durante la implementación](../azure-resource-manager/resource-manager-keyvault-parameter.md)|
|Servicio de cifrado de volúmenes de Azure Disk Encryption|Permitir el acceso a BitLocker Key (máquina virtual de Windows) o a la frase de contraseña de DM (máquina virtual de Linux) y a la clave de cifrado durante la implementación de al máquina virtual para habilitar [Azure Disk Encryption](../security/azure-security-disk-encryption.md).|
|Azure Backup|Permitir la opción de copia de seguridad y restauración de claves y secretos importantes durante la copia de seguridad de la máquina virtual de Azure, mediante [Azure Backup](../backup/backup-introduction-to-azure-backup.md).|
|Exchange Online y SharePoint Online|Permitir el acceso a la clave de cliente para Service Encryption con la [clave de cliente](https://support.office.com/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697).|
|Azure Information Protection|Permitir el acceso a la clave de inquilino para [Azure Information Protection.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|Servicios de aplicaciones|[Implementar un certificado de Azure Web App mediante Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/).|
|Azure SQL|[Cifrado de datos transparente con compatibilidad con Bring Your Own Key para Azure SQL Database y Data Warehouse](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current).|
|Azure Storage|[Cifrado del servicio Storage mediante claves administradas por el cliente en Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md).|
|Azure Data Lake Store|[Cifrado de datos en Azure Data Lake Store](../data-lake-store/data-lake-store-encryption.md) con una clave administrada de cliente.|



> [!NOTE]
> Las directivas de acceso del almacén de claves relevante deben establecerse para permitir que los servicios correspondientes obtengan acceso al almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

* [Protección de Key Vault](key-vault-secure-your-key-vault.md)
* [Configurar firewalls y redes virtuales de Azure Key Vault](key-vault-network-security.md)