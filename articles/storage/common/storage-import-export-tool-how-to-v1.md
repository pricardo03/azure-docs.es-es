---
title: Uso de la herramienta de Azure Import/Export (versión 1) | Microsoft Docs
description: Descubra cómo usar la herramienta Import/Export a fin de preparar los discos duros para un trabajo de importación, así como reparar un trabajo de importación o de exportación.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 1/15/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 90b5697da0f134ccb3c9ddb66b5da138c8849189
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523417"
---
# <a name="using-the-azure-importexport-tool-classic-deployment-model"></a>Uso de la herramienta Azure Import/Export (modelo de implementación clásica)

La herramienta de Azure Import/Export (WAImportExport.exe) se utiliza para crear y administrar trabajos del servicio Azure Import/Export y le permite transferir grandes volúmenes de datos a Azure Blob Storage o desde él.

Esta documentación está destinada al modelo de implementación clásica de la herramienta Azure Import/Export. Para obtener más información sobre la versión más reciente de la herramienta, consulte [Uso de la herramienta de Azure Import/Export Tool ](../storage-import-export-tool-how-to.md).

Los artículos siguientes le indican cómo:

- Instalar y configurar la herramienta de Import/Export.
- Preparar las unidades de disco duro para un trabajo en el que se importen los datos de las unidades a Azure Blob Storage.
- Revisar el estado de un trabajo con archivos de registro de copia. 
- Reparar un trabajo de importación. 
- Reparar un trabajo de exportación. 
- Solucionar problemas de la herramienta de Azure Import/Export, en el caso de que le haya surgido algún problema durante el proceso. 

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de la herramienta WAImportExport](../storage-import-export-tool-how-to.md)