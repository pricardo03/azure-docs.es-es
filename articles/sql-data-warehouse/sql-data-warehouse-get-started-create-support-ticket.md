---
title: Procedimiento para crear una incidencia de soporte técnico
description: Creación de una incidencia de soporte técnico en Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 04fd6267782ab89e12288dadb64d28d1ef1746b5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717828"
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Creación de una incidencia de soporte técnico para SQL Data Warehouse
Si tiene problema con su instancia de SQL Data Warehouse, cree una incidencia de soporte técnico para que el equipo de ingenieros de soporte pueda ayudarle.

## <a name="create-a-support-ticket"></a>Creación de una incidencia de soporte técnico
1. Abra [Azure Portal](https://portal.azure.com/).
2. En la pantalla de inicio, haga clic en la pestaña **Ayuda y soporte técnico**.
   
    ![Ayuda y soporte técnico](./media/sql-data-warehouse-get-started-create-support-ticket/MainPage.PNG)
3. En la hoja Ayuda y soporte técnico, haga clic en **Nueva solicitud de soporte técnico** y rellene la hoja **Datos básicos**.

   Seleccione su [Plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).
   
   * El soporte técnico para **facturación, cuota y administración de suscripciones** está disponible en todos los niveles.
   * El soporte técnico para problemas del tipo **break-fix** se proporciona en los niveles [Desarrollador](https://azure.microsoft.com/support/plans/developer/), [Estándar](https://azure.microsoft.com/support/plans/standard/), [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) o [Premier](https://azure.microsoft.com/support/plans/premier/). Los problemas "break-fix" son aquellos que experimentan los clientes mientras usan Azure en los que hay una posibilidad razonable de que hayan sido causados por Microsoft.
   * El **aprendizaje para desarrolladores** y los **servicios de asesoramiento** están disponibles en los niveles de soporte técnico [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) y [Premier](https://azure.microsoft.com/support/plans/premier/). 
     
     Si tiene un plan de soporte técnico Premier, también puede informar sobre problemas relacionados con SQL Data Warehouse en el [portal Microsoft Premier Online](https://premier.microsoft.com/). Consulte [Planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) para obtener más información sobre los diversos planes de soporte técnico, incluido su ámbito, los tiempos de respuesta, el precio, etc.  Si quiere ver las preguntas más frecuentes sobre el soporte técnico de Azure, consulte [Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/).  
        
     ![Hoja Datos básicos](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_1.PNG)
     ![Hoja Datos básicos 1](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_2.PNG)
4. Rellene la hoja **Problema**.

    ![Problem_blade](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_3.PNG)
   
   > [!NOTE]
   > De forma predeterminada, cada servidor SQL (por ejemplo, myserver.database.windows.net) tiene una **cuota de DTU** de 45 000. Esta cuota es simplemente un límite de seguridad. Puede aumentar la cuota creando una incidencia de soporte técnico y seleccionando *Cuota* como el tipo de solicitud. Para calcular cuántas DTU se necesitan, multiplique 7,5 por el total de [DWU](sql-data-warehouse-overview-what-is.md) necesarias. Por ejemplo, desea hospedar dos DW6000s en un servidor SQL, así que debe solicitar una cuota de DTU de 90 000.  Puede ver el consumo de DTU actual en la hoja del servidor SQL en el portal. Tanto las bases de datos en pausa como las no pausadas cuentan en la cuota de DTU. 
   > 
   > 
   
5. Rellene la **información de contacto**.

   ![Contact_information](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_4.PNG)

    
6. Haga clic en **Crear** para enviar la solicitud de soporte técnico.

## <a name="monitor-a-support-ticket"></a>Supervisión de una incidencia de soporte técnico
Una vez que ha enviado la solicitud de asistencia, el equipo de soporte técnico de Azure se pondrá en contacto con usted. Para comprobar tanto el estado de la solicitud como los detalles de la misma, haga clic en **Todas las solicitudes de soporte técnico** en el panel.

![Comprobar estado](./media/sql-data-warehouse-get-started-create-support-ticket/Monitor_ticket.PNG)

## <a name="other-resources"></a>Otros recursos
Puede conectar con la comunidad de SQL Data Warehouse en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/) o en el [foro de MSDN de Azure SQL Data Warehouse](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/).

 
