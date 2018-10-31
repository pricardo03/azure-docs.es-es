---
title: Administración del ciclo de vida de Azure Storage
description: Obtenga información sobre cómo crear reglas de directiva de ciclo de vida para volver a realizar la transición de datos del nivel de acceso frecuente al nivel de almacenamiento de acceso y al nivel de acceso de archivo.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.component: common
ms.openlocfilehash: 05e7a7e3c2824a9b47ff723e91103611871d7ed2
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429565"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Administración del ciclo de vida de Azure Blob Storage (versión preliminar)

Los conjuntos de datos tienen ciclos de vida únicos. A algunos datos se accede con frecuencia al principio del ciclo de vida, pero la necesidad de acceso se reduce drásticamente a medida que pasa el tiempo. Algunos datos permanecen inactivos en la nube y, una vez almacenados, no se suele acceder a ellos. Algunos datos expiran días o meses después de su creación, mientras que otros conjuntos de datos se leen y modifican de forma activa en el transcurso de sus ciclos de vida. La administración del ciclo de vida de Azure Blob Storage (versión preliminar) ofrece una directiva enriquecida basada en reglas que se puede usar para realizar la transición de los datos al nivel de acceso mejorado y para hacer que los datos expiren cuando finalice su ciclo de vida.

La directiva de administración del ciclo de vida permite:

- Realizar la transición de blobs a un nivel de almacenamiento de acceso más esporádico (frecuente a esporádico, frecuente a archivo o esporádico a archivo) para optimizar el rendimiento y el costo
- Eliminar los blobs al final de sus ciclos de vida
- Definir reglas que se ejecutarán una vez al día al nivel de cuenta de almacenamiento (admite las cuentas GPv2 y de almacenamiento de blobs)
- Aplicar reglas a contenedores o a un subconjunto de blobs (mediante prefijos como filtros)

Considere el caso de que un conjunto de datos al que se accede con frecuencia durante la fase temprana del ciclo de vida solo sea necesario en ocasiones después de dos semanas y al que se acceda en raras ocasiones transcurrido un mes y más tiempo. En este escenario, el almacenamiento de acceso frecuente es mejor durante las fases tempranas, el almacenamiento de acceso esporádico es más apropiado para el acceso ocasional y el almacenamiento de archivo es el mejor nivel cuando los datos tienen más de un mes. Con el ajuste de los niveles de almacenamiento en relación con la antigüedad de los datos, puede designar las opciones de almacenamiento menos caras para satisfacer sus necesidades. Para conseguir esta transición, las directivas de administración del ciclo de vida se encuentran disponibles para mover los datos que tienen más tiempo a niveles de almacenamiento de acceso más esporádico.

## <a name="storage-account-support"></a>Compatibilidad con la cuenta de almacenamiento

La directiva de administración del ciclo de vida está disponible con la cuenta de uso general v2 (GPv2) y con la cuenta de Blob Storage. Puede convertir una cuenta existente de uso general (GPv1) en una cuenta de GPv2 con tan solo hacer un clic en Azure Portal. Para más información sobre las cuentas de almacenamiento, consulte [Introducción a las cuentas de Azure Storage](../common/storage-account-overview.md).  

## <a name="pricing"></a>Precios 

