---
ms.assetid: ''
title: Eliminación temporal de Azure Key Vault | Microsoft Docs
ms.service: key-vault
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/19/2019
ms.openlocfilehash: f222b37e8ca6efcfe28146ee948511d887f547a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201342"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Información general sobre la eliminación temporal de Azure Key Vault

La característica de eliminación temporal de Key Vault permite recuperar almacenes y objetos de almacenes eliminados, lo que se conoce como eliminación temporal. En concreto, se tratan los siguientes escenarios:

- Compatibilidad con la eliminación recuperable de una instancia de Key Vault
- Compatibilidad con la eliminación recuperable de objetos de Key Vault (por ejemplo, claves, secretos y certificados)

## <a name="supporting-interfaces"></a>Interfaces admitidas

La característica de eliminación temporal está disponible inicialmente a través de la [REST](/rest/api/keyvault/), [CLI](key-vault-soft-delete-cli.md), [PowerShell](key-vault-soft-delete-powershell.md) y [.NET /C# ](/dotnet/api/microsoft.azure.keyvault?view=azure-dotnet) interfaces.

## <a name="scenarios"></a>Escenarios

Los almacenes Azure Key Vault son recursos controlados que se administran por medio de Azure Resource Manager. Azure Resource Manager especifica además un comportamiento bien definido para su eliminación, lo que conlleva que una operación DELETE ejecutada correctamente debería traducirse en que ese recurso deje de estar accesible. La característica de eliminación temporal aborda la recuperación del objeto eliminado, con independencia de que la eliminación haya sido voluntaria o involuntaria.

1. En un escenario típico, un usuario puede haber eliminado por accidente una instancia o un objeto de Key Vault; si esa instancia o ese objeto de Key Vault fueran recuperables durante un período predeterminado, el usuario podría deshacer la eliminación y recuperar sus datos.

2. En un escenario diferente, un usuario malintencionado podría intentar eliminar una instancia o un objeto de Key Vault, como una clave dentro de un almacén, para provocar una interrupción de la actividad empresarial. El hecho de que al eliminar la instancia o el objeto de Key Vault no se eliminen los datos subyacentes puede resultar útil como medida de seguridad ya que, por ejemplo, se pueden limitar los permisos sobre la eliminación de datos a un rol diferente que sea de confianza. Este enfoque requiere un cuórum efectivo para una operación que, en caso contrario, podría provocar una pérdida de datos inmediata.

### <a name="soft-delete-behavior"></a>Comportamiento de eliminación temporal

Con esta característica, la operación DELETE sobre un objeto o instancia de Key Vault es una operación temporal que retiene los recursos durante un período determinado (90 días) durante el cual parece que el objeto se ha eliminado realmente. El servicio proporciona además un mecanismo para recuperar el objeto eliminado, básicamente deshaciendo la eliminación. 

La eliminación temporal es un comportamiento opcional de Key Vault y no **está habilitado de forma predeterminada** en esta versión. Se puede activar a través de [CLI](key-vault-soft-delete-cli.md) o [Powershell](key-vault-soft-delete-powershell.md).

### <a name="purge-protection"></a>Purgar la protección 

Al purgar la protección está activada, un almacén o un objeto en estado eliminado no se puede purgar hasta que haya transcurrido el período de retención de 90 días. Estos almacenes y objetos todavía se pueden recuperar, garantiza que los clientes que se seguirá la directiva de retención. 

Purgar la protección es un comportamiento opcional de Key Vault y es **no habilitado de forma predeterminada**. Se puede activar a través de [CLI](key-vault-soft-delete-cli.md#enabling-purge-protection) o [Powershell](key-vault-soft-delete-powershell.md#enabling-purge-protection).

### <a name="permitted-purge"></a>Purga permitida

La purga permanente de una instancia de Key Vault es posible a través de una operación POST en el recurso de proxy y requiere privilegios especiales. Por lo general, el propietario de la suscripción es el único que puede purgar una instancia de Key Vault. La operación POST activa la eliminación inmediata y no recuperable de esa instancia. 

Las excepciones son:
- Cuando la suscripción de Azure se ha marcado como *no eliminable*. En este caso, solo el servicio puede realizar la eliminación real, y lo hace como un proceso programado. 
- Cuando la protección de purgas habilitar marca está habilitada en el propio almacén. En este caso, Key Vault espera durante 90 días a partir de que el objeto de secreto original se marcara para eliminación para eliminar permanentemente el objeto.

### <a name="key-vault-recovery"></a>Recuperación de Key Vault

Tras eliminar una instancia de Key Vault, el servicio crea un recurso de proxy en la suscripción, agregando metadatos suficientes para la recuperación. El recurso de proxy es un objeto almacenado, disponible en la misma ubicación que la instancia de Key Vault eliminada. 

### <a name="key-vault-object-recovery"></a>Recuperación de un objeto de Key Vault

Tras eliminar un objeto de almacén de claves, como una clave, el servicio pondrá el objeto en estado eliminado, que no será accesible para las operaciones de recuperación. Mientras esté en ese estado, el objeto de Key Vault solo se puede colocar en una lista, recuperar o eliminar por la fuerza/permanentemente. 

Al mismo tiempo, Key Vault programará la eliminación de los datos subyacentes correspondientes al objeto o la instancia de Key Vault eliminados para su ejecución tras un intervalo de retención predeterminado. El registro DNS correspondiente al almacén también se conserva durante la duración del intervalo de retención.

### <a name="soft-delete-retention-period"></a>Período de retención de la eliminación temporal

Los recursos eliminados temporalmente se conservan durante un período de tiempo determinado: 90 días. Durante el intervalo de retención de eliminación temporal, se dan las circunstancias siguientes:

- Puede confeccionar un listado con todos los objetos e instancias de Key Vault que se encuentran en estado de eliminación temporal en su suscripción, así como tener acceso a la información de eliminación y recuperación sobre ellos.
    - Solo los usuarios con permisos especiales pueden consultar los almacenes eliminados. Se recomienda que los usuarios creen un rol personalizado con estos permisos especiales para tratar los almacenes eliminados.
- No es posible crear una instancia de Key Vault con el mismo nombre en la misma ubicación; en consecuencia, no se puede crear un objeto de Key Vault en un almacén determinado si esa instancia de Key Vault contiene un objeto con el mismo nombre y está en estado eliminado. 
- Solo un usuario con privilegios determinados puede restaurar una instancia o un objeto de Key Vault mediante la emisión de un comando de recuperación en el recurso de proxy correspondiente.
    - El usuario, miembro del rol personalizado, que tiene privilegios para crear una instancia de Key Vault en el grupo de recursos puede restaurar el almacén.
- Solo un usuario con privilegios determinados puede eliminar por la fuerza una instancia o un objeto de Key Vault mediante la emisión de un comando de eliminación en el recurso de proxy correspondiente.

Una vez que finalice el intervalo de retención, el servicio lleva a cabo una purga de la instancia o el objeto de Key Vault eliminados temporalmente y de su contenido, a menos que se hayan recuperado. La eliminación de recursos no se puede volver a programar.

### <a name="billing-implications"></a>Implicaciones de facturación

En general, cuando un objeto (un almacén de claves, una clave o un secreto) está en estado eliminado, solo existen dos operaciones posibles: "purgarlo" y "recuperarlo". Se producirá un error en el resto de operaciones. Por consiguiente, aunque el objeto exista, no se puede realizar ninguna operación y, por tanto, no se producirá ningún uso ni ninguna factura. Sin embargo, existen las siguientes excepciones:

- las acciones "purgar" y "recuperar" contarán para las operaciones normales del almacén de claves y se facturarán.
- Si el objeto es una clave HSM, se aplicará el cargo de "Clave HSM protegida" por versión de clave al mes si se ha utilizado una versión de la clave en los últimos 30 días. Después, como el objeto está en estado eliminado, no podrá realizarse ninguna operación en él, por lo que no se aplicará ningún cargo.

## <a name="next-steps"></a>Pasos siguientes

Las dos guías siguientes ofrecen los escenarios de uso principal para uso de la eliminación temporal.

- [Uso de la eliminación temporal de Key Vault con PowerShell](key-vault-soft-delete-powershell.md) 
- [Uso de la eliminación temporal de Key Vault con la CLI](key-vault-soft-delete-cli.md)

