---
title: Imágenes de Red Hat Enterprise Linux en Azure | Microsoft Docs
description: Obtenga información sobre las imágenes de Red Hat Enterprise Linux en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: 61991e271a68c9160a34d0de99fe4c9259ca41cb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476942"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Imágenes de Red Hat Enterprise Linux (RHEL) disponibles en Azure
Azure ofrece una variedad de imágenes de RHEL para diferentes casos de uso.

## <a name="list-of-rhel-images"></a>Lista de imágenes de RHEL
Esta es una lista de imágenes de RHEL disponibles en Azure. A menos que se indique lo contrario, todas las imágenes tienen particiones LVM y se acoplarán en los repositorios de RHEL convencionales (no EUS ni E4S). Las imágenes siguientes están disponibles actualmente para uso general:

Oferta| SKU | Creación de particiones | Aprovisionamiento | Notas
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | RAW    | Agente Linux |
|             | 6,8      | RAW    | Agente Linux |
|             | 6.9      | RAW    | Agente Linux |
|             | 6.10     | RAW    | Agente Linux |
|             | 7-RAW    | RAW    | Agente Linux | Familia de imágenes de RHEL 7.x. <br> Se acopla a repositorios convencionales de forma predeterminada (no EUS).
|             | 7-LVM    | LVM    | Agente Linux | Familia de imágenes de RHEL 7.x. <br> Se acopla a repositorios convencionales de forma predeterminada (no EUS).
|             | 7-RAW-CI | RAW-CI | Cloud-init  | Familia de imágenes de RHEL 7.x. <br> Se acopla a repositorios convencionales de forma predeterminada (no EUS).
|             | 7.2      | RAW    | Agente Linux |
|             | 7.3      | RAW    | Agente Linux |
|             | 7.4      | RAW    | Agente Linux | Adjuntada a repositorios de EUS de forma predeterminada a partir de abril de 2019.
|             | 7.5      | RAW    | Agente Linux | Adjuntada a repositorios de EUS de forma predeterminada a partir de junio de 2019.
|             | 7.6      | RAW    | Agente Linux | Adjuntada a repositorios de EUS de forma predeterminada a partir de mayo de 2019.
|             | 7,7      | LVM    | Agente Linux | Adjuntada a repositorios de EUS de forma predeterminada.
|             | 8        | LVM    | Agente Linux | Familia de imágenes de RHEL 8.x
|             | 8-gen2   | LVM    | Agente Linux | Generación 2 de Hyper-V: familia de imágenes de RHEL 8.x.
RHEL-SAP      | 7.4      | LVM    | Agente Linux | RHEL 7.4 for SAP HANA y Business Apps. Se acopla a los repositorios de E4S, cobrará un recargo para SAP y RHEL, así como la tarifa de proceso básica.
|             | 7.5      | LVM    | Agente Linux | RHEL 7.5 for SAP HANA y Business Apps. Se acopla a los repositorios de E4S, cobrará un recargo para SAP y RHEL, así como la tarifa de proceso básica.
|             | 7.6      | LVM    | Agente Linux | RHEL 7.5 for SAP HANA y Business Apps. Se acopla a los repositorios de E4S, cobrará un recargo para SAP y RHEL, así como la tarifa de proceso básica.
|             | 7,7      | LVM    | Agente Linux | RHEL 7.5 for SAP HANA y Business Apps. Se acopla a los repositorios de E4S, cobrará un recargo para SAP y RHEL, así como la tarifa de proceso básica.
RHEL-SAP-HANA | 6.7      | RAW    | Agente Linux | RHEL 6.7 for SAP HANA. Obsoleto en favor de las imágenes de RHEL-SAP.
|             | 7.2      | LVM    | Agente Linux | RHEL 7.2 for SAP HANA. Obsoleto en favor de las imágenes de RHEL-SAP.
|             | 7.3      | LVM    | Agente Linux | RHEL 7.3 for SAP HANA. Obsoleto en favor de las imágenes de RHEL-SAP.
RHEL-SAP-APPS | 6,8      | RAW    | Agente Linux | RHEL 6.8 for SAP Business Applications. Obsoleto en favor de las imágenes de RHEL-SAP.
|             | 7.3      | LVM    | Agente Linux | RHEL 7.3 for SAP Business Applications. Obsoleto en favor de las imágenes de RHEL-SAP.
RHEL-HA       | 7.4      | LVM    | Agente Linux | RHEL 7.4 con el complemento de alta disponibilidad. Cobrará un recargo por alta disponibilidad y RHEL además de la tarifa de proceso básica.
|             | 7.5      | LVM    | Agente Linux | RHEL 7.5 con el complemento de alta disponibilidad. Cobrará un recargo por alta disponibilidad y RHEL además de la tarifa de proceso básica.
|             | 7.6      | LVM    | Agente Linux | RHEL 7.6 con el complemento de alta disponibilidad. Cobrará un recargo por alta disponibilidad y RHEL además de la tarifa de proceso básica.
RHEL-SAP-HA   | 7.4      | LVM    | Agente Linux | RHEL 7.4 for SAP con el complemento de alta disponibilidad. Se acopla a los repositorios de E4S. Cobrará un recargo para los repositorios de SAP y alta disponibilidad así como RHEL, además de las tarifas de proceso básicas.
|             | 7.5      | LVM    | Agente Linux | RHEL 7.5 for SAP con el complemento de alta disponibilidad. Se acopla a los repositorios de E4S. Cobrará un recargo para los repositorios de SAP y alta disponibilidad así como RHEL, además de las tarifas de proceso básicas.
|             | 7.6      | LVM    | Agente Linux | RHEL 7.6 for SAP con el complemento de alta disponibilidad. Se acopla a los repositorios de E4S. Cobrará un recargo para los repositorios de SAP y alta disponibilidad así como RHEL, además de las tarifas de proceso básicas.
rhel-byos     |rhel-lvm74| LVM    | Agente Linux | Las imágenes BYOS de RHEL 7.4, no acopladas a ningún origen de actualizaciones, no cobrarán un recargo de RHEL.
|             |rhel-lvm75| LVM    | Agente Linux | Las imágenes BYOS de RHEL 7.5, no acopladas a ningún origen de actualizaciones, no cobrarán un recargo de RHEL.
|             |rhel-lvm76| LVM    | Agente Linux | Las imágenes BYOS de RHEL 7.6, no acopladas a ningún origen de actualizaciones, no cobrarán un recargo de RHEL.
|             |rhel-lvm77| LVM    | Agente Linux | Las imágenes BYOS de RHEL 7.7, no acopladas a ningún origen de actualizaciones, no cobrarán un recargo de RHEL.
|             |rhel-lvm8 | LVM    | Agente Linux | Las imágenes BYOS de RHEL 8 (la versión secundaria de RHEL se muestra en el valor de la versión de la imagen), no acopladas a ningún origen de actualizaciones, no cobrarán un recargo de RHEL.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre las [imágenes de Red Hat en Azure](./redhat-images.md).
* Obtenga más información sobre la [infraestructura de actualización de Red Hat](./redhat-rhui.md).
* Obtenga más información sobre la [oferta BYOS de RHEL](./redhat-byos.md).
* Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux).