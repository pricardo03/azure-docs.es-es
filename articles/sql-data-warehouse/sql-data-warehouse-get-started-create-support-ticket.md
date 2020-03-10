---
title: Procedimiento para crear una incidencia de soporte técnico
description: Procedimiento para crear una incidencia de soporte técnico de Azure Synapse Analytics.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 9623a878ea009ec7363501c09c324baa6d0fa7ad
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195718"
---
# <a name="how-to-create-a-support-ticket-for-azure-synapse-analytics"></a>Procedimiento para crear una incidencia de soporte técnico de Azure Synapse Analytics
Si tiene algún problema con Azure Synapse Analytics, cree una incidencia de soporte técnico para que el equipo de soporte técnico de ingeniería pueda ayudarle.

## <a name="create-a-support-ticket"></a>Creación de una incidencia de soporte técnico
1. Abra [Azure Portal](https://portal.azure.com/).
1. En la pantalla de inicio, haga clic en la pestaña **Ayuda y soporte técnico**.
   
    ![Ayuda y soporte técnico](./media/sql-data-warehouse-get-started-create-support-ticket/main-page.png)

1. Revise el [Plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).
   
   * El soporte técnico para **facturación, cuota y administración de suscripciones** está disponible en todos los niveles.
   * El soporte técnico para problemas del tipo **break-fix** se proporciona en los niveles [Desarrollador](https://azure.microsoft.com/support/plans/developer/), [Estándar](https://azure.microsoft.com/support/plans/standard/), [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) o [Premier](https://azure.microsoft.com/support/plans/premier/). Los problemas "break-fix" son aquellos que experimentan los clientes mientras usan Azure en los que hay una posibilidad razonable de que hayan sido causados por Microsoft.
   * El **aprendizaje para desarrolladores** y los **servicios de asesoramiento** están disponibles en los niveles de soporte técnico [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) y [Premier](https://azure.microsoft.com/support/plans/premier/). 
     
     Si tiene un plan de soporte técnico Premier, también puede informar sobre problemas relacionados con SQL Data Warehouse en el [portal Microsoft Premier Online](https://premier.microsoft.com/). Consulte [Planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) para obtener más información sobre los diversos planes de soporte técnico, incluido su ámbito, los tiempos de respuesta, el precio, etc.  Si quiere ver las preguntas más frecuentes sobre el soporte técnico de Azure, consulte [Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/).
1. En la página **Ayuda y soporte técnico**, seleccione **Nueva solicitud de soporte técnico**. Seleccione un tipo de problema en el menú desplegable. Luego siga rellenando la información de la pestaña **Datos básicos**. Escriba un **Resumen** del problema, seleccione un **Tipo de problema** en el menú y luego seleccione Guardar.

    ![Ayuda y soporte técnico](./media/sql-data-warehouse-get-started-create-support-ticket/issue-type.png)

   > [!NOTE]
   > De forma predeterminada, cada servidor SQL (por ejemplo, myserver.database.windows.net) tiene una **cuota de DTU** de 45 000. Esta cuota es simplemente un límite de seguridad. Puede aumentar la cuota creando una incidencia de soporte técnico y seleccionando *Cuota* como el tipo de solicitud. Para calcular cuántas DTU se necesitan, multiplique 7,5 por el total de [DWU](sql-data-warehouse-overview-what-is.md) necesarias. Por ejemplo, desea hospedar dos DW6000s en un servidor SQL, así que debe solicitar una cuota de DTU de 90 000.  Puede ver el consumo de DTU actual en la hoja del servidor SQL en el portal. Tanto las bases de datos en pausa como las no pausadas cuentan en la cuota de DTU. 
   > 

1. Es posible que vea soluciones que le ayuden a resolver el problema. Si las soluciones presentadas no resuelven el problema, seleccione **Siguiente: Detalles**. Envíe los detalles del problema y la información de contacto. Seleccione **Siguiente: Revisar y crear**
![Detalles](./media/sql-data-warehouse-get-started-create-support-ticket/details.png).

    
1. Revise la información y seleccione **Crear** en la parte inferior del formulario para enviar la solicitud de soporte técnico.

## <a name="monitor-a-support-ticket"></a>Supervisión de una incidencia de soporte técnico
Una vez que ha enviado la solicitud de asistencia, el equipo de soporte técnico de Azure se pondrá en contacto con usted. Para comprobar tanto el estado de la solicitud como los detalles de la misma, haga clic en **Todas las solicitudes de soporte técnico** en el panel.

![Comprobar estado](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Otros recursos
Puede conectar con la comunidad de SQL Data Warehouse en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/) o en el [foro de MSDN de Azure SQL Data Warehouse](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/).

 
