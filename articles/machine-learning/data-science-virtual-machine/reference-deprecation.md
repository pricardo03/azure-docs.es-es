---
title: 'Referencia: Desuso de las imágenes de DSVM'
description: Detalles sobre el desuso que afecta a Azure Data Science Virtual Machine (DSVM)
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 10/14/2019
ms.topic: reference
ms.openlocfilehash: 252f9c66034dbadaf7a2e9e6f78665c26d414deb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456286"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referencia: Desuso de las imágenes de DSVM

A continuación se describen sugerencias para tratar con las versiones que próximamente caerán en desuso de Azure Data Science Virtual Machine.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Migración de discos de datos

Dejaremos de admitir la imagen de DSVM Windows 2012 el 31 de diciembre de 2019. Para migrar a un disco de datos de la versión de DSVM Windows 2012 existente a Windows 2016, realice los pasos siguientes:

1. Cree una nueva instancia de DSVM Windows 2016 siguiendo las instrucciones que se muestran [aquí](./provision-vm.md#create-your-dsvm).
1. Desasocie los discos de datos existentes de la imagen de Windows 2012 según [estas instrucciones](../../virtual-machines/windows/detach-disk.md).
1. Conecte el disco del paso anterior a la imagen de Windows 2016 según [estas instrucciones](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm).
