---
title: Cree contenedores y bases de datos de Azure Cosmos con el rendimiento del modo Autopilot.
description: Obtenga información sobre las ventajas, los casos de uso y la manera de aprovisionar los contenedores y bases de datos de Azure Cosmos en el modo Autopilot.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 598dc6394e8be8b3372f4ed61a522454830a22d6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510113"
---
# <a name="create-azure-cosmos-containers-and-databases-with-provisioned-throughput-in-autopilot-mode-preview"></a>Creación de contenedores y bases de datos de Azure Cosmos con el rendimiento aprovisionado del modo Autopilot (versión preliminar)

Azure Cosmos DB le permite aprovisionar el rendimiento de los contenedores en modo manual o Autopilot. En este artículo se describen las ventajas y los casos de uso del modo Autopilot.

> [!NOTE]
> El modo Autopilot está disponible actualmente en versión preliminar pública.

Además del aprovisionamiento manual del rendimiento, ahora puede configurar contenedores de Azure Cosmos en el modo Autopilot. Los contenedores y las bases de datos de Azure Cosmos configurados en el modo Autopilot **escalarán de forma automática e instantánea el rendimiento aprovisionado en función de las necesidades de la aplicación sin poner en peligro los SLA.**

Ya no es necesario administrar manualmente el rendimiento aprovisionado ni controlar los problemas de limitación de velocidad. Los contenedores de Azure Cosmos configurados en el modo Autopilot se pueden escalar al instante en respuesta a la carga de trabajo sin que ello afecte a la disponibilidad, la latencia, el rendimiento ni el rendimiento de la carga de trabajo globalmente. Con un uso intensivo, los contenedores de Azure Cosmos configurados en el modo Autopilot se pueden escalar o reducir verticalmente sin que ello afecte a las operaciones en curso.

Al configurar contenedores y bases de datos en el modo Autopilot, debe especificar que el rendimiento máximo de `Tmax` no se supere. Los contenedores se pueden escalar inmediatamente en función de las necesidades de carga de trabajo dentro del intervalo de `0.1*Tmax < T < Tmax`. En otras palabras, los contenedores y las bases de datos se escalan de forma instantánea en función de las necesidades de carga de trabajo, desde un 10 % como mínimo del valor de rendimiento configurado hasta el valor máximo especificado configurado. Puede cambiar el valor de rendimiento máximo (Tmax) en la base de datos o el contenedor de Autopilot en cualquier momento.

## <a name="benefits-of-autopilot-mode"></a>Ventajas del modo Autopilot

Los contenedores de Azure Cosmos configurados en el modo Autopilot tienen las siguientes ventajas:

* **Simple:** los contenedores en el modo Autopilot quitan la complejidad de administrar el rendimiento aprovisionado (RU) y la capacidad manualmente para varios contenedores.

* **Escalable:** los contenedores del modo Autopilot escalan sin problemas la capacidad de rendimiento aprovisionada según sea necesario. No hay ninguna interrupción en las conexiones de cliente ni en las aplicaciones y no afectan a los contratos de nivel de usuario existentes.

* **Rentable:** al usar contenedores de Azure Cosmos configurados en el modo Autopilot, solo paga por los recursos que las cargas de trabajo necesitan por hora.

* **Alta disponibilidad:** los contenedores de Azure Cosmos en modo Autopilot usan el mismo back-end distribuido globalmente, tolerante a errores y de alta disponibilidad para garantizar la durabilidad de los datos y la alta disponibilidad en todo momento.

## <a name="use-cases-of-autopilot-mode"></a>Casos de uso del modo Autopilot

Los casos de uso de los contenedores de Azure Cosmos configurados en el modo Autopilot incluyen:

* **Cargas de trabajo de variables:** cuando se ejecuta una aplicación que se usa poco con un uso máximo de 1 a pocas horas al día, o varias veces al año. Entre los ejemplos se incluyen las aplicaciones para recursos humanos, presupuestos e informes operativos. En estos escenarios, se pueden usar los contenedores configurados en el modo Autopilot y ya no es necesario el aprovisionamiento manual para una capacidad máxima o media.

* **Cargas de trabajo imprevisibles:** cuando se ejecutan cargas de trabajo en las que se usa la base de datos a lo largo del día, pero también picos de actividad que son difíciles de predecir. Un ejemplo incluye un sitio de tráfico que experimenta un aumento de actividad cuando el pronóstico meteorológico cambia. Los contenedores configurados en el modo Autopilot ajustan la capacidad para satisfacer las necesidades de la carga máxima de la aplicación y la reducen verticalmente cuando se supera el pico de actividad.

* **Aplicaciones nuevas:** si está implementando una nueva aplicación y no está seguro del rendimiento aprovisionado (es decir, el número de RU) que necesita. Con los contenedores configurados en el modo Autopilot, puede escalar automáticamente a las necesidades de capacidad y los requisitos de la aplicación.

* **Aplicaciones usadas con poca frecuencia:** si tiene una aplicación que solo se usa durante unas horas varias veces al día, la semana o el mes, como un sitio de aplicación, web o blog de bajo volumen.

* **Bases de datos de desarrollo y pruebas:** los desarrolladores usan las cuentas de Azure Cosmos durante el horario laboral, pero no las necesitan por la noche ni los fines de semana. Con los contenedores configurados en el modo Autopilot, se escalan verticalmente al mínimo cuando no están en uso.

* **Cargas de trabajo o consultas de producción programadas:** si tiene una serie de solicitudes, operaciones o consultas programadas en un solo contenedor, y si hay períodos de inactividad en los que desea ejecutar con un rendimiento absoluto bajo, ahora puede hacerlo fácilmente. Cuando se envía una consulta o una solicitud programada a un contenedor configurado en el modo Autopilot, se escalará verticalmente de forma automática en la medida necesaria y ejecutará la operación.

Las soluciones a los problemas anteriores no solo requieren una cantidad enorme de tiempo de implementación, sino que también presentan complejidad en la configuración o el código, y suelen requerir la intervención manual para abordarlos. El modo Autopilot habilita los escenarios anteriores de forma predefinida, de modo que ya no es necesario preocuparse por estos problemas.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Comparación: contenedores configurados en modo manual frente al modo Autopilot

|  | Contenedores configurados en el modo manual  | Contenedores configurados en el modo Autopilot |
|---------|---------|---------|
| **Rendimiento aprovisionado** | Aprovisionado manualmente | Escalado de forma proactiva y reactiva según los patrones de uso de las cargas de trabajo. |
| **Limitación de velocidad de las solicitudes o las operaciones (429)**  | Puede ocurrir si el consumo supera la capacidad aprovisionada. | No se producirá.  |
| **Planificación de capacidad** |  Debe realizar un planeamiento inicial de la capacidad y aprovisionar el rendimiento necesario. |    No tiene que preocuparse por el planeamiento de la capacidad. El sistema se encarga automáticamente del planeamiento y la administración de la capacidad. |
| **Precios** | RU/s por hora aprovisionadas manualmente. | En el caso de las cuentas de una sola región de escritura, se paga por el rendimiento que se usa cada hora, con la tarifa de RU/s por hora de Autopilot. <br/><br/>En el caso de las cuentas con varias regiones de escritura, no se aplica ningún cargo adicional por Autopilot. Paga por el rendimiento que se usa por hora con la misma tarifa de RU/s por hora de la arquitectura multimaestro. |
| **Adecuado para los tipos de carga de trabajo** |  Cargas de trabajo predecibles y estables|   Cargas de trabajo impredecibles y variables  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Creación de una base de datos o un contenedor con el modo Autopilot

Puede configurar Autopilot para bases de datos o contenedores mientras los crea. Siga los pasos que se indican a continuación para una base de datos o un contenedor nuevos, habilite Autopilot y especifique el rendimiento máximo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) o en el [explorador de Azure Cosmos](https://cosmos.azure.com/).

1. Vaya a la cuenta de Azure Cosmos y abra la pestaña **Explorador de datos**.

1. Seleccione **Nueva base de datos** y escriba un nombre para la base de datos. Para la opción **Autopilot**, elija **Habilitado** y especifique el rendimiento máximo que la base de datos no puede superar al usar la opción Autopilot.

   ![Creación de una base de datos en el modo Autopilot](./media/provision-throughput-autopilot/create-database-autopilot-mode.png)

1. Seleccione **Aceptar**.

Con pasos similares, también puede crear un contenedor con rendimiento aprovisionado en el modo Autopilot.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).