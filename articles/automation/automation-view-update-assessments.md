---
title: Visualización de las evaluaciones de actualizaciones de Azure Update Management
description: En este artículo se describe cómo ver las evaluaciones de actualizaciones para las implementaciones de actualizaciones.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e88622ede6437086b86a33081d6ec9b9ea50ef65
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377441"
---
# <a name="view-azure-update-management-update-assessments"></a>Visualización de las evaluaciones de actualizaciones de Azure Update Management

En la cuenta de Automation, seleccione **Update Management** para ver el estado de sus máquinas.

Esta vista proporciona información sobre las máquinas, las actualizaciones que faltan, las implementaciones de actualizaciones y las implementaciones de actualizaciones programadas. En la columna **Cumplimiento**, puede ver la última vez que se evaluó la máquina. En la columna **Preparación de actualizaciones del agente**, puede ver el estado del agente de actualización. Si hay un problema, seleccione el vínculo para dirigirse a la documentación de solución de problemas, que le ayudará a conocer los pasos que debe realizar para corregirlo.

Para ejecutar una búsqueda de registros que devuelva información sobre la máquina, las actualizaciones o las implementaciones, seleccione el elemento de la lista. Se abre el panel **Búsqueda de registros** con una consulta para el elemento seleccionado:

![Vista predeterminada de Update Management](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Visualización de actualizaciones que faltan

Haga clic en **Actualizaciones que faltan** para ver la lista de actualizaciones que faltan en las máquinas. Se muestran todas las actualizaciones, y se pueden seleccionar. Aparece información sobre el número de máquinas que requieren la actualización, el sistema operativo y un vínculo para más información. El panel **Búsqueda de registros** muestra más información sobre las actualizaciones.

![Actualizaciones que faltan](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="update-classifications"></a>Clasificaciones de actualizaciones

En las siguientes tablas se muestran las clasificaciones de actualizaciones en Update Management, junto con una definición de cada clasificación.

### <a name="windows"></a>Windows

|clasificación  |DESCRIPCIÓN  |
|---------|---------|
|Actualizaciones críticas     | Actualización para un problema específico que resuelve un error crítico no relacionado con la seguridad.        |
|Actualizaciones de seguridad     | Actualización para un problema específico del producto relacionado con la seguridad.        |
|Paquetes acumulativos de actualizaciones     | Conjunto acumulativo de revisiones que se empaquetan para facilitar la implementación.        |
|Feature Packs     | Nuevas características del producto que se distribuyen fuera de una versión del producto.        |
|Service Packs     | Un conjunto acumulativo de revisiones que se aplican a una aplicación.        |
|Actualizaciones de definiciones     | Una actualización de archivos de definiciones de virus o de otra índole.        |
|Herramientas     | Utilidad o característica que ayuda a realizar una o más tareas.        |
|Actualizaciones     | Actualización de una aplicación o archivo que están instalados actualmente.        |

### <a name="linux-2"></a>Linux

|clasificación  |DESCRIPCIÓN  |
|---------|---------|
|Actualizaciones críticas y de seguridad     | Actualizaciones para un problema específico o un problema de un producto específico relacionado con la seguridad.         |
|Otras actualizaciones     | Todas las demás actualizaciones que ni son críticas por naturaleza ni son actualizaciones de seguridad.        |

En Linux, Update Management puede distinguir entre actualizaciones críticas y de seguridad en la nube y mostrar al mismo tiempo datos de evaluación debido al enriquecimiento de datos en la nube. Para aplicar revisiones, Update Management se basa en los datos de clasificación disponibles en la máquina. A diferencia de otras distribuciones, CentOS no dispone de esta información de forma nativa. Si tiene máquinas de CentOS configuradas para devolver datos de seguridad para el siguiente comando, Update Management podrá aplicar revisiones basadas en clasificaciones.

```bash
sudo yum -q --security check-update
```

Actualmente no hay ningún método compatible para habilitar la disponibilidad de datos de clasificación nativos en CentOS. En este momento, solo se proporciona soporte técnico a clientes que pueden haber habilitado esto por su cuenta.

## <a name="next-steps"></a>Pasos siguientes

Después de ver las evaluaciones de actualizaciones, puede programar una implementación de actualizaciones siguiendo los pasos descritos en [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).