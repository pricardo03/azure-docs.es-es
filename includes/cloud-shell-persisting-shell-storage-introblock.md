---
author: jluk
ms.service: cloud-shell
ms.topic: persist-storage
ms.date: 9/7/2018
ms.author: juluk
ms.openlocfilehash: c28441b6fe25b3480a55b79682d5067b19e3023a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60200261"
---
# <a name="persist-files-in-azure-cloud-shell"></a>Persistencia de archivos en Azure Cloud Shell
Cloud Shell utiliza Azure File Storage para conservar los archivos entre sesiones. En el primer inicio, Cloud Shell le pedirá que asocie un recurso compartido de archivos nuevo o existente para conservar los archivos entre sesiones.

> [!NOTE]
> Bash y PowerShell comparten el mismo recurso compartido de archivos. Solo un recurso compartido de archivos se puede asociar con el montaje automático en Cloud Shell.

## <a name="create-new-storage"></a>creación de nuevo almacenamiento

Al usar la configuración básica y seleccionar solo una suscripción, Cloud Shell crea tres recursos en su nombre en la región admitida que esté más próxima:
* Grupos de recursos: `cloud-shell-storage-<region>`
* Cuenta de almacenamiento: `cs<uniqueGuid>`
* Recurso compartido de archivos: `cs-<user>-<domain>-com-<uniqueGuid>`

![La configuración de la suscripción](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

El recurso compartido de archivos se monta como `clouddrive` en su directorio `$Home`. Se trata de una acción única y el recurso compartido de archivos se monta automáticamente en sesiones posteriores. 

> [!NOTE]
> Por motivos de seguridad, cada usuario debe aprovisionar su propia cuenta de almacenamiento.  En el control de acceso basado en roles (RBAC), los usuarios deben tener acceso como colaboradores, o un rol superior, en el nivel de la cuenta de almacenamiento.

En Bash, el recurso compartido de archivos también contiene una imagen de 5 GB que se crea automáticamente que conserva automáticamente los datos en su directorio `$Home`. Esto se aplica para Bash y PowerShell.

## <a name="use-existing-resources"></a>Uso de recursos existentes

Mediante la opción avanzada puede asociar recursos existentes. Al seleccionar una región de Cloud Shell, debe seleccionar una cuenta de almacenamiento de respaldo coubicada en la misma región. Por ejemplo, si la región asignada es Oeste de EE. UU. y debe asociar un recurso compartido de archivos que también reside en Oeste de EE. UU.

Cuando aparezca el mensaje del programa de instalación, seleccione **Mostrar configuración avanzada** para ver otras opciones. Las opciones de almacenamiento rellenadas se filtran por cuentas de almacenamiento con redundancia local (LRS), almacenamiento con redundancia geográfica (GRS) y almacenamiento con redundancia de zona (ZRS). 

> [!NOTE]
> Se recomienda usar cuentas de almacenamiento con redundancia geográfica o con redundancia de zona para contar con resistencia adicional para el recurso compartido de archivos de respaldo. El tipo de redundancia depende de los objetivos y las preferencias de precio. [Obtenga más información sobre las opciones de replicación para las cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

![La configuración de grupo de recursos](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="supported-storage-regions"></a>Regiones de almacenamiento admitidas
Las cuentas de almacenamiento de Azure asociadas tienen que residir en la misma región que la máquina de Cloud Shell en la que se montan. Para encontrar la región actual puede ejecutar `env` en Bash y buscar la variable `ACC_LOCATION`. Los recursos compartidos de archivos existentes reciben una imagen de 5 GB para que el usuario conserve el directorio `$Home`.

Las máquinas de Cloud Shell existen en las regiones siguientes:

|Ámbito|Region|
|---|---|
|América|Este de EE. UU., centro-sur de EE. UU. y oeste de EE. UU.|
|Europa|Norte de Europa y Oeste de Europa|
|Asia Pacífico|India central, Sudeste Asiático|

## <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Restringir la creación de recursos con una directiva de recursos de Azure
Las cuentas de almacenamiento creadas en Cloud Shell se etiquetan con `ms-resource-usage:azure-cloud-shell`. Si quiere impedir que los usuarios creen cuentas de almacenamiento en Cloud Shell, cree una [directiva de recursos de Azure para etiquetas](../articles/azure-policy/json-samples.md) que se desencadene mediante esta etiqueta específica.
