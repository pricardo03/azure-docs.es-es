---
title: 'Aplicaciones de representación: Azure Batch'
description: Aplicaciones de representación de Batch preinstaladas
services: batch
ms.service: batch
author: laurenhughes
ms.author: lahugh
ms.date: 03/26/2018
ms.topic: conceptual
ms.openlocfilehash: 84b2ca30f1ccd49e18e2f9d42133f8672d3f8ad6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776176"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Aplicaciones preinstaladas en imágenes de máquina virtual de representación

Todas las aplicaciones de representación se pueden usar con Azure Batch. Sin embargo, las imágenes de máquina virtual de Azure Marketplace están disponibles con aplicaciones comunes preinstaladas.

Si procede, hay licencias de pago por uso disponibles para las aplicaciones de representación preinstaladas. Cuando se crea un grupo de Batch, se pueden especificar las aplicaciones necesarias y el costo tanto de la máquina virtual como de las aplicaciones se facturará por minuto. Los precios de las aplicaciones se muestran en la [página de precios de Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Algunas aplicaciones solo admiten Windows, pero la mayoría son compatibles con Windows y con Linux.

## <a name="applications-on-centos-7-rendering-images"></a>Aplicaciones en representar imágenes de CentOS 7

* Autodesk Maya I/O 2017 Actualización 5 (versión 201708032230)
* Autodesk Maya I/O 2018 Actualización 2 (versión 201711281015)
* Autodesk Arnold para Maya 2017 (Arnold versión 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold para Maya 2018 (Arnold versión 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray para Maya 2017 (versión 3.60.04)
* Chaos Group V-Ray para Maya 2018 (versión 3.60.04)
* Blender (2.68)

## <a name="applications-on-latest-windows-server-2016-rendering-images"></a>Aplicaciones en representar imágenes de Windows Server 2016 más recientes

En la lista siguiente se aplica a Windows Server 2016, representar imágenes de versión 1.3.4.

* Autodesk Maya I/O 2017 Actualización 5 (versión 17.4.5459)
* Autodesk Maya I/O 2018 Actualización 4 (versión 18.4.0.7622)
* Autodesk 3ds Max I/O 2019 Actualización 1 (versión 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Actualización 4 (versión 20.4.0.4254)
* Autodesk Arnold para Maya 2017 (Arnold versión 5.2.0.1) MtoA-3.1.0.1-2017
* Autodesk Arnold para Maya 2018 (Arnold versión 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold para 3ds Max (Arnold versión 5.0.2.4 )(versión 1.2.926)
* Caos grupo V-Ray para Maya 2018 (versión 3.52.03)
* Caos grupo V-Ray para 3ds Max 2018 (versión 3.60.02)
* Caos grupo V-Ray para Maya 2019 (versión 3.52.03)
* Caos grupo V-Ray para 3ds Max 2019 (versión 4.10.01)
* Blender (2.79)

> [!NOTE]
> Caos grupo V-Ray para 3ds Max 2019 (versión 4.10.01) presenta cambios importantes a V-ray. Para usar la versión anterior (versión 3.60.02), use Windows Server 2016, los nodos de representación de la versión 1.3.2.

## <a name="applications-on-previous-windows-server-2016-rendering-images"></a>Aplicaciones en las imágenes anteriores de representación de Windows Server 2016

En la lista siguiente se aplica a Windows Server 2016, las imágenes de representación de la versión 1.3.2.

* Autodesk Maya I/O 2017 Actualización 5 (versión 17.4.5459)
* Autodesk Maya I/O 2018 Actualización 4 (versión 18.4.0.7622)  
* Autodesk 3ds Max I/O 2019 Actualización 1 (versión 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Actualización 4 (versión 20.4.0.4254)
* Autodesk Arnold para Maya 2017 (Arnold versión 5.2.0.1) MtoA-3.1.0.1-2017
* Autodesk Arnold para Maya 2018 (Arnold versión 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold para 3ds Max (Arnold versión 5.0.2.4 )(versión 1.2.926)
* Caos grupo V-Ray para Maya 2019 (versión 3.52.03)
* Caos grupo V-Ray para 3ds Max 2018 (versión 3.60.02)
* Blender (2.79)

## <a name="next-steps"></a>Pasos siguientes

Para usar las imágenes de máquina virtual de representación, deben especificarse en la configuración del grupo cuando se crea; consulte las [funcionalidades del grupo de Batch para la representación](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).