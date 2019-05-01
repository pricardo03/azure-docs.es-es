---
title: Visualización de las tendencias de costos mensuales estimados de laboratorio en Azure DevTest Labs | Microsoft Docs
description: Aprenda sobre el gráfico de tendencias de costos mensuales estimados de Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2019
ms.author: spelluru
ms.openlocfilehash: f761af3a5a3f08e4da89d8869aea5d666ecd69d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60868286"
---
# <a name="track-costs-associated-with-a-lab-in-azure-devtest-labs"></a>Realizar un seguimiento de los costos asociados con un laboratorio en Azure DevTest Labs
En este artículo se proporciona información sobre cómo controlar el costo del laboratorio. Muestra cómo ver el estimado costo trent durante el mes actual para el laboratorio. El artículo también muestra cómo ver el costo del mes hasta la fecha por recurso en el laboratorio.

## <a name="view-the-monthly-estimated-lab-cost-trend"></a>Ver las tendencias de costos mensuales estimados de laboratorio 
En esta sección, obtendrá información sobre cómo usar el **tendencias de costos mensuales estimados** gráfico para ver estimado costo hasta la fecha del mes de calendario actual y el costo de fin de mes previsto para el mes de calendario actual. También obtendrá información sobre cómo administrar los costos de laboratorio estableciendo objetivos y umbrales de gastos, al alcanzarlos, desencadenan que DevTest Labs para notificar los resultados para usted.

