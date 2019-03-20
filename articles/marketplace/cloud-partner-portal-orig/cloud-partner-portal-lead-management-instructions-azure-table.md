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
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 31a800e851eb8a2f533cc8170856b16cd43c21cf
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850724"
---
# <a name="lead-management-instructions-for-azure-table"></a>Instrucciones de administración de clientes potenciales con Azure Table

En este artículo se describe cómo configurar Azure Table para almacenar clientes potenciales de ventas. Azure Table le permite almacenar y personalizar la información del cliente.

## <a name="to-configure-azure-table"></a>Configuración de Azure Table

1.  Si no tiene una cuenta de Azure, puede [crear una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

2.  Cuando la cuenta de Azure esté activa, inicie sesión en [Azure Portal](https://portal.azure.com).
3.  En Azure Portal, cree una cuenta de almacenamiento. La captura de pantalla siguiente muestra cómo crear una cuenta de almacenamiento. Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

    ![Pasos para crear una cuenta de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Copie la cadena de conexión de la cuenta de almacenamiento para la clave y péguela en el campo **Cadena de conexión de cuenta de almacenamiento** en Cloud Partner Portal. Este es un ejemplo de una cadena de conexión: `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net `
    
    ![Claves de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Puede usar el [Explorador de Azure Storage](https://azurestorageexplorer.codeplex.com/) o cualquier otra herramienta para ver los datos en la tabla de almacenamiento. También puede exportar los datos en Azure Table.
datos.

## <a name="optional-use-microsoft-flow-with-an-azure-table"></a>**(Opcional)**  Uso de Microsoft Flow con una tabla de Azure

Puede usar [Microsoft Flow](https://docs.microsoft.com/flow/) para automatizar las notificaciones cada vez que un cliente potencial se agregue a la tabla de Azure. Si aún no tiene una cuenta, puede [registrarse para obtener una gratuita](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Ejemplo de notificación de cliente potencial

Utilice este ejemplo como guía para crear un flujo simple que envíe automáticamente una notificación por correo electrónico cuando se agregue un nuevo cliente potencial a una tabla de Azure. En este ejemplo se configura una periodicidad para enviar información de clientes potenciales cada hora si se actualiza el almacenamiento en las tablas.

1. Inicie sesión en la cuenta de Microsoft Flow.
2. En la barra de navegación de la izquierda, seleccione **My flows** (Mis flujos).
3. En la barra de navegación de la izquierda, seleccione **+ New** (+ Nuevo).  
4. En la lista desplegable, seleccione **+ Crear desde cero**.
5. En Crear un flujo a partir de un documento en blanco, seleccione **Crear desde cero**.

   ![Creación de un flujo a partir de un documento en blanco](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

6. En la página de búsqueda de los conectores y desencadenadores, escriba **Desencadenadores**.
7. En **Desencadenadores**, seleccione **Periodicidad**.
8. En la ventana **Periodicidad**, mantenga el valor predeterminado de 1 para **Intervalo**. Desde la lista desplegable **Frecuencia**, seleccione **Hora**.

   >[!NOTE] 
   >Aunque en este ejemplo se usa un intervalo de 1 hora, puede seleccionar el intervalo y la frecuencia más apropiada para sus necesidades empresariales.

   ![Establecimiento de la frecuencia de 1 hora para la periodicidad](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

9. Seleccione **+ New step**(+ Nuevo paso).
10. Busque "Obtener la hora pasada" y, a continuación, seleccione **Obtener la hora pasada** en Acciones. 

    ![Búsqueda y selección de la acción de obtener la hora pasada](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

11. En la ventana **Obtener la hora pasada**, establezca el **Intervalo** en 1.  Desde la lista desplegable **Unidad de tiempo**, seleccione **Hora**.
    >[!IMPORTANT] 
    >Asegúrese de que este intervalo y unidad de tiempo coinciden con el intervalo y la frecuencia que se ha configurado para la periodicidad.

    ![Establecimiento del valor para obtener el intervalo de tiempo pasado](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >Puede comprobar el flujo en cualquier momento para comprobar que cada paso está configurado correctamente. Para comprobar el flujo, seleccione **Comprobador de flujo** en la barra de menús Flujo.

En el siguiente conjunto de pasos, se conectará a la tabla de Azure y configurará la lógica de procesamiento para controlar nuevos clientes potenciales.

1. Después del paso para obtener la hora pasada, seleccione **+ Nuevo paso** y, a continuación, busque "Obtener entidades".
2. En **Acciones**, seleccione **Obtener entidades** y, a continuación, seleccione **Mostrar opciones avanzadas**.
3. En la ventana **Obtener entidades**, proporcione información para los campos siguientes:

   - **Tabla**: escriba el nombre de Azure Table Storage. La captura de pantalla siguiente muestra el símbolo del sistema cuando se escribe "MarketPlaceLeads" en este ejemplo. 

     ![Elija un valor personalizado para el nombre de la tabla de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Consulta de filtro**: haga clic en este campo y el icono de Obtener la hora pasada se mostrará en una ventana emergente. Seleccione **Hora pasada** para usarla como marca de tiempo para filtrar la consulta. Como alternativa, puede pegar esta función en el campo: `gt datetime'@{body('Get_past_time')}'`

     ![Configuración de la función de consulta de filtro](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

4. Seleccione **Nuevo paso** para agregar una condición para examinar la tabla de Azure y comprobar si hay nuevos clientes potenciales.

   ![Uso del paso Nuevo para agregar una condición para examinar la tabla de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

5. En la ventana **Elegir una acción**, seleccione **Acciones** y, a continuación, seleccione el control **Condición**.

     ![Adición de un control a la condición](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

6. En la ventana **Condición**, seleccione el campo **Elegir un valor** y, a continuación, seleccione **Expresión** en la ventana emergente.
7. Pegue `length(body('Get_entities')?['value'])` en el campo ***fx***. Seleccione **Aceptar** para agregar esta función. Para finalizar la configuración de la condición:

   - Seleccione "es mayor que" en la lista desplegable.
   - Escriba 0 como valor. 

     ![Adición de una función a la condición](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

8. Configure la acción que se realizará basándose en el resultado de la condición.

     ![Configuración de las acciones según los resultados de la condición](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

9. Si la condición se resuelve como **En caso negativo**, no haga nada. 
10. Si la condición se resuelve como **En caso positivo**, desencadene una acción que se conecte a su cuenta de Office 365 para enviar un correo electrónico. Seleccione **Add an action**(Agregar una acción).
11. Seleccione **Enviar un correo electrónico**. 
12. En la ventana **Enviar un correo electrónico**, proporcione información para los campos siguientes:

    - **Para**: escriba una dirección de correo electrónico para todos los usuarios que reciban esta notificación.
    - **Asunto**: proporcione un asunto para el correo electrónico. Por ejemplo:  Nuevos clientes potenciales.
    - **Cuerpo**:   Agregue el texto que desee incluir en cada correo electrónico (opcional) y, a continuación, pegue en el cuerpo `('Get_entities')?['value']` como una función para insertar información de los clientes potenciales.

      >[!NOTE] 
      >Puede insertar puntos de datos estáticos o dinámicos adicionales en el cuerpo de este correo electrónico.

       ![Configuración del correo electrónico de notificación de clientes potenciales](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

13. Seleccione **Guardar** para guardar el flujo. Microsoft Flow probará automáticamente el flujo de errores. Si no hay errores, el flujo comienza a ejecutarse después de guardarlo.

La captura de pantalla siguiente muestra un ejemplo de cómo debe ser el último flujo.

 ![Secuencia del flujo final](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

### <a name="managing-your-flow"></a>Administración del flujo

Es fácil administrar el flujo una vez se está ejecutando.  Tiene control completo sobre el flujo. Por ejemplo, puede detenerlo, editarlo, ver un historial de ejecución y obtener el análisis. La captura de pantalla siguiente muestra las opciones disponibles para administrar un flujo. 

 ![Administración de un flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

El flujo seguirá ejecutándose hasta que lo detenga mediante la opción **Desactivar el flujo**.

Si no recibe notificaciones de correo electrónico de clientes potenciales, significa que no se han agregado nuevos clientes potenciales a la tabla de Azure. Si se produce un error de flujo, recibirá un correo electrónico similar al del ejemplo de la siguiente captura de pantalla.

 ![Notificación por correo electrónico de los errores del flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)

## <a name="next-steps"></a>Pasos siguientes

[Configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)