---
title: Aplicaciones de representación de Batch
description: Aplicaciones de representación de Batch preinstaladas
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 28acd1b7275694d38a52f14d2b2c32b79cc8183e
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "40036828"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Aplicaciones preinstaladas en imágenes de máquina virtual de representación

Todas las aplicaciones de representación se pueden usar con Azure Batch. Sin embargo, las imágenes de máquina virtual de Azure Marketplace están disponibles con aplicaciones comunes preinstaladas.

Si procede, hay licencias de pago por uso disponibles para las aplicaciones de representación preinstaladas. Cuando se crea un grupo de Batch, se pueden especificar las aplicaciones necesarias y el costo tanto de la máquina virtual como de las aplicaciones se facturará por minuto. Los precios de las aplicaciones se muestran en la [página de precios de Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Algunas aplicaciones solo admiten Windows, pero la mayoría son compatibles con Windows y con Linux.

## <a name="applications-on-centos-7-rendering-nodes"></a>Aplicaciones en nodos de representación de CentOS 7

* Autodesk Maya I/O 2017 Actualización 5 (versión 201708032230)
* Autodesk Maya I/O 2018 Actualización 2 (versión 201711281015)
* Autodesk Arnold para Maya 2017 (Arnold versión 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold para Maya 2018 (Arnold versión 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray para Maya 2017 (versión 3.60.04)
* Chaos Group V-Ray para Maya 2018 (versión 3.60.04)
* Blender (2.68)

## <a name="applications-on-windows-server-2016-rendering-nodes"></a>Aplicaciones en nodos de representación de Windows Server 2016

* Autodesk Maya I/O 2017 Actualización 5 (versión 17.4.5459)
* Autodesk Maya I/O 2018 Actualización 3 (versión 18.3.0.7040)  
* Autodesk 3ds Max I/O 2019 Actualización 1 (versión 21.10.1314)
* Autodesk 3ds Max I/O 2018 Actualización 4 (versión 20.4.0.4254)
* Autodesk Arnold para Maya (Arnold versión 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold para Maya (Arnold versión 5.0.1.4) MtoA-2.0.2.3-2018
* Autodesk Arnold para 3ds Max (Arnold versión 5.0.2.4 )(versión 1.2.926)
* Chaos Group V-Ray para Maya (versión 3.52.03)
* Chaos Group V-Ray para 3ds Max (versión 3.60.02)
* Blender (2.79)

## <a name="next-steps"></a>Pasos siguientes

Para usar las imágenes de máquina virtual de representación, deben especificarse en la configuración del grupo cuando se crea; consulte las [funcionalidades del grupo de Batch para la representación](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).