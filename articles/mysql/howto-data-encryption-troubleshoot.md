---
title: Solución de problemas del cifrado de datos para Azure Database for MySQL
description: Aprenda a solucionar problemas del cifrado de datos para Azure Database for MySQL.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 4b517a463ec949d804798787ad4b35b53145a4a8
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371490"
---
# <a name="troubleshooting-data-encryption-with-customer-managed-keys-in-azure-database-for-mysql"></a>Solución de problemas del cifrado de datos con claves administradas por el cliente en Azure Database for MySQL
En este artículo se describe cómo identificar y resolver problemas y errores comunes que se producen en una instancia de Azure Database for MySQL configurada con cifrado de datos mediante una clave administrada por el cliente.

## <a name="introduction"></a>Introducción
Cuando se configura el cifrado de datos para que use una clave administrada por el cliente en Azure Key Vault, es necesario el acceso continuo a esta clave para que el servidor permanezca disponible. Si el servidor pierde el acceso a la clave administrada por el cliente en Azure Key Vault, el servidor empezará a denegar todas las conexiones con su correspondiente mensaje de error y cambiará su estado a ***Inaccesible*** en Azure Portal.

Si ya no se necesita un servidor de Azure Database for MySQL inaccesible, puede eliminarse inmediatamente para dejar de incurrir en gastos. No se permite ninguna otra acción en el servidor hasta que se restaure el acceso a Azure Key Vault y el servidor vuelva a estar disponible. Tampoco es posible cambiar en un servidor inaccesible la opción de cifrado de datos de "Sí" (administrada por el cliente) a "No" (administrada por el servicio) mientras el servidor está cifrado con una clave administrada por el cliente. Debe volver a validar la clave manualmente para que el servidor vuelva a estar disponible. Esto es necesario para proteger los datos contra el acceso no autorizado mientras se han revocado los permisos para la clave administrada por el cliente.

## <a name="common-errors-causing-server-to-become-inaccessible"></a>Errores comunes que hacen que el servidor sea inaccesible

La mayoría de los problemas que se producen cuando se usa el cifrado de datos con Azure Key Vault se debe a una de las configuraciones erróneas siguientes:

El almacén de claves no está disponible o no existe.

* El almacén se claves se eliminó por error.
* Un error de red intermitente hace que el almacén de claves no esté disponible.

No existen permisos para acceder al almacén de claves o la clave no existe.

* La clave se ha eliminado accidentalmente, se ha deshabilitado o ha expirado.
* La identidad administrada por la instancia de Azure Database for MySQL se eliminó accidentalmente.
* Los permisos concedidos a la identidad administrada de Azure Database for MySQL para las claves no son suficientes (no incluyen los permisos Get, Wrap ni Unwrap).
* Los permisos para la identidad administrada por la instancia del servidor de Azure Database for MySQL se revocaron.

## <a name="identify-and-resolve-common-errors"></a>Identificación y resolución de los errores comunes
### <a name="errors-on-the-key-vault"></a>Errores en el almacén de claves

#### <a name="disabled-key-vault"></a>Almacén de claves deshabilitado
* AzureKeyVaultKeyDisabledMessage
* **Explicación**: La operación no pudo completarse en el servidor porque la clave de Azure Key Vault está deshabilitada.

#### <a name="missing-key-vault-permissions"></a>Permisos de Key Vault que faltan
* AzureKeyVaultMissingPermissionsMessage
* El servidor no tiene los permisos Get, Wrap y Unwrap necesarios para los permisos de Azure Key Vault. Conceda los permisos que faltan a la entidad de servicio con identificador.

### <a name="mitigation"></a>Mitigación
* Confirme que la clave administrada por el cliente está presente en Key Vault:
* Identifique el almacén de claves y vaya a él en Azure Portal.
* Asegúrese de que está la clave identificada por el URI de clave.


## <a name="next-steps"></a>Pasos siguientes
[Configuración del cifrado de datos con una clave administrada por el cliente en Azure Database for MySQL mediante Azure Portal](howto-data-encryption-portal.md).