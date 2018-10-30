---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 10/19/2018
ms.author: mabrigg
ms.openlocfilehash: d23ba90f7cc0c5a03db2ef3eed4a662abe7ff27e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650560"
---
En los flujos de trabajo de validación, para **programar** una prueba se usan los parámetros comunes de nivel de flujo de trabajo que especificó durante la creación del flujo de trabajo (consulte [Parámetros comunes del flujo de trabajo en la validación de Azure Stack como servicio](../azure-stack-vaas-parameters.md)). Si alguno de los valores de parámetro de prueba deja de ser válido, debe volver a suministrarlo como se indica en [Modify workflow parameters](../azure-stack-vaas-monitor-test.md#change-workflow-parameters) (Modificación de los parámetros de flujo de trabajo).

> [!NOTE]
> La programación a través de una prueba de validación a través de una instancia existente creará una nueva instancia en el lugar de la anterior en el portal. Los registros de la instancia anterior se conservarán, pero no se podrá acceder a ellos desde el portal.  
Una vez finalizada una prueba correctamente, la acción **Schedule** (Programar) pasará a estar deshabilitada.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Seleccione **Schedule** (Programar) desde el menú contextual para abrir un símbolo del sistema y programar la instancia de prueba.

1. Revise los parámetros de prueba y, luego, seleccione **Submit** (Enviar) para programar la ejecución de la prueba.