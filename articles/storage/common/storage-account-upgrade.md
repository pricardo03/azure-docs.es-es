---
title: 'Actualización a una cuenta de almacenamiento de uso general v2: Azure Storage | Microsoft Docs'
description: Actualización a una cuenta de almacenamiento de uso general v2.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: tamram
ms.openlocfilehash: 6e77c4836531a7efd0b52b9a411ac40ff6a613fa
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224501"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Actualización a una cuenta de almacenamiento de uso general v2

Las cuentas de almacenamiento de uso general v2 son compatibles con las últimas características de Azure Storage, e incorporan todas las funcionalidades de las cuentas de Blob Storage y de uso general v1. Las cuentas de uso general v2 se recomiendan para la mayoría de los escenarios de almacenamiento. Las cuentas de uso general v2 ofrecen los precios de capacidad por gigabyte más bajos para Azure Storage, así como los precios de transacción más competitivos del sector.

Actualizar a una cuenta de almacenamiento de uso general v2 desde la cuenta de uso general v1 o de Blob Storage es sencillo. Puede usar Azure Portal, PowerShell, o la CLI de Azure para realizar la actualización. La actualización de una cuenta no se puede invertir y puede provocar cargos de facturación.

## <a name="upgrade-using-the-azure-portal"></a>Actualización con Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a la cuenta de almacenamiento.
3. En la sección **Configuración**, haga clic en **Configuración**.
4. En **Tipo de cuenta**, haga clic en **Actualizar**.
5. En **Confirmar actualización**, escriba el nombre de la cuenta. 
6. Haga clic en **Actualizar** en la parte inferior de la hoja.

## <a name="upgrade-with-powershell"></a>Actualización con PowerShell

Para actualizar una cuenta de uso general v1 a una cuenta de uso general v2 mediante PowerShell, primero hay que actualizar PowerShell, con el fin de usar la versión más reciente del módulo **AzureRm.Storage**. Para obtener información acerca de cómo instalar PowerShell, consulte [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). 

Luego, llame al siguiente comando para actualizar la cuenta, pero sustituya el nombre de su grupo de recursos y cuenta de almacenamiento:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Actualización con la CLI de Azure

Para actualizar una cuenta de uso general v1 a una cuenta de uso general v2 mediante la CLI de Azure, en primer lugar instale la versión más reciente de la CLI de Azure. Para obtener información acerca de cómo instalar la CLI, consulte [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (Instalación de la CLI de Azure 2.0). 

Luego, llame al siguiente comando para actualizar la cuenta, pero sustituya el nombre de su grupo de recursos y cuenta de almacenamiento:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
``` 

## <a name="specify-an-access-tier-for-blob-data"></a>Especificación de un nivel de acceso para los datos de blob

Las cuentas de uso general v2 son compatibles con todos los servicios de almacenamiento y objetos de datos de Azure, pero los niveles de acceso están disponibles solo para blobs en Blob Storage. Al actualizar a una cuenta de almacenamiento de uso general v2, puede especificar un nivel de acceso para los datos de blob. 

Los niveles de acceso le permiten elegir el almacenamiento más rentable en función de los patrones de uso previsto. Los blobs en bloques pueden almacenarse en nivel con un acceso frecuente, esporádico o de archivo. Para más información acerca de los niveles de acceso, consulte [Azure Blob Storage: niveles de almacenamiento de archivo, esporádico y frecuente](../blobs/storage-blob-storage-tiers.md).

De forma predeterminada, se crea una nueva cuenta de almacenamiento en el nivel de almacenamiento de acceso frecuente y se actualiza una cuenta de almacenamiento de uso general v1 a la capa de acceso frecuente. Si está explorando qué nivel de acceso debe usar para los datos después de la actualización, considere su escenario. Hay dos escenarios típicos del usuario para migrar a una cuenta de uso general v2:

* Tiene una cuenta de almacenamiento de uso general v1 y desea evaluar un cambio a una cuenta de almacenamiento de uso general v2 con la capa de almacenamiento correcta para datos de blob.
* Ha decidido usar una cuenta de almacenamiento de uso general v2 o ya tiene una y quiere evaluar si debe usar el nivel de almacenamiento de acceso frecuente o esporádico para los datos de blob.

En ambos casos, la principal prioridad es estimar el costo de almacenamiento, acceso y uso de los datos almacenados en una cuenta de almacenamiento de uso general v2 y compararlo con los costos actuales.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Calculo de los costos de los patrones de uso actual

Para calcular el costo de almacenamiento y acceso a los datos de blob en una cuenta de almacenamiento de uso general v2 en un nivel específico, es preciso evaluar el patrón de uso existente o realizar una aproximación al patrón de uso esperado. En general, será preciso conocer:

* El consumo de almacenamiento de blobs, en gigabytes, incluido:
    - ¿Cuántos datos se almacenan en la cuenta de almacenamiento?
    - ¿Cómo cambia el volumen de datos mensualmente? Los datos nuevos, ¿reemplazan constantemente a los antiguos?
* El patrón de acceso principal para los datos de almacenamiento de blobs, incluido:
    - ¿Cuántos datos se leen y se escriben en la cuenta de almacenamiento? 
    - ¿Cuántas operaciones de lectura frente a operaciones de escritura se producen en los datos de la cuenta de almacenamiento?

Para decidir sobre el mejor nivel de acceso para sus necesidades, puede resultar útil determinar cuánta capacidad están usando los datos de blob, y cómo se están usando esos datos. 

Para recopilar datos de uso de la cuenta de almacenamiento antes de la migración, puede supervisar la cuenta de almacenamiento mediante [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor realiza el registro y proporciona datos de métricas para servicios de Azure, incluido Azure Storage. 

Para supervisar los datos de consumo para blobs en la cuenta de almacenamiento, habilite las métricas de capacidad en Azure Monitor. Las métricas de capacidad graban datos sobre cuánto almacenamiento usan a diario los blobs en su cuenta. Las métricas de capacidad pueden utilizarse para calcular el costo de almacenamiento de datos en la cuenta de almacenamiento. Para informarse sobre cómo se fija el precio de capacidad de almacenamiento de blobs para cada tipo de cuenta, consulte [Precios de los blob en bloque](https://azure.microsoft.com/pricing/details/storage/blobs/).

Para supervisar los patrones de acceso a los datos para Blob Storage, es preciso habilitar las métricas de transacción en Azure Monitor. Se puede filtrar por diferentes operaciones de Azure Storage para calcular la frecuencia con la que se llama a cada uno. Para más información sobre cómo los distintos tipos de transacciones tienen un precio para los blobs en bloques y blobs en anexos para cada tipo de cuenta, consulte [Precios de los blob en bloque](https://azure.microsoft.com/pricing/details/storage/blobs/).  

Para más información acerca de la recopilación de métricas de Azure Monitor, consulte [Métricas de Azure Storage en Azure Monitor](storage-metrics-in-azure-monitor.md).

## <a name="next-steps"></a>Pasos siguientes

- [crear una cuenta de almacenamiento](storage-quickstart-create-account.md)
- [Administración de cuentas de Azure Storage](storage-account-manage.md)