---
title: Supervisión de los cambios en máquinas virtuales con Azure Event Grid y Logic Apps | Microsoft Docs
description: Compruebe si hay cambios de configuración en las máquinas virtuales (VM) mediante el uso de Azure Event Grid y Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: 33634773b436114f4a5f2942028710ae50e0e703
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65801146"
---
# <a name="tutorial-monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Tutorial: Supervisión de los cambios en máquinas virtuales con Azure Event Grid y Logic Apps

Puede iniciar automáticamente el [flujo de trabajo de una aplicación lógica](../logic-apps/logic-apps-overview.md) cuando se producen eventos específicos en recursos de Azure o de otros fabricantes. Estos recursos pueden publicar esos eventos en una [cuadrícula de eventos Azure](../event-grid/overview.md). A su vez, la cuadrícula de eventos envía esos eventos a los suscriptores que tienen colas, webhooks o [centros de eventos](../event-hubs/event-hubs-what-is-event-hubs.md) como puntos de conexión. Como suscriptor, su aplicación lógica puede esperar esos eventos desde la cuadrícula de eventos antes de ejecutar flujos de trabajo automatizados para llevar a cabo ciertas tareas, sin escribir código.

Por ejemplo, aquí incluimos algunos eventos que los publicadores pueden enviar a los suscriptores a través del servicio Azure Event Grid:

* Crear, leer, actualizar o eliminar un recurso. Por ejemplo, puede supervisar los cambios que podrían incurrir en gastos en su suscripción de Azure y afectar a la facturación. 
* Agregar o quitar a una persona de una suscripción de Azure.
* La aplicación realiza una acción concreta.
* Aparece un nuevo mensaje en una cola.

En este tutorial se crea una aplicación lógica que supervisa los cambios realizados en una máquina virtual y envía mensajes de correo electrónico sobre dichos cambios. Cuando crea una aplicación lógica con una suscripción a eventos de un recurso de Azure, los eventos fluyen desde ese recurso a través de una cuadrícula de eventos a la aplicación lógica. El tutorial le guía en la creación de esta aplicación lógica:

