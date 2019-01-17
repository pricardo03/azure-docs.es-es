---
title: 'Tutorial: Creación y administración de presupuestos de Azure | Microsoft Docs'
description: Este tutorial le ayuda a planear y tener en cuenta los costos de los servicios de Azure que usted consume.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 7ffceaf761d459667e4118e3e10b733898ea2710
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54053983"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Tutorial: Creación y administración de presupuestos de Azure

Los presupuestos en Cost Management le ayudan a planear y dirigir la presentación de cuentas de la organización. Con presupuestos, puede tener en cuenta los servicios de Azure que consume o a los que se suscribe durante un período específico. Le ayudan a informar a otros usuarios sobre sus gastos a fin de administrar de manera proactiva los costos y supervisar cómo avanza el gasto a lo largo del tiempo. Cuando se superan los umbrales presupuestarios que ha creado, solo se desencadenan las notificaciones. Ninguno de los recursos se ve afectado y no se detiene el consumo. Puede usar los presupuestos para comparar y realizar un seguimiento de gastos para analizar los costos.

Los presupuestos mensuales se evalúan en función del gasto cada cuatro horas. Sin embargo, las notificaciones y datos de los recursos consumidos están disponibles pasadas ocho horas.  

Los presupuestos se restablecen automáticamente al final de un período (mensual, trimestral o anualmente) para el mismo importe presupuestario al seleccionar una fecha de expiración futura. Dado que se restablecen con el mismo importe presupuestario, deberá crear presupuestos independientes cuando los importes presupuestarios en moneda difieran para períodos futuros.

Los ejemplos de este tutorial le guiarán a través de la creación y edición de un presupuesto para una suscripción de Contrato Enterprise (EA) de Azure.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un presupuesto en Azure Portal
> * Editar un presupuesto

## <a name="prerequisites"></a>Requisitos previos

Los presupuestos están disponibles para todos los clientes de EA de Azure. Debe tener acceso de lectura a los presupuestos de visualización de suscripción de EA de Azure. Para crear y administrar presupuestos, debe tener permiso de colaborador. Puede crear presupuestos individuales para las suscripciones de EA y los grupos de recursos. Sin embargo, no se pueden crear presupuestos para cuentas de facturación de EA.

Se admiten los siguientes permisos de Azure por suscripción para los presupuestos por usuario y grupo:

- Propietario: puede crear, modificar o eliminar los presupuestos para una suscripción.
- Colaborador y Colaborador de Cost Management: puede crear, modificar o eliminar sus propios presupuestos. Puede modificar el importe presupuestario para los presupuestos creados por otros usuarios.
- Lector y Lector de Cost Management: puede ver los presupuestos para los que tiene permiso.

Para más información sobre cómo asignar permisos a los datos de Cost Management, consulte [Asignación del acceso a los datos de Cost Management](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

- Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-budget-in-the-azure-portal"></a>Crear un presupuesto en Azure Portal

Puede crear un presupuesto de suscripción de Azure durante un período mensual, trimestral o anual. El contenido de navegación en Azure Portal determina si crea un presupuesto para una suscripción o para un grupo de recursos. Por ejemplo, en Azure Portal, vaya a **Suscripciones** &gt; seleccione una suscripción &gt; **Presupuestos**. En este ejemplo, el presupuesto que cree es para la suscripción seleccionada. Si desea crear un presupuesto para un grupo de recursos, vaya a **Grupos de recursos** > seleccione un grupo de recursos > **Presupuestos**.

Después de crear los presupuestos, muestran a un lado una vista sencilla de su gasto actual.

Haga clic en **Agregar**.

![Presupuestos de Cost Management que se muestran en Azure Portal](./media/tutorial-acm-create-budgets/budgets01.png)

En la ventana **Crear presupuesto**, escriba un nombre de presupuesto y el importe presupuestario. A continuación, elija un período mensual, trimestral o anual. A continuación, seleccione una fecha de finalización. Los presupuestos requieren al menos un umbral de costos (% del presupuesto) y una dirección de correo electrónico correspondiente. De manera opcional, puede incluir hasta cinco umbrales y cinco direcciones de correo electrónico en un único presupuesto. Cuando se alcanza un umbral de presupuesto, las notificaciones por correo electrónico se reciben normalmente en menos de ocho horas.

Este es un ejemplo de creación de un presupuesto mensual para 4500 USD. Se genera una alerta por correo electrónico cuando se alcanza el 90 % del presupuesto.

![Información de ejemplo que se muestra en el cuadro para crear un presupuesto](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Cuando crea un presupuesto trimestral, funciona de la misma manera que un presupuesto mensual. La diferencia es que el importe presupuestario para el trimestre se divide de manera uniforme entre los tres meses del trimestre. Como cabría esperar, un importe presupuestario anual se divide de manera uniforme entre los 12 meses del año natural.

El gasto actual respecto a presupuestos se actualiza cada vez que Cost Management recibe datos actualizados de facturación. Normalmente, esto sucede a diario.

![Información de ejemplo que muestra el gasto actual con respecto a los presupuestos](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Después de crear un presupuesto, se muestra en el análisis de costos. Ver el presupuesto en relación con la tendencia del gasto es uno de los primeros pasos cuando empieza a [analizar los costos y los gastos](quick-acm-cost-analysis.md).

![Presupuesto y gasto de ejemplo que se muestra en el análisis de costos](./media/tutorial-acm-create-budgets/cost-analysis.png)

En el ejemplo anterior, creó un presupuesto para una suscripción. Sin embargo, también puede crear un presupuesto para un grupo de recursos. Si quiere crear un presupuesto para un grupo de recursos, vaya a **Administración de costos + facturación** &gt; **Suscripciones** &gt; seleccione una suscripción > **Grupos de recursos** > seleccione un grupo de recursos > **Presupuestos** > y, luego, **Agregar** un presupuesto.

## <a name="edit-a-budget"></a>Editar un presupuesto

Según el nivel de acceso que tenga, puede editar un presupuesto para cambiar sus propiedades. En el ejemplo siguiente, algunas de las propiedades son de solo lectura porque el usuario solo tiene permiso de colaborador en la suscripción. Actualmente, la **Fecha de expiración** está deshabilitada y no puede modificarse una vez establecida.

![Ejemplo de edición de un presupuesto para cambiar varias propiedades](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear un presupuesto en Azure Portal
> * Editar un presupuesto

Pase al siguiente tutorial para crear una exportación periódica de los datos de administración de costos.

> [!div class="nextstepaction"]
> [Creación y administración de datos exportados](tutorial-export-acm-data.md)