La característica de administración del ciclo de vida es gratuita en la versión preliminar. A los clientes se les cobra el costo operativo habitual para las llamadas API [Mostrar blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) y [Establecer el nivel del blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier). Vea los [precios de blobs en bloques](https://azure.microsoft.com/pricing/details/storage/blobs/) para obtener más información sobre los precios.

## <a name="register-for-preview"></a>Registro en la versión preliminar 
Para inscribirse en la versión preliminar pública, debe enviar una solicitud para registrar esta característica en la suscripción. Una vez aprobada su solicitud (en el plazo de unos días), cualquier cuenta existente o nueva de GPv2 o Blob Storage de las regiones Oeste de EE. UU. 2, Centro-oeste de EE. UU., Este de EE. UU. 2 y Europa Occidental tendrá la característica habilitada. Durante la versión preliminar, solo se admiten los blobs en bloques. Como sucede con la mayoría de las versiones preliminares, esta característica no debe usarse para cargas de trabajo de producción hasta que se lance la disponibilidad general.

Para enviar una solicitud, ejecute los siguientes comandos de PowerShell o de la CLI.

### <a name="powershell"></a>PowerShell

Para enviar una solicitud:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Puede comprobar el estado de aprobación del registro con el siguiente comando:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Si la característica se aprueba y registra correctamente, debe recibir el estado "Registrado".

### <a name="azure-cli"></a>Azure CLI

Para enviar una solicitud: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Puede comprobar el estado de aprobación del registro con el siguiente comando:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Si la característica se aprueba y registra correctamente, debe recibir el estado "Registrado". 


## <a name="add-or-remove-policies"></a>Adición o eliminación de directivas 

Puede agregar, editar o eliminar una directiva en Azure Portal, con [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), [API REST](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/managementpolicies_createorupdate) o herramientas de cliente en los siguientes lenguajes: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0) y [Ruby](   https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Para desplazarse a su cuenta de almacenamiento, seleccione Todos los recursos y, después, seleccione la cuenta de almacenamiento.

3. En la hoja Configuración, haga clic en **Administración del ciclo de vida**, que se agrupa bajo Blob service, para ver o cambiar las directivas.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Si habilita reglas de firewall para la cuenta de almacenamiento, puede que se bloqueen las solicitudes de administración del ciclo de vida. Puede proporcionar excepciones para desbloquearlas. Para más información, vea la sección Excepciones en [Configuración de redes virtuales y firewalls de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>Directivas

Una directiva de administración del ciclo de vida es una colección de reglas en un documento JSON:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "rule1",
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


Dentro de una directiva, se necesitan dos parámetros:

| Nombre de parámetro | Tipo de parámetro | Notas |
|----------------|----------------|-------|
| version        | Una cadena expresada como `x.x` | El número de versión preliminar es 0.5 |
| reglas          | Una matriz de objetos de regla | Se requiere al menos una regla en cada directiva. Durante la versión preliminar, puede especificar hasta cuatro reglas por directiva. |

Los parámetros necesarios dentro de una regla son:

| Nombre de parámetro | Tipo de parámetro | Notas |
|----------------|----------------|-------|
| NOMBRE           | string | Un nombre de regla puede contener cualquier combinación de caracteres alfanuméricos. El nombre de regla distingue mayúsculas de minúsculas. Debe ser único dentro de una directiva. |
| Tipo           | Un valor de enumeración | El valor válido para la versión preliminar es `Lifecycle` |
| definición     | Un objeto que define la regla del ciclo de vida | Cada definición se compone de un conjunto de filtros y un conjunto de acciones. |

## <a name="rules"></a>Reglas

Cada definición de regla incluye un conjunto de filtros y un conjunto de acciones. La regla de ejemplo siguiente modifica el nivel de blobs en bloques base con el prefijo `container1/foo`. En la directiva, estas reglas se definen como:

- Establecer el nivel del blob en almacenamiento de acceso esporádico 30 días después de la última modificación
- Establecer el nivel del blob en almacenamiento de archivo 90 días después de la última modificación
- Eliminar el blob 2555 días (7 años) después de la última modificación
- Eliminar instantáneas de blob 90 días después de la creación de las instantáneas

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "ruleFoo", 
      "type": "Lifecycle", 
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### <a name="rule-filters"></a>Filtros de reglas

Los filtros limitan las acciones de regla a un subconjunto de blobs dentro de la cuenta de almacenamiento. Si se han definido varios filtros, un valor lógico `AND` se aplica en todos los filtros.

Durante la versión preliminar, los filtros válidos incluyen:

| Nombre de filtro | Tipo de filtro | Notas | Es obligatorio |
|-------------|-------------|-------|-------------|
| blobTypes   | Una matriz de valores de enumeración predefinidos. | En la versión preliminar, solo se admite `blockBlob`. | SÍ |
| prefixMatch | Una matriz de cadenas de prefijos con los que debe hacer coincidencias. Una cadena de prefijos debe comenzar con el nombre de un contenedor. Por ejemplo, si todos los blobs bajo "https://myaccount.blob.core.windows.net/mycontainer/mydir/.." deben coincidir con una regla, el prefijo es "mycontainer/mydir". | Si no está definida, esta regla se aplica a todos los blobs dentro de la cuenta. | Sin  |

### <a name="rule-actions"></a>Acciones de regla

Las acciones se aplican a los blobs filtrados cuando se cumple la condición de ejecución.

En la versión preliminar, la administración del ciclo de vida admite el cambio de niveles y la eliminación de blobs e instantáneas de blob. Cada regla debe tener al menos una acción definida en blobs o instantáneas de blob.

| .        | Blob de base                                   | Instantánea      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Admite blobs actualmente en el nivel de acceso frecuente         | No compatible |
| tierToArchive | Admite blobs actualmente en el nivel de acceso frecuente o esporádico | No compatible |
| delete        | Compatible                                   | Compatible     |

>[!NOTE] 
Si se define más de una acción en el mismo blob, la administración del ciclo de vida aplica la acción menos cara al blob. (por ejemplo, la acción `delete` resulta más económica que la acción `tierToArchive`. La acción `tierToArchive` es más económica que la acción `tierToCool`).

En la versión preliminar, las condiciones de ejecución de la acción se basan en la antigüedad. El blob de base utiliza la hora de la última modificación para realizar el seguimiento de la antigüedad, y las instantáneas de blob usan la hora de creación de la instantánea para realizar el seguimiento de la antigüedad.

| Condición de ejecución de la acción | Valor de la condición | DESCRIPCIÓN |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Valor entero que indica la antigüedad en días | Condición válida para las acciones de blob de base |
| daysAfterCreationGreaterThan     | Valor entero que indica la antigüedad en días | Condición válida para las acciones de instantánea de blob | 

## <a name="examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo abordar escenarios comunes con las reglas de directivas del ciclo de vida.

### <a name="move-aging-data-to-a-cooler-tier"></a>Cambio de los datos antiguos a un nivel de acceso más esporádico

En el ejemplo siguiente se demuestra cómo realizar la transición de blobs en bloques con el prefijo `container1/foo` o `container2/bar`. La directiva realiza la transición de los blobs que no se han modificado durante más de 30 días al almacenamiento de acceso esporádico, y los blobs no modificados en 90 días al nivel de acceso de archivo:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "agingRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "container1/foo", "container2/bar" ]
          },
          "actions": {
            "baseBlob": {
              "tierToCool": { "daysAfterModificationGreaterThan": 30 },
              "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Archivado de datos en la ingesta 

Algunos datos permanecen inactivos en la nube y, una vez almacenados, no se accede a ellos prácticamente nunca. Es mejor archivar estos datos en cuanto se ingieren. La siguiente directiva del ciclo de vida está configurada para archivar datos en la ingesta. Este ejemplo realiza la transición de los blobs en bloques en la cuenta de almacenamiento en el contenedor `archivecontainer` inmediatamente al nivel de acceso de archivo. La transición inmediata se realiza al actuar en los blobs 0 días después de la hora de la última modificación:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "archiveRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "archivecontainer" ]
          },
          "actions": {
            "baseBlob": { 
                "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
            }
          }
        }      
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Expiración de datos en función de la antigüedad

Se espera que algunos datos expiren días o meses después de su creación, a fin de reducir costos o cumplir las normativas gubernamentales. Se puede configurar una directiva de administración del ciclo de vida para que expire en una fecha mediante su eliminación en función de la antigüedad de los datos. En el ejemplo siguiente se muestra una directiva que elimina todos los blobs en bloques (sin prefijo especificado) con una antigüedad superior a 365 días.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "expirationRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ]
          },
          "actions": {
            "baseBlob": {
              "delete": { "daysAfterModificationGreaterThan": 365 }
            }
          }
        }      
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Eliminación de instantáneas antiguas

