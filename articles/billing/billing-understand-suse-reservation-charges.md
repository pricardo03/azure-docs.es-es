---
title: Comprender el descuento del plan de reserva de SUSE y uso - Azure | Microsoft Docs
description: Obtenga información sobre cómo se aplican los descuentos de plan SUSE al software SUSE en máquinas virtuales.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/28/2018
ms.author: banders
ms.openlocfilehash: 4305db991a8129b0ae4205300051391df893c52c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58917794"
---
# <a name="understand-how-the-suse-linux-enterprise-software-reservation-plan-discount-is-applied-for-azure"></a>Información sobre cómo se aplica el descuento de plan de reserva de software de SUSE Linux Enterprise para Azure

Después de comprar un plan SUSE Linux, el descuento se aplica automáticamente a las máquinas virtuales (VM) SUSE implementadas que coinciden con la reserva. Un plan SUSE Linux cubre el costo de ejecutar el software SUSE en una VM de Azure.

Para comprar el plan SUSE Linux adecuado, debe saber qué VM SUSE ejecuta y el número de vCPU de dichas VM. Use las secciones siguientes para ayudar a identificar, a partir del archivo CSV de uso, el plan que debe comprar.

## <a name="discount-applies-to-different-vm-sizes"></a>Descuento se aplica a los diferentes tamaños de VM

Del mismo modo que con las instancias de VM reservadas, las compras de planes SUSE ofrecen flexibilidad de tamaño de instancia. Esto significa que el descuento se aplica incluso cuando se implementa una VM con un recuento de vCPU diferente. El descuento se aplica a los diferentes tamaños de VM del plan de software.

El importe de descuento depende de la relación que aparece en las tablas siguientes. La relación compara la superficie relativa de cada medidor del grupo. La relación depende de las vCPU de VM. Use el valor de relación para calcular cuántas instancias de VM obtienen el descuento del plan SUSE Linux.

Por ejemplo, si compra un plan para SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria para una VM con 3 o 4 vCPU, la relación de esa reserva es 2. El descuento cubre el costo de software SUSE para:

- 2 VM implementadas con 1 o 2 vCPU,
- 1 VM implementada con 3 o 4 vCPU,
- o 0,77 o, aproximadamente, 77 % de una VM con 5 o más vCPU.

La proporción de 5 o más vCPU es 2,6. Por lo tanto, una reserva para SUSE con una máquina virtual con 5 o más vCPU cubre una única parte del costo de software, que es aproximadamente 77%.

## <a name="understand-suse-vm-usage-before-you-buy"></a>Entender el uso de SUSE VM antes de comprar

Las tablas siguientes muestran los planes de software para los que puede comprar una reserva, los medidores de uso asociados y las relaciones para cada uno.

### <a name="suse-linux-enterprise-server-for-hpc-priority"></a>SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria

Nombre de marketplace de Azure Portal:

- SLES 12 SP3 para HPC (Prioritaria)

|VM SUSE | Id. del medidor| Relación| Tamaño de VM de ejemplo|
| -------| ------------------------| --- |--- |
|SLES para 1-2 vCPU HPC|e275a668-ce79-44e2-a659-f43443265e98|1|D2s_v3|
|SLES para 3-4 vCPU HPC|e531e1c0-09c9-4d83-b7d0-a2c6741faa22|2|D4s_v3|
|SLES para más de 5 vCPU HPC|4edcd5a5-8510-49a8-a9fc-c9721f501913|2.6|D8s_v3|

### <a name="suse-linux-enterprise-server-for-hpc-standard"></a>SUSE Linux Enterprise Server para informática de alto rendimiento Estándar

Nombre de marketplace de Azure Portal:

- SLES 12 SP3 para HPC

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- | --- | ------------------------| --- |
|SLES para 1-2 vCPU HPC |8c94ad45-b93b-4772-aab1-ff92fcec6610|1|D2s_v3|
|SLES para 3-4 vCPU HPC|4ed70d2d-e2bb-4dcd-b6fa-42da71861a1c|1,92308|D4s_v3|
|SLES para más de 5 vCPU HPC |907a85de-024f-4dd6-969c-347d47a1bdff|2;92308|D8s_v3|

### <a name="suse-linux-enterprise-server-for-sap-priority"></a>SUSE Linux Enterprise Server for SAP Prioritaria

Nombres de marketplace de Azure Portal:

- SLES for SAP 15 (Prioritaria)
- SLES for SAP 12 SP3 (Prioritaria)
- SLES for SAP 12 SP2 (Prioritaria)

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- |------------------------| --- | --- |
|SLES for SAP Prioritaria (1-2 vCPU)|497fe0b6-fa3c-4e3d-a66b-836097244142|1|D2s_v3|
|SLES for SAP Prioritaria (3-4 vCPU) |847887de-68ce-4adc-8a33-7a3f4133312f|2|D4s_v3|
|SLES for SAP Prioritaria (más de 5 vCPU) |18ae79cd-dfce-48c9-897b-ebd3053c6058|2,41176|D8s_v3|

### <a name="suse-linux-enterprise-server-priority"></a>SUSE Linux Enterprise Server Prioritaria

Nombres de marketplace de Azure Portal:

- SLES 15 (Prioritaria)
- SLES 12 SP3 (Prioritaria)
- SLES 11 SP4 (Prioritaria)

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- |------------------------| --- |--- |
|SLES 1 vCPU|462cd632-ec6b-4663-b79f-39715f4e8b38|1|B1ms|
|SLES 2-4 vCPU |924bee71-5eb8-424f-83ed-a58823c33908|2|D4s_v3|
|SLES 2-4 vCPU |60b3ae9d-e77a-46b2-9cdf-92fa87407969|2|D4s_v3|
|SLES 6 vCPU |e8862232-6131-4dbe-bde4-e2ae383afc6f|3||
|SLES 8 vCPU |e11331a8-fd32-4e71-b60e-4de2a818c67a|3.2|D8s_v3|
|SLES 12 núcleos/vCPU |a5afd00d-d3ef-4bcd-8b42-f158b2799782|3.2||
|SLES 16 vCPU |bb21066f-fe46-46d3-8006-b326b1663e52|3.2| D16s_v3|
|SLES 20 vCPU |c5228804-1de6-4bd4-a61c-501d9003acc8|3.2| |
|SLES 24 núcleos/vCPU |-005d-4075-ac11-822ccde9e8f6|3.2| ND24s|
|SLES 32 vCPU |180c1a0a-b0a5-4de3-a032-f92925a4bf90|3.2| D32s_v3|
|SLES 40 núcleos/vCPU |a161d3d3-0592-4956-9b64-6829678b6506|3.2||
|SLES 64 vCPU |7f5a36ed-d5b5-4732-b6bb-837dbf0fb9d8|3.2| D64s_v3|
|SLES 72 núcleos/vCPU |93329a72-24d7-4faa-93d9-203f367ed334|3.2|F72s_v2|
|SLES 96 núcleos/vCPU |2018c3a8-ff13-41f8-b64d-9558c5206547|3.2||
|SLES 128 núcleos/vCPU |ac27e4d7-44b5-4fee-bc1a-78ac5b4abaf7|3.2| M128ms|

### <a name="suse-linux-enterprise-server-standard"></a>SUSE Linux Enterprise Server Estándar

Nombres de marketplace de Azure Portal:

- SLES 15
- SLES 15 (Estándar)
- SLES 12 SP3 (Estándar)

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- |------------------------| --- |--- |
|SLES 1-2 núcleos/vCPU |4b2fecfc-b110-4312-8f9d-807db1cb79ae|1|D2s_v3|
|SLES 3-4 núcleos/vCPU |0c3ebb4c-db7d-4125-b45a-0534764d4bda|1,92308|D4s_v3|
|SLES de más de 5 vCPU |7b349b65-d906-42e5-833f-b2af38513468|2,30769| D8s_v3|

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reservas, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado para planes de software SUSE con Azure Reservations](../virtual-machines/linux/prepay-suse-software-charges.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