![Introducción: Supervisión de los cambios en máquinas virtuales con Azure Event Grid y Logic Apps](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación lógica que supervisa eventos procedentes de una cuadrícula de eventos.
> * Incorporar una condición que comprueba específicamente cambios en la máquina virtual.
> * Enviar un mensaje de correo electrónico cuando la máquina virtual cambia.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una cuenta de correo electrónico de cualquier proveedor de correo electrónico que sea compatible con Azure Logic Apps, como Office 365 Outlook, Outlook.com o Gmail, para el envío de notificaciones. En el caso de otros proveedores, [consulte la lista de conectores que se muestra aquí](/connectors/). 

  Este tutorial utiliza una cuenta de Office 365 Outlook. Si utiliza una cuenta de correo electrónico diferente, los pasos generales siguen siendo los mismos pero la interfaz de usuario podría ser ligeramente distinta.

* Una [máquina virtual](https://azure.microsoft.com/services/virtual-machines). Si aún no lo ha hecho, cree una máquina virtual siguiendo un [tutorial para crear una máquina virtual](../virtual-machines/windows/quick-create-portal.md). Para hacer que la máquina virtual publique eventos, [no es necesario hacer nada más](../event-grid/overview.md).

## <a name="create-blank-logic-app"></a>Creación de una aplicación lógica en blanco

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con sus credenciales de su cuenta de Azure. 

1. En el menú principal de Azure, seleccione **Crear un recurso** > **Integración** > **Aplicación lógica**.

   ![Creación de la aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

1. En **Aplicación lógica**, proporcione información acerca de la aplicación lógica. Cuando termine, seleccione **Crear**.

   ![Proporcione los detalles de la aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Propiedad | Valor sugerido | Descripción |
   | -------- | --------------- | ----------- |
   | **Nombre** | <*nombre-de-la-aplicación-lógica*> | Proporcione un nombre único para la aplicación lógica. |
   | **Suscripción** | <*Azure-subscription-name*> | Seleccione la misma suscripción de Azure para todos los servicios de este tutorial. |
   | **Grupos de recursos** | <*Azure-resource-group*> | Seleccione el mismo grupo de recursos de Azure para todos los servicios de este tutorial. |
   | **Ubicación** | <*región del centro de datos de Azure*> | Seleccione la misma región para todos los servicios de este tutorial. |
   |||

   Ahora ha creado un recurso de Azure para la aplicación lógica. 

1. Una vez que Azure ha implementado la aplicación lógica, el Diseñador de aplicaciones lógicas muestra una página con un vídeo de introducción y desencadenadores utilizados frecuentemente. Desplácese más allá del vídeo y los desencadenadores. 

1. En **plantillas**, elija **Aplicación lógica en blanco**.

   ![Selección de la plantilla de aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   El Diseñador de aplicaciones lógicas ahora muestra los [*desencadenadores*](../logic-apps/logic-apps-overview.md#logic-app-concepts) que puede usar para iniciar la aplicación lógica. Cada aplicación lógica debe comenzar con un desencadenador, que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. 
   Cada vez que el desencadenador se activa, Azure Logic Apps crea una instancia de flujo de trabajo que ejecuta la aplicación lógica.

## <a name="add-event-grid-trigger"></a>Incorporación del desencadenador de Event Grid 

Ahora, agregue el desencadenador de Event Grid que supervisa el grupo de recursos de la máquina virtual. 

1. En el diseñador, en el cuadro de búsqueda, escriba "event grid" como filtro. En la lista de desencadenadores, seleccione este desencadenador: **Cuando se produce un evento de recursos: Azure Event Grid**

   ![Seleccione este desencadenador: "On a resource event"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

1. Cuando se le solicite, inicie sesión en Azure Event Grid con las credenciales de la cuenta de Azure. En la lista **Inquilino**, que muestra el inquilino de Azure Active Directory asociado con la suscripción de Azure, compruebe que aparece al inquilino correcto.

   ![Inicie sesión con sus credenciales de Azure](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Si inició sesión con una cuenta personal de Microsoft, como @outlook.com o @hotmail.com, el desencadenador de Event Grid podría no aparecer correctamente. Como alternativa, elija [Conectar con entidad de servicio](../active-directory/develop/howto-create-service-principal-portal.md) o autenticarse como un miembro de Azure Active Directory que esté asociado a la suscripción de Azure, por ejemplo, *nombre-de-usuario*@emailoutlook.onmicrosoft.com.

1. Ahora suscriba la aplicación lógica a los eventos de publicador. Proporcione los detalles de la suscripción a eventos como se especifica en la tabla siguiente:

   ![Proporcione detalles de la suscripción de eventos](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   | -------- | -------- | ----- | ----------- |
   | **Suscripción** | Sí | <*event-publisher-Azure-subscription-name*> | Seleccione el nombre de la suscripción de Azure asociada con el publicador de eventos. Para este tutorial, seleccione el nombre de la suscripción de Azure de la máquina virtual. |
   | **Tipo de recurso** | Sí | <*event-publisher-Azure-resource-type*> | Seleccione el tipo de recurso de Azure para el publicador de eventos. Para este tutorial, seleccione este valor para supervisar los grupos de recursos de Azure: <p><p>**Microsoft.Resources.ResourceGroups** |
   | **Nombre de recurso** |  Sí | <*event-publisher-Azure-resource-name*> | Seleccione el nombre del recurso de Azure para el publicador de eventos. Esta lista varía según el tipo de recurso que ha seleccionado. Para este tutorial, seleccione el nombre del grupo de recursos de Azure de la máquina virtual. |
   | **Elemento de tipo de evento** |  Sin  | <*event-types*> | Seleccione uno o varios tipos de evento específicos para filtrar y enviar a Event Grid. Por ejemplo, si lo desea puede agregar estos tipos de eventos para detectar cuándo se cambian o eliminan los recursos: <p><p>- **Microsoft.Resources.ResourceActionSuccess** <br>- **Microsoft.Resources.ResourceDeleteSuccess** <br>- **Microsoft.Resources.ResourceWriteSuccess** <p>Para más información, consulte los temas siguientes: <p><p>- [Descripción del filtrado de eventos](../event-grid/event-filtering.md) <br>- [Filtrado de eventos para Event Grid](../event-grid/how-to-filter-events.md) <br>- [Esquema de eventos de Azure Event Grid para grupos de recursos](../event-grid/event-schema-resource-groups.md) |
   | **Subscription Name** | Sin  | <*event-subscription-name*> | Proporcione un nombre único para la suscripción de eventos. |
   | Para configuraciones opcionales, elija **Agregar nuevo parámetro**. | Sin  | {ver descripciones} | * **Filtro de prefijo**: En este tutorial, deje esta propiedad vacía. El comportamiento predeterminado coincide con todos los valores. Sin embargo, puede especificar una cadena de prefijo como un filtro, por ejemplo, una ruta de acceso y un parámetro para un recurso concreto. <p>* **Filtro de sufijo**: En este tutorial, deje esta propiedad vacía. El comportamiento predeterminado coincide con todos los valores. Sin embargo, puede especificar una cadena de sufijo como un filtro, por ejemplo, una extensión de nombre de archivo, si desea solo determinados tipos de archivos. |
   |||

   Cuando haya terminado, el desencadenador de Event Grid podría parecerse a este ejemplo:

   ![Detalles del desencadenador de Event Grid de ejemplo](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

1. Guarde la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**. Para contraer y ocultar los detalles de una acción en la aplicación lógica, elija la barra de título de la acción.

   ![Guardado de la aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Cuando se guarda la aplicación lógica con un desencadenador de Event Grid, Azure crea automáticamente una suscripción a eventos para la aplicación lógica en el recurso seleccionado. Así, cuando el recurso publica un evento en la cuadrícula de eventos, esa cuadrícula de eventos inserta automáticamente el evento en la aplicación lógica. Este evento desencadena la aplicación lógica, que crea y ejecuta una instancia del flujo de trabajo que se define en los pasos siguientes.

La aplicación lógica ahora está activa y escucha los eventos de Event Grid, pero no hace nada hasta que agregue acciones al flujo de trabajo. 

## <a name="add-condition"></a>Incorporación de una condición

Para ejecutar la aplicación lógica solo cuando se produzca un evento específico, agregue una condición que compruebe si hay operaciones de escritura en la máquina virtual. Cuando esta condición se cumpla, la aplicación lógica le enviará un mensaje de correo electrónico con detalles acerca de la máquina virtual actualizada.

1. En el Diseñador de aplicaciones lógicas, bajo el desencadenador de Event Grid, elija **Nuevo paso**.

   ![Elección de "Nuevo paso"](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-new-step-condition.png)

1. En el cuadro de búsqueda, escriba "condición" como filtro. En la lista de acciones, seleccione esta acción: **Condition**

   ![Agregar una condición](./media/monitor-virtual-machine-changes-event-grid-logic-app/select-condition.png)

   El Diseñador de aplicaciones lógicas agrega una condición vacía al flujo de trabajo, incluidas las rutas de acceso de la acción que se deben seguir en función de si la condición se cumple o no.

   ![Condición vacía](./media/monitor-virtual-machine-changes-event-grid-logic-app/empty-condition.png)

1. Cambie el nombre del título de la condición a `If a virtual machine in your resource group has changed`. En la barra de título de la condición, elija el botón de los puntos suspensivos ( **...** ) y seleccione **Cambiar nombre**.

   ![Cambiar nombre de condición](./media/monitor-virtual-machine-changes-event-grid-logic-app/rename-condition.png)

1. Cree una condición que comprueba el evento `body` de un objeto `data` donde la propiedad `operationName` es igual a la operación `Microsoft.Compute/virtualMachines/write`. Más información sobre el [Esquema de eventos de la cuadrícula de eventos](../event-grid/event-schema.md).

   1. En la primera fila, debajo de **And**, haga clic en el cuadro izquierdo. En la lista de contenido dinámico que aparece, elija **Expression** (Expresión).

      ![Elección de "Expression"](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-choose-expression.png)

   1. En el editor de expresiones, escriba esta expresión y elija **Aceptar**: 

      `triggerBody()?['data']['operationName']`

      Por ejemplo: 

      ![Elección de "Expression"](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-add-data-operation-name.png)

   1. En el cuadro central, mantenga el operador **es igual que**.

   1. En el cuadro de la derecha, escriba este valor:

      `Microsoft.Compute/virtualMachines/write`

   Ahora, la condición finalizada se parecerá a este ejemplo:

   ![Condición completada](./media/monitor-virtual-machine-changes-event-grid-logic-app/complete-condition.png)

   Si cambia de la vista de diseño a la vista de código y vuelve a la vista de diseño, la expresión que especificó en la condición se resuelve en el token **data.operationName**:

   ![Condición resuelta](./media/monitor-virtual-machine-changes-event-grid-logic-app/resolved-condition.png)

1. Guarde la aplicación lógica.

## <a name="send-email-notifications"></a>Envío de notificaciones por correo electrónico

Ahora, agregue una [ *acción* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) para recibir un mensaje de correo electrónico cuando se cumpla la condición especificada.

1. En el cuadro **If true** (Si es true) de la condición, elija **Agregar una acción**.

   ![Agregar una acción para cuando la condición se cumpla](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-true-add-action.png)

1. En el cuadro de búsqueda, escriba "enviar correo electrónico" como filtro. En función de su proveedor de correo electrónico, busque y seleccione el conector correspondiente. A continuación, seleccione la acción "enviar correo electrónico" para el conector. Por ejemplo:  

   * Para una cuenta profesional o educativa de Azure, seleccione el conector de Office 365 Outlook. 

   * Para las cuentas de Microsoft personales, seleccione el conector de Outlook.com. 

   * Para las cuentas de Gmail, seleccione el conector de Gmail. 

   Este tutorial continúa con el conector de Office 365 Outlook. 
   Si utiliza un proveedor diferente, los pasos siguen siendo los mismos, pero la interfaz de usuario podría ser ligeramente diferente. 

   ![Seleccione la acción "send email"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

1. Si ya no tiene una conexión para el proveedor de correo electrónico, inicie sesión en su cuenta de correo electrónico cuando se le pida que se autentique.

1. Cambie el nombre del título del correo electrónico de envío por este título: `Send email when virtual machine updated`

1. Proporcione detalles para el correo electrónico como se especifica en la tabla siguiente:

   ![Acción de correo electrónico vacía](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Para seleccionar desde los resultados de los pasos anteriores en el flujo de trabajo, haga clic en un cuadro de edición para que aparezca la lista de contenido dinámico o elija **Agregar contenido dinámico**. Para más resultados, elija **Ver más** para cada sección de la lista. Para cerrar la lista de contenido dinámico, elija de nuevo **Agregar contenido dinámico**.

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   | -------- | -------- | ----- | ----------- |
   | **To** | Sí | <*destinatario\@dominio*> | Escriba la dirección de correo electrónico del destinatario. Para realizar pruebas, puede usar su propia dirección de correo electrónico. |
   | **Asunto** | Sí | Recurso actualizado: **Asunto** | Escriba el contenido del asunto del correo electrónico. Para este tutorial, escriba el texto especificado y seleccione el campo **Asunto** del evento. En este caso, el asunto del correo electrónico incluye el nombre del recurso actualizado (máquina virtual). |
   | **Cuerpo** | Sí | Recurso: **Tema.** <p>Tipo de evento: **Tipo de evento**<p>Id. de evento: **Id**<p>Hora: **Hora del evento** | Escriba el contenido del cuerpo del correo electrónico. Para este tutorial, escriba el texto especificado y seleccione los campos **Tema**, **Tipo de evento**, **ID** y **Hora del evento** del evento para que el correo electrónico incluya el recurso que desencadenó el evento, el tipo de evento, la marca de tiempo del evento y el identificador del evento de la actualización. Para este tutorial, el recurso es el grupo de recursos de Azure seleccionado en el desencadenador. <p>Para agregar líneas en blanco en el contenido, presione MAYÚS + ENTRAR. |
   ||||

   > [!NOTE]
   > Si selecciona un campo que representa una matriz, el diseñador agrega automáticamente un bucle **For each** alrededor de la acción que haga referencia a la matriz. De este modo, la aplicación lógica realiza la acción en cada elemento de la matriz.

   Ahora, la acción de correo electrónico podría parecerse a este ejemplo:

   ![Selección de los datos que se incluirán en el correo electrónico](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Y la aplicación lógica terminada podría parecerse a este ejemplo:

   ![Aplicación lógica terminada](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

1. Guarde la aplicación lógica. Para contraer y ocultar los detalles de cada acción en la aplicación lógica, elija la barra de título de la acción.

   La aplicación lógica ahora está activa, pero espera a que haya cambios en la máquina virtual antes de hacer algo. 
   Para probar la aplicación lógica ahora, pase a la sección siguiente.

## <a name="test-your-logic-app-workflow"></a>Comprobación del flujo de trabajo de la aplicación lógica

1. Para comprobar que la aplicación lógica está recibiendo los eventos especificados, actualice la máquina virtual.

   Por ejemplo, puede cambiar el tamaño de la máquina virtual en Azure Portal o [cambiar el tamaño de la máquina virtual con Azure PowerShell](../virtual-machines/windows/resize-vm.md).

   Transcurridos unos instantes, debería recibir un correo electrónico. Por ejemplo: 

   ![Correo electrónico acerca de la actualización de una máquina virtual](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

1. Para revisar el historial de ejecución y desencadenamiento de la aplicación lógica, en el menú de la aplicación lógica, elija **Información general**. Para ver más detalles acerca de una ejecución, seleccione la fila correspondiente a esa ejecución.

   ![Historial de ejecuciones de la aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

1. Para ver las entradas y salidas de cada paso, expanda el paso que desea revisar. Esta información puede ayudarle a diagnosticar y depurar los problemas de la aplicación lógica.

   ![Detalles del historial de ejecución de la aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Enhorabuena, ha creado y ejecutado una aplicación lógica que supervisa los eventos del recurso mediante una cuadrícula de eventos y le envía por correo electrónico un mensaje cuando se producen esos eventos. También ha aprendido cómo puede crear fácilmente flujos de trabajo que automatizan los procesos y a integrar sistemas y servicios en la nube.

Puede supervisar otros cambios de configuración con las cuadrículas de eventos y las aplicaciones lógicas, por ejemplo:

* Una máquina virtual obtiene derechos de control de acceso basado en roles (RBAC).
* Se realizan cambios en un grupo de seguridad de red (NSG) en una interfaz de red (NIC).
* Los discos para una máquina virtual se agregan o se quitan.
* Se asigna una dirección IP pública a una máquina virtual de NIC.

## <a name="clean-up-resources"></a>Limpieza de recursos

Este tutorial utiliza recursos y realiza acciones que generan gastos en su suscripción de Azure. Cuando haya terminado con el tutorial y las pruebas, asegúrese de deshabilitar o eliminar todos los recursos si no desea que generen gastos.

* Para detener la ejecución de la aplicación lógica sin eliminar el trabajo, deshabilítela. En el menú de la aplicación lógica, seleccione **Introducción**. En la barra de herramientas, elija **Deshabilitar**.

  ![Desactive la aplicación lógica](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Si no ve el menú de la aplicación lógica, intente volver al panel de Azure y vuelva a abrir la aplicación lógica.

* Para eliminar la aplicación lógica de forma definitiva, en el menú de la aplicación lógica, seleccione **Información general**. En la barra de herramientas, elija **Eliminar**. Confirme que quiere eliminar la aplicación lógica y elija **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación y enrutamiento de eventos personalizados con Event Grid](../event-grid/custom-event-quickstart.md)
