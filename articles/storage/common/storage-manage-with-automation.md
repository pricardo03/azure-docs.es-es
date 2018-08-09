---
title: Administración de Azure Storage mediante Azure Automation
description: Obtenga información acerca de cómo puede usarse el servicio Azure Automation para administrar Azure Storage a escala.
services: storage, automation
author: jodoglevy
ms.service: storage
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.component: common
ms.openlocfilehash: 82ec929c8d3055187a83179432fc601baa191cc4
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526375"
---
# <a name="managing-azure-storage-using-azure-automation"></a>Administración de Azure Storage mediante Azure Automation
Esta guía le ofrece el servicio Azure Automation y cómo se puede usar para simplificar la administración de blobs, tablas y colas de Azure Storage.

## <a name="what-is-azure-automation"></a>¿Qué es Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) es un servicio de Azure para simplificar la administración en la nube mediante la automatización de procesos. Mediante Azure Automation, se pueden automatizar las tareas de ejecución prolongada, manuales, propensas a errores y que se repiten con frecuencia para aumentar la confiabilidad y la eficiencia y reducir el tiempo de amortización para su organización.

Azure Automation proporciona un motor de ejecución de flujo de trabajo altamente confiable y de alta disponibilidad que realiza la escalación para satisfacer sus necesidades a medida que crece la organización. En Azure Automation, los sistemas de terceros pueden interrumpir los procesos manualmente o en intervalos programados para que las tareas se realicen justo cuando sea necesario.

Reduzca la sobrecarga operativa y libere al personal de TI/DevOps para concentrarse en el trabajo que proporciona valor al negocio trasladando las tareas de administración en la nube para que se ejecuten automáticamente mediante Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-storage"></a>¿Cómo puede ayudar el servicio Azure Automation a administrar Azure Storage?
Azure Storage se puede administrar en Azure Automation mediante los cmdlets de PowerShell que están disponibles en [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation tiene estos cmdlets de PowerShell de Storage disponibles directamente para que pueda realizar todas las tareas de administración de blobs, tablas y colas dentro del servicio. También puede emparejar estos cmdlets en Azure Automation con los cmdlets para otros servicios de Azure, para automatizar tareas complejas a través de los servicios de Azure y sistemas de terceros.

La [Galería de runbooks de Azure Automation](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contiene una gran variedad de runbooks de comunidad y equipo de producto para empezar a automatizar la administración de Azure Storage, otros servicios de Azure y sistemas de terceros. Los runbooks de la Galería incluyen:

* [Quitar blobs de Azure Storage que llevan varios días mediante el servicio Automation](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [Descargar un Blob desde Azure Storage](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [Hacer una copia de seguridad de todos los discos para una sola VM de Azure o para todas las VM en un servicio en la nube](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido los aspectos básicos de Azure Automation y cómo se puede usar para administrar blobs, tablas y colas de Azure Storage, siga estos vínculos para obtener más información acerca de Azure Automation.

Consulte el tutorial de Azure Automation [Crear o importar un Runbook en Azure Automation](../../automation/automation-creating-importing-runbook.md).

