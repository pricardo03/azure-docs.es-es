---
title: Almacenamiento inmutable en Azure Storage Blobs | Microsoft Docs
description: Azure Storage ofrece compatibilidad WORM (escribir una vez, leer muchas) con el almacenamiento de Blob (objeto), lo que permite a los usuarios almacenar datos en un estado no modificable durante un intervalo de tiempo especificado.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/01/2019
ms.author: tamram
ms.reviewer: hux
ms.subservice: blobs
ms.openlocfilehash: d58c596421cec2e69210dd39a5d4a9708c154b44
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66492756"
---
# <a name="store-business-critical-data-in-azure-blob-storage"></a>Almacenamiento de los datos críticos para la empresa en Azure Blob Storage

Almacenamiento inmutable para el almacenamiento de blobs de Azure permite a los usuarios almacenar objetos de datos empresariales críticos en un estado WORM (una sola escritura, lectura muchas). En este estado, los usuarios no pueden borrar ni modificar los datos durante el intervalo de tiempo especificado por el usuario. Los objetos de BLOB pueden ser creados y leer, pero no modificar o eliminar, durante el intervalo de retención. Almacenamiento inmutable está habilitado para uso General v2 y cuentas de Blob Storage en todas las regiones de Azure.

## <a name="overview"></a>Información general

Le ayuda a almacenamiento inmutable de atención médica, instituciones financieras e industrias relacionadas--especialmente organizaciones corredores--para almacenar datos de forma segura. También puede usarse en cualquier escenario para proteger datos críticos de modificación o eliminación. 

Las aplicaciones típicas son:

