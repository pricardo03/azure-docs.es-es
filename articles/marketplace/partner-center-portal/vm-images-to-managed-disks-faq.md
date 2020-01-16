---
title: Migración de imágenes de máquina virtual (VM) a almacenamiento de disco administrado en Azure Marketplace
description: Para proporcionar almacenamiento más rápido y confiable y compatibilidad con nuevas características y funcionalidades de Marketplace, vamos a migrar imágenes de máquina virtual de Marketplace a almacenamiento de disco administrado.
author: MaggiePucciEvans
manager: evansma
ms.author: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 5eeb223601a3990ef899d7eea96378a8b2a28eee
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75933528"
---
# <a name="were-moving-virtual-machine-vm-images-on-azure-marketplace-to-managed-disk-storage"></a>Migración de imágenes de máquina virtual (VM) a almacenamiento de disco administrado en Azure Marketplace

Para proporcionar almacenamiento más rápido y confiable y compatibilidad con nuevas características y funcionalidades de Marketplace, vamos a migrar imágenes de máquina virtual de Marketplace a almacenamiento de disco administrado.

A partir del 2 de enero de 2020, se van a migrar imágenes de máquina virtual a almacenamiento de disco administrado por fases. En la primera fase, solo se migrarán las imágenes sin nuevas implementaciones o máquinas virtuales en ejecución en los 90 días anteriores. Antes de migrar cualquiera de las imágenes, enviaremos un correo electrónico para que el anunciante sepa qué imágenes se migrarán y cuándo.

Ni los anunciantes ni los consumidores tienen que hacer nada y los usuarios no se verán afectados. Las ofertas de Marketplace seguirán estando disponibles y los clientes podrán seguir implementando máquinas virtuales administradas a partir de estas imágenes, durante y después de la migración.

Si le queda alguna duda, póngase en [contacto con nosotros](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff).

## <a name="faqs"></a>Preguntas más frecuentes

### <a name="would-the-users-of-my-vm-images-experience-an-outage"></a>¿Los usuarios de las imágenes de máquina virtual experimentarán una interrupción?

No. 

En la primera fase, solo se migrarán las imágenes de máquina virtual que no tengan máquinas virtuales en ejecución. Como no hay ningún usuario para estas imágenes, nadie resultará afectado. Tampoco las fases posteriores tendrán efecto alguno sobre los usuarios.

### <a name="how-long-does-it-take-for-the-process-to-complete"></a>¿Cuánto tiempo tardará el proceso en realizarse?

La migración puede tardar hasta 24 horas en realizarse.

### <a name="do-i-need-to-take-any-action"></a>¿Es necesario hacer algo?

No. Ni los anunciantes ni los consumidores tiene que hacer nada.

### <a name="do-i-have-to-update-my-system-to-call-the-cloud-portal-apis-in-a-different-way-after-they-are-moved-to-managed-disk-storage"></a>¿Debo actualizar el sistema para que llame a las API del portal en la nube de una manera diferente después de que se hayan migrado a almacenamiento de disco administrado?

No. Las llamadas API existentes seguirán funcionando.

### <a name="would-all-my-vm-images-be-moved-to-managed-disk-at-the-same-time"></a>¿Todas mis imágenes de máquina virtual se migrarán a un disco administrado al mismo tiempo?

Todas las imágenes de máquina virtual se migrarán en el mismo día. Recibirá una notificación una vez finalizado el proceso.

### <a name="can-i-request-to-schedule-the-move-of-my-vm-images-to-a-later-time"></a>¿Puedo solicitar programar la migración de las imágenes de máquina virtual en otro momento?

Se recomienda migrar las imágenes en la fecha programada. Sin embargo, si tiene problemas, póngase en contacto con nosotros para volver a programar la migración.

### <a name="can-i-publish-updates-to-my-vm-images-during-the-move"></a>¿Puedo publicar actualizaciones en mis imágenes de máquina virtual durante la migración?

No se pueden realizar actualizaciones en las imágenes de máquina virtual durante la migración.

## <a name="next-steps"></a>Pasos siguientes

Visite la página [Guía de publicación de ofertas de máquinas virtuales](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines).
