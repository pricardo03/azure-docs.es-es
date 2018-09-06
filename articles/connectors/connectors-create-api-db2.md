---
title: Conexión a IBM DB2 - Azure Logic Apps | Microsoft Docs
description: Administración de recursos con las API de REST de IBM DB2 y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: plarsen, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/23/2018
tags: connectors
ms.openlocfilehash: 354e67183a36f511811d74a0685dea2e23d6c0e2
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818882"
---
# <a name="manage-ibm-db2-resources-with-azure-logic-apps"></a>Administración de recursos de IBM DB2 con Azure Logic Apps

Con Azure Logic Apps y el conector de IBM DB2, puede crear tareas y flujos de trabajo automatizados en función de los recursos almacenados en la base de datos de DB2. Los flujos de trabajo pueden conectarse a los recursos de la base de datos, leer y enumerar las tablas de base de datos, agregar filas, cambiar filas, eliminar filas y mucho más. Puede incluir acciones en las aplicaciones lógicas que obtengan respuestas de la base de datos y que permitan que la salida esté disponible para otras acciones. 

En este artículo se explica cómo puede crear una aplicación lógica que lleve a cabo diversas operaciones de base de datos. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="supported-platforms-and-versions"></a>Versiones y plataformas compatibles

El conector de DB2 incluye un cliente de Microsoft para comunicarse con servidores de DB2 remotos a través de una red TCP/IP. Puede usar este conector para acceder a bases de datos en la nube, como IBM Bluemix dashDB o IBM DB2, para Windows en ejecución en la plataforma de virtualización de Azure. También puede acceder a bases de datos de DB2 locales después de [instalar y configurar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-connection.md). 

Este conector de IBM DB2 admite las siguientes plataformas y versiones de IBM DB2, así como los productos compatibles de este (por ejemplo, IBM Bluemix dashDB) que admiten las versiones 10 y 11 de SQL Access Manager (SQLAM) de la arquitectura distribuida de bases de datos relacionales (DRDA):

| Plataforma | Versión | 
|----------|---------|
| IBM DB2 para z/OS | 11.1, 10.1 |
| IBM DB2 para i | 7.3, 7.2, 7.1 |
| IBM DB2 para LUW | 11, 10.5 |
|||

## <a name="supported-database-operations"></a>Operaciones compatibles de base de datos

El conector de IBM DB2 admite estas operaciones de base de datos, que se asignan a las acciones correspondientes en el conector:

| Operación de base de datos | Acción del conector | 
|--------------------|------------------|
| Mostrar tablas de base de datos | Obtener tablas | 
| Leer una fila mediante SELECT | Obtener fila | 
| Leer todas las filas mediante SELECT | Obtener filas | 
| Agregar una fila mediante INSERT | Insertar fila | 
| Editar una fila mediante UPDATE | Actualizar fila | 
| Quitar una fila mediante DELETE | Eliminar fila | 
|||

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* Una base de datos de IBM DB2, ya sea local o en la nube

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica donde quiere acceder a la base de datos de DB2. Este conector ofrece únicamente las acciones, por lo que, para iniciar la aplicación lógica, seleccione un desencadenador independiente; por ejemplo, el desencadenador de **periodicidad**.
Los ejemplos de este artículo usan el desencadenador de **periodicidad**.

<a name="add-db2-action"></a>

## <a name="add-db2-action---get-tables"></a>Adición de acción de DB2: Obtener tablas

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el Diseñador de aplicación lógica, si aún no la ha abierto.

1. En el desencadenador, elija **New step** (Nuevo paso).

1. En el cuadro de búsqueda, escriba "db2" como filtro. En este ejemplo, en la lista de acciones, seleccione esta acción: **Obtener tablas (versión preliminar)**
   
   ![Acción Select](./media/connectors-create-api-db2/select-db2-action.png)

   Ahora se le pedirá que proporcione los detalles de conexión para la base de datos de DB2. 

1. Siga estos pasos para crear conexiones para las [bases de datos en la nube](#cloud-connection) o las [bases de datos locales](#on-premises-connection).

<a name="cloud-connection"></a>

## <a name="connect-to-cloud-db2"></a>Conexión a DB2 en la nube

Para establecer la conexión, proporcione estos detalles de conexión cuando se le solicite, elija **Crear** y después guarde la aplicación lógica:

| Propiedad | Obligatorio | DESCRIPCIÓN | 
|----------|----------|-------------| 
| **Conexión mediante puerta de enlace local** | Sin  | Solo se aplica a conexiones locales. | 
| **Nombre de la conexión** | SÍ | El nombre de la conexión; por ejemplo, "MyLogicApp-DB2-connection" |
| **Servidor** | SÍ | La dirección o el alias seguido de dos puntos y el número de puerto del servidor de DB2; por ejemplo, "myDB2server.cloudapp.net:50000" <p><p>**Nota**: Este valor es una cadena que representa una dirección TCP/IP o alias en formato IPv4 o IPv6, seguido de dos puntos y un número de puerto TCP/IP. |
| **Base de datos** | SÍ | El nombre de la base de datos <p><p>**Nota**: Este valor es una cadena que representa un nombre de base de datos relacional de DRDA (RDBNAM): <p>- DB2 para z/OS acepta una cadena de 16 bytes, donde la base de datos se conoce como una ubicación "IBM DB2 para z/OS". <br>- DB2 para DB2/OS acepta una cadena de 18 bytes, donde la base de datos se conoce como una base de datos relacional "IBM DB2 para i". <br>- DB2 para LUW acepta una cadena de 8 bytes. |
| **Nombre de usuario** | SÍ | El nombre de usuario de la base de datos <p><p>**Nota**: Este valor es una cadena cuya longitud se basa en una base de datos específica: <p><p>- DB2 para z/OS acepta una cadena de 8 bytes. <br>- DB2 para i acepta una cadena de 10 bytes. <br>- DB2 para Linux o UNIX acepta una cadena de 8 bytes. <br>- DB2 para Windows acepta una cadena de 30 bytes. | 
| **Contraseña** | SÍ | La contraseña de la base de datos | 
|||| 

Por ejemplo: 

![Detalles de conexión de las bases de datos basadas en la nube](./media/connectors-create-api-db2/create-db2-cloud-connection.png)

<a name="on-premises-connection"></a>

## <a name="connect-to-on-premises-db2"></a>Conexión a DB2 local

Antes de crear la conexión, ya debe tener la puerta de enlace de datos local instalada. En caso contrario, no puede terminar de configurar la conexión. Si ya dispone de la instalación de la puerta de enlace, proporcione estos detalles de conexión y después elija **Crear**.

| Propiedad | Obligatorio | DESCRIPCIÓN | 
|----------|----------|-------------| 
| **Conexión mediante puerta de enlace local** | SÍ | Se aplica cuando se desea una conexión local y se muestra las propiedades de la conexión local. | 
| **Nombre de la conexión** | SÍ | El nombre de la conexión; por ejemplo, "MyLogicApp-DB2-connection" | 
| **Servidor** | SÍ | La dirección o el alias seguido de dos puntos y el número de puerto del servidor de DB2; por ejemplo, "myDB2server:50000" <p><p>**Nota**: Este valor es una cadena que representa una dirección TCP/IP o alias en formato IPv4 o IPv6, seguido de dos puntos y un número de puerto TCP/IP. | 
| **Base de datos** | SÍ | El nombre de la base de datos <p><p>**Nota**: Este valor es una cadena que representa un nombre de base de datos relacional de DRDA (RDBNAM): <p>- DB2 para z/OS acepta una cadena de 16 bytes, donde la base de datos se conoce como una ubicación "IBM DB2 para z/OS". <br>- DB2 para DB2/OS acepta una cadena de 18 bytes, donde la base de datos se conoce como una base de datos relacional "IBM DB2 para i". <br>- DB2 para LUW acepta una cadena de 8 bytes. | 
| **Autenticación** | SÍ | El tipo de autenticación para la conexión; por ejemplo, "Basic" <p><p>**Nota**: Seleccione este valor en la lista, que incluye Basic o Windows (Kerberos). | 
| **Nombre de usuario** | SÍ | El nombre de usuario de la base de datos <p><p>**Nota**: Este valor es una cadena cuya longitud se basa en una base de datos específica: <p><p>- DB2 para z/OS acepta una cadena de 8 bytes. <br>- DB2 para i acepta una cadena de 10 bytes. <br>- DB2 para Linux o UNIX acepta una cadena de 8 bytes. <br>- DB2 para Windows acepta una cadena de 30 bytes. | 
| **Contraseña** | SÍ | La contraseña de la base de datos | 
| **Gateway** | SÍ | El nombre de la puerta de enlace de datos local instalada <p><p>**Nota**: Seleccione este valor en la lista, que incluye todas las puertas de enlace de datos instaladas en el grupo de recursos y las suscripciones de Azure. | 
|||| 

Por ejemplo: 

![Detalles de conexión de las bases de datos locales](./media/connectors-create-api-db2/create-db2-on-premises-connection.png)

### <a name="view-output-tables"></a>Visualización de las tablas de salida

Para ejecutar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Cuando finalice la ejecución de la aplicación lógica, puede ver la salida de la ejecución.

1. En el menú de la aplicación lógica, seleccione **Introducción**. 

1. En **Resumen**, en la sección **Historial de ejecuciones**, seleccione la ejecución más reciente, que es el primer elemento de la lista. 

   ![Visualización del historial de ejecuciones](./media/connectors-create-api-db2/run-history.png)

1. En **Ejecución de aplicación lógica**, ahora puede revisar el estado, las entradas y las salidas para cada paso de la aplicación lógica. Expanda la acción **Obtener tablas**.

   ![Acción Expandir](./media/connectors-create-api-db2/expand-action-step.png)

1. Para ver las entradas, elija **Mostrar entradas sin procesar**. 

1. Para ver las salidas, elija **Mostrar salidas sin procesar**. 

   Las salidas incluyen una lista de tablas. 
   
   ![Visualización de las tablas de salida](./media/connectors-create-api-db2/db2-connector-get-tables-outputs.png)

## <a name="get-row"></a>Obtener fila

Para capturar un registro de una tabla de base de datos de DB2, use la acción **Obtener fila** en la aplicación lógica. Esta acción ejecuta una instrucción `SELECT WHERE` de DB2; por ejemplo, `SELECT FROM AREA WHERE AREAID = '99999'`.

1. Si nunca antes ha usado las acciones de DB2 en la aplicación lógica, revise los pasos de la sección [Adición de acción de DB2: Obtener tablas](#add-db2-action), pero, en su lugar, agregue la acción **Obtener fila** y luego vuelva aquí para continuar. 

   Después de agregar la acción **Obtener fila**, esta será la apariencia de la aplicación lógica de ejemplo:

   ![Acción Obtener fila](./media/connectors-create-api-db2/db2-get-row-action.png)

1. Especifique los valores de todas las propiedades obligatorias (*). Después de seleccionar una tabla, la acción muestra las propiedades pertinentes que son específicas de los registros de esa tabla.

   | Propiedad | Obligatorio | DESCRIPCIÓN | 
   |----------|----------|-------------| 
   | **Nombre de la tabla** | SÍ | La tabla que tiene el registro deseado, como "AREA" en este ejemplo | 
   | **Id. de área** | SÍ | El identificador del registro que desee, como "99999" en este ejemplo | 
   |||| 

   ![Seleccionar tabla](./media/connectors-create-api-db2/db2-get-row-action-select-table.png)

1. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador. 

### <a name="view-output-row"></a>Visualización de la fila de salida

Para ejecutar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Cuando finalice la ejecución de la aplicación lógica, puede ver la salida de la ejecución.

1. En el menú de la aplicación lógica, seleccione **Introducción**. 

1. En **Resumen**, en la sección **Historial de ejecuciones**, seleccione la ejecución más reciente, que es el primer elemento de la lista. 

1. En **Ejecución de aplicación lógica**, ahora puede revisar el estado, las entradas y las salidas para cada paso de la aplicación lógica. Expanda la acción **Obtener fila**.

1. Para ver las entradas, elija **Mostrar entradas sin procesar**. 

1. Para ver las salidas, elija **Mostrar salidas sin procesar**. 

   Las salidas incluyen la fila especificada. 
   
   ![Visualización de la fila de salida](./media/connectors-create-api-db2/db2-connector-get-row-outputs.png)

## <a name="get-rows"></a>Obtener filas

Para capturar todos los registros de una tabla de base de datos de DB2, use la acción **Obtener filas** en la aplicación lógica. Esta acción ejecuta una instrucción `SELECT` de DB2; por ejemplo, `SELECT * FROM AREA`.

1. Si nunca antes ha usado las acciones de DB2 en la aplicación lógica, revise los pasos de la sección [Adición de acción de DB2: Obtener tablas](#add-db2-action), pero, en su lugar, agregue la acción **Obtener filas** y luego vuelva aquí para continuar. 

   Después de agregar la acción **Obtener filas**, esta será la apariencia de la aplicación lógica de ejemplo:

   ![Acción Obtener filas](./media/connectors-create-api-db2/db2-get-rows-action.png)

1. Abra la lista **Nombre de tabla** y luego seleccione la tabla que desee, que es "AREA" en este ejemplo: 

   ![Seleccionar tabla](./media/connectors-create-api-db2/db2-get-rows-action-select-table.png)

1. Para especificar un filtro o consultar resultados, elija **Mostrar opciones avanzadas**.

1. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador. 

### <a name="view-output-rows"></a>Visualización de filas de salida

Para ejecutar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Cuando finalice la ejecución de la aplicación lógica, puede ver la salida de la ejecución.

1. En el menú de la aplicación lógica, seleccione **Introducción**. 

1. En **Resumen**, en la sección **Historial de ejecuciones**, seleccione la ejecución más reciente, que es el primer elemento de la lista. 

1. En **Ejecución de aplicación lógica**, ahora puede revisar el estado, las entradas y las salidas para cada paso de la aplicación lógica. Expanda la acción **Obtener filas**.

1. Para ver las entradas, elija **Mostrar entradas sin procesar**. 

1. Para ver las salidas, elija **Mostrar salidas sin procesar**. 

   Las salidas incluyen todos los registros de la tabla especificada.
   
   ![Visualización de filas de salida](./media/connectors-create-api-db2/db2-connector-get-rows-outputs.png)

## <a name="insert-row"></a>Insertar fila

Para agregar un único registro de una tabla de base de datos de DB2, use la acción **Insertar fila** en la aplicación lógica. Esta acción ejecuta una instrucción `INSERT` de DB2; por ejemplo, `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

1. Si nunca antes ha usado las acciones de DB2 en la aplicación lógica, revise los pasos de la sección [Adición de acción de DB2: Obtener tablas](#add-db2-action), pero, en su lugar, agregue la acción **Insertar fila** y luego vuelva aquí para continuar. 

   Después de agregar la acción **Insertar fila**, esta será la apariencia de la aplicación lógica de ejemplo:

   ![Acción Insertar fila](./media/connectors-create-api-db2/db2-insert-row-action.png)

1. Especifique los valores de todas las propiedades obligatorias (*). Después de seleccionar una tabla, la acción muestra las propiedades pertinentes que son específicas de los registros de esa tabla. 

   Para este ejemplo, estas son las propiedades:

   | Propiedad | Obligatorio | DESCRIPCIÓN | 
   |----------|----------|-------------| 
   | **Nombre de la tabla** | SÍ | La tabla a la que agregar el registro, como "AREA" | 
   | **Id. de área** | SÍ | El identificador del área que se va a agregar, como "99999" | 
   | **Descripción del área** | SÍ | La descripción del área que se va a agregar, como "Area 99999" | 
   | **Id. de región** | SÍ | El identificador de la región que se va a agregar, como "102" | 
   |||| 

   Por ejemplo: 

   ![Seleccionar tabla](./media/connectors-create-api-db2/db2-insert-row-action-select-table.png)

1. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador. 

### <a name="view-insert-row-outputs"></a>Visualización de las salidas de Insertar filas

Para ejecutar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Cuando finalice la ejecución de la aplicación lógica, puede ver la salida de la ejecución.

1. En el menú de la aplicación lógica, seleccione **Introducción**. 

1. En **Resumen**, en la sección **Historial de ejecuciones**, seleccione la ejecución más reciente, que es el primer elemento de la lista. 

1. En **Ejecución de aplicación lógica**, ahora puede revisar el estado, las entradas y las salidas para cada paso de la aplicación lógica. Expanda la acción **Insertar fila**.

1. Para ver las entradas, elija **Mostrar entradas sin procesar**. 

1. Para ver las salidas, elija **Mostrar salidas sin procesar**. 

   Las salidas incluyen el registro agregado a la tabla especificada.
   
   ![Visualización de la salida con la fila insertada](./media/connectors-create-api-db2/db2-connector-insert-row-outputs.png)

## <a name="update-row"></a>Actualizar fila

Para actualizar un único registro de una tabla de base de datos de DB2, use la acción **Actualizar fila** en la aplicación lógica. Esta acción ejecuta una instrucción `UPDATE` de DB2; por ejemplo, `UPDATE AREA SET AREAID = '99999', AREADESC = 'Updated 99999', REGIONID = 102)`.

1. Si nunca antes ha usado las acciones de DB2 en la aplicación lógica, revise los pasos de la sección [Adición de acción de DB2: Obtener tablas](#add-db2-action), pero, en su lugar, agregue la acción **Actualizar fila** y luego vuelva aquí para continuar. 

   Después de agregar la acción **Actualizar fila**, esta será la apariencia de la aplicación lógica de ejemplo:

   ![Acción Actualizar fila](./media/connectors-create-api-db2/db2-update-row-action.png)

1. Especifique los valores de todas las propiedades obligatorias (*). Después de seleccionar una tabla, la acción muestra las propiedades pertinentes que son específicas de los registros de esa tabla. 

   Para este ejemplo, estas son las propiedades:

   | Propiedad | Obligatorio | DESCRIPCIÓN | 
   |----------|----------|-------------| 
   | **Nombre de la tabla** | SÍ | La tabla en la que actualizar el registro, como "AREA" | 
   | **Id. de fila** | SÍ | Identificador del registro que se va a actualizar, como "99999" | 
   | **Id. de área** | SÍ | El nuevo identificador de área, como "99999" | 
   | **Descripción del área** | SÍ | La nueva descripción del área, como "Updated 99999" | 
   | **Id. de región** | SÍ | El nuevo identificador de región, como "102" | 
   |||| 

   Por ejemplo: 

   ![Seleccionar tabla](./media/connectors-create-api-db2/db2-update-row-action-select-table.png)

1. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador. 

### <a name="view-update-row-outputs"></a>Visualización de las salidas de Actualizar fila

Para ejecutar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Cuando finalice la ejecución de la aplicación lógica, puede ver la salida de la ejecución.

1. En el menú de la aplicación lógica, seleccione **Introducción**. 

1. En **Resumen**, en la sección **Historial de ejecuciones**, seleccione la ejecución más reciente, que es el primer elemento de la lista. 

1. En **Ejecución de aplicación lógica**, ahora puede revisar el estado, las entradas y las salidas para cada paso de la aplicación lógica. Expanda la acción **Actualizar fila**.

1. Para ver las entradas, elija **Mostrar entradas sin procesar**. 

1. Para ver las salidas, elija **Mostrar salidas sin procesar**. 

   Las salidas incluyen el registro actualizado en la tabla especificada.
   
   ![Visualización de la salida con la fila actualizada](./media/connectors-create-api-db2/db2-connector-update-row-outputs.png)

## <a name="delete-row"></a>Eliminar fila

Para eliminar un único registro de una tabla de base de datos de DB2, use la acción **Eliminar fila** en la aplicación lógica. Esta acción ejecuta una instrucción `DELETE` de DB2; por ejemplo, `DELETE FROM AREA WHERE AREAID = '99999'`.

1. Si nunca antes ha usado las acciones de DB2 en la aplicación lógica, revise los pasos de la sección [Adición de acción de DB2: Obtener tablas](#add-db2-action), pero, en su lugar, agregue la acción **Eliminar fila** y luego vuelva aquí para continuar. 

   Después de agregar la acción **Eliminar fila**, esta será la apariencia de la aplicación lógica de ejemplo:

   ![Acción Eliminar fila](./media/connectors-create-api-db2/db2-delete-row-action.png)

1. Especifique los valores de todas las propiedades obligatorias (*). Después de seleccionar una tabla, la acción muestra las propiedades pertinentes que son específicas de los registros de esa tabla. 

   Para este ejemplo, estas son las propiedades:

   | Propiedad | Obligatorio | DESCRIPCIÓN | 
   |----------|----------|-------------| 
   | **Nombre de la tabla** | SÍ | La tabla de la que eliminar el registro, como "AREA" | 
   | **Id. de fila** | SÍ | Identificador del registro que se va a eliminar, como "99999" | 
   |||| 

   Por ejemplo: 

   ![Seleccionar tabla](./media/connectors-create-api-db2/db2-delete-row-action-select-table.png)

1. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador. 

### <a name="view-delete-row-outputs"></a>Visualización de las salidas de Eliminar fila

Para ejecutar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. Cuando finalice la ejecución de la aplicación lógica, puede ver la salida de la ejecución.

1. En el menú de la aplicación lógica, seleccione **Introducción**. 

1. En **Resumen**, en la sección **Historial de ejecuciones**, seleccione la ejecución más reciente, que es el primer elemento de la lista. 

1. En **Ejecución de aplicación lógica**, ahora puede revisar el estado, las entradas y las salidas para cada paso de la aplicación lógica. Expanda la acción **Eliminar fila**.

1. Para ver las entradas, elija **Mostrar entradas sin procesar**. 

1. Para ver las salidas, elija **Mostrar salidas sin procesar**. 

   Las salidas ya no incluyen el registro eliminado de la tabla especificada.
   
   ![Visualización de la salida sin la fila eliminada](./media/connectors-create-api-db2/db2-connector-delete-row-outputs.png)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnica, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo Swagger del conector, consulte la [página de referencia del conector](/connectors/db2/). 

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
