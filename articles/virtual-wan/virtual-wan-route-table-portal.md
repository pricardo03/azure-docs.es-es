---
title: Crear una tabla de ruta de concentrador virtual WAN Virtual de Azure - portal de Azure | Microsoft Docs
description: Tabla de rutas de concentrador virtual WAN virtual para dirigir el tráfico a un dispositivo de red virtual mediante el portal.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a route table using the portal.
ms.openlocfilehash: 2c8b3b4671fd14f9b10b8491861ae2c652f0188b
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580589"
---
# <a name="create-a-virtual-wan-hub-route-table-for-nvas-azure-portal"></a>Crear una tabla de rutas de concentrador Virtual WAN para aplicaciones virtuales de red: Azure Portal

En este artículo se muestra cómo dirigir el tráfico de un concentrador para un dispositivo Virtual de red (NVA).

![Diagrama de Virtual WAN](./media/virtual-wan-route-table/vwanroute.png)

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que se cumplen los criterios siguientes:

*  Tiene un dispositivo Virtual de red (NVA). Una aplicación Virtual de red es un software de terceros de su elección que normalmente se aprovisiona en Azure Marketplace en una red virtual.

    * Una dirección IP privada debe asignarse a la interfaz de red NVA.

    * El dispositivo virtual de red no está implementado en el concentrador virtual. Se debe implementar en una red virtual independiente.

    *  La red virtual de NVA pueden tener una o varias redes virtuales conectados a él. En este artículo, nos referimos a la red virtual de NVA como un 'indirecta VNet de spoke'. Estas redes virtuales pueden estar conectadas a la red virtual de NVA mediante emparejamiento de VNet.
*  Ha creado redes virtuales de 2. Se usará como redes virtuales de radios.

    * Para este ejercicio, los espacios de direcciones de red virtual spoke son: VNet1: 10.0.2.0/24 y VNet2: 10.0.3.0/24. Si necesita más información sobre cómo crear una red virtual, consulte [crear una red virtual](../virtual-network/quick-create-portal.md).

    * Asegúrese de que no hay ningún puertas de enlace de red virtual en cualquiera de las redes virtuales.
    * Para esta configuración, estas redes virtuales no requieren una subred de puerta de enlace.

## <a name="signin"></a>1. Iniciar sesión

Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

## <a name="vwan"></a>2. Creación de una instancia de Virtual WAN

Cree una WAN virtual. Para los fines de este ejercicio, puede usar los siguientes valores:

* **Nombre de la WAN virtual:** myVirtualWAN
* **Grupo de recursos:** testRG
* **Ubicación:** Oeste de EE. UU.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Crear un concentrador

Crear el centro. Para los fines de este ejercicio, puede usar los siguientes valores:

* **Ubicación:** Oeste de EE. UU.
* **Nombre:** westushub
* **Espacio de direcciones privadas de Hub:** 10.0.1.0/24

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="route"></a>4. Crear y aplicar una tabla de rutas de concentrador

Actualizar el centro con una tabla de rutas de concentrador. Para los fines de este ejercicio, puede usar los siguientes valores:

* **Espacios de direcciones de red virtual de radio indirectos:** 10.0.3.0/24 y 10.0.2.0/24 (VNet1 y VNet2)
* **Red Perimetral NVA interfaz privada dirección IP de red:** 10.0.4.5

1. Vaya a la red WAN virtual.
2. Haga clic en el concentrador para el que desea crear una tabla de rutas.
3. Haga clic en el **...** y, a continuación, haga clic en **concentrador virtual edición**.
4. En el **concentrador virtual edición** página, desplácese hacia abajo y seleccione la casilla de verificación **tabla de uso para el enrutamiento**.
5. En el **si el prefijo de destino es** columna, agregar los espacios de direcciones. En el **enviar al próximo salto** columna, agregar la dirección IP privada de interfaz con red NVA de la red Perimetral.
6. Haga clic en **confirmar** para actualizar el recurso del centro con la configuración de la tabla de enrutamiento.

## <a name="connections"></a>5. Creación de las conexiones de red virtual

Crear una conexión desde cada radio indirecta de red virtual (VNet1 y VNet2) al concentrador. A continuación, cree una conexión de la red virtual de NVA al concentrador.

 Este paso, puede usar los siguientes valores:

| Nombre de red virtual| Nombre de conexión|
| --- | --- |
| VNet1 | testconnection1 |
| VNet2 | testconnection2 |
| NVAVNet | testconnection3 |

Repita el procedimiento siguiente para cada red virtual que desea conectarse.

1. En la página de la red WAN virtual, haga clic en **Conexiones de red virtual**.
2. En la página de conexión de red virtual, haga clic en **+ Agregar conexión**.
3. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.
4. Haga clic en **Aceptar** para crear la conexión.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).