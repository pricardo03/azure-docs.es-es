---
title: Solución de errores comunes
description: Aprenda a solucionar problemas de creación, asignación y eliminación de planos técnicos.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: troubleshooting
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 3621ca198ec2bac24791a2515ade51b990194bf2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58003856"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Solución de problemas de errores con instancias de Azure Blueprint

Es posible que, al crear o asignar planos técnicos, se encuentre con errores. En este artículo se describen diversos errores que pueden producirse y cómo resolverlos.

## <a name="finding-error-details"></a>Búsqueda de detalles del error

Muchos errores serán el resultado de asignar un plano técnico a un ámbito. Cuando se produce un error en una asignación, el plano técnico proporciona detalles sobre la implementación errónea. Esta información indica el problema, de modo que se pueda corregir y la siguiente implementación se realice correctamente.

1. Seleccione **Todos los servicios** en el panel izquierdo. Busque y seleccione **Planos técnicos**.

1. Seleccione **asignado planos** desde la página a la izquierda y use el cuadro de la búsqueda para filtrar la asignación del plano técnico para encontrar la asignación con error. También puede ordenar la tabla de asignaciones por la columna **Estado de aprovisionamiento** para ver todas las asignaciones con error agrupadas conjuntamente.

1. Haga clic en el proyecto con el _Failed_ estado o con el botón secundario y seleccione **ver detalles de asignación**.

1. En la parte superior de la página de asignación de planos técnicos, un banner rojo de advertencia indica que la asignación dio un error. Haga clic en cualquier lugar del banner para obtener más detalles.

Es habitual que el error se deba a un artefacto y no al plano técnico en su conjunto. Si un artefacto crea un almacén de claves y Azure Policy impide la creación, se producirá un error en toda la asignación.

## <a name="general-errors"></a>Errores generales

### <a name="policy-violation"></a>Escenario: Infracción de la directiva

#### <a name="issue"></a>Problema

Se produjo un error en la implementación de la plantilla debido a la infracción de la directiva.

#### <a name="cause"></a>Causa

Una directiva puede entrar en conflicto con la implementación por una serie de motivos:

- El recurso que se crea está restringido por la directiva (normalmente restricciones de SKU o ubicación)
- La implementación establece campos que están configurados por la directiva (normalmente con etiquetas)

#### <a name="resolution"></a>Resolución

Cambie el plano técnico para que no entre en conflicto con las directivas en los detalles del error. Si este cambio no es posible, una opción alternativa es hacer que el ámbito de la asignación de directivas cambie para que el plano técnico deje de estar en conflicto con la directiva.

### <a name="escape-function-parameter"></a>Escenario: El parámetro de plano técnico es una función

#### <a name="issue"></a>Problema

Los parámetros de plano técnico que son funciones se procesan antes de que se pasen a los artefactos.

#### <a name="cause"></a>Causa

Pasar un parámetro de plano técnico que usa una función, como `[resourceGroup().tags.myTag]`, a un artefacto genera el resultado procesado de la función que se va a establecer en el artefacto, en lugar de la función dinámica.

#### <a name="resolution"></a>Resolución

Para pasar una función como parámetro, escape la cadena completa con `[`, de modo que el parámetro de plano técnico sea similar a `[[resourceGroup().tags.myTag]`. El carácter de escape hace que los planos técnicos traten el valor como una cadena al procesar el plano técnico. El plano técnico luego coloca la función en el artefacto para que pueda ser dinámico, según lo previsto. Para obtener más información, consulte [estructura de archivos de plantilla: sintaxis](../../../azure-resource-manager/resource-group-authoring-templates.md#syntax).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure a través de [foros de Azure](https://azure.microsoft.com/support/forums/).
- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
- Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.