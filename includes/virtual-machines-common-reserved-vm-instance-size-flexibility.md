---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: 05820cc5f7b7d61d83f73ea5b62b05f8712e0997
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60771213"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances

Con una instancia reservada de máquina virtual optimizada para conseguir flexibilidad en el tamaño de la instancia, la reserva que adquiera se puede aplicar a los tamaños de las máquinas virtuales del mismo grupo de serie de tamaño. Por ejemplo, si compra una reserva para un tamaño de máquina virtual de la tabla de la serie DSv2 como, por ejemplo, Standard_DS5_v2, el descuento por la reserva se puede aplicar a los otros cuatro tamaños que aparecen en la misma tabla:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Pero ese descuento de reserva no se aplica a los tamaños de máquinas virtuales que aparecen en tablas diferentes, como los que hay en la tabla de memoria alta de la serie DSv2: Standard_DS11_v2, Standard_DS12_v2, etc.

Dentro del grupo de serie de tamaño, el número de máquinas virtuales al que se aplica el descuento por la reserva depende del tamaño de máquina virtual que elija al comprar una reserva. También depende de los tamaños de las máquinas virtuales que tenga en ejecución. La columna de relación que aparece en las tablas siguientes compara la superficie relativa para cada tamaño de máquina virtual de ese grupo. Use el valor de relación para calcular cómo se aplica el descuento por la reserva a las máquinas virtuales que tiene en ejecución.

## <a name="examples"></a>Ejemplos

Los ejemplos siguientes usan los tamaños y relaciones en la tabla de la serie DSv2.

 Va a comprar una instancia reservada de máquina virtual con el tamaño Standard_DS4_v2 en la que la relación o superficie relativa comparada con los otros tamaños de esa serie es 8.

- Escenario 1: Ejecución de ocho máquinas virtuales de tamaño Standard_DS1_v2 con una relación de 1. El descuento por la reserva se aplica a las ocho máquinas virtuales.
- Escenario 2: Ejecución de máquinas virtuales de tamaño Standard_DS2_v2 con una relación de 2 cada una. También se ejecuta una máquina virtual con tamaño Standard_DS3_v2 con una relación de 4. La superficie total es 2+2+4=8. Por tanto, el descuento por la reserva se aplica a las tres máquinas virtuales.
- Escenario 3: Ejecución de una máquina virtual de tamaño Standard_DS5_v2 con una relación de 16. El descuento por la reserva se aplicaría a la mitad del costo de proceso de esa máquina virtual.

En las siguientes secciones se muestra qué tamaños están en el mismo grupo de serie de tamaño cuando compra una instancia reservada de máquina virtual optimizada con flexibilidad de tamaño.

## <a name="b-series"></a>Serie B

| Tamaño | Relación|
|---|---|
| Standard_B1s | 1 |
|Standard_B2s|4|

