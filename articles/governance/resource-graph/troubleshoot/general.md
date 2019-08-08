---
title: Solución de errores comunes
description: Aprenda a solucionar problemas relacionados con la consulta de recursos de Azure con Azure Resource Graph.
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/24/2019
ms.topic: troubleshooting
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 511d170f90e8ed34b00a3960d084223ec73d99dd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480468"
---
# <a name="troubleshoot-errors-using-azure-resource-graph"></a>Solución problemas relativos a errores con Azure Resource Graph

Puede que se produzcan errores al consultar los recursos de Azure con Azure Resource Graph. En este artículo se describen diversos errores que pueden producirse y cómo resolverlos.

## <a name="finding-error-details"></a>Búsqueda de detalles del error

La mayoría de los errores son el resultado de un problema al ejecutar una consulta con Azure Resource Graph. Cuando se produce un error en una consulta, el SDK proporciona detalles sobre la consulta errónea. Esta información indica el problema, de modo que se pueda corregir y que una consulta posterior se realice correctamente.

## <a name="general-errors"></a>Errores generales

### <a name="toomanysubscription"></a>Escenario: Demasiadas suscripciones

#### <a name="issue"></a>Problema

Los clientes con acceso a más de 1000 suscripciones, incluidas las suscripciones entre inquilinos con [Azure Lighthouse](../../../lighthouse/overview.md), no pueden capturar datos en todas las suscripciones en una única llamada a Azure Resource Graph.

#### <a name="cause"></a>Causa

La CLI de Azure y PowerShell reenvían solo las primeras 1000 suscripciones a Azure Resource Graph. La API REST para Azure Resource Graph acepta un número máximo de suscripciones en las que realizar la consulta.

#### <a name="resolution"></a>Resolución

Solicitudes por lotes de la consulta con un subconjunto de suscripciones para permanecer debajo del límite de suscripción de 1000. La solución es usar el parámetro **Subscription** en PowerShell.

```azurepowershell-interactive
# Replace this query with your own
$query = 'project type'

# Fetch the full array of subscription IDs
$subscriptions = Get-AzSubscription
$subscriptionIds = $subscriptions.Id

# Create a counter, set the batch size, and prepare a variable for the results
$counter = [PSCustomObject] @{ Value = 0 }
$batchSize = 1000
$response = @()

# Group the subscriptions into batches
$subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

# Run the query for each batch
foreach ($batch in $subscriptionsBatch){ $response += Search-AzGraph -Query $query -Subscription $batch.Group }

# View the completed results of the query on all subscriptions
$response
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
- Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.