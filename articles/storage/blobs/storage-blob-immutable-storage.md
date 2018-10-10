---
title: Almacenamiento inmutable en Azure Storage Blobs | Microsoft Docs
description: Azure Storage ofrece compatibilidad WORM (escribir una vez, leer muchas) con el almacenamiento de Blob (objeto), lo que permite a los usuarios almacenar datos en un estado no modificable durante un intervalo de tiempo especificado.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 09/18/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 14b5dfb0a12df6c5251ee9f9e6b35a7ce527a1d3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961984"
---
# <a name="store-business-critical-data-in-azure-blob-storage"></a>Almacenamiento de los datos críticos para la empresa en Azure Blob Storage

El almacenamiento inmutable para Azure Blob (objeto) Storage permite a los usuarios almacenar los datos críticos para la empresa en un estado WORM (escribir una vez, leer muchas). En este estado, los usuarios no pueden borrar ni modificar los datos durante el intervalo de tiempo especificado por el usuario. Los blobs se pueden crear y leer, aunque no modificar ni eliminar, durante el periodo de retención.

## <a name="overview"></a>Información general

El almacenamiento inmutable ayuda a las instituciones financieras y a las industrias relacionadas (sobre todo las organizaciones de agente-distribuidor) a almacenar los datos de forma segura. También puede utilizarse en cualquier escenario para proteger los datos críticos e impedir que se eliminen.  

Las aplicaciones típicas son:

- **Cumplimiento de reglamentaciones**: el almacenamiento inmutable para Azure Blob Storage ayuda a las organizaciones a cumplir SEC 17a-4(f), CFTC 1.31(d), FINRA y otras regulaciones.

- **Protección de la retención de documentos**: Blob Storage garantiza que ningún usuario puede modificar ni eliminar datos, incluidos los usuarios que tienen privilegios administrativos en la cuenta.

- **Suspensión legal**: el almacenamiento inmutable para Azure Blob Storage permite a los usuarios almacenar información confidencial que es crítica para los juicios o las investigaciones criminales en un estado a prueba de manipulación durante el tiempo deseado.

El almacenamiento inmutable permite:

- **Compatibilidad con directivas de retención con duración definida:** los usuarios establecen directivas para almacenar los datos durante un intervalo especificado.

- **Compatibilidad con directivas de suspensión legal**: cuando no se conoce el intervalo de retención, los usuarios pueden establecer suspensiones legales para almacenar los datos de una manera inmutable hasta que desaparezcan.  Cuando se establece una retención legal, los blobs se pueden crear y leer, pero no modificar ni eliminar. Cada retención legal está asociada a una etiqueta alfanumérica definida por el usuario que se utiliza como una cadena de identificador (por ejemplo, un identificador de caso).

- **Compatibilidad con todos los niveles de blobs:** las directivas WORM son independientes del nivel de Azure Blob Storage y se aplican a todos los niveles: frecuente, esporádico y archivo. Los usuarios pueden transferir sus datos al nivel que les ofrezca la mayor optimización de costos de acuerdo con sus cargas de trabajo sin alterar la inmutabilidad de los datos.

- **Configuración en el nivel de contenedor:** los usuarios configurar las directivas de retención con duración definida y las etiquetas de suspensión legal en el nivel de contenedor. Utilizando valores de configuración sencillos en el nivel de contenedor, los usuarios pueden crear y bloquear las directivas de retención con duración definida, ampliar los intervalos de retención, establecer y eliminar retenciones legales, etc. Estas directivas se aplican a todos los blobs del contenedor, tanto a los nuevos como a los existentes.

