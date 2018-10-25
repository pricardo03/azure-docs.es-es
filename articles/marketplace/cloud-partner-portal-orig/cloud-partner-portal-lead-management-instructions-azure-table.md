---
title: Azure Table | Microsoft Docs
description: Configure la administración de clientes potenciales para Azure Table.
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
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 60e3e3d81b07bf7ae681b5cef2d6d9681877a35f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809028"
---
<a name="lead-management-instructions-for-azure-table"></a>Instrucciones de administración de clientes potenciales con Azure Table
============================================

En este artículo se describe cómo configurar Azure Table para almacenar clientes potenciales de ventas. Azure Table le permite almacenar y personalizar la información del cliente.

## <a name="to-configure-azure-table"></a>Configuración de Azure Table

1.  Si no tiene una cuenta de Azure, puede [crear una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

2.  Cuando la cuenta de Azure esté activa, inicie sesión en [Azure Portal](https://portal.azure.com).
3.  En Azure Portal, cree una cuenta de almacenamiento. La captura de pantalla siguiente muestra cómo crear una cuenta de almacenamiento. Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

    ![Pasos para crear una cuenta de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Copie la cadena de conexión de la cuenta de almacenamiento para la clave y péguela en el campo **Cadena de conexión de cuenta de almacenamiento** en Cloud Partner Portal. Este es un ejemplo de una cadena de conexión: `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net `
    
    ![Claves de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Puede usar el [Explorador de Azure Storage](http://azurestorageexplorer.codeplex.com/) o cualquier otra herramienta para ver los datos en la tabla de almacenamiento. También puede exportar los datos en Azure Table.
datos.

## <a name="optional-to-use-azure-functions-with-an-azure-table"></a>**(Opcional)**  Para usar Azure Functions con una tabla de Azure

Si desea personalizar cómo se están recibiendo los clientes potenciales, use [Azure Functions](https://azure.microsoft.com/services/functions/) con una tabla de Azure. El servicio Azure Functions le permite automatizar el proceso de generación de clientes potenciales.

Los pasos siguientes le muestran cómo crear una función de Azure que usa un temporizador. Cada cinco minutos, la función busca en la tabla de Azure nuevos registros y, a continuación, usa el servicio SendGrid para enviar una notificación por correo electrónico.


1.  [Cree](https://portal.azure.com/#create/SendGrid.SendGrid) una cuenta de servicio de SendGrid gratuita en la suscripción de Azure.

    ![Creación de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/createsendgrid.png)

2.  Creación de una clave de API de SendGrid 
    - Seleccione **Administrar** para ir a la interfaz de usuario de SendGrid
    - Seleccione **Configuración**, **Claves de API** y, a continuación, cree una clave que tenga Enviar correo -\> Acceso completo
    - Guarde la clave de API.


    ![Clave de API de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridkey.png)


3.  [Cree](https://portal.azure.com/#create/Microsoft.FunctionApp) una aplicación de Azure Functions mediante la utilización de la opción de Plan de hospedaje denominada "Plan de consumo".

    ![Creación de Azure Function App](./media/cloud-partner-portal-lead-management-instructions-azure-table/createfunction.png)


4.  Cree una nueva definición de función.

    ![Creación de definición de función de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/createdefinition.png)
 

5.  Si desea que la función envíe una actualización en un momento dado, seleccione **TimerTrigger-CSharp** como la opción de inicio.

     ![Opción de desencadenador de hora de función de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/timetrigger.png)


6.  Sustituya el código "Develop", por el siguiente ejemplo de código. Edite las direcciones de correo electrónico con las direcciones que desee usar para el remitente y el receptor.

        #r "Microsoft.WindowsAzure.Storage"
        #r "SendGrid"
        using Microsoft.WindowsAzure.Storage.Table;
        using System;
        using SendGrid;
        using SendGrid.Helpers.Mail;
        public class MyRow : TableEntity
        {
            public string Name { get; set; }
        }
        public static void Run(TimerInfo myTimer, IQueryable<MyRow> inputTable, out Mail message, TraceWriter log)
        {
            // UTC datetime that is 5.5 minutes ago while the cron timer schedule is every 5 minutes
            DateTime dateFrom = DateTime.UtcNow.AddSeconds(-(5 * 60 + 30));
            var emailFrom = "YOUR EMAIL";
            var emailTo = "YOUR EMAIL";
            var emailSubject = "Azure Table Notification";
            // Look in the table for rows that were added recently
            var rowsList = inputTable.Where(r => r.Timestamp > dateFrom).ToList();
            // Check how many rows were added
            int rowsCount = rowsList.Count;
            if (rowsCount > 0)
            {
                log.Info($"Found {rowsCount} rows added since {dateFrom} UTC");
                // Configure the email message describing how many rows were added
                message = new Mail
                {
                    From = new Email(emailFrom),
                    Subject = emailSubject + " (" + rowsCount + " new rows)"
                };
                var personalization = new Personalization();
                personalization.AddTo(new Email(emailTo));
                message.AddPersonalization(personalization);
                var content = new Content
                {
                    Type = "text/plain",
                    Value = "Found " + rowsCount + " new rows added since " + dateFrom.ToString("yyyy-MM-dd HH:mm:ss") + " UTC"
                };
                message.AddContent(content);
            }
            else
            {
                // Do not send the email if no new rows were found
                message = null;
            }
        }

    ![Fragmento de código de función de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/code.png)


7.  Haga clic en **Integrar** y **Entradas** para definir la conexión a Azure Table.

    ![Integración de función de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/integrate.png)


8.  Escriba el nombre de la tabla y defina la cadena de conexión; para ello, seleccione **Nuevo**.


    ![Conexión de tabla de función de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/configtable.png)

9.  Ahora defina la salida como SendGrid y mantenga todos los valores predeterminados.

    ![Salida de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutput.png)

    ![Valores predeterminados de salida de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutputdefaults.png)

10. Agregue la clave de API de SendGrid a la configuración de Function App con el nombre "SendGridApiKey" y el valor obtenido de las claves de API en la interfaz de usuario de SendGrid.

    ![Administración de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanage.png)
    ![Clave de administración de SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanagekey.png)

Una vez haya terminado de configurar la función, el código de la sección "Integrar" debe ser similar al del ejemplo siguiente.

    {
      "bindings": [
        {
          "name": "myTimer",
          "type": "timerTrigger",
          "direction": "in",
          "schedule": "0 */5 * * * *"
        },
        {
          "type": "table",
          "name": "inputTable",
          "tableName": "MarketplaceLeads",
          "take": 50,
          "connection": "yourstorageaccount_STORAGE",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "message",
          "apiKey": "SendGridApiKey",
          "direction": "out"
        }
      ],
      "disabled": false
    }

11. El último paso es ir a la interfaz de usuario de desarrollo de la función y, después, seleccionar **Ejecutar** para iniciar el temporizador. A partir de ahora recibirá una notificación cada vez que entre un nuevo cliente potencial.
