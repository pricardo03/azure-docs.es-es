---
title: Administración del ciclo de vida de Azure Storage
description: Aprenda a crear reglas de directiva de ciclo de vida para realizar la transición de datos antiguos del nivel de almacenamiento de acceso frecuente al nivel de almacenamiento de acceso esporádico y al nivel de almacenamiento de archivo.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 11/04/2018
ms.author: yzheng
ms.subservice: common
ms.openlocfilehash: 1428c2925ab57642899732bd4504b2d5b38781a8
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315154"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Administración del ciclo de vida de Azure Blob Storage (versión preliminar)

Los conjuntos de datos tienen ciclos de vida únicos. Al principio del ciclo de vida, las personas acceden con frecuencia a algunos datos. Pero la necesidad de acceso desciende drásticamente a medida que los datos se hacen más antiguos. Algunos datos permanecen inactivos en la nube y, una vez almacenados, no se suele acceder a ellos. Otros datos expiran en días o meses después de su creación, mientras que otros conjuntos de datos se leen y modifican de forma activa en el transcurso de sus ciclos de vida. La administración del ciclo de vida de Azure Blob Storage (versión preliminar) ofrece una directiva enriquecida basada en reglas para las cuentas de GPv2 y Blob Storage. Use la directiva para realizar la transición de los datos a los niveles de acceso adecuados o hacer que expiren al final de su ciclo de vida.

La directiva de administración del ciclo de vida le permite:

- Realizar la transición de los blobs a un nivel de almacenamiento de acceso esporádico (frecuente a esporádico, frecuente a archivo o esporádico a archivo) para optimizar el rendimiento y el costo
- Eliminar los blobs al final de sus ciclos de vida
- Definir reglas que se ejecutarán una vez al día en el nivel de cuenta de almacenamiento
- Aplicar reglas a contenedores o a un subconjunto de blobs (mediante prefijos como filtros)

Considere el escenario donde un conjunto de datos tiene acceso frecuente durante las primeras fases del ciclo de vida, pero luego solo ocasionalmente al cabo de dos semanas. Transcurrido el primer mes, rara vez se accede al conjunto de datos. En este escenario, es mejor el almacenamiento de acceso frecuente durante las primeras etapas. El almacenamiento de acceso esporádico es más adecuado para acceso ocasional y el almacenamiento de archivo es la mejor opción de nivel después de que los datos tienen una antigüedad de un mes. Con el ajuste de los niveles de almacenamiento en relación con la antigüedad de los datos, puede designar las opciones de almacenamiento menos caras para satisfacer sus necesidades. Para conseguir esta transición, las reglas de directivas de administración del ciclo de vida se encuentran disponibles para mover los datos antiguos a niveles de almacenamiento de acceso más esporádico.

## <a name="storage-account-support"></a>Compatibilidad con la cuenta de almacenamiento

La directiva de administración del ciclo de vida está disponible con cuentas de uso general v2 (GPv2) y de Blob Storage. En Azure Portal, puede convertir una cuenta existente de uso general (GPv1) en una cuenta de GPv2 con tan solo un clic. Para más información sobre las cuentas de almacenamiento, vea [Introducción a las cuentas de Azure Storage](../common/storage-account-overview.md).  

## <a name="pricing"></a>Precios 