- **Compatibilidad con el registro de auditoría**: todos los contenedores incluyen un registro de auditoría. Muestra un máximo cinco comandos de retención con duración definida para las directivas de retención con duración definida bloqueadas, con un máximo de tres registros para las extensiones del intervalo de retención. En el caso de la retención con duración definida, el registro contiene el identificador del usuario, el tipo de comando, las marcas de tiempo y el intervalo de retención. En el caso de las suspensiones legales, el registro contiene el identificador del usuario, el tipo de comando, las marcas de tiempo y las etiquetas de suspensión legal. Este registro se conserva mientras dure el contenedor, de acuerdo con las directrices de regulación SEC 17a-4(f). El [registro de actividad de Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) muestra un registro más completo de todas las actividades del plano de control. Es responsabilidad del usuario almacenar dichos registros de forma persistente, ya que podría ser obligatorio por ley o por otros fines.

El almacenamiento inmutable está habilitada en todas las regiones públicas de Azure.

## <a name="how-it-works"></a>Cómo funciona

Almacenamiento inmutable para Azure Blob Storage admite dos tipos de directivas inmutables o WORM: retención con duración definida y suspensiones legales. Para más información acerca de cómo crear estas directivas inmutables, consulte la sección [Introducción](#Getting-started).

Cuando se aplica una directiva de retención con duración definida o una suspensión legal a un contenedor, todos los blobs existentes pasarán al estado inmutable (protegido contra escritura y eliminación). Todos los nuevos blobs que se carguen en el contenedor también pasarán al estado inmutable.

> [!IMPORTANT]
> Una directiva de retención con duración definida debe estar *bloqueada* para que el blob esté en estado inmutable (protegido frente a escritura y eliminación) y, por consiguiente, se cumplan SEC 17a-4(f) y otras regulaciones. Se recomienda bloquear la directiva en un período razonable, normalmente antes de 24 horas. No se recomienda usar el estado *desbloqueado* con ningún fin que no sea evaluar la característica a corto plazo.

Cuando se aplica una directiva de retención con duración definida a un contenedor, todos los blobs de este permanecen en estado inmutable durante el período de retención *efectivo*. El período de retención efectivo para los blobs existentes es igual a la diferencia entre la hora de creación de blob y el intervalo de retención especificado por el usuario.

Para los nuevos blobs, el período de retención efectivo es igual al intervalo de retención especificado por el usuario. Como los usuarios pueden ampliar el intervalo de retención, el almacenamiento inmutable utiliza el valor más reciente del intervalo de retención especificado por el usuario para calcular el período de retención efectivo.

> [!TIP]
> Ejemplo:
>
> un usuario crea una directiva de retención con duración definida con un intervalo de retención de cinco años.
>
> El blob que hay en ese contenedor, testblob1, se creó hace un año. El período de retención efectivo de testblob1 es de cuatro años.
>
> Un nuevo blob, testblob2, se carga ahora en el contenedor. El período de retención efectivo de este nuevo blob es de cinco años.

### <a name="legal-holds"></a>Retenciones legales

Si se establece una suspensión legal, tanto los blobs nuevos como los existentes permanecen en estado inmutable hasta que se elimine dicha suspensión. Para más información acerca de cómo establecer y eliminar suspensiones legales, consulte la sección [Introducción](#Getting-started).

Un contenedor puede tener una suspensión legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

La tabla siguiente muestra los tipos de operaciones de blob que se deshabilitan para los diferentes escenarios de inmutabilidad. Para más información, consulte la documentación de la [API del servicio Azure Blob](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

|Escenario  |Estado del blob  |Operaciones de blob no permitidas  |
|---------|---------|---------|
|El intervalo de retención efectivo del blob no ha expirado todavía o se ha establecido una retención legal     |Inmutable: protegido frente a eliminación y escritura         |Delete Container, Delete Blob, Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|El intervalo de retención efectivo del blob ha expirado     |Protegido contra escritura únicamente (están permitidas las operaciones de eliminación)         |Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Todas las suspensiones legales eliminadas y sin ninguna directiva de retención basada en el tiempo establecida en el contenedor     |Mutable         |None         |
|No se crea ninguna directiva WORM (retención basada en el tiempo o suspensión legal)     |Mutable         |None         |

<sup>1</sup> La aplicación puede llamar una vez a esta operación para crear un blob. Todas las operaciones que se realicen posteriormente en el blob no estarán permitidas.

> [!NOTE]
>
> El almacenamiento inmutable solo está disponible en las cuentas de Blob Storage y General Purpose V2. La cuenta debe crearse mediante [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="pricing"></a>Precios

El uso de esta característica no tiene costo adicional. El precio de los datos inmutables es el mismo que el de los datos normales, mutables. Para información detallada sobre los precios de Azure Blob Storage, consulte la [página de precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).


## <a name="getting-started"></a>Introducción

Las versiones más recientes de [Azure Portal](http://portal.azure.com), [la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) y la versión preliminar de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May2018) son compatibles con el almacenamiento inmutable de Azure Blob Storage.

### <a name="azure-portal"></a>Azure Portal

1. Cree un nuevo contenedor o seleccione un contenedor existente para almacenar los blobs que se deben conservar en estado inmutable.
 El contenedor debe estar en una cuenta de Blob Storage o GPv2.
2. Seleccione **Access policy** (Directiva de acceso) en la configuración del contenedor. Luego, seleccione **+ Agregar directiva** en **Almacenamiento de blobs inmutable**.

    ![Configuración del contenedor en el portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Para habilitar la retención con duración definida, seleccione **Retención con duración definida** en el menú desplegable.

    !["Retención con duración definida" seleccionado en "Tipo de directiva"](media/storage-blob-immutable-storage/portal-image-2.png)

4. Escriba el intervalo de retención, en días (el mínimo es un día).

    ![Cuadro "Actualizar el período de retención a"](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    Como se puede ver en la captura de pantalla, el estado inicial de la directiva es desbloqueado. Puede probar la característica con un intervalo de retención menor y realizar cambios en la directiva antes de bloquearla. El bloqueo es esencial para poder cumplir normas como SEC 17a-4.

5. Bloquee la directiva. Haga clic con el botón derecho en los puntos suspensivos (**...** ) y aparecerá el siguiente menú:

    !["Directiva de bloqueo" en el menú](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    Seleccione **Directiva de bloqueo** y el estado de la directiva aparece ahora como bloqueado. Una vez que la directiva se bloquee, no se podrá eliminar y solo se permitirán las ampliaciones del intervalo de retención.

6. Para habilitar las suspensiones legales, seleccione **+ Agregar directiva**. Seleccione **Suspensión legal** en el menú desplegable.

    !["Suspensión legal" en el menú en "Tipo de directiva"](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Cree una suspensión legal con una o varias etiquetas.

    ![Cuadro "Nombre de etiqueta" en el tipo de directiva](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

8. Para eliminar una retención legal, basta con quitar la etiqueta.

### <a name="azure-cli"></a>Azure CLI

La característica se incluye en los siguientes grupos de comandos: `az storage container immutability-policy` y `az storage container legal-hold`. Ejecute `-h` en ellos para ver los comandos.

### <a name="powershell"></a>PowerShell

[PowerShell versión 4.4.0 (versión preliminar)](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May20180) admite el almacenamiento inmutable.
Para habilitar la característica, siga estos pasos:

1. Asegúrese de tener instalada la versión más reciente de PowerShellGet: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Elimine todas las instalaciones anteriores de Azure PowerShell.
3. Instale AzureRM: `Install-Module AzureRM –Repository PSGallery –AllowClobber`. Azure se puede instalar de forma similar desde este repositorio.
4. Instale la versión preliminar de los cmdlets del plano de administración de Storage: `Install-Module -Name AzureRM.Storage -AllowPrerelease -Repository PSGallery -AllowClobber`.

La sección acerca del [código de PowerShell de ejemplo](#sample-powershell-code), que encontrará en este mismo artículo, muestra el uso de la característica.

## <a name="client-libraries"></a>Bibliotecas de clientes

Las siguientes bibliotecas de cliente admiten el almacenamiento inmutable para Azure Blob Storage:

- [Biblioteca de cliente .NET, versión 7.2.0 (versión preliminar) y versiones posteriores](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [Biblioteca de cliente Node.js, versión 4.0.0 y versiones posteriores](https://www.npmjs.com/package/azure-arm-storage)
- [Biblioteca de cliente Python, versión 2.0.0 Release Candidate 2 y versiones posteriores](https://pypi.org/project/azure-mgmt-storage/2.0.0rc2/)
- [Biblioteca cliente de Java](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/storage/resource-manager/Microsoft.Storage/preview/2018-03-01-preview)

## <a name="supported-values"></a>Valores admitidos

- El intervalo de retención mínimo es un día. El valor máximo de 400 años.
- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con directivas de inmutabilidad bloqueadas es 1.000.
- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con un valor de suspensión legal es 1.000.
- En un contenedor, el número máximo de etiquetas de suspensión legal es 10.
- La longitud máxima de una etiqueta de retención legal es de 23 caracteres alfanuméricos. La longitud mínima es de tres caracteres.
- En un contenedor, el número máximo de extensiones del intervalo de retención permitidas para las directivas de inmutabilidad bloqueadas es de tres.
- En un contenedor con una directiva de inmutabilidad bloqueada, se conservan un máximo de cinco registros de la directiva de retención con duración definida y un máximo de diez registros de la directiva de la suspensión legal mientras dure el contenedor.

## <a name="faq"></a>Preguntas más frecuentes

**¿La característica se aplica solo a los blobs en bloques o también a los blobs en páginas y a los blobs en anexos?**

El almacenamiento inmutable puede utilizarse con cualquier tipo de blob, pero se recomienda usarlo principalmente con blobs en bloques. A diferencia de los blobs en bloques, los blobs en páginas y los blobs en anexos se deben crear fuera de un contenedor WORM y luego se deben copiar en él. Después de copiar estos blobs en un contenedor WORM, no se permiten más *anexos* en un blob en anexos ni cambios en un blob en páginas.

**Para usar esta característica, ¿debo crear siempre una nueva cuenta de almacenamiento?**

Puede utilizar el almacenamiento inmutable con cualquier cuenta de Blob Storage y General Purpose V2 existente o que acabe de crearse. Esta característica solo está disponible en Blob Storage.

**¿Qué ocurre si se intenta eliminar un contenedor con una directiva de retención basada en el tiempo o una retención legal *bloqueada*?**

La operación Delete Container producirá un error si hay al menos un blob con una directiva de retención de duración definida o una retención legal. La operación Delete Container solo se ejecutará correctamente si no existe ningún intervalo de retención activo y no hay retenciones legales. Para poder eliminar el contenedor, antes debe eliminar los blobs.

**¿Qué ocurre si se intenta eliminar una cuenta de almacenamiento con un contenedor WORM que tiene una directiva de retención basada en el tiempo o una retención legal*bloqueada*?**

Se producirá un error en la eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una retención legal o un blob con un intervalo de retención activo.  Para poder eliminar la cuenta de almacenamiento, antes debe eliminar todos los contenedores WORM. Para obtener información acerca de la eliminación de contenedores, vea la pregunta anterior.

**¿Puedo mover los datos entre distintos niveles de blob (nivel de acceso frecuente, esporádico, poco frecuente) cuando el blob está en estado inmutable?**

Sí, puede usar el comando Set Blob Tier para mover datos entre los niveles de blob manteniendo los datos en estado inmutable. El almacenamiento inmutable solamente se admite en los niveles de blob de acceso frecuente, de acceso esporádico y de archivo.

**¿Qué ocurre si dejo de pagar y no ha expirado el intervalo de retención?**

En caso de impago, las directivas de retención de datos normales se aplicarán como estipulado en los términos y condiciones de su contrato con Microsoft.

**¿Se ofrece una evaluación o un período de gracia para probar la característica?**

Sí. Cuando se crea por primera vez una directiva de retención basada en el tiempo, está en estado *desbloqueado*. En este estado, puede realizar los cambios que desee en el intervalo de retención, como por ejemplo, aumentarlo o disminuirlo, e incluso eliminar la directiva. Una vez que la directiva se bloquea, permanece bloqueada indefinidamente para impedir su eliminación. Además, ya no se puede disminuir el intervalo de retención cuando la directiva está bloqueada. Se recomienda encarecidamente que utilice el estado *desbloqueado* únicamente para fines de prueba y que bloquee la directiva en un periodo de 24 horas. Estas prácticas ayudan a cumplir SEC 17a-4(f) y otras normativas.

**¿Está disponible la característica en las nubes nacionales y gubernamentales?**

Actualmente, el almacenamiento inmutable solo está disponible en las regiones públicas de Azure. Si está interesado en alguna nube nacional concreta, envíe un correo electrónico a azurestoragefeedback@microsoft.com.

## <a name="sample-powershell-code"></a>Código de PowerShell de ejemplo

El siguiente script de PowerShell de ejemplo se utiliza como referencia. Dicho script crea una nueva cuenta de almacenamiento y un contenedor. Luego muestra cómo establecer y eliminar suspensiones legales, crear y bloquear una directiva de retención con duración definida (también conocida como directiva de inmutabilidad) y amplíe el intervalo de retención.

Configuración y comprobación de la cuenta de Azure Storage:

```powershell
$ResourceGroup = "<Enter your resource group>”
$StorageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$container2 = "<Enter another container name>”
$location = "<Enter the storage account location>"

# Log in to the Azure Resource Manager account
Login-AzureRMAccount
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzureRmResourceGroup -Name $ResourceGroup -Location $location

# Create your Azure storage account
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroup -StorageAccountName `
    $StorageAccount -SkuName Standard_LRS -Location $location -Kind Storage

# Create a new container
New-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Create Container 2 with a storage account object
$accountObject = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount
New-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Get a container
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Get a container with an account object
$containerObject = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container

# List containers
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount

# Remove a container (add -Force to dismiss the prompt)
Remove-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container2

# Remove a container with an account object
Remove-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Remove a container with a container object
$containerObject2 = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2
Remove-AzureRmStorageContainer -InputObject $containerObject2
```

Establecimiento y eliminación de retenciones legales:

```powershell
# Set a legal hold
Add-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag1>,<tag2>,...

# with an account object
Add-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>

# with a container object
Add-AzureRmStorageContainerLegalHold -Container $containerObject -Tag <tag4>,<tag5>,...

# Clear a legal hold
Remove-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag2>

# with an account object
Remove-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>,<tag5>

# with a container object
Remove-AzureRmStorageContainerLegalHold -Container $containerObject -Tag <tag4>
```

Creación o actualización de directivas de inmutabilidad:
```powershell
# with an account name or container name
Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container -ImmutabilityPeriod 10

# with an account object
Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -ImmutabilityPeriod 1 -Etag $policy.Etag

# with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 7

# with an immutability policy object
Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -ImmutabilityPeriod 5
```

Recuperación de directivas de inmutabilidad:
```powershell
# Get an immutability policy
Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container

# with an account object
Get-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container

# with a container object
Get-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject
```

Bloqueo de directivas de inmutabilidad (agregue - Force para descartar el mensaje):
```powershell
# with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -force

# with an account name or container name
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -Etag $policy.Etag

# with a container object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -Etag $policy.Etag -force
```

Ampliación de directivas de inmutabilidad:
```powershell

# with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container

$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy

# with an account name or container name
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -ImmutabilityPeriod 11 -Etag $policy.Etag -ExtendPolicy

# with an account object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -ImmutabilityPeriod 12 -Etag `
    $policy.Etag -ExtendPolicy

# with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 13 -Etag $policy.Etag -ExtendPolicy
```

Eliminación de una directiva de inmutabilidad (agregue - Force para descartar el mensaje):
```powershell
# with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
Remove-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy

# with an account name or container name
Remove-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
Remove-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -Etag $policy.Etag

# with a container object
Remove-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject `
    -Etag $policy.Etag

```
