---
title: Solución de problemas de errores con instancias de Azure Blueprint
description: Aprenda a solucionar problemas de creación y asignación de planos técnicos.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b910f90e70af4ce6d4243c06bfe5bd03d25d74d6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092941"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Solución de problemas de errores con instancias de Azure Blueprint

Es posible que, al crear o asignar planos técnicos, se encuentre con errores. En este artículo se describen diversos errores que pueden producirse y cómo resolverlos.

## <a name="finding-error-details"></a>Búsqueda de detalles del error

Muchos errores serán el resultado de asignar un plano técnico a un ámbito. Cuando se produce un error en una asignación, el plano técnico proporciona detalles sobre la implementación errónea. Esta información indica el problema, de modo que se pueda corregir y la siguiente implementación se realice correctamente.

1. Haga clic en **Todos los servicios** y busque y seleccione **Directiva** en el panel izquierdo. En la página **Directiva**, haga clic en **Planos técnicos**.

1. En la página de la izquierda seleccione **Planos técnicos asignados** y use el cuadro de búsqueda para filtrar las asignaciones de plano técnico a fin de encontrar las asignaciones con error. También puede ordenar la tabla de asignaciones por la columna **Estado de aprovisionamiento** para ver todas las asignaciones con error agrupadas conjuntamente.

1. Haga clic en el plano técnico con el estado _Error_ o haga clic con el botón derecho y seleccione **Ver los detalles de la asignación**.

1. En la parte superior de la página de asignación de planos técnicos, un banner rojo de advertencia indica que la asignación dio un error. Haga clic en cualquier lugar del banner para obtener más detalles.

Es habitual que el error se deba a un artefacto y no al plano técnico en su conjunto. Si un artefacto crea un almacén de claves y Azure Policy impide la creación, se producirá un error en toda la asignación.

## <a name="general-errors"></a>Errores generales

### <a name="policy-violation"></a>Escenario: infracción de directivas

#### <a name="issue"></a>Problema

Se produjo un error en la implementación de la plantilla debido a la infracción de la directiva.

#### <a name="cause"></a>Causa

Una directiva puede entrar en conflicto con la implementación por una serie de motivos:

- El recurso que se crea está restringido por la directiva (normalmente restricciones de SKU o ubicación)
- La implementación establece campos que están configurados por la directiva (normalmente con etiquetas)

#### <a name="resolution"></a>Resolución

Cambie el plano técnico para que no entre en conflicto con las directivas en los detalles del error. Si este cambio no es posible, una opción alternativa es hacer que el ámbito de la asignación de directivas cambie para que el plano técnico deje de estar en conflicto con la directiva.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
- Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.