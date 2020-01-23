---
title: Cifrado de datos de Azure Database for MySQL con una clave administrada por el cliente
description: El cifrado de datos de Azure Database for MySQL con una clave administrada por el cliente habilita Bring Your Own Key (BYOK) para la protección de datos en reposo y permite a las organizaciones implementar la separación de tareas en la administración de claves y datos.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 7c54b3010b42d56ffa9b701b76c7aef51095404c
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028651"
---
# <a name="azure-database-for-mysql-data-encryption-with-customer-managed-key"></a>Cifrado de datos de Azure Database for MySQL con clave administrada por el cliente

> [!NOTE]
> En este momento, debe solicitar acceso para poder usar esta funcionalidad. Para ello, póngase en contacto con AskAzureDBforMySQL@service.microsoft.com.

El cifrado de datos de Azure Database for MySQL con una clave administrada por el cliente habilita Bring Your Own Key (BYOK) para la protección de datos en reposo y permite a las organizaciones implementar la separación de tareas en la administración de claves y datos. Con el cifrado de datos administrado por el cliente, es responsable y tiene el control total del ciclo de vida de una clave (creación, carga, rotación y eliminación), de los permisos de uso de claves y de la auditoría de operaciones con clave.

Para Azure Database for MySQL, el cifrado de datos se establece en el nivel de servidor. Con esta modalidad de cifrado de datos, la clave que se usa para el cifrado de la clave de cifrado de base de datos (DEK) es una clave asimétrica administrada por el cliente que se almacena en una instancia propiedad del cliente y administrada por él de [Azure Key Vault (AKV)](../key-vault/key-Vault-secure-your-key-Vault.md), un sistema de administración de claves externas basado en la nube. Azure Key Vault tiene una gran disponibilidad y ofrece un almacenamiento seguro escalable para claves criptográficas RSA respaldado opcionalmente por módulos de seguridad de hardware (HSM) con certificación FIPS 140-2 nivel 2. No permite el acceso directo a una clave almacenada, sino que proporciona servicios de cifrado y descifrado mediante el uso de la clave en las entidades autorizadas. La clave puede generarse desde Key Vault, así como importarse o [transferirse a Key Vault desde un dispositivo HSM local](../key-vault/key-Vault-hsm-protected-keys.md).

> [!NOTE]
> Esta característica está disponible en todas las regiones de Azure donde Azure Database for MySQL admita los planes de tarifa de uso general y optimizado para memoria.

## <a name="benefits"></a>Ventajas

El cifrado de datos para Azure Database for MySQL proporciona las siguientes ventajas:

* Mayor transparencia, control granular y administración para la clave de cifrado
* Administración central y organización de claves al hospedarlas en Azure Key Vault.
* Capacidad de implementar la separación de tareas para la administración de claves y datos en la organización.
* Administración de claves independiente de la administración de datos dentro de una organización, de manera que un administrador de Key Vault pueda revocar los permisos de acceso de clave para hacer que la base de datos cifrada sea inaccesible.
* Mayor confianza de los clientes finales, ya que Azure Key Vault está diseñado para que Microsoft no pueda ver ni extraer las claves de cifrado.

## <a name="terminology-and-description"></a>Terminología y descripción

**Clave de cifrado de datos (DEK)** : Una clave simétrica AES256 usada para cifrar una partición o un bloque de datos. Cifrar cada bloque de datos con una clave diferente dificulta los ataques de análisis criptográficos. Se necesita acceso a las DEK por la instancia de proveedor o aplicación de recursos que cifra y descifra un bloque específico. Cuando se reemplaza una DEK por una nueva clave, solo se deben volver a cifrar los datos de su bloque asociado con la nueva clave.

**Clave de cifrado de claves (KEK)** : una clave de cifrado utilizada para cifrar las claves de cifrado de datos. El uso de una clave de cifrado de claves que siempre permanece en Key Vault permite a las propias claves de cifrado de datos cifrarse y controlarse. La entidad que tiene acceso a la KEK puede ser diferente de la entidad que requiere la DEK. Puesto que la KEK es necesaria para descrifrar la DEK, la KEK es de manera eficaz un punto único por el que se pueden eliminar de forma eficaz las DEK mediante la eliminación de la KEK.

Las claves de cifrado de datos (DEK) cifradas con las claves de cifrado de claves se almacenan por separado y solo una entidad con acceso a la clave de cifrado de claves puede descifrar dichas claves de cifrado de datos. Para obtener más información, consulte [Seguridad del cifrado en reposo](../security/fundamentals/encryption-atrest.md).

## <a name="how-data-encryption-with-customer-managed-key-works"></a>Funcionamiento del cifrado de datos con una clave administrada por el cliente

![Introducción a Bring Your Own Key](media/concepts-data-access-and-security-data-encryption/mysqloverview.png)

Para que el servidor MySQL pueda usar las claves administradas por el cliente almacenadas en Azure Key Vault para cifrar las DEK, el administrador de Key Vault debe conceder los siguientes derechos de acceso al servidor mediante su identidad única:

* **get**: para recuperar la parte pública y las propiedades de la clave en Key Vault
* **wrapKey**: para poder cifrar las DEK
* **unwrapKey**: para poder descifrar las DEK

El administrador de Key Vault también puede [habilitar el registro de eventos de auditoría de Key Vault](../azure-monitor/insights/azure-key-vault.md), por lo que se pueden auditar más adelante.

Cuando el servidor está configurado para utilizar la clave administrada por el cliente que se almacena en Key Vault, el servidor envía las DEK a Key Vault para los cifrados. Key Vault devuelve las DEK cifradas, la cual se almacena en la base de datos del usuario. Cuando es necesario, el servidor envía las DEK protegidas a Key Vault para que se descifre. Los auditores pueden usar Azure Monitor para revisar los registros de AuditEvent de Key Vault, si está habilitado el registro.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-mysql"></a>Requisitos de configuración del cifrado de datos para Azure Database for MySQL

### <a name="requirements-for-configuring-akv"></a>Requisitos de configuración de AKV

* Key Vault y Azure Database for MySQL deben pertenecer al mismo inquilino de Azure Active Directory (AAD). No se admiten las interacciones de servidor y Key Vault entre inquilinos. Para mover los recursos posteriormente, es necesario volver a configurar el cifrado de datos. Obtenga más información sobre el movimiento de recursos.
* La característica de eliminación temporal debe estar habilitada en Key Vault para obtener protección contra la pérdida de datos en caso de que se eliminen las claves (o Key Vault) de manera involuntaria. Los recursos eliminados temporalmente se conservan durante 90 días, a menos que el cliente los recupere o los purgue dentro de ese plazo. Las acciones de recuperación y purga tienen permisos propios asociados a una directiva de acceso a Key Vault. La característica de eliminación temporal está desactivada de forma predeterminada y se puede habilitar a través de PowerShell o de la CLI. No se puede habilitar mediante Azure Portal.
* Conceda a Azure Database for MySQL acceso a Key Vault con los permisos **get, wrapKey, unwrapKey** mediante su identidad administrada única. Cuando se usa Azure Portal, la identificación única se crea automáticamente cuando se habilita el cifrado de datos en MySQL. Consulte [Configuración del cifrado de datos para MySQL](howto-data-encryption-portal.md) para obtener instrucciones paso a paso detalladas al usar Azure Portal.

* Al usar un firewall con AKV, debe habilitar la opción *Permitir que los servicios de confianza de Microsoft omitan el firewall*.

### <a name="requirements-for-configuring-customer-key"></a>Requisitos de configuración de la clave del cliente

* La clave administrada por el cliente que se va a usar para cifrar las DEK solo puede ser asimétrica, RSA 2028.
* La fecha de activación de la clave (si se establece) debe ser una fecha y hora del pasado. La fecha de expiración (si se establece) debe ser una fecha y hora del futuro.
* El estado de la clave debe ser *Habilitada*.
* Si va a importar una clave existente en Key Vault, asegúrese de proporcionarla en uno de los formatos de archivo compatibles (`.pfx`, `.byok`, `.backup`).

## <a name="recommendations-when-using-data-encryption-using-customer-managed-key"></a>Recomendaciones al usar el cifrado de datos mediante la clave administrada por el cliente

### <a name="recommendation-for-configuring-akv"></a>Recomendaciones para la configuración de Azure Key Vault

* Establezca un bloqueo de recursos en Key Vault para controlar quién puede eliminar este recurso crítico y para evitar la eliminación accidental o no autorizada. Obtenga más información acerca de los bloqueos de recursos.
* Habilite la auditoría y la generación de informes en todas las claves de cifrado: Key Vault proporciona registros que son fáciles de insertar en otras herramientas de administración de eventos e información de seguridad. Log Analytics de Azure Monitor es un ejemplo de un servicio que ya está integrado.

* Asegúrese de que Key Vault y Azure Database for MySQL residen en la misma región para garantizar un acceso más rápido para las operaciones de encapsulado y desencapsulado de DEK.