Para ver el gráfico de tendencias de costos mensuales estimados, siga estos pasos: 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el suyo.  
4. Seleccione **configuración y directivas** en el menú izquierdo.  
4. Seleccione **tendencia de costos** en el **seguimiento de costos** sección en el menú izquierdo. Captura de pantalla siguiente muestra un ejemplo de un gráfico de costos. 
   
    ![Gráfico de costos](./media/devtest-lab-configure-cost-management/graph.png)

    El valor **Costo estimado** es el costo estimado hasta la fecha del mes de calendario actual. El valor de **Projected cost** (Costo previsto) es el costo estimado para el mes de calendario actual completo, calculado con el costo de laboratorio para los cinco días anteriores.

    Los importes de los costos se redondean al siguiente número entero. Por ejemplo:  

   * 5,01 se redondea a 6. 
   * 5,50 se redondea a 6.
   * 5,99 se redondea a 6.

     Como indica anteriormente el gráfico, los costos que se ven de forma predeterminada en el gráfico son costos *estimados* con tarifas de oferta de [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/). También puede establecer sus propios objetivos de gastos que se muestran en los gráficos mediante la [administración de los objetivos de costos para un laboratorio](#managing-cost-targets-for-your-lab).

     Son los siguientes costos *no* incluido en el cálculo de costos:

   * Actualmente no se admiten suscripciones de DreamSpark y CSP, puesto que Azure DevTest Labs emplea las [API de facturación de Azure](../billing/billing-usage-rate-card-overview.md) para calcular el costo de laboratorio, y estas suscripciones no las admiten.
   * Las tarifas de su oferta. Actualmente, no puede usar las tarifas de oferta (que aparecen en su suscripción) negociadas con Microsoft o con asociados de Microsoft. Solo se usan tarifas de pago por uso.
   * Los impuestos
   * Los descuentos
   * La divisa de facturación. Actualmente, el costo de laboratorio solo se muestra en divisa USD.

### <a name="managing-cost-targets-for-your-lab"></a>Administración de objetivos de costos para un laboratorio
DevTest Labs le permite administrar mejor los costos de su laboratorio estableciendo un objetivo de gasto que puede ver en el gráfico de tendencias de costos mensuales estimados. DevTest Labs también puede enviarle una notificación cuando se alcanza el umbral o el gasto objetivo especificado. 

1. En el **tendencia de costos** página, seleccione **administrar objetivo**.

    ![Botón Administrar objetivo](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)
2. En el **administrar objetivo** , especifique un objetivo de gasto y los umbrales. También puede establecer si se informa de cada umbral seleccionado en el gráfico de tendencias de costos o mediante una notificación de webhook.

    ![Panel Administrar objetivo](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Seleccione un período de tiempo durante el que desea que se realice el seguimiento de los objetivos de costos.
      - **Mensual**: se realiza un seguimiento de los objetivos de costos mensual.
      - **Fijo**: se realiza un seguimiento de los objetivos de costos para el intervalo de fechas que especifique en las fechas de inicio y finalización. Normalmente, estos valores representan ¿durante cuánto tiempo el proyecto está programado para ejecutarse.
   - Especifique un **Costo objetivo**. Por ejemplo, ¿cuánto planea gastar en este laboratorio en el período de tiempo definido.
   - Seleccione habilitar o deshabilitar cualquier umbral del que desee que le informen, en incrementos del 25 %, hasta un 125 % del **Costo objetivo** que ha especificado.
      - **Notificar a**: Cuando se alcanza este umbral, se le notificará mediante una dirección URL de webhook que especifique.
      - **Vykreslit v grafu**: Cuando se alcanza este umbral, los resultados se trazan en el gráfico de tendencia de costos que se puede ver, como se describe en la visualización del gráfico de tendencias de costos mensuales estimados.
   - Si opta por **Notificar** cuando se alcanza el umbral, debe especificar una dirección URL de webhook. En el área de integraciones de costos, seleccione **Haga clic aquí para agregar una integración**. Escriba un **dirección URL del Webhook** en el panel Configurar notificación y seleccione **Aceptar**.

       ![Panel Configurar notificación](./media/devtest-lab-configure-cost-management/configure-notification.png)

     - Si especifica **Notificar**, debe definir una dirección URL de webhook.
     - Del mismo modo, si define una dirección URL de webhook, debe establecer **Notificación** en **Activado** en el panel Umbral de costo.
     - Tiene que crear un webhook antes de especificarlo aquí.  

       Para obtener más información sobre los webhooks, consulte [Creación de un webhook o una función de API de Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

## <a name="view-cost-by-resource"></a>Ver costo por recurso 
La característica de tendencia de costo mensual en labs permite ver cuánto ha empleado en el mes en curso. También se muestra la proyección de los gastos hasta el final del mes, en función de sus gastos en los últimos siete días. Para ayudarle a entender por qué el gasto en el laboratorio está cumpliendo los umbrales, puede usar el **coste por recurso** característica que se muestra el costo del mes hasta la fecha **por recurso** en una tabla.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el laboratorio que desee.  
4. Seleccione **configuración y directivas** en el menú izquierdo.
5. Seleccione **coste por recurso** en el **seguimiento de costos** sección en el menú izquierdo. Ver los costos asociados con cada recurso asociado a un laboratorio. 

    ![Coste por recurso](./media/devtest-lab-configure-cost-management/cost-by-resource.png)

Esta característica le ayuda a identificar fácilmente los recursos que cuestan más para que pueda tomar acciones para reducir el gasto de laboratorio. Por ejemplo, el costo de una máquina virtual se basa en el tamaño de la máquina virtual. Cuanto mayor sea el tamaño de la máquina virtual más es el costo. Puede encontrar fácilmente el tamaño de una máquina virtual y el propietario, por lo que puede hablar con el propietario de la máquina virtual para comprender por qué es necesario este tamaño de máquina virtual y si hay una posibilidad para reducir el tamaño.

[Directiva de apagado automático](devtest-lab-get-started-with-lab-policies.md#set-auto-shutdown) le ayuda a reducir el costo por apagar las máquinas virtuales de laboratorio en un momento determinado del día. Sin embargo, un usuario de laboratorio puede rechazar la directiva de apagado, lo que aumenta el costo de la máquina virtual en ejecución. Puede seleccionar una máquina virtual en la tabla para ver si se ha se ha optado por espera de la directiva de apagado automático. Si es así, puede hablar con el propietario de la máquina virtual para buscar por qué la máquina virtual se ha optado por espera de la directiva.
 
## <a name="next-steps"></a>Pasos siguientes
Vea algunas acciones que puede probar a continuación:

* [Definición de directivas de laboratorio](devtest-lab-set-lab-policy.md): obtenga información sobre cómo establecer las distintas directivas que se emplean para controlar el uso del laboratorio y sus máquinas virtuales. 
* [Creación de una imagen personalizada](devtest-lab-create-template.md): cuando cree una máquina virtual, tendrá que especificar una base, que puede ser una imagen personalizada o una imagen de Marketplace. Este artículo muestra cómo crear una imagen personalizada desde un archivo VHD.
* [Configuración de imágenes de Marketplace](devtest-lab-configure-marketplace-images.md): DevTest Labs admite la creación de máquinas virtuales basadas en imágenes de Azure Marketplace. En este artículo se muestra cómo especificar las imágenes de Azure Marketplace, si corresponde, que se pueden usar en el momento de crear máquinas virtuales en un laboratorio.
* [Creación de una máquina virtual en un laboratorio](devtest-lab-add-vm.md): se muestra cómo crear una máquina virtual desde una imagen base (ya sea personalizada o de Marketplace) y cómo trabajar con artefactos en la máquina virtual.

