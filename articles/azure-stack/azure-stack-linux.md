---
title: Agregar imágenes de Linux a Azure Stack
description: Aprenda a agregar imágenes de Linux a Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 277af6f139e815f46894f5b8df82a1d92ef573d1
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537140"
---
# <a name="add-linux-images-to-azure-stack"></a>Agregar imágenes de Linux a Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede implementar máquinas virtuales Linux en Azure Stack agregando una imagen basada en Linux a Marketplace de Azure Stack. La manera más fácil de agregar una imagen de Linux a Azure Stack es con la administración de Marketplace. Estas imágenes se prepararon y probaron para que sean compatibles con Azure Stack.

## <a name="marketplace-management"></a>Administración de Marketplace

Para descargar las imágenes de Linux desde Azure Marketplace, use los procedimientos descritos en el artículo [Descarga de elementos de Marketplace desde Azure a Azure Stack](azure-stack-download-azure-marketplace-item.md). Seleccione las imágenes de Linux que quiera ofrecer a los usuarios en Azure Stack. 

Tenga en cuenta que hay actualizaciones frecuentes de estas imágenes, así que compruebe la administración de Marketplace a menudo para estar al día.

## <a name="prepare-your-own-image"></a>Preparación de su propia imagen

Siempre que sea posible, descargue las imágenes disponibles en Marketplace Management, que se han preparado y probado para Azure Stack.

Es necesario el agente Linux de Azure (suele denominarse `WALinuxAgent` o `walinuxagent`), y no todas las versiones del agente funcionarán en Azure Stack. Debe usar la versión más reciente de WALA o la versión 2.2.20 si crea su propia imagen. Tenga en cuenta que las versiones entre 2.2.20 y 2.2.35.1 (exclusiva) no funcionan en Azure Stack. Tenga en cuenta que no se admite [cloud-init](https://cloud-init.io/) en Azure Stack en este momento.

Puede preparar su propia imagen de Linux mediante las siguientes instrucciones:

* [Distribuciones basadas en CentOS](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES y openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>Incorporación de la imagen a Marketplace

Consulte [Incorporación de la imagen a Marketplace](azure-stack-add-vm-image.md). Asegúrese de que el `OSType` parámetro está establecido en `Linux`.

Después de agregar la imagen a Marketplace, se crea un elemento de Marketplace y los usuarios podrán implementar una máquina virtual Linux.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes artículos:

- [Descarga de elementos de Marketplace desde Azure a Azure Stack](azure-stack-download-azure-marketplace-item.md)
- [Información general de Azure Stack Marketplace](azure-stack-marketplace.md)