Para más información, consulte [Tamaños de las máquinas virtuales ampliables serie B](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="b-series-high-memory"></a>Memoria alta de la serie B

| Tamaño | Relación|
|---|---|
| Standard_B1ms |1|
|Standard_B2ms|4|
|Standard_B4ms|8|
|Standard_B8ms|16|

Para más información, consulte [Tamaños de las máquinas virtuales ampliables serie B](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="d-series"></a>Serie D

| Tamaño | Relación|
|---|---|
| Standard_D1|1|
|Standard_D2|2|
|Standard_D3|4|
|Standard_D4|8|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="d-series-high-memory"></a>Memoria alta de la serie D

| Tamaño | Relación|
|---|---|
| Standard_D11|1|
|Standard_D12|2|
|Standard_D13|4|
|Standard_D14|8|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series"></a>Serie Ds

| Tamaño | Relación|
|---|---|
|Standard_DS1|1|
|Standard_DS2|2|
|Standard_DS3|4|
|Standard_DS4|8|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series-high-memory"></a>Memoria alta de la serie Ds

| Tamaño | Relación|
|---|---|
|Standard_DS11|1|
|Standard_DS12|2|
|Standard_DS13|4|
|Standard_DS14|8|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series"></a>DSv2-series

| Tamaño | Relación|
|---|---|
|Standard_DS1_v2|1|
|Standard_DS2_v2|2|
|Standard_DS3_v2|4|
|Standard_DS4_v2|8|
|Standard_DS5_v2|16|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series-high-memory"></a>Memoria alta de la serie DSv2

| Tamaño | Relación|
|---|---|
|Standard_DS11_v2|1|
|Standard_DS11-1_v2|1|
|Standard_DS12_v2|2|
|Standard_DS12-1_v2|2|
|Standard_DS12-2_v2|2|
|Standard_DS13_v2|4|
|Standard_DS13-2_v2|4|
|Standard_DS13-4_v2|4|
|Standard_DS14_v2|8|
|Standard_DS14-4_v2|8|
|Standard_DS14-8_v2|8|
|Standard_DS15_v2|10|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv3-series"></a>Serie DSv3

| Tamaño | Relación|
|---|---|
|Standard_D2s_v3|1|
|Standard_D4s_v3|2|
|Standard_D8s_v3|4|
|Standard_D16s_v3|8|
|Standard_D32s_v3|16|
|Standard_D64s_v3|32|

Para más información, consulte [Tamaños de máquina virtual de uso general](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-1).

## <a name="dv2-series"></a>Serie Dv2

| Tamaño | Relación|
|---|---|
|Standard_D1_v2|1|
|Standard_D2_v2|2|
|Standard_D3_v2|4|
|Standard_D4_v2|8|
|Standard_D5_v2|16|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv2-series-high-memory"></a>Memoria alta de la serie Dv2

| Tamaño | Relación|
|---|---|
|Standard_D11_v2|1|
|Standard_D12_v2|2|
|Standard_D13_v2|4|
|Standard_D14_v2|8|
|Standard_D15_v2|10|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv3-series"></a>Serie Dv3

| Tamaño | Relación|
|---|---|
| Standard_D2_v3|1|
|Standard_D4_v3|2|
|Standard_D8_v3|4|
|Standard_D16_v3|8|
|Standard_D32_v3|16|
|Standard_D64_v3|32|

Para más información, consulte [Tamaños de máquina virtual de uso general](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1).

## <a name="esv3-series"></a>Serie ESv3

| Tamaño | Relación|
|---|---|
|Standard_E2s_v3|1|
|Standard_E4s_v3|2|
|Standard_E4-2s_v3|2|
|Standard_E8s_v3|4|
|Standard_E8-2s_v3|4|
|Standard_E8-4s_v3|4|
|Standard_E16s_v3|8|
|Standard_E16-4s_v3|8|
|Standard_E16-8s_v3|8|
|Standard_E20s_v3|10|
|Standard_E32s_v3|16|
|Standard_E32-8s_v3|16|
|Standard_E32-16s_v3|16|
|Standard_E64s_v3|28.8|
|Standard_E64-16s_v3|28.8|
|Standard_E64-32s_v3|28.8|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#esv3-series).

## <a name="ev3-series"></a>Serie Ev3

| Tamaño | Relación|
|---|---|
| Standard_E2_v3|1|
|Standard_E4_v3|2|
|Standard_E8_v3|4|
|Standard_E16_v3|8|
|Standard_E20_v3|10|
|Standard_E32_v3|16|
|Standard_E64_v3|32|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#ev3-series).

## <a name="f-series"></a>Serie F

| Tamaño | Relación|
|---|---|
| Standard_F1|1|
|Standard_F2|2|
|Standard_F4|4|
|Standard_F8|8|
Standard_F16|16|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fs-series"></a>Serie Fs

| Tamaño | Relación|
|---|---|
| Standard_F1s|1|
|Standard_F2s|2|
|Standard_F4s|4|
|Standard_F8s|8|
|Standard_F16s|16|

Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fsv2-series"></a>Serie Fsv2

| Tamaño | Relación|
|---|---|
|Standard_F2s_v2|1|
|Standard_F4s_v2|2|
|Standard_F8s_v2|4|
|Standard_F16s_v2|8|
|Standard_F32s_v2|16|
|Standard_F64s_v2|32|
|Standard_F72s_v2|36|

Para más información, consulte [Tamaños de máquina virtual optimizada para proceso](../articles/virtual-machines/windows/sizes-compute.md#fsv2-series-1).

## <a name="h-series"></a>Serie H

| Tamaño | Relación|
|---|---|
| Standard_H8|1|
|Standard_H16|2|

Para más información, consulte [Tamaños de máquina virtual de procesos de alto rendimiento](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="h-series-high-memory"></a>Memoria alta de la serie H

| Tamaño | Relación|
|---|---|
| Standard_H8m|1|
|Standard_H16m|2|

Para más información, consulte [Tamaños de máquina virtual de procesos de alto rendimiento](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="ls-series"></a>Serie Ls

| Tamaño | Relación|
|---|---|
| Standard_L4s|1|
|Standard_L8s|2|
|Standard_L16s|4|
|Standard_L32s|8|

Para más información, consulte [Tamaños de máquina virtual optimizada para almacenamiento](../articles/virtual-machines/windows/sizes-storage.md).

## <a name="m-series"></a>Serie M

| Tamaño | Relación|
|---|---|
| Standard_M64s|1|
|Standard_M128s|2|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional"></a>Serie M fraccional

| Tamaño | Relación|
|---|---|
| Standard_M16s|1|
|Standard_M32s|2|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-high-memory"></a>Memoria alta de la serie M fraccional

| Tamaño | Relación|
|---|---|
|Standard_M8ms|1|
|Standard_M8-2ms|1|
|Standard_M8-4ms|1|
|Standard_M16ms|2|
|Standard_M16-4ms|2|
|Standard_M16-8ms|2|
|Standard_M32ms|4|
|Standard_M32-8ms|4|
|Standard_M32-16ms|4|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-large"></a>Serie M fraccional grande

| Tamaño | Relación|
|---|---|
| Standard_M32ls|1|
|Standard_M64ls|2|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-high-memory"></a>Memoria alta de la serie M

| Tamaño | Relación|
|---|---|
| Standard_M64ms|1|
|Standard_M64-16ms|1|
|Standard_M64-32ms|1|
|Standard_M128ms|2|
|Standard_M128-32ms|2|
|Standard_M128-64ms|2|

Para más información, consulte [Tamaños de máquina virtual optimizada para memoria](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="nc-series"></a>Serie NC

| Tamaño | Relación|
|---|---|
| Standard_NC6|1|
|Standard_NC12|2|
|Standard_NC24|4|

Para más información, consulte [Tamaños de máquinas virtuales optimizadas para GPU](../articles/virtual-machines/windows/sizes-gpu.md).

## <a name="ncv2-series"></a>Serie NCv2

| Tamaño | Relación|
|---|---|
| Standard_NC6s_v2|1|
|Standard_NC12s_v2|2|
|Standard_NC24s_v2|4|

Para más información, consulte [Tamaños de máquinas virtuales optimizadas para GPU](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series).

## <a name="ncv3-series"></a>Serie NCv3

| Tamaño | Relación|
|---|---|
| Standard_NC6s_v3|1|
|Standard_NC12s_v3|2|
|Standard_NC24s_v3|4|

Para más información, consulte [Tamaños de máquinas virtuales optimizadas para GPU](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series).

## <a name="nd-series"></a>Serie ND

| Tamaño | Relación|
|---|---|
| Standard_ND6s|1|
|Standard_ND12s|2|
|Standard_ND24s|4|

Para más información, consulte [Tamaños de máquinas virtuales optimizadas para GPU](../articles/virtual-machines/windows//sizes-gpu.md#nd-series).

## <a name="nv-series"></a>Serie NV

| Tamaño | Relación|
|---|---|
| Standard_NV6|1|
|Standard_NV12|2|
|Standard_NV24|4|

Para más información, consulte [Tamaños de máquinas virtuales optimizadas para GPU](../articles/virtual-machines/windows//sizes-gpu.md#nv-series).


