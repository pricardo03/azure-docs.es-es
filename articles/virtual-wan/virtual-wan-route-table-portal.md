---
title: 'Virtual WAN: Creación de una tabla de rutas de concentrador virtual en NVA: Portal de Azure'
description: Tabla de rutas de concentrador virtual de Virtual WAN para dirigir el tráfico a un dispositivo virtual de red con el portal.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a route table using the portal.
ms.openlocfilehash: 8f24b94226daffb769993c9f6659909fdff039b6
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74014984"
---
# <a name="create-a-virtual-wan-hub-route-table-for-nvas-azure-portal"></a>Cree una tabla de rutas de concentrador de Virtual WAN para dispositivos virtuales de red: Portal de Azure

En este artículo se muestra cómo dirigir el tráfico de un concentrador a un dispositivo virtual de red (NVA).

![Diagrama de Virtual WAN](./media/virtual-wan-route-table/vwanroute.png)

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que se cumplen los criterios siguientes:

*  Tiene un dispositivo virtual de red (NVA). Un dispositivo virtual de red es un software de terceros de su elección que normalmente se aprovisiona en Azure Marketplace en una red virtual.

    * Debe asignarse una dirección IP privada a la interfaz de red NVA.

    * El NVA no se implementa en el concentrador virtual. Se debe implementar en una red virtual independiente.

    *  La red virtual NVA puede tener una o varias redes virtuales conectadas a ella. En este artículo, nos referimos a la red virtual de NVA como "red virtual de radio indirecta". Estas redes virtuales pueden conectarse a la red virtual de NVA mediante emparejamiento de redes virtuales.
*  Ha creado dos redes virtuales. Se usarán como redes virtuales de radio.

    * En este ejercicio, los espacios de direcciones de red virtual de radio son: VNet1: 10.0.2.0/24 y VNet2: 10.0.3.0/24. Si necesita más información sobre cómo crear una red virtual, consulte [Creación de una red virtual](../virtual-network/quick-create-portal.md).

    * Asegúrese de que no haya ninguna puerta de enlace de red virtual en ninguna de las redes virtuales.
    * Para esta configuración, estas redes virtuales no requieren una subred de puerta de enlace.

## <a name="signin"></a>1. Iniciar sesión

Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

## <a name="vwan"></a>2. Creación de una instancia de Virtual WAN

Cree una WAN virtual. A efectos de este ejercicio, puede utilizar los valores siguientes:

* **Nombre de Virtual WAN:** myVirtualWAN
* **Grupo de recursos:** TestRG
* **Ubicación:** Oeste de EE. UU.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Crear un concentrador

Cree el concentrador. A efectos de este ejercicio, puede utilizar los valores siguientes:

* **Ubicación:** Oeste de EE. UU.
* **Nombre:** westushub
* **Espacio de direcciones privadas del concentrador:** 10.0.1.0/24

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="route"></a>4. Creación y aplicación de una tabla de rutas de concentrador

Actualice el concentrador con una tabla de rutas de concentrador. A efectos de este ejercicio, puede utilizar los valores siguientes:

* **Espacios de direcciones de red virtual de radio indirecta:** (VNet1 y VNet2) 10.0.2.0/24 y 10.0.3.0/24
* **Dirección IP privada de interfaz con red NVA de la red perimetral:** 10.0.4.5

1. Navegue a la instancia de Virtual WAN.
2. Haga clic en el concentrador para el que desea crear una tabla de rutas.
3. Haga clic en los puntos suspensivos **...** y luego, en **Editar concentrador virtual**.
4. En la página **Editar concentrador virtual**, desplácese hacia abajo y marque la casilla **Usar tabla para el enrutamiento**.
5. En la columna **Si el prefijo de destino es**, agregue los espacios de direcciones. En la columna **Enviar al próximo salto**, agregue la dirección IP privada de interfaz con red NVA de la red perimetral.
6. Haga clic en **Confirmar** para actualizar el recurso del centro de conectividad con la configuración de la tabla de rutas.

## <a name="connections"></a>5. Creación de las conexiones de red virtual

Cree una conexión desde cada red virtual de radio indirecta (VNet1 y VNet2) al concentrador. Después, cree una conexión de la red virtual de NVA al concentrador.

 En este paso se pueden usar los siguientes valores:

| Nombre de red virtual| Nombre de conexión|
| --- | --- |
| VNet1 | testconnection1 |
| VNet2 | testconnection2 |
| NVAVNet | testconnection3 |

Repita los pasos siguientes para cada red virtual que quiera conectar.

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