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
ms.openlocfilehash: fcc5c4008c0fdef3b77e436761d8958fe31458d8
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71257369"
---
# <a name="store-business-critical-data-in-azure-blob-storage-immutably"></a>Almacenamiento inmutable de los datos críticos para la empresa en Azure Blob Storage 

El almacenamiento inmutable para Azure Blob Storage permite a los usuarios almacenar objetos de datos críticos para la empresa en un estado WORM (escribir una vez, leer muchas). En este estado, los usuarios no pueden borrar ni modificar los datos durante el intervalo de tiempo especificado por el usuario. Los objetos de blob se pueden crear y leer, aunque no modificar ni eliminar, durante el intervalo de retención. El almacenamiento inmutable está habilitado para cuentas de uso general v2 y cuentas de Blob Storage en todas las regiones de Azure.

## <a name="overview"></a>Información general

El almacenamiento inmutable ayuda a los centros de atención médica, las instituciones financieras y las industrias relacionadas (sobre todo las organizaciones de agente-distribuidor) a almacenar los datos de forma segura. También puede usarse en cualquier escenario para proteger datos críticos e impedir que se eliminen o modifiquen. 

Las aplicaciones típicas son:

- **Cumplimiento normativo**: el almacenamiento inmutable para Azure Blob Storage ayuda a las organizaciones a cumplir SEC 17a-4(f), CFTC 1.31(d), FINRA y otras regulaciones. En una nota técnica de producto de Cohasset Associates se detalla cómo el almacenamiento inmutable aborda estos requisitos reguladores. Este documento se puede descargar en el [portal de confianza de servicios de Microsoft](https://aka.ms/AzureWormStorage). El [Centro de confianza de Azure](https://www.microsoft.com/trustcenter/compliance/compliance-overview) contiene información detallada sobre nuestras certificaciones de cumplimiento.

- **Retención segura de documentos**: el almacenamiento inmutable de Azure Blob Storage garantiza que ningún usuario puede modificar ni eliminar datos, incluidos los usuarios que tienen privilegios administrativos en la cuenta.

- **Suspensión legal**: el almacenamiento inmutable de Azure Blob Storage permite a los usuarios almacenar información confidencial crítica para procedimientos judiciales o uso empresarial en un estado a prueba de manipulación durante el tiempo deseado hasta que se elimina la suspensión. Esta característica no se limita solo a los casos de uso legales, sino que también puede considerarse una suspensión basada en eventos o un bloqueo de la empresa, donde es necesario proteger los datos basados en desencadenadores de eventos o directivas corporativas.

El almacenamiento inmutable admite lo siguiente:

- **[Compatibilidad con directivas de retención con duración definida](#time-based-retention)** : los usuarios pueden establecer directivas para almacenar datos durante un intervalo especificado. Cuando se establece una directiva de retención con duración definida, se pueden crear y leer blobs, pero no se pueden modificar ni eliminar. Una vez transcurrido el período de retención, los blobs se pueden eliminar pero no sobrescribir.

- **[Compatibilidad con directivas de suspensión legal](#legal-holds)** : si el intervalo de retención no se conoce, los usuarios pueden establecer suspensiones legales para almacenar los datos de una manera inmutable hasta que estas desaparezcan.  Cuando se establece una directiva de suspensión legal, se pueden crear y leer blobs, pero no se pueden modificar ni eliminar. Cada suspensión legal está asociada a una etiqueta alfanumérica definida por el usuario que se usa como una cadena de identificación (por ejemplo, un identificador de caso, un nombre de evento, etc.). 

- **Compatibilidad con todos los niveles de blobs:** las directivas WORM son independientes del nivel de Azure Blob Storage y se aplican a todos los niveles: de archivo, frecuente y esporádico. Los usuarios pueden transferir sus datos al nivel que les ofrezca la mayor optimización de costos de acuerdo con sus cargas de trabajo sin alterar la inmutabilidad de los datos.

- **Configuración en el nivel de contenedor**: los usuarios pueden configurar las directivas de retención con duración definida y las etiquetas de suspensión legal a nivel del contenedor. Mediante valores de configuración sencillos en el nivel de contenedor, los usuarios pueden crear y bloquear las directivas de retención con duración definida, ampliar los intervalos de retención, establecer y eliminar suspensiones legales, etc. Estas directivas se aplican a todos los blobs del contenedor, tanto a los nuevos como a los existentes.

- **Compatibilidad con el registro de auditoría**: todos los contenedores incluyen un registro de auditoría de directiva. En él se muestran hasta siete comandos de retención con duración definida para las directivas de retención con duración definida bloqueadas y contiene el identificador de usuario, el tipo de comando, las marcas de tiempo y el intervalo de retención. En el caso de las suspensiones legales, el registro contiene el identificador del usuario, el tipo de comando, las marcas de tiempo y las etiquetas de suspensión legal. Este registro se conserva mientras dure la directiva, de acuerdo con las directrices de regulación SEC 17a-4(f). El [registro de actividad de Azure](../../azure-monitor/platform/activity-logs-overview.md) muestra un registro más completo de todas las actividades del plano de control, mientras que al habilitar los [registros de diagnóstico de Azure](../../azure-monitor/platform/resource-logs-overview.md) se conservan y se muestran las operaciones del plano de datos. Es responsabilidad del usuario almacenar dichos registros de forma persistente, ya que podría ser obligatorio por ley o por otros fines.

## <a name="how-it-works"></a>Cómo funciona

Almacenamiento inmutable para Azure Blob Storage admite dos tipos de directivas inmutables o WORM: retención con duración definida y suspensiones legales. Cuando se aplica una directiva de retención con duración definida o una suspensión legal a un contenedor, todos los blobs existentes pasan al estado WORM inmutable en menos de 30 segundos. Todos los nuevos blobs que se carguen en el contenedor también pasan al estado inmutable. Una vez que todos los blobs han pasado al estado inmutable, la directiva inmutable se confirma y no se permite ninguna operación de sobrescritura o eliminación de los objetos nuevos y existentes en el contenedor inmutable.

Tampoco se permite la eliminación de contenedores y cuentas si no hay ningún blob protegido por una directiva inmutable. La operación Delete Container producirá un error si hay al menos un blob con una directiva de retención de duración definida o una retención legal. Se producirá un error en la eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una retención legal o un blob con un intervalo de retención activo. 

### <a name="time-based-retention"></a>Retención con duración definida

> [!IMPORTANT]
> Una directiva de retención con duración definida debe estar *bloqueada* para que el blob esté en estado inmutable (protegido frente a escritura y eliminación) y, por consiguiente, se cumplan SEC 17a-4(f) y otras regulaciones. Se recomienda bloquear la directiva en un período razonable, normalmente antes de 24 horas. El estado inicial de una directiva de retención con duración definida es *desbloqueada*, lo que permite probar la característica y realizar cambios en la directiva antes de bloquearla. Aunque el estado *desbloqueada* proporciona protección de inmutabilidad, no se recomienda su uso para otros fines que no sean las evaluaciones de la característica a corto plazo. 

Cuando se aplica una directiva de retención con duración definida a un contenedor, todos los blobs de este permanecen en estado inmutable durante el período de retención *efectivo*. El período de retención efectivo para los blobs existentes es igual a la diferencia entre la hora de creación de blob y el intervalo de retención especificado por el usuario.

Para los nuevos blobs, el período de retención efectivo es igual al intervalo de retención especificado por el usuario. Como los usuarios pueden ampliar el intervalo de retención, el almacenamiento inmutable utiliza el valor más reciente del intervalo de retención especificado por el usuario para calcular el período de retención efectivo.

> [!TIP]
> **Ejemplo:** un usuario crea una directiva de retención con duración definida con un intervalo de retención de cinco años.
>
> El blob que hay en ese contenedor, _testblob1_, se creó hace un año. El período de retención efectivo de _testblob1_ es de cuatro años.
>
> Un nuevo blob, _testblob2_, se carga ahora en el contenedor. El período de retención efectivo de este nuevo blob es de cinco años.

Se recomienda una directiva de retención con duración definida desbloqueada solo para probar la característica; no obstante, una directiva debe estar bloqueada para el cumplimiento de SEC 17a-4(f) y otras normas. Una vez que se ha bloqueado una directiva de retención con duración definida, no se puede quitar la directiva y se permite un máximo de 5 aumentos al período de retención efectivo. Para más información sobre cómo establecer y bloquear las directivas de retención con duración definida, consulte la sección [Introducción](#getting-started).

### <a name="legal-holds"></a>Retenciones legales

Si se establece una suspensión legal, tanto los blobs nuevos como los existentes permanecen en estado inmutable hasta que se elimine dicha suspensión. Para más información acerca de cómo establecer y eliminar suspensiones legales, consulte la sección [Introducción](#getting-started).

Un contenedor puede tener una suspensión legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

La tabla siguiente muestra los tipos de operaciones de blob que se deshabilitan para los diferentes escenarios de inmutabilidad. Para más información, consulte la documentación de la [API del servicio Azure Blob](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

|Escenario  |Estado del blob  |Operaciones de blob no permitidas  |
|---------|---------|---------|
|El intervalo de retención efectivo del blob no ha expirado todavía o se ha establecido una retención legal     |Inmutable: protegido frente a eliminación y escritura         | Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Delete Container, Delete Blob, Set Blob Metadata, Put Page, Set Blob Properties,  Snapshot Blob, Incremental Copy Blob, Append Block         |
|El intervalo de retención efectivo del blob ha expirado     |Protegido contra escritura únicamente (están permitidas las operaciones de eliminación)         |Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Todas las suspensiones legales eliminadas y sin ninguna directiva de retención basada en el tiempo establecida en el contenedor     |Mutable         |None         |
|No se crea ninguna directiva WORM (retención basada en el tiempo o suspensión legal)     |Mutable         |None         |

<sup>1</sup> la aplicación permite que estas operaciones creen un blob una vez. Todas las sucesivas operaciones de sobrescritura en una ruta de acceso de blob existente en un contenedor inmutable no se permiten.

## <a name="supported-values"></a>Valores admitidos

### <a name="time-based-retention"></a>Retención con duración definida
- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con directivas inmutables con duración definida bloqueadas es 1000.
- El intervalo de retención mínimo es un día. El máximo es 146 000 días (400 años).
- Para un contenedor, el número máximo de modificaciones para ampliar un intervalo de retención para las directivas inmutables con duración definida bloqueadas es 5.
- En el caso de un contenedor, se retiene un máximo de siete registros de auditoría de la directiva de retención con duración definida para una directiva bloqueada.

### <a name="legal-hold"></a>Suspensión legal
- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con un valor de suspensión legal es 1.000.
- En un contenedor, el número máximo de etiquetas de suspensión legal es 10.
- La longitud mínima de una etiqueta de suspensión legal es de tres caracteres alfanuméricos. La longitud máxima es de 23 caracteres alfanuméricos.
- Para un contenedor, se conservan hasta 10 registros de auditoría de directiva de suspensión legal lo que dura la directiva.

## <a name="pricing"></a>Precios

El uso de esta característica no tiene costo adicional. El precio de los datos inmutables es el mismo que el de los datos normales, mutables. Para información detallada sobre los precios de Azure Blob Storage, consulte la [página de precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="getting-started"></a>Introducción
El almacenamiento inmutable solo está disponible en las cuentas de Blob Storage y uso general v2. Estas cuentas deben administrarse mediante [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Para información sobre la actualización de una cuenta de almacenamiento de uso general v1 existente, consulte [Actualización de una cuenta de almacenamiento](../common/storage-account-upgrade.md).

Las versiones más recientes de [Azure Portal](https://portal.azure.com), la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) y [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) admiten el almacenamiento inmutable para Azure Blob Storage. También se proporciona [compatibilidad con bibliotecas cliente](#client-libraries).

### <a name="azure-portal"></a>Portal de Azure

1. Cree un nuevo contenedor o seleccione un contenedor existente para almacenar los blobs que se deben conservar en estado inmutable.
 El contenedor debe estar en una cuenta de Blob Storage o GPv2.
2. Seleccione **Access policy** (Directiva de acceso) en la configuración del contenedor. Luego, seleccione **+ Agregar directiva** en **Almacenamiento de blobs inmutable**.

    ![Configuración del contenedor en el portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Para habilitar la retención con duración definida, seleccione **Retención con duración definida** en el menú desplegable.

    !["Retención con duración definida" seleccionado en "Tipo de directiva"](media/storage-blob-immutable-storage/portal-image-2.png)

4. Especifique el intervalo de retención en días (los valores aceptables son de 1 a 146 000 días).

    ![Cuadro "Actualizar el período de retención a"](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    El estado inicial de la directiva es desbloqueada, lo que permite probar la característica y realizar cambios en la directiva antes de bloquearla. El bloqueo es esencial para el cumplimiento de normas como SEC 17a-4.

5. Bloquee la directiva. Haga clic con el botón derecho en los puntos suspensivos ( **...** ) y aparecerá el siguiente menú con acciones adicionales:

    !["Directiva de bloqueo" en el menú](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

6. Seleccione **Directiva de bloqueo** y confirme el bloqueo. Ahora la directiva se bloquea y no se puede eliminar; solo se permiten ampliaciones del intervalo de retención. No se permiten eliminaciones e invalidaciones del blob. 

    ![Confirmación de "Directiva de bloqueo" en el menú](media/storage-blob-immutable-storage/portal-image-5-lock-policy.png)

7. Para habilitar las suspensiones legales, seleccione **+ Agregar directiva**. Seleccione **Suspensión legal** en el menú desplegable.

    !["Suspensión legal" en el menú en "Tipo de directiva"](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

8. Cree una suspensión legal con una o varias etiquetas.

    ![Cuadro "Nombre de etiqueta" en el tipo de directiva](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

9. Para borrar una suspensión legal, basta con quitar la etiqueta de identificador de suspensión legal aplicada.

### <a name="azure-cli"></a>CLI de Azure

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

**¿Se puede proporcionar documentación de cumplimiento WORM?**

Sí. Para documentar el cumplimiento, Microsoft usó una de las principales empresas de valoración independientes especializada en la administración de registros y la gobernanza de la información, Cohasset Associates. Dicha empresa se encargó de evaluar el almacenamiento de blobs inmutable de Azure y su cumplimiento de los requisitos específicos del sector de servicios financieros. Cohasset validó que el almacenamiento de blobs inmutable de Azure, cuando se usó para conservar los blobs con duración definida en un estado WORM, cumplía los requisitos de almacenamiento pertinentes de las reglas CFTC 1.31(c)-(d), FINRA 4511 y SEC 17a-4. Microsoft se centró en este conjunto de reglas, ya que representan la guía más prescriptiva globalmente para la retención de registros en las instituciones financieras. El informe de Cohasset está disponible en el [Centro de confianza de servicios de Microsoft](https://aka.ms/AzureWormStorage). Para solicitar una carta de certificación de Microsoft con respecto al cumplimiento WORM, póngase en contacto con el soporte técnico de Azure.

**¿La característica se aplica solo a los blobs en bloques o también a los blobs en páginas y a los blobs en anexos?**

El almacenamiento inmutable se puede usar con cualquier tipo de blobs cuando se establece en el nivel de contenedor, pero se recomienda usar WORM para los contenedores que almacenan principalmente blobs en bloques. A diferencia de los blobs en bloques, los nuevos blobs en páginas y los blobs en anexos se deben crear fuera de un contenedor WORM y luego se deben copiar en él. Después de copiar estos blobs en un contenedor WORM, no se permiten más *anexos* a un blob en anexos ni cambios en un blob en páginas. Por lo tanto, no se recomienda establecer una directiva de WORM en un contenedor que almacene discos duros virtuales (blobs en páginas) para cualquier máquina virtual activa, ya que bloqueará el disco de la máquina virtual.

**¿Es necesario crear una cuenta de almacenamiento para usar esta característica?**

Puede usar el almacenamiento inmutable con cualquier cuenta de Blob Storage y de uso general V2 existente o recién creada. Esta característica está pensada para su uso con blobs en bloques en cuentas de GPv2 y Blob Storage. Las cuentas de almacenamiento de uso general v1 no se admiten, pero se pueden actualizar fácilmente a uso general v2. Para información sobre la actualización de una cuenta de almacenamiento de uso general v1 existente, consulte [Actualización de una cuenta de almacenamiento](../common/storage-account-upgrade.md).

**¿Se puede aplicar una suspensión legal y una directiva de retención con duración definida a la vez?**

Sí, un contenedor puede tener una suspensión legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

**¿Las directivas de suspensión legal son solo para procedimientos judiciales o pueden emplearse en otros casos?**

No, la suspensión legal es simplemente el término general que se usa con la directiva de retención sin duración definida. Su uso no se aplica solamente a procedimientos judiciales. También es útil para deshabilitar la sobrescritura y las eliminaciones con el fin de proteger los datos WORM importantes de la empresa, cuando el período de retención se desconoce. Se puede usar como una directiva de empresa para proteger las cargas de trabajo WORM críticas o como una directiva de almacenamiento provisional antes de que un desencadenador de eventos personalizado requiera el uso de una directiva de retención con duración definida. 

**¿Se puede quitar una suspensión legal o una directiva de retención con duración definida _bloqueada_?**

Solo las directivas de retención con duración definida desbloqueadas se pueden quitar de un contenedor. Una vez que se ha bloqueado una directiva de retención con duración definida, no se puede quitar; solo se permiten extensiones del período de retención efectivo. Se pueden eliminar las etiquetas de suspensión legal. Cuando se eliminan todas las etiquetas legales, se quita la suspensión legal.

**¿Qué ocurre si se intenta eliminar un contenedor con una directiva de retención basada en el tiempo o una retención legal *bloqueada*?**

La operación Delete Container producirá un error si hay al menos un blob con una directiva de retención de duración definida o una retención legal. La operación Delete Container solo se ejecutará correctamente si no existe ningún intervalo de retención activo y no hay retenciones legales. Para poder eliminar el contenedor, antes debe eliminar los blobs.

**¿Qué ocurre si se intenta eliminar una cuenta de almacenamiento con un contenedor WORM que tiene una directiva de retención basada en el tiempo o una retención legal*bloqueada*?**

Se producirá un error en la eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una retención legal o un blob con un intervalo de retención activo. Para poder eliminar la cuenta de almacenamiento, antes debe eliminar todos los contenedores WORM. Para obtener información acerca de la eliminación de contenedores, vea la pregunta anterior.

**¿Puedo mover los datos entre distintos niveles de blob (nivel de acceso frecuente, esporádico, poco frecuente) cuando el blob está en estado inmutable?**

Sí, puede usar el comando Set Blob Tier para mover datos entre los niveles de blob y mantenerlos al mismo tiempo en estado inmutable compatible. El almacenamiento inmutable solamente se admite en los niveles de blob de acceso frecuente, de acceso esporádico y de archivo.

**¿Qué ocurre si dejo de pagar y no ha expirado el intervalo de retención?**

En caso de impago, las directivas de retención de datos normales se aplicarán como estipulado en los términos y condiciones de su contrato con Microsoft.

**¿Se ofrece una evaluación o un período de gracia para probar la característica?**

Sí. Cuando se crea por primera vez una directiva de retención con duración definida, tiene el estado *desbloqueada*. En este estado, puede realizar los cambios que desee en el intervalo de retención, como por ejemplo, aumentarlo o disminuirlo, e incluso eliminar la directiva. Una vez bloqueada la directiva, permanece en este estado hasta que expira el intervalo de retención. Esta directiva bloqueada evita la eliminación y la modificación del intervalo de retención. Se recomienda encarecidamente que utilice el estado *desbloqueado* únicamente para fines de prueba y que bloquee la directiva en un periodo de 24 horas. Estas prácticas ayudan a cumplir SEC 17a-4(f) y otras normativas.

**¿Se puede usar la eliminación temporal junto con las directivas de blob inmutables?**

Sí. La [eliminación temporal para Azure Blob Storage](storage-blob-soft-delete.md) se aplica a todos los contenedores dentro de una cuenta de almacenamiento, con independencia de que exista una directiva de retención con duración definida o una suspensión legal. Se recomienda habilitar la eliminación temporal para mayor protección antes de aplicar y confirmar las directivas WORM inmutables. 

**¿Dónde está disponible la característica?**

El almacenamiento inmutable está disponible en las regiones Government, China y Azure público. Si el almacenamiento inmutable no está disponible en su región, envíe un correo electrónico al servicio de soporte técnico a la dirección azurestoragefeedback@microsoft.com.

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

Eliminación de una directiva de inmutabilidad desbloqueada (agregue - Force para descartar el mensaje):
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