La característica de administración del ciclo de vida es gratuita en la versión preliminar. A los clientes se les cobra el costo operativo habitual para las llamadas API [Mostrar blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) y [Establecer el nivel del blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier). Para más información sobre los precios, consulte [Precios de los blobs en bloques](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="register-for-preview"></a>Registro en la versión preliminar 
Para inscribirse en la versión preliminar pública, debe enviar una solicitud para registrar esta característica en la suscripción. Las solicitudes se aprueban normalmente al cabo de 72 horas. Tras la aprobación, todas las cuentas nuevas y existentes de GPv2 o de Blob Storage de las siguientes regiones incluyen la característica: Oeste de EE. UU. 2, Centro-oeste de EE. UU., Este de EE. UU. 2 y Europa Occidental. La versión preliminar solo admite blobs en bloques. Como sucede con la mayoría de las versiones preliminares, esta característica no debe usarse para cargas de trabajo de producción hasta que se lance la disponibilidad general.

Para enviar una solicitud, ejecute los siguientes comandos de PowerShell o de la CLI.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para enviar una solicitud:

```powershell
Register-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Puede comprobar el estado de aprobación del registro con el siguiente comando:
```powershell
Get-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Con el registro adecuado y la aprobación, recibirá el estado *Registrado* cuando envíe las solicitudes anteriores.

### <a name="azure-cli"></a>Azure CLI

Para enviar una solicitud: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Puede comprobar el estado de aprobación del registro con el siguiente comando:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Con el registro adecuado y la aprobación, recibirá el estado *Registrado* cuando envíe las solicitudes anteriores.


## <a name="add-or-remove-a-policy"></a>Incorporación o eliminación de una directiva 

Puede agregar, editar o quitar una directiva mediante Azure Portal, [PowerShell](https://www.powershellgallery.com/packages/Az.Storage), la [CLI de Azure](https://docs.microsoft.com/cli/azure/ext/storage-preview/storage/account/management-policy?view=azure-cli-latest#ext-storage-preview-az-storage-account-management-policy-create), las [API REST](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/createorupdate) o herramientas de cliente en los siguientes lenguajes: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Seleccione **Todos los recursos** y elija su cuenta de almacenamiento.

3. Seleccione **Administración del ciclo de vida (versión preliminar)** agrupado en Blob Service para ver o cambiar la directiva.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }'

Set-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

### <a name="azure-cli"></a>Azure CLI

```
az account set --subscription "[subscriptionName]”
az extension add --name storage-preview
az storage account management-policy show --resource-group [resourceGroupName] --account-name [accountName]
```

> [!NOTE]
> Si habilita reglas de firewall para la cuenta de almacenamiento, puede que se bloqueen las solicitudes de administración del ciclo de vida. Puede desbloquear estas solicitudes si proporciona excepciones. Para más información, consulte la sección Excepciones en [Configuración de firewalls y redes virtuales](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policy"></a>Directiva

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


una directiva requiere dos parámetros:

| Nombre de parámetro | Tipo de parámetro | Notas |
|----------------|----------------|-------|
| version        | Una cadena expresada como `x.x` | El número de versión preliminar es 0.5. |
| reglas          | Una matriz de objetos de regla | Se requiere al menos una regla en cada directiva. Durante la versión preliminar, puede especificar hasta cuatro reglas por directiva. |

Cada regla dentro de la directiva requiere tres parámetros:

| Nombre de parámetro | Tipo de parámetro | Notas |
|----------------|----------------|-------|
| NOMBRE           | string | Un nombre de regla puede incluir cualquier combinación de caracteres alfanuméricos. El nombre de regla distingue mayúsculas de minúsculas. Debe ser único dentro de una directiva. |
| Tipo           | Un valor de enumeración | El valor válido para la versión preliminar es `Lifecycle`. |
| definición     | Un objeto que define la regla del ciclo de vida | Cada definición se compone de un conjunto de filtros y un conjunto de acciones. |

## <a name="rules"></a>Reglas

Cada definición de regla incluye un conjunto de filtros y un conjunto de acciones. El [conjunto de filtros](#rule-filters) limita las acciones de regla a un determinado conjunto de objetos dentro de un contenedor o nombres de objetos. El [conjunto de acciones ](#rule-actions) aplica las acciones de nivel o eliminación al conjunto filtrado de objetos.

### <a name="sample-rule"></a>Ejemplo de regla
El siguiente ejemplo de regla filtra la cuenta para ejecutar las acciones solo en `container1/foo`. Ejecute las siguientes acciones para todos los objetos que existen dentro de `container1` **Y** que comienzan con `foo`: 

- Establecer el nivel de blob en nivel esporádico 30 días después de la última modificación
- Establecer el nivel de blob en nivel de almacenamiento de archivo 90 días después de la última modificación
- Eliminar el blob 2555 días (siete años) después de la última modificación
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

Los filtros limitan las acciones de regla a un subconjunto de blobs dentro de la cuenta de almacenamiento. Si se define más de un filtro, se ejecuta un valor lógico `AND` en todos los filtros.

Durante la versión preliminar, los filtros válidos incluyen:

| Nombre de filtro | Tipo de filtro | Notas | Es obligatorio |
|-------------|-------------|-------|-------------|
| blobTypes   | Una matriz de valores de enumeración predefinidos. | La versión preliminar solo admite `blockBlob`. | Sí |
| prefixMatch | Una matriz de cadenas de prefijos con los que debe hacer coincidencias. Una cadena de prefijos debe comenzar con el nombre de un contenedor. Por ejemplo, si desea hacer coincidir todos los blobs "https:\//myaccount.blob.core.windows.net/container1/foo/..." para una regla, es el prefixMatch `container1/foo`. | Si no define prefixMatch, las reglas se aplican a todos los blobs dentro de la cuenta. | Sin  |

### <a name="rule-actions"></a>Acciones de regla

Las acciones se aplican a los blobs filtrados cuando se cumple la condición de ejecución.

En la versión preliminar, la administración del ciclo de vida admite el cambio de niveles y la eliminación de blobs e instantáneas de blob. Defina al menos una acción para cada regla en los blobs o las instantáneas de blob.

| .        | Blob de base                                   | Instantánea      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Admite blobs actualmente en el nivel de acceso frecuente.         | No compatible |
| tierToArchive | Admite blobs actualmente en el nivel de acceso frecuente o esporádico. | No compatible |
| delete        | Compatible                                   | Compatible     |

> [!NOTE]
> Si define más de una acción en el mismo blob, la administración del ciclo de vida aplica la acción menos cara al blob. Por ejemplo, la acción `delete` es más económica que la acción `tierToArchive`. La acción `tierToArchive` es más económica que la acción `tierToCool`.

En la versión preliminar, las condiciones de ejecución de la acción se basan en la antigüedad. Para realizar el seguimiento de la antigüedad, los blobs de base usan la hora de la última modificación y las instantáneas de blob usan la hora de creación de la instantánea.

| Condición de ejecución de la acción | Valor de la condición | DESCRIPCIÓN |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Valor entero que indica la antigüedad en días | Condición válida para las acciones de blob de base |
| daysAfterCreationGreaterThan     | Valor entero que indica la antigüedad en días | Condición válida para las acciones de instantánea de blob | 

## <a name="examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo abordar escenarios comunes con las reglas de directivas del ciclo de vida.

### <a name="move-aging-data-to-a-cooler-tier"></a>Cambio de los datos antiguos a un nivel de acceso más esporádico

En este ejemplo se muestra cómo realizar la transición de blobs en bloques con el prefijo `container1/foo` o `container2/bar`. La directiva realiza la transición de los blobs que no se han modificado durante más de 30 días al almacenamiento de acceso esporádico, y los blobs no modificados en 90 días al nivel de acceso de archivo:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "agingRule",
      "type": "Lifecycle",
      "definition": {
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

Algunos datos permanecen inactivos en la nube y, una vez almacenados, no se accede a ellos prácticamente nunca. Archive estos datos inmediatamente una vez ingeridos. La siguiente directiva del ciclo de vida está configurada para archivar datos en la ingesta. Este ejemplo realiza la transición de los blobs en bloques en la cuenta de almacenamiento en el contenedor `archivecontainer` inmediatamente al nivel de acceso de archivo. La transición inmediata se realiza al actuar en los blobs 0 días después de la hora de la última modificación:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "archiveRule",
      "type": "Lifecycle",
      "definition": {
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

Se espera que algunos datos expiren días o meses después de su creación con el fin de reducir costos o cumplir los requisitos gubernamentales. Puede configurar una directiva de administración del ciclo de vida para que los datos expiren mediante eliminación en función de su antigüedad. En el ejemplo siguiente se muestra una directiva que elimina todos los blobs en bloques (sin prefijo especificado) con una antigüedad superior a 365 días.

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "expirationRule",
      "type": "Lifecycle",
      "definition": {
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
    "definition": {
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
## <a name="faq---i-created-a-new-policy-why-are-the-actions-not-run-immediately"></a>Preguntas frecuentes: He creado una directiva, ¿por qué las acciones no se ejecutan inmediatamente? 

La plataforma ejecuta la directiva del ciclo de vida una vez al día. Una vez establecida una nueva directiva, puede que algunas acciones (por ejemplo, organización en niveles y eliminación) tarden en iniciarse y ejecutarse hasta 24 horas.  

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo recuperar datos tras la eliminación accidental:

- [Eliminación temporalmente de blobs de Azure Storage](../blobs/storage-blob-soft-delete.md)
