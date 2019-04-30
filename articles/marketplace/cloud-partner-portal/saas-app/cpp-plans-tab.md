---
title: Planes de oferta de aplicación SaaS de Azure | Microsoft Docs
description: Cree un plan para una oferta de aplicación SaaS en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: 0394fa759f4b6f6af59e075d9bc67668886d8075
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62120212"
---
# <a name="saas-application-plans-tab"></a>Pestaña Planes de aplicación SaaS

Use la pestaña Planes para crear un nuevo plan. Debe agregarse al menos un plan si usa la opción de venta a través de Microsoft para la aplicación SaaS.

![Creación de un nuevo plan](./media/saas-plans-new-plan.png)

## <a name="create-a-new-plan"></a>Creación de un nuevo plan

Para crear un nuevo plan:

1. En **Planes**, seleccione **+ Nuevo plan**.
2. En la ventana emergente **Nuevo plan**, escriba un **Id. de plan**. La longitud máxima es de 50 caracteres. Este id. debe constar únicamente de caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. No se puede cambiar este identificador una vez publicada la oferta.
3. Haga clic en **Aceptar** para guardar el id. de plan.

   ![Id. de plan para el nuevo plan](./media/saas-plans-plan-ID.png)

### <a name="to-configure-the-plan"></a>Para configurar el plan:

1. En **Detalles del plan**, proporcione información para los campos siguientes:

   - Título: Dé un título al plan. El título está limitado a 50 caracteres.
   - Descripción: Escriba una descripción. La descripción está limitada a un máximo de 500 caracteres.
   - ¿Es este un plan privado? - Si el plan solo está disponible para un grupo selecto de clientes, seleccione **Sí**.
   - Disponibilidad de país/región: El plan debe estar disponible para al menos un país o región. Haga clic en **Seleccionar regiones**. Elija un país o región desde la lista **Select Country/Region availability** (Seleccionar disponibilidad de país/región) y, luego, seleccione **Aceptar**. 
   - Legacy Pricing (Precios heredados): Proporcione el costo, en USD al mes.

2. En **Simplified Currency Pricing** (Precios en divisas simplificados), proporcione la siguiente información:

   - Billing Term (Término de facturación): Precio mensual está seleccionado de forma predeterminada. También puede proporcionar precios anuales.
   - Precio mensual: Proporcione el precio mensual, que debe coincidir con el precio heredado.

   >[!NOTE] 
   >Si agrega Precio anual al término de facturación, se le pedirá para el **Precio anual** en USD por año.

3. Seleccione **Guardar** para terminar de configurar el plan.

   >[!NOTE] 
   >Después de guardar los cambios de precios, puede exportar/importar los datos de precios.

![Configuración de los detalles del plan](./media/saas-plans-plan-details.png)

## <a name="next-steps"></a>Pasos siguientes

[Pestaña Información del canal](./cpp-channel-info-tab.md)