### <a name="recommendation-for-configuring-customer-managed-key"></a>Recomendación de configuración de clave administrada por el cliente

* Almacene una copia de la clave administrada por el cliente (KEK) en un lugar seguro o en el servicio de custodia.

* Si la clave se genera en Key Vault, cree una copia de seguridad de esta antes de usarla en Azure Key Vault por primera vez. La copia de seguridad solo se puede restaurar en Azure Key Vault. Obtenga más información sobre el comando [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyVault/backup-azkeyVaultkey).

## <a name="inaccessible-customer-managed-key-condition"></a>Condición de clave administrada por el cliente inaccesible

Cuando se configura el cifrado de datos con la clave administrada por el cliente en Azure Key Vault (AKV), es necesario el acceso continuo a esta clave para que el servidor permanezca en línea. Si el servidor pierde el acceso la clave administrada por el cliente en Azure Key Vault, pasados como máximo 10 minutos, el servidor comenzará a denegar todas las conexiones con el mensaje de error correspondiente y cambiará su estado a **Inaccesible**. La única acción que se puede realizar en una base de datos con el estado Inaccesible es eliminarla.

### <a name="accidental-key-access-revocation-from-the-azure-key-vault-akv"></a>Revocación de acceso a la clave accidental desde Azure Key Vault (AKV)

Es posible que alguien con los derechos de acceso suficientes a Key Vault deshabilite accidentalmente el acceso del servidor a la clave al realizar alguna de las siguientes acciones:

* revocar los permisos get, wrapKey o unwrapKey de Key Vault desde el servidor
* Eliminar la clave.
* eliminar Key Vault
* cambiar las reglas de firewall de Key Vault

* Eliminar la identidad administrada del servidor en Azure Active Directory.

## <a name="monitoring-of-the-customer-managed-key-in-the-key-vault"></a>Supervisión de la clave administrada por el cliente en Key Vault

Para supervisar el estado de la base de datos y habilitar las alertas para la pérdida de acceso al protector de TDE, configure las siguientes características de Azure:

* [Azure Resource Health](../service-health/resource-health-overview.md): una base de datos inaccesible que haya perdido el acceso a la clave del cliente aparecerá como "Inaccesible" después de que se haya denegado la primera conexión a la base de datos.
* [Registro de actividad](../service-health/alerts-activity-log-service-notifications.md): cuando se produce un error de acceso a la clave de cliente en Key Vault administrado por el cliente, las entradas se agregan al registro de actividad. La creación de alertas para estos eventos le permitirá restablecer el acceso lo antes posible.

* Los [grupos de acciones](../azure-monitor/platform/action-groups.md) se pueden definir para que envíen notificaciones y alertas en función de las preferencias, por ejemplo, Correo electrónico/SMS/Inserción/Voz, aplicación lógica, webhook, ITSM o Runbook de Automation.

## <a name="restore-and-replica-with-customers-managed-key-in-the-key-vault"></a>Restauración y réplica con la clave administrada del cliente en Key Vault

Una vez que se cifra una Azure Database for MySQL con la clave administrada por el cliente almacenada en Key Vault, cualquier copia recién creada del servidor (ya sea mediante una operación local o de restauración geográfica o a través de réplicas de lectura) también se cifra con la misma clave administrada por el cliente. Sin embargo, pueden cambiarse para reflejar la nueva clave administrada por el cliente para el cifrado. Cuando se cambia la clave administrada por el cliente, las copias de seguridad antiguas del servidor comenzarán a usar la clave más reciente.

Para evitar incidencias al establecer la configuración del cifrado de datos administrados por el cliente durante la restauración o la creación de réplicas de lectura, es importante seguir estos pasos en el servidor restaurado y en el servidor réplica:

* Inicie el proceso de restauración o creación de réplica de lectura desde la Azure Database for MySQL maestra.
* El servidor recién creado (restaurado o réplica) se mantiene en un estado Inaccesible, ya que su identidad única todavía no tiene permisos para Azure Key Vault (AKV)
* En el servidor restaurado/réplica, vuelva a validar la clave administrada por el cliente en la configuración de cifrado de datos para asegurarse de que el servidor recién creado tiene los permisos de ajuste y desajuste para la clave almacenada en Azure Key Vault.

* Ambos pasos anteriores deben realizarse para asegurarse de que el cifrado de datos se conserva en el servidor principal, así como en el servidor restaurado o réplica.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [Configurar el cifrado de datos con la clave administrada por el cliente para MySQL mediante Azure Portal](howto-data-encryption-portal.md).