En el caso de los datos que se modifican y a los que se accede con regularidad a lo largo de su ciclo de vida, las instantáneas suelen utilizarse para realizar un seguimiento de las versiones anteriores de los datos. Puede crear una directiva que elimine las instantáneas anteriores en función de la antigüedad de la instantánea. La antigüedad de la instantánea se determina mediante la evaluación de la hora de creación de la instantánea. Esta regla de directiva elimina las instantáneas de blobs en bloques en el contenedor `activedata` con una antigüedad de 90 días o más a contar desde el momento de la creación de la instantánea.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "snapshotRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "activedata" ]
          },
          "actions": {            
            "snapshot": {
              "delete": { "daysAfterCreationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```
## <a name="faq"></a>Preguntas más frecuentes
### <a name="i-created-a-new-policy-why-are-the-actions-specified-not-executed-immediately"></a>He creado una directiva nueva, ¿por qué las acciones especificadas no se ejecutan inmediatamente? 

La plataforma ejecuta las directivas de ciclo de vida una vez al día. Una vez que se establece una nueva directiva, las acciones, como la organización en niveles o la eliminación, pueden tardar hasta 24 horas en iniciarse o ejecutarse.  

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo recuperar datos tras la eliminación accidental:

- [Eliminación temporal de blobs de Azure Storage ](../blobs/storage-blob-soft-delete.md)