- **Cumplimiento normativo**: el almacenamiento inmutable para Azure Blob Storage ayuda a las organizaciones a cumplir SEC 17a-4(f), CFTC 1.31(d), FINRA y otras regulaciones. Notas del producto técnicas por Cohasset Associates detallan cómo inmutable direcciones de almacenamiento es que se pueden descargar a través de estos requisitos normativos el [Microsoft Service Trust Portal](https://aka.ms/AzureWormStorage). El [Azure Trust Center](https://www.microsoft.com/trustcenter/compliance/compliance-overview) contiene información detallada acerca de nuestras certificaciones de cumplimiento.

- **Retención segura de documentos**: Almacenamiento inmutable para Azure Blob storage garantiza que datos no pueden ser modificados o eliminados por cualquier usuario, incluidos los usuarios con privilegios de administrador de cuenta.

- **Suspensión legal**: Almacenamiento inmutable para el almacenamiento de blobs de Azure permite a los usuarios almacenar información confidencial que resulta fundamental para el uso de juicio o empresariales en un estado de prueba de manipulaciones para la duración deseada hasta que se haya quitado la suspensión. Esta característica no se limita solo a los casos de uso legal, pero también puede considerarse de como una espera basada en eventos o un bloqueo de la empresa, donde se requiere la necesidad de proteger los datos en función de los desencadenadores de eventos o directiva corporativa.

Almacenamiento inmutable admite lo siguiente:

- **[Compatibilidad con la directiva de retención con duración definida](#time-based-retention)** : Los usuarios pueden establecer directivas para almacenar los datos de un intervalo especificado. Cuando una directiva de retención con duración definida es establecido, los blobs pueden ser creados y leer, pero no modificados o eliminados. Una vez transcurrido el período de retención, se pueden eliminar pero no sobrescriben los blobs.

- **[Compatibilidad con la directiva de suspensión legal](#legal-holds)** : Si no se conoce el intervalo de retención, los usuarios pueden establecer suspensiones legales para almacenar datos immutably hasta que se borre la retención legal.  Cuando se establece una directiva de retención legal, blobs se pueden crear y leer, pero no modificados o eliminados. Cada suspensión legal se asocia con una definido por el usuario alfanuméricos etiqueta (por ejemplo, un identificador de caso, el nombre de evento, etc.) que se usa como una cadena de identificador. 

- **Compatibilidad con todos los niveles de blobs:** las directivas WORM son independientes del nivel de Azure Blob Storage y se aplican a todos los niveles: de archivo, frecuente y esporádico. Los usuarios pueden transferir sus datos al nivel que les ofrezca la mayor optimización de costos de acuerdo con sus cargas de trabajo sin alterar la inmutabilidad de los datos.

- **Configuración en el nivel de contenedor**: los usuarios pueden configurar las directivas de retención con duración definida y las etiquetas de suspensión legal a nivel del contenedor. Mediante el uso de la configuración de nivel de contenedor simple, los usuarios pueden crear y las directivas de retención basada en tiempo de bloqueo, ampliar los intervalos de retención, conjunto y suspensiones legales claro y mucho más. Estas directivas se aplican a todos los blobs del contenedor, tanto a los nuevos como a los existentes.

- **Compatibilidad con el registro de auditoría**: Cada contenedor incluye un registro de auditoría de directiva. Muestra hasta siete basado en tiempo de retención de los comandos para las directivas de retención basada en el tiempo bloqueado y contiene el Id. de usuario, tipo de comando, las marcas de tiempo y el intervalo de retención. En el caso de las suspensiones legales, el registro contiene el identificador del usuario, el tipo de comando, las marcas de tiempo y las etiquetas de suspensión legal. Este registro se conserva durante la vigencia de la directiva, según las instrucciones de las normas de 17a-4(f) seg. El [Azure Activity Log](../../azure-monitor/platform/activity-logs-overview.md) muestra un registro más completo de todas las actividades del plano de control; al habilitar [los registros de diagnóstico de Azure](../../azure-monitor/platform/diagnostic-logs-overview.md) conserva y se muestran las operaciones del plano de datos. Es responsabilidad del usuario almacenar dichos registros de forma persistente, ya que podría ser obligatorio por ley o por otros fines.

## <a name="how-it-works"></a>Cómo funciona

Almacenamiento inmutable para Azure Blob Storage admite dos tipos de directivas inmutables o WORM: retención con duración definida y suspensiones legales. Cuando se aplica una directiva de retención con duración definida o suspensión legal en un contenedor, todos los blobs existentes se mueva a un estado inmutable de gusano en menos de 30 segundos. Todos los nuevos blobs que se cargan en ese contenedor se moverán también en el estado inmutable. Una vez que han movido todos los blobs en el estado inmutable, la directiva inmutable se confirma y todos sobrescriben o eliminarán no se permiten las operaciones para los objetos nuevos y existentes en el contenedor inmutable.

Eliminación de la cuenta y de contenedor también no se permiten si no hay ningún BLOB protegido por una directiva inmutable. La operación Delete Container producirá un error si hay al menos un blob con una directiva de retención de duración definida o una retención legal. Se producirá un error en la eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una retención legal o un blob con un intervalo de retención activo. 

### <a name="time-based-retention"></a>Basado en tiempo de retención

> [!IMPORTANT]
> Debe ser una directiva de retención con duración definida *bloqueado* para el blob en un conforme inmutable (escritura y eliminación protegido) estado 17a-4(f) s y las otras normas. Se recomienda que bloquee la directiva en un período razonable de tiempo, normalmente menos de 24 horas. Es el estado inicial de una política de retención basada en tiempo de *desbloqueado*, lo que permite probar la característica y realizar cambios en la directiva antes de que lo bloquea. Mientras el *desbloqueado* estado proporciona protección de inmutabilidad, no se recomienda usar la *desbloqueado* estado para cualquier propósito distinto de las evaluaciones de función a corto plazo. 

Cuando se aplica una directiva de retención con duración definida a un contenedor, todos los blobs de este permanecen en estado inmutable durante el período de retención *efectivo*. El período de retención eficaz para los blobs existentes es igual a la diferencia entre la hora de modificación del blob y el intervalo de retención especificado por el usuario.

Para los nuevos blobs, el período de retención efectivo es igual al intervalo de retención especificado por el usuario. Como los usuarios pueden ampliar el intervalo de retención, el almacenamiento inmutable utiliza el valor más reciente del intervalo de retención especificado por el usuario para calcular el período de retención efectivo.

> [!TIP]
> **Ejemplo:** un usuario crea una directiva de retención con duración definida con un intervalo de retención de cinco años.
>
> El blob existente en ese contenedor, _testblob1_, se creó hace un año. El período de retención efectiva _testblob1_ es desde hace cuatro años.
>
> Un nuevo blob, _testblob2_, ahora se carga en el contenedor. El período de retención efectivo de este nuevo blob es de cinco años.

Se recomienda una directiva de retención de duración definida desbloqueado sólo para probar la característica y una directiva debe estar bloqueada para ser compatible con s 17a-4(f) y las otras normas. Una vez que se ha bloqueado una directiva de retención con duración definida, no se puede quitar la directiva y se permite un máximo de 5 aumenta el período de retención efectiva. Para obtener más información sobre cómo establecer y directivas de retención basada en tiempo de bloqueo, consulte el [Introducción](#getting-started) sección.

### <a name="legal-holds"></a>Retenciones legales

Si se establece una suspensión legal, tanto los blobs nuevos como los existentes permanecen en estado inmutable hasta que se elimine dicha suspensión. Para más información acerca de cómo establecer y eliminar suspensiones legales, consulte la sección [Introducción](#getting-started).

Un contenedor puede tener una suspensión legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

La tabla siguiente muestra los tipos de operaciones de blob que se deshabilitan para los diferentes escenarios de inmutabilidad. Para más información, consulte la documentación de la [API del servicio Azure Blob](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

|Escenario  |Estado del blob  |Operaciones de blob no permitidas  |
|---------|---------|---------|
|El intervalo de retención efectivo del blob no ha expirado todavía o se ha establecido una retención legal     |Inmutable: protegido frente a eliminación y escritura         | Crear Blob<sup>1</sup>, coloque bloque<sup>1</sup>, Put Block List<sup>1</sup>, eliminar metadatos del contenedor, Blob Delete, conjunto de Blob, Put Page, establecer las propiedades del Blob, instantánea de Blob, copia Incremental de Blob, Anexar bloque         |
|El intervalo de retención efectivo del blob ha expirado     |Protegido contra escritura únicamente (están permitidas las operaciones de eliminación)         |Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Todas las suspensiones legales eliminadas y sin ninguna directiva de retención basada en el tiempo establecida en el contenedor     |Mutable         |None         |
|No se crea ninguna directiva WORM (retención basada en el tiempo o suspensión legal)     |Mutable         |None         |

<sup>1</sup> la aplicación permite que estas operaciones crear un nuevo blob una vez. Todas las sobrescribir no se permiten operaciones en una ruta de acceso de blob existente en un contenedor inmutable.

## <a name="supported-values"></a>Valores admitidos

### <a name="time-based-retention"></a>Basado en tiempo de retención
- Para una cuenta de almacenamiento, el número máximo de contenedores con directivas de inmutable basado en tiempo bloqueados es 1.000.
- El intervalo de retención mínimo es 1 día. El máximo es 146,000 días (400 años).
- Para un contenedor, el número máximo de modificaciones para ampliar un intervalo de retención para directivas de inmutable basado en tiempo bloqueados es 5.
- Para un contenedor, un máximo de 7 registros de auditoría de directiva de retención basada en el tiempo se conservan durante la duración de la directiva.

### <a name="legal-hold"></a>Suspensión legal
- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con un valor de suspensión legal es 1.000.
- En un contenedor, el número máximo de etiquetas de suspensión legal es 10.
- La longitud mínima de una etiqueta de suspensión legal es de 3 caracteres alfanuméricos. La longitud máxima es 23 caracteres alfanuméricos.
- Para un contenedor, un máximo de 10 legal mantenga los registros se conservan durante la duración de la directiva de auditoría de directiva.

## <a name="pricing"></a>Precios

El uso de esta característica no tiene costo adicional. El precio de los datos inmutables es el mismo que el de los datos normales, mutables. Para información detallada sobre los precios de Azure Blob Storage, consulte la [página de precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="getting-started"></a>Introducción
Almacenamiento inmutable está disponible solo para uso General v2 y cuentas de Blob Storage. Estas cuentas deben administrarse a través de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Para obtener información sobre cómo actualizar una cuenta de almacenamiento de uso General v1 existente, vea [actualizar una cuenta de almacenamiento](../common/storage-account-upgrade.md).

Las versiones más recientes de la [portal Azure](https://portal.azure.com), [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), y [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) admite el almacenamiento inmutable para Azure Blob storage. [Compatibilidad con bibliotecas de cliente](#client-libraries) también se proporciona.

### <a name="azure-portal"></a>Azure Portal

1. Cree un nuevo contenedor o seleccione un contenedor existente para almacenar los blobs que se deben conservar en estado inmutable.
 El contenedor debe estar en una cuenta de Blob Storage o GPv2.
2. Seleccione **Access policy** (Directiva de acceso) en la configuración del contenedor. Luego, seleccione **+ Agregar directiva** en **Almacenamiento de blobs inmutable**.

    ![Configuración del contenedor en el portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Para habilitar la retención con duración definida, seleccione **Retención con duración definida** en el menú desplegable.

    !["Retención con duración definida" seleccionado en "Tipo de directiva"](media/storage-blob-immutable-storage/portal-image-2.png)

4. Especifique el intervalo de retención en días (valores aceptables de 1 a 146000 días).

    ![Cuadro "Actualizar el período de retención a"](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    El estado inicial de la directiva está desbloqueado que permite probar la característica y realizar cambios en la directiva antes de que lo bloquea. La directiva de bloqueo es esencial para el cumplimiento de normas como SEC 17a-4.

5. Bloquee la directiva. Haga clic en el botón de puntos suspensivos ( **...** ), y aparecerá el siguiente menú con acciones adicionales:

    !["Directiva de bloqueo" en el menú](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

6. Seleccione **directiva de bloqueo** y confirme el bloqueo. La directiva ahora está bloqueada y no se puede eliminar, se permitirá solo las extensiones que el intervalo de retención. Elimina el BLOB y no se permiten las invalidaciones. 

    ![Confirmar "Directiva de bloqueo" en el menú](media/storage-blob-immutable-storage/portal-image-5-lock-policy.png)

7. Para habilitar las suspensiones legales, seleccione **+ Agregar directiva**. Seleccione **Suspensión legal** en el menú desplegable.

    !["Suspensión legal" en el menú en "Tipo de directiva"](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

8. Cree una suspensión legal con una o varias etiquetas.

    ![Cuadro "Nombre de etiqueta" en el tipo de directiva](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

9. Para borrar una suspensión legal, basta con quitar la etiqueta de identificador aplicado retención legal.

### <a name="azure-cli"></a>Azure CLI

La característica se incluye en los siguientes grupos de comandos: `az storage container immutability-policy` y `az storage container legal-hold`. Ejecute `-h` en ellos para ver los comandos.

### <a name="powershell"></a>PowerShell

El módulo Az.Storage admite almacenamiento inmutable.  Para habilitar la característica, siga estos pasos:

1. Asegúrese de tener instalada la versión más reciente de PowerShellGet: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Elimine todas las instalaciones anteriores de Azure PowerShell.
3. Instale Azure PowerShell: `Install-Module Az –Repository PSGallery –AllowClobber`.

La sección acerca del [código de PowerShell de ejemplo](#sample-powershell-code), que encontrará en este mismo artículo, muestra el uso de la característica.

## <a name="client-libraries"></a>Bibliotecas de clientes

Las siguientes bibliotecas de cliente admiten el almacenamiento inmutable para Azure Blob Storage:

- [Biblioteca de cliente .NET, versión 7.2.0 (versión preliminar) y versiones posteriores](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [Biblioteca de cliente Node.js, versión 4.0.0 y versiones posteriores](https://www.npmjs.com/package/azure-arm-storage)
- [Biblioteca de cliente Python, versión 2.0.0 Release Candidate 2 y versiones posteriores](https://pypi.org/project/azure-mgmt-storage/2.0.0rc2/)
- [Biblioteca cliente de Java](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/storage/resource-manager/Microsoft.Storage/preview/2018-03-01-preview)

## <a name="faq"></a>Preguntas más frecuentes

**¿Puede proporcionar documentación de cumplimiento de gusano?**

Sí. Cumplimiento de documento, Microsoft mantiene una empresa líder de valoración independiente que se especializa en registros información y administración de control, Cohasset Associates, evaluar inmutable de Azure Blob Storage y su compatibilidad con los requisitos específicos en el sector de servicios financieros. Cohasset valida que inmutable Blob de Azure Storage, cuando se utiliza para conservar los Blobs basado en tiempo en estado de un gusano, cumple los requisitos de almacenamiento relevantes de regla CFTC 1.31(c)-(d) FINRA regla 4511 y SEC Rule 17a-4. Microsoft había destinado a este conjunto de reglas, ya que representan el asesoramiento más global para la retención de registros para las instituciones financieras. El informe Cohasset está disponible en el [Microsoft Service Trust Center](https://aka.ms/AzureWormStorage). Para solicitar una carta de certificación de Microsoft con respecto al cumplimiento de gusano, póngase en contacto con soporte técnico de Azure.

**¿La característica se aplica solo a los blobs en bloques o también a los blobs en páginas y a los blobs en anexos?**

El almacenamiento inmutable puede utilizarse con cualquier tipo de blob, pero se recomienda usarlo principalmente con blobs en bloques. A diferencia de los blobs en bloques, los blobs en páginas y los blobs en anexos se deben crear fuera de un contenedor WORM y luego se deben copiar en él. Después de copiar estos blobs en un contenedor de gusano, no más *anexa* para anexar blob o los cambios realizados en un blob en páginas están permitidos.

**¿Es necesario crear una nueva cuenta de almacenamiento para usar esta característica?**

No, puede usar almacenamiento inmutable con v2 de propósito General existente o recién creado o cuentas de Blob storage. Esta característica está pensada para uso con blobs en bloques en cuentas de GPv2 y Blob Storage. Cuentas de almacenamiento de v1 de uso generales no se admiten, pero se pueden actualizar fácilmente a uso General v2. Para obtener información sobre cómo actualizar una cuenta de almacenamiento de uso General v1 existente, vea [actualizar una cuenta de almacenamiento](../common/storage-account-upgrade.md).

**¿Puedo aplicar una suspensión legal y la directiva de retención con duración definida?**

Sí, un contenedor puede tener una retención legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

**¿Las directivas de retención legal solo son para un proceso legal o hay otros escenarios de uso?**

No, la retención Legal es simplemente el término general que se usa para una directiva de retención no basados en tiempo. No es necesario que solo se usará para litigios relacionados con un procedimiento. Las directivas de retención legales son útiles para deshabilitar la sobrescritura y las eliminaciones para proteger la empresa importante datos gusano, donde el período de retención es desconocido. Puede usar como una directiva de empresa para proteger los gusano cargas de trabajo críticas o utilizarlo como una directiva de almacenamiento provisional antes de que un desencadenador de evento personalizado requiere el uso de una directiva de retención con duración definida. 

**¿Puedo quitar una *bloqueado* directiva de retención con duración definida o suspensión legal?**

Solo las directivas de retención de duración definida desbloqueado se pueden quitar de un contenedor. Una vez que se ha bloqueado una directiva de retención con duración definida, no se puede quitar; solo se permiten período de retención efectiva las extensiones. Se pueden eliminar las etiquetas de retención legal. Cuando se eliminan todas las etiquetas legales, se quita la retención legal.

**¿Qué ocurre si se intenta eliminar un contenedor con una directiva de retención basada en el tiempo o una retención legal *bloqueada*?**

La operación Delete Container producirá un error si hay al menos un blob con una directiva de retención de duración definida o una retención legal. La operación Delete Container solo se ejecutará correctamente si no existe ningún intervalo de retención activo y no hay retenciones legales. Para poder eliminar el contenedor, antes debe eliminar los blobs.

**¿Qué ocurre si se intenta eliminar una cuenta de almacenamiento con un contenedor WORM que tiene una directiva de retención basada en el tiempo o una retención legal*bloqueada*?**

Se producirá un error en la eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una retención legal o un blob con un intervalo de retención activo. Para poder eliminar la cuenta de almacenamiento, antes debe eliminar todos los contenedores WORM. Para obtener información acerca de la eliminación de contenedores, vea la pregunta anterior.

**¿Puedo mover los datos entre distintos niveles de blob (nivel de acceso frecuente, esporádico, poco frecuente) cuando el blob está en estado inmutable?**

Sí, puede usar el comando Set Blob Tier para mover datos entre los niveles de blob al tiempo que mantiene los datos en el estado inmutable compatible. El almacenamiento inmutable solamente se admite en los niveles de blob de acceso frecuente, de acceso esporádico y de archivo.

**¿Qué ocurre si dejo de pagar y no ha expirado el intervalo de retención?**

En caso de impago, las directivas de retención de datos normales se aplicarán como estipulado en los términos y condiciones de su contrato con Microsoft.

**¿Se ofrece una evaluación o un período de gracia para probar la característica?**

Sí. Cuando se crea una directiva de retención con duración definida, se encuentra en un *desbloqueado* estado. En este estado, puede realizar los cambios que desee en el intervalo de retención, como por ejemplo, aumentarlo o disminuirlo, e incluso eliminar la directiva. Una vez bloqueada la directiva, permanece en este estado hasta que expira el intervalo de retención. Esta directiva bloqueada impide su eliminación y modificación en el intervalo de retención. Se recomienda encarecidamente que utilice el estado *desbloqueado* únicamente para fines de prueba y que bloquee la directiva en un periodo de 24 horas. Estas prácticas ayudan a cumplir SEC 17a-4(f) y otras normativas.

**¿Puedo usar la eliminación temporal junto con las directivas de blobs inmutable?**

Sí. [Eliminación temporal para Azure Blob storage](storage-blob-soft-delete.md) se aplica a todos los contenedores dentro de una cuenta de almacenamiento, independientemente de una directiva de retención con duración definida o suspensión legal. Se recomienda habilitar eliminación temporal para obtener protección adicional antes de aplicar las directivas de gusano inmutable y confirmadas. 

**¿Dónde está disponible la característica?**

El almacenamiento inmutable está disponible en las regiones Government, China y Azure público. Si almacenamiento inmutable no está disponible en su región, póngase en contacto con soporte técnico y el correo electrónico azurestoragefeedback@microsoft.com.

## <a name="sample-powershell-code"></a>Código de PowerShell de ejemplo

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

El siguiente script de PowerShell de ejemplo se utiliza como referencia. Dicho script crea una nueva cuenta de almacenamiento y un contenedor. Luego muestra cómo establecer y eliminar suspensiones legales, crear y bloquear una directiva de retención con duración definida (también conocida como directiva de inmutabilidad) y amplíe el intervalo de retención.

Configuración y comprobación de la cuenta de Azure Storage:

```powershell
$ResourceGroup = "<Enter your resource group>”
$StorageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$container2 = "<Enter another container name>”
$location = "<Enter the storage account location>"

# Log in to the Azure Resource Manager account
Login-AzAccount
Register-AzResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzResourceGroup -Name $ResourceGroup -Location $location

# Create your Azure storage account
New-AzStorageAccount -ResourceGroupName $ResourceGroup -StorageAccountName `
    $StorageAccount -SkuName Standard_LRS -Location $location -Kind StorageV2

# Create a new container
New-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Create Container 2 with a storage account object
$accountObject = Get-AzStorageAccount -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount
New-AzStorageContainer -StorageAccount $accountObject -Name $container2

# Get a container
Get-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Get a container with an account object
$containerObject = Get-AzStorageContainer -StorageAccount $accountObject -Name $container

# List containers
Get-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount

# Remove a container (add -Force to dismiss the prompt)
Remove-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container2

# Remove a container with an account object
Remove-AzStorageContainer -StorageAccount $accountObject -Name $container2

# Remove a container with a container object
$containerObject2 = Get-AzStorageContainer -StorageAccount $accountObject -Name $container2
Remove-AzStorageContainer -InputObject $containerObject2
```

Establecimiento y eliminación de retenciones legales:

```powershell
# Set a legal hold
Add-AzRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag1>,<tag2>,...

# with an account object
Add-AzRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>

# with a container object
Add-AzRmStorageContainerLegalHold -Container $containerObject -Tag <tag4>,<tag5>,...

# Clear a legal hold
Remove-AzRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag2>

# with an account object
Remove-AzRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>,<tag5>

# with a container object
Remove-AzRmStorageContainerLegalHold -Container $containerObject -Tag <tag4>
```

Creación o actualización de directivas de inmutabilidad:
```powershell
# with an account name or container name
Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container -ImmutabilityPeriod 10

# with an account object
Set-AzRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -ImmutabilityPeriod 1 -Etag $policy.Etag

# with a container object
$policy = Set-AzRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 7

# with an immutability policy object
Set-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -ImmutabilityPeriod 5
```

Recuperación de directivas de inmutabilidad:
```powershell
# Get an immutability policy
Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container

# with an account object
Get-AzRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container

# with a container object
Get-AzRmStorageContainerImmutabilityPolicy -Container $containerObject
```

Bloqueo de directivas de inmutabilidad (agregue - Force para descartar el mensaje):
```powershell
# with an immutability policy object
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
$policy = Lock-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -force

# with an account name or container name
$policy = Lock-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
$policy = Lock-AzRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -Etag $policy.Etag

# with a container object
$policy = Lock-AzRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -Etag $policy.Etag -force
```

Ampliación de directivas de inmutabilidad:
```powershell

# with an immutability policy object
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container

$policy = Set-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy

# with an account name or container name
$policy = Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -ImmutabilityPeriod 11 -Etag $policy.Etag -ExtendPolicy

# with an account object
$policy = Set-AzRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -ImmutabilityPeriod 12 -Etag `
    $policy.Etag -ExtendPolicy

# with a container object
$policy = Set-AzRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 13 -Etag $policy.Etag -ExtendPolicy
```

Quitar una directiva de inmutabilidad desbloqueado (agregar - Force para descartar el símbolo del sistema):
```powershell
# with an immutability policy object
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
Remove-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy

# with an account name or container name
Remove-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
Remove-AzRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -Etag $policy.Etag

# with a container object
Remove-AzRmStorageContainerImmutabilityPolicy -Container $containerObject `
    -Etag $policy.Etag

```
