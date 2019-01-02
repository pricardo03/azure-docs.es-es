---
title: 'Tutorial: Reducir los costos de Azure con las recomendaciones de optimización | Microsoft Docs'
description: Este tutorial le ayudará a reducir los costos de Azure cuando siga las recomendaciones de optimización.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 2b9702dbae0414ba597b6e1f6080d9de86f624fc
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077089"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Tutorial: Optimización de los costos a partir de las recomendaciones

Azure Cost Management funciona con Azure Advisor para proporcionar recomendaciones de optimización de costos. Azure Advisor le ayuda a optimizar y mejorar la eficiencia mediante la identificación de recursos inactivos e infrautilizados. Este tutorial le guiará a través de un ejemplo donde identificará los recursos de Azure infrautilizados y, a continuación, adoptará medidas para reducir los costos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Ver las recomendaciones de optimización de costos para conocer las posibles deficiencias de uso
> * Actuar en función de una recomendación para cambiar el tamaño de una máquina virtual a una opción más rentable
> * Comprobar la acción para asegurarse de que la máquina virtual cambió el tamaño correctamente

## <a name="prerequisites"></a>Requisitos previos
Las recomendaciones están disponibles para todos los clientes con [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Debe tener al menos acceso de lectura a uno o varios de los siguientes ámbitos para ver datos de costos.

- Subscription
- Grupos de recursos

Debe tener máquinas virtuales activas con al menos 14 días de actividad.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

## <a name="view-cost-optimization-recommendations"></a>Visualización de las recomendaciones de optimización de costos

En Azure Portal, haga clic en **Administración de costos + facturación** en la lista de servicios. A continuación, en la lista bajo **Cost Management**, seleccione **Recomendaciones de Advisor**. Se muestran las recomendaciones de Advisor.

![Recomendaciones de Advisor para Cost Management en Azure Portal](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

La lista de recomendaciones identifica las deficiencias de uso o muestra las recomendaciones de compra que pueden ayudarle a ahorrar más dinero. El **Ahorro potencial anual** totalizado muestra el importe total que se puede ahorrar si apaga o desasigna todas las VM que cumplan las reglas de recomendación. Si no quiere apagarlas, considere la posibilidad de cambiar su tamaño a una SKU de VM menos costosa.

La categoría **Impacto**, junto con el **Ahorro potencial anual**, está diseñada para ayudar a identificar las recomendaciones con el máximo potencial de ahorro. Las recomendaciones de alto impacto son [Compra de instancias reservadas de máquina virtual para ahorrar dinero en los costos de pago por uso](../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) y [Optimización del gasto en máquinas virtuales mediante la adecuación del tamaño o el apagado en instancias infrautilizadas](../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances). Las recomendaciones de impacto moderado son [Reducción de los costos mediante la eliminación de circuitos ExpressRoute no aprovisionados](../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits) y [Reducción de costos mediante la eliminación o reconfiguración de puertas de enlace de red virtual inactivas](../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways).

## <a name="act-on-a-recommendation"></a>Actuar en función de una recomendación

Azure Advisor supervisa el uso de las máquinas virtuales durante 14 días e identifica aquellas infrautilizadas. Se considera que una máquina virtual tiene una utilización escasa si su uso de la CPU es del 5 % o menos y el de la red es de 7 MB o menos durante cuatro o más días.

La configuración de uso de la CPU del 5 % o menos es el valor predeterminado, pero se puede ajustar. Para obtener más información sobre cómo ajustar la configuración, consulte el artículo [Recomendación de la configuración de la regla de uso promedio de CPU ](../advisor/advisor-get-started.md#configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation)[para una máquina virtual con poco uso](../advisor/advisor-get-started.md#configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation).

Si bien determinados escenarios pueden dar lugar a un uso escaso debido al diseño, a menudo puede ahorrar dinero al cambiar el tamaño de las máquinas virtuales a otros menos costosos. Los ahorros reales pueden variar si elige una acción de cambio de tamaño. Veamos un ejemplo de cambio de tamaño de una máquina virtual.

En la lista de recomendaciones, haga clic en la recomendación **Apague las máquinas virtuales infrautilizadas o cambie su tamaño**. En la lista de candidatos de máquina virtual, elija una máquina virtual para cambiar su tamaño y, luego, haga clic en la máquina virtual. Se muestran los detalles de la máquina virtual para que pueda comprobar las métricas de uso. El valor **Ahorro potencial anual** es lo que puede ahorrar si apaga o quita la VM. Con el cambio de tamaño de una VM, probablemente, ahorre dinero, pero no ahorrará el importe total del ahorro potencial anual.

![Ejemplo de los detalles de recomendación](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

En los detalles de la VM, compruebe el uso de la máquina virtual para confirmar que es un candidato adecuado para cambiar el tamaño.

![Ejemplo de detalles de la máquina virtual que muestran el uso histórico](./media/tutorial-acm-opt-recommendations/vm-details.png)

Anote el tamaño actual de la máquina virtual. Después de comprobar que debe cambiarse el tamaño de la máquina virtual, cierre los detalles de la VM para ver la lista de máquinas virtuales.

En la lista de candidatos para apagar o cambiar el tamaño, seleccione **Cambiar el tamaño de la máquina virtual**.
![Recomendaciones de ejemplo con la opción de cambiar el tamaño de la máquina virtual](./media/tutorial-acm-opt-recommendations/resize-vm.png)

A continuación, se le presentará una lista de opciones de cambio de tamaño disponibles. Elija la que ofrezca el mejor rendimiento y rentabilidad para su escenario. En el ejemplo siguiente, la opción elegida cambia el tamaño de una **DS14\_V2** a una **DS13\_V2**. Seguir la recomendación ahorra 551,30 USD/mes o 6615,60 USD/año.

![Lista de ejemplo de los tamaños de máquina virtual disponibles donde puede elegir un tamaño](./media/tutorial-acm-opt-recommendations/choose-size.png)

Después de elegir un tamaño adecuado, haga clic en **Seleccionar** para iniciar la acción de cambio de tamaño.

El cambio de tamaño requiere que se reinicie la máquina virtual en ejecución activa. Si la máquina virtual está en un entorno de producción, se recomienda ejecutar la operación de cambio de tamaño después del horario comercial. Programar el reinicio puede reducir las interrupciones causadas por la falta de disponibilidad momentánea.

## <a name="verify-the-action"></a>Comprobación de la acción

Cuando el cambio de tamaño de la VM finalice correctamente, se mostrará una notificación de Azure.

![Notificación de que el tamaño de la máquina virtual se cambió correctamente](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Ver las recomendaciones de optimización de costos para conocer las posibles deficiencias de uso
> * Actuar en función de una recomendación para cambiar el tamaño de una máquina virtual a una opción más rentable
> * Comprobar la acción para asegurarse de que la máquina virtual cambió el tamaño correctamente

Si aún no ha leído el artículo de prácticas recomendadas de Cost Management, allí encontrará orientación de alto nivel y principios a tener en cuenta para ayudar a administrar los costos.

> [!div class="nextstepaction"]
> [Prácticas recomendadas de Cost Management](cost-mgt-best-practices.md)
