---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/23/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ee5faedd4f59aa791424a1f178f0462922f21d28
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58114572"
---
Cada punto de conexión cuenta con un *puerto público* y un *puerto privado*:

* El puerto público que usa el equilibrador de carga de Azure para escuchar el tráfico que entra a la máquina virtual desde Internet.
* La máquina virtual usa el puerto privado para escuchar el tráfico entrante, normalmente destinado a una aplicación o servicio que se ejecuta en la máquina virtual.

Se proporcionan los valores predeterminados del protocolo IP y de los puertos TCP o UDP de los protocolos de red conocidos al crear puntos de conexión con Azure Portal. Para los puntos de conexión personalizados, especifique el protocolo IP correcto (TCP o UDP) y los puertos públicos y privados. Para distribuir el tráfico entrante de forma aleatoria entre varias máquinas virtuales, cree un conjunto con equilibrio de carga que conste de varios puntos de conexión.

Tras la creación de un extremo, puede utilizar una lista de control de acceso (ACL) para definir reglas que permitan o denieguen el tráfico entrante al puerto público del extremo, en función de su dirección IP de origen. Sin embargo, si la máquina virtual está en una red virtual de Azure, use grupos de seguridad de red en su lugar. Para obtener más información, consulte [Acerca de los grupos de seguridad de red](../articles/virtual-network/security-overview.md).

> [!NOTE]
> La configuración del firewall para máquinas virtuales de Azure se realiza automáticamente para los puertos asociados con los puntos de conexión de conectividad remotos que Azure configura automáticamente. Para los puertos especificados para todos los demás extremos, no se realiza ninguna configuración automáticamente en el firewall de la máquina virtual. Cuando cree un punto de conexión para la máquina virtual, asegúrese de que el firewall de la máquina virtual también permite el tráfico para el protocolo y el puerto privado correspondiente a la configuración del punto de conexión. Para configurar el firewall, consulte la documentación o la Ayuda en línea para el sistema operativo que se ejecuta en la máquina virtual.
>
>

## <a name="create-an-endpoint"></a>Creación de un extremo
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Haga clic en **Máquinas virtuales**y luego seleccione la máquina virtual que desea configurar.

3. Seleccione **Puntos de conexión** en el grupo **Configuración**. Aparece la página **Puntos de conexión** en donde se enumeran todos los puntos de conexión actuales de la máquina virtual. (Este ejemplo es para una máquina virtual Windows. Una VM Linux mostrará de forma predeterminada un punto de conexión para SSH).

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Extremos](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)


4. En la barra de comandos, encima de las entradas de punto de conexión, seleccione **Agregar**. Aparecerá la página **Agregar punto de conexión**.

5. En **Nombre** escriba el nombre del punto de conexión.

6. En **Protocolo**, elija **TCP** o **UDP**.

7. En **Puerto público** escriba el número de puerto para el tráfico que entra desde Internet. 

8. En **Puerto privado** escriba el número de puerto en el que escucha la máquina virtual. Los números de puerto público y privado pueden ser diferentes. Asegúrese de que el firewall de la máquina virtual se ha configurado para que permita el tráfico correspondiente al protocolo y al puerto privado.

9. Seleccione **Aceptar**.

El nuevo punto de conexión se muestra en la página **Puntos de conexión**.

![Creación correcta del extremo](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>Administración de la ACL en un extremo
Para definir el conjunto de equipos que pueden enviar tráfico, la ACL en un extremo puede restringir el tráfico en función de la dirección IP de origen. Siga estos pasos para agregar, modificar o quitar una ACL en un extremo.

> [!NOTE]
> Si el extremo forma parte de un conjunto con equilibrio de carga, los cambios que realice en la ACL en un extremo se aplican a todos los extremos del conjunto.
>
>

Si la máquina virtual está en una red virtual de Azure, use los grupos de seguridad de red en lugar de listas de control de acceso. Para obtener más información, consulte [Acerca de los grupos de seguridad de red](../articles/virtual-network/security-overview.md).

1. Inicie sesión en el Portal de Azure.

2. Seleccione **Máquinas virtuales** y luego seleccione el nombre de la máquina virtual que desea configurar.

3. Seleccione **Puntos de conexión**. Seleccione el punto de conexión apropiado en la lista de puntos de conexión. La lista de ACL está en la parte inferior de la página.

   ![Especificar los detalles de ACL](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Use las filas de la lista para agregar, eliminar o editar las reglas de una ACL y cambiar su orden. El valor **SUBRED REMOTA** es un intervalo de direcciones IP para el tráfico entrante de Internet que el equilibrador de carga de Azure usa para permitir o denegar el tráfico en función de la dirección IP de origen. Asegúrese de especificar el intervalo de direcciones IP en formato de enrutamiento de interdominios sin clases (CIDR), también conocido como formato de prefijo de dirección. Por ejemplo, `10.1.0.0/8`.

   ![Nueva entrada de ACL](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Puede usar reglas para permitir solo el tráfico desde equipos específicos correspondientes a los equipos en Internet o para denegar el tráfico desde intervalos concretos de direcciones conocidas.

Las reglas se evalúan en orden, comenzando por la primera regla y terminando por la última. Por lo tanto, las reglas deben estar ordenadas de menos restrictivas a más restrictivas. Para más información, consulte [¿Qué es una lista de control de acceso de puntos de conexión?](../articles/virtual-network/virtual-networks-acl.md)
