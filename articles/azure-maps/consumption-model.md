---
title: Modelo de consumo en Azure Maps | Microsoft Docs
description: Información acerca del modelo de consumo de Azure Maps.
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: cf20c7dbfbf7cd3f09579b03b835148c1c295137
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600636"
---
# <a name="consumption-model"></a>Modelo de consumo

El enrutamiento en línea proporciona un conjunto de parámetros para una descripción detallada del modelo de consumo específico del vehículo.
Según el valor de **vehicleEngineType**, se admiten dos modelos principales de consumo: _Combustion_ y _Electric_. La especificación de parámetros que pertenecen a diferentes modelos en la misma solicitud es un error.
El modelo de consumo no se puede usar con los valores de **travelMode**_bicycle_ y _pedestrian_.

## <a name="parameter-constraints-for-consumption-model"></a>Restricciones de parámetros en el modelo de consumo

En ambos modelos de consumo, para especificar explícitamente algunos parámetros es necesario especificar también otros. Estas dependencias son:

* Todos los parámetros requieren que el usuario especifique **constantSpeedConsumption**. Es un error especificar cualquier otro parámetro de modelo de consumo, a excepción de **vehicleWeight**, si no se ha especificado **constantSpeedConsumption**\*.
* **accelerationEfficiency** y **decelerationEfficiency** se deben especificar siempre como un par (es decir, ambos o ninguno).
* Si se especifican **accelerationEfficiency** y **decelerationEfficiency**, el producto de los valores no debe ser mayor que 1 (para evitar el movimiento perpetuo).
* **uphillEfficiency** y **downhillEfficiency** se deben especificar siempre como un par (es decir, ambos o ninguno).
* Si se especifican **uphillEfficiency** y **downhillEfficiency**, el producto de los valores no debe ser mayor que 1 (para evitar el movimiento perpetuo).
* Si el usuario especifica los parámetros de \***Efficiency**, se debe especificar entonces **vehicleWeight**. Cuando **vehicleEngineType** es _combustion_, se debe especificar también **fuelEnergyDensityInMJoulesPerLiter**.
* **maxChargeInkWh** y **currentChargeInkWh** siempre deben especificarse como un par (es decir, ambos o ninguno).

> [!NOTE]
> Si solo se especifica **constantSpeedConsumption**, no se tiene en cuenta ningún otro aspecto de consumo, como pendientes o aceleración del vehículo, en los cálculos de consumo.

## <a name="combustion-consumption-model"></a>Modelo de consumo de combustión

El modelo de consumo de combustión se usa cuando **vehicleEngineType** se establece en _combustion_.
A continuación se muestra la lista de parámetros que pertenecen a este modelo. Consulte la sección Parámetros para ver una descripción más detallada.

* constantSpeedConsumptionInLitersPerHundredkm
* vehicleWeight
* currentFuelInLiters
* auxiliaryPowerInLitersPerHour
* fuelEnergyDensityInMJoulesPerLiter
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Modelo de consumo eléctrico

El modelo de consumo eléctrico se usa cuando **vehicleEngineType** se establece en _electric_.
A continuación se muestra la lista de parámetros que pertenecen a este modelo. Consulte la sección Parámetros para ver una descripción más detallada.

* constantSpeedConsumptionInkWhPerHundredkm
* vehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Valores razonables de parámetros de consumo

Se puede rechazar un determinado conjunto de parámetros de consumo, incluso aunque puedan satisfacer todos los requisitos explícitos especificados anteriormente. Esto sucede cuando el valor de un parámetro concreto, o una combinación de valores de varios parámetros, se considera que conduce a magnitudes irrazonables de valores de consumo. En este caso, probablemente sea indicio de un error en la entrada, dado que se tiene el debido cuidado para admitir todos los valores razonables de los parámetros de consumo. Si un determinado conjunto de parámetros de consumo se rechace, el mensaje de error acompañante contendrá una explicación textual de los motivos.
En las descripciones detalladas de los parámetros se incluyen de ejemplos de valores razonables para ambos modelos.
