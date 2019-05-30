---
title: Tabla de Azure | Azure Marketplace
description: Configure la administración de clientes potenciales para Azure Table.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: pabutler
ms.openlocfilehash: a1bcab9816627b453ba8b20b7bcd9402c2dfd151
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240456"
---
# <a name="lead-management-instructions-for-azure-table"></a>Instrucciones de administración de clientes potenciales de Azure Table Storage

En este artículo se describe cómo configurar Azure Table para almacenar clientes potenciales de ventas. Azure Table le permite almacenar y personalizar la información del cliente.


## <a name="how-to-configure-azure-table"></a>Cómo configurar la tabla de Azure

1. Si no tiene una cuenta de Azure, puede [crear una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
2. Una vez activa la cuenta de Azure, inicie sesión en el [portal Azure](https://portal.azure.com).
3. En el portal de Azure, cree una cuenta de almacenamiento mediante el procedimiento siguiente.  
    1. Seleccione **+ crear un recurso** en la barra de menús izquierdo.  El **New** (hoja) del panel se mostrará a la derecha.
    2. Seleccione **almacenamiento** en el **New** panel.  Un **destacado** se muestra a la derecha.
    3. Seleccione **cuenta de almacenamiento** para iniciar la creación de cuenta.  Siga las instrucciones del artículo [crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

    ![Pasos para crear una cuenta de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

    Para obtener más información acerca de las cuentas de almacenamiento, seleccione [tutorial de inicio rápido](https://docs.microsoft.com/azure/storage/).  Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

4. Espere hasta que se aprovisiona la cuenta de almacenamiento, un proceso que normalmente tarda unos minutos.  A continuación, obtener acceso a la cuenta de almacenamiento desde el **inicio** página del portal de Azure mediante la selección de **ver todos los recursos** o seleccionando **todos los recursos** desde el panel de navegación izquierdo barra de menús del portal de Azure.

    ![Acceder a su cuenta de almacenamiento de Azure](./media/cloud-partner-portal-lead-management-instructions-azure-table/azure-storage-access.png)

5. En el panel de la cuenta de almacenamiento, copie la cadena de conexión de la cuenta de almacenamiento para la clave y péguelo en el **cadena de conexión de cuenta de almacenamiento** campo en Cloud Partner Portal. Un ejemplo de una cadena de conexión es:

```sql
DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
```

  ![Claves de Azure Storage](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Puede usar [Explorador de Azure storage](https://azurestorageexplorer.codeplex.com/) u otra herramienta similar para ver los datos en la tabla de almacenamiento. También puede exportar los datos de las tablas de Azure.


## <a name="use-microsoft-flow-with-an-azure-table-optional"></a>Usar Microsoft Flow con una tabla de Azure (*opcional*) 

Puede usar [Microsoft Flow](https://docs.microsoft.com/flow/) para automatizar las notificaciones cada vez que un cliente potencial se agregue a la tabla de Azure. Si no tiene una cuenta, puede [registrarse para obtener una cuenta gratuita](https://flow.microsoft.com/).


### <a name="lead-notification-example"></a>Ejemplo de notificación de cliente potencial

Utilice este ejemplo como guía para crear un flujo básico que se envía automáticamente una notificación por correo electrónico cuando se agrega un nuevo cliente potencial a una tabla de Azure. En este ejemplo se configura una periodicidad para enviar información de clientes potenciales cada hora si se actualiza el almacenamiento en las tablas.

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

   ![Establecer la frecuencia de 1 hora para la periodicidad](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

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

   - **Consulta de filtro** : haga clic en este campo y el **obtener la hora pasada** icono se muestra en una ventana emergente. Seleccione **Hora pasada** para usarla como marca de tiempo para filtrar la consulta. Como alternativa, puede pegar la siguiente función en el campo: CreatedTime `gt datetime'@{body('Get_past_time')}'` 

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
    - **Asunto**: proporcione un asunto para el correo electrónico. Por ejemplo: Nuevos clientes potenciales.
    - **Cuerpo**:   Agregue el texto que desee incluir en cada correo electrónico (opcional) y, a continuación, pegue en el cuerpo `body('Get_entities')?['value']` como una función para insertar información de los clientes potenciales.

      >[!NOTE] 
      >Puede insertar puntos de datos estáticos o dinámicos adicionales en el cuerpo de este correo electrónico.

      ![Configuración del correo electrónico de notificación de clientes potenciales](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

13. Seleccione **Guardar** para guardar el flujo. Microsoft Flow probará automáticamente el flujo de errores. Si no hay errores, el flujo comienza a ejecutarse después de guardarlo.

La captura de pantalla siguiente muestra un ejemplo de cómo debe ser el último flujo.

[![Secuencia de final del flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end-thmb.png)](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

(*Haga clic en la imagen para ampliarla.* )


### <a name="manage-your-flow"></a>Administre el flujo

Es fácil administrar el flujo una vez se está ejecutando.  Tiene control completo sobre el flujo. Por ejemplo, puede detenerlo, editarlo, ver un historial de ejecución y obtener el análisis. La captura de pantalla siguiente muestra las opciones disponibles para administrar un flujo. 

 ![Administración de un flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

El flujo seguirá ejecutándose hasta que lo detenga mediante la opción **Desactivar el flujo**.

Si no recibe notificaciones de correo electrónico de clientes potenciales, significa que no se han agregado nuevos clientes potenciales a la tabla de Azure. Si se produce un error de flujo, recibirá un correo electrónico similar al del ejemplo de la siguiente captura de pantalla.

 ![Notificación por correo electrónico de los errores del flujo](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)


## <a name="next-steps"></a>Pasos siguientes

[Configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)
