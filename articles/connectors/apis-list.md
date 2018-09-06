---
title: Conectores para Azure Logic Apps | Microsoft Docs
description: Automatización de flujos de trabajo con conectores para Azure Logic Apps, que incluyen la cuenta integrada, administrada y local y los conectores empresariales
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 6b31882ec3916e60ac7dc7b8117328176abef1b4
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818541"
---
# <a name="connectors-for-azure-logic-apps"></a>Conectores para Azure Logic Apps

Los conectores juegan un papel fundamental cuando se crean flujos de trabajo automatizados con Azure Logic Apps. Usar conectores en las aplicaciones lógicas permite ampliar las funcionalidades de las aplicaciones locales y en la nube para realizar tareas con los datos que ya tiene y con los que cree. 

Aunque Logic Apps ofrece [más de 200 conectores](https://docs.microsoft.com/connectors), en este artículo se describen los conectores populares y más utilizados que miles de aplicaciones y millones de ejecuciones usan correctamente para el procesamiento de datos e información. Los conectores están disponibles como conectores integrados o administrados. 

> [!NOTE]
> Para obtener la lista completa de conectores y la información de referencia de cada conector, por ejemplo, acciones, desencadenadores y límites, vea la [Introducción a los conectores](https://docs.microsoft.com/connectors).

* [**Elementos integrados**](#built-ins): estas acciones y desencadenadores integrados ayudan a crear aplicaciones lógicas que se ejecutan en programaciones personalizadas, se comunican con otros puntos de conexión, reciben solicitudes y responden a ellas, y llaman a funciones de Azure, aplicaciones web de Azure API Apps, sus propias API administradas y publicadas con Azure API Management, y aplicaciones lógicas anidadas que pueden recibir solicitudes. También puede usar acciones integradas que le ayudarán a organizar y controlar el flujo de trabajo de la aplicación lógica, y a trabajar con datos.

* **Conectores administrados**: estos conectores proporcionan desencadenadores y acciones para tener acceso a otros servicios y sistemas. Algunos conectores requieren crear primero las conexiones que Azure Logic Apps administrará. Los conectores administrados se organizan en estos grupos:

  |   |   |
  |---|---|
  | [**Conectores de API administrados**](#managed-api-connectors) | Cree aplicaciones lógicas que usan servicios tales como Azure Blob Storage, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online y muchos más. | 
  | [**Conectores locales**](#on-premises-connectors) | Después de instalar y configurar la [puerta de enlace de datos local][gateway-doc], estos conectores ayudan a las aplicaciones lógicas a acceder a los sistemas locales, como SQL Server, SharePoint Server, Oracle DB, recursos compartidos de archivos y otros. | 
  | [**Conectores de la cuenta de integración**](#integration-account-connectors) | Disponibles cuando se crea y se paga una cuenta de integración, estos conectores transforman y validan el código XML, codifican y descodifican archivos sin formato y procesan mensajes de negocio a negocio (B2B) con protocolos AS2, EDIFACT y X12. | 
  | [**Conectores de empresa**](#enterprise-connectors) | Proporcionan acceso a sistemas empresariales como SAP e IBM MQ por un costo adicional. |
  ||| 

  Por ejemplo, si usa Microsoft BizTalk Server, las aplicaciones lógicas podrán conectarse y comunicarse con BizTalk Server utilizando el [conector de BizTalk Server](#on-premises-connectors). 
  Después, puede ampliar o realizar operaciones de BizTalk en las aplicaciones lógicas mediante los [conectores de la cuenta de integración](#integration-account-connectors). 

> [!NOTE] 
> Para obtener la lista completa de conectores y la información de referencia de cada conector, como acciones y desencadenadores, que se definen mediante una descripción de Swagger, además de los límites, vea la [Introducción a los conectores](/connectors/). Para obtener información sobre precios, consulte los [detalles de precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) y el [modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Integrados

Logic Apps proporciona desencadenadores y acciones integrados para que pueda crear flujos de trabajo basados en programación, ayudar a las aplicaciones lógicas a comunicarse con otras aplicaciones y servicios, controlar el flujo de trabajo a través de las aplicaciones lógicas y administrar o manipular los datos. 

|   |   |   |   | 
|---|---|---|---| 
| [![Icono de API][schedule-icon]<br/>**Programación**][recurrence-doc] | - Ejecute la aplicación lógica según una programación especificada, con periodicidades básicas o complejas, con el desencadenador **Periodicidad**. <p>- Pause la aplicación lógica durante un tiempo especificado con la acción **Retraso**. <p>- Pause la aplicación lógica hasta la fecha y hora especificadas con la acción **Retraso hasta**. | [![Icono de API][http-icon]<br/>**HTTP**][http-doc] | Comuníquese con cualquier punto de conexión mediante HTTP con desencadenadores y acciones para HTTP, HTTP + Swagger y HTTP + Webhook. | 
| [![Icono de API][http-request-icon]<br/>**Solicitud**][http-request-doc] | - Haga que su aplicación lógica se pueda llamar desde otras aplicaciones o servicios, desencadene ante eventos de recursos de Event Grid o desencadene ante respuestas a las alertas de Azure Security Center con el desencadenador **Solicitud**. <p>- Envíe respuestas a una aplicación o servicio con la acción **Respuesta**. | [![Icono de la API][batch-icon]<br/>**Lotes**][batch-doc] | - Procese mensajes por lotes con el desencadenador **Mensajes del lote**. <p>- Llame a aplicaciones lógicas que tengan desencadenadores por lotes con la acción **Enviar mensajes al lote**. | 
| [![Icono de API][azure-functions-icon]<br/>**Azure Functions**][azure-functions-doc] | Llame a funciones de Azure que ejecuten fragmentos de código personalizados (C# o Node.js) desde las aplicaciones lógicas. | [![Icono de API][azure-api-management-icon]</br>**Azure API Management**][azure-api-management-doc] | Llame a los desencadenadores y las acciones definidos por las API que administre y publique con Azure API Management. | 
| [![Icono de API][azure-app-services-icon]<br/>**Azure App Services**][azure-app-services-doc] | Llame a las aplicaciones de API o las aplicaciones web de Azure hospedadas en Azure App Service. Los desencadenadores y las acciones definidos por estas aplicaciones aparecen como cualquier otro desencadenador y acción de primera clase cuando se incluye Swagger. | [![Icono de API][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps**][nested-logic-app-doc] | Llame a otras aplicaciones lógicas que comiencen con un desencadenador Solicitud. | 
||||| 

### <a name="control-workflow"></a>Controlar el flujo de trabajo

Estas son las acciones integradas para estructurar y controlar las acciones en el flujo de trabajo de la aplicación lógica:

|   |   |   |   | 
|---|---|---|---| 
| [![Icono integrado][condition-icon]<br/>**Condición**][condition-doc] | Evalúa una condición y ejecuta acciones diferentes según si la condición es true o false. | [![Icono integrado][for-each-icon]</br>**Para cada uno**][for-each-doc] | Realiza las mismas acciones en todos los elementos de una matriz. | 
| [![Icono integrado][scope-icon]<br/>**Ámbito**][scope-doc] | Agrupa las acciones en *ámbitos*, que obtienen su propio estado después de que las acciones del ámbito terminen de ejecutarse. | [![Icono integrado][switch-icon]</br>**Conmutador**][switch-doc] | Agrupa las acciones en *casos* a los que se asignan valores únicos, excepto el caso predeterminado. Ejecuta solo el caso cuyo valor asignado coincida con el resultado de una expresión, objeto o token. Si no hay coincidencias, ejecuta el caso predeterminado. | 
| [![Icono integrado][terminate-icon]<br/>**Finalizar**][terminate-doc] | Detiene el flujo de trabajo que la aplicación lógica está ejecutando activamente. | [![Icono integrado][until-icon]<br/>**Hasta**][until-doc] | Repite las acciones hasta que la condición especificada sea true o cambie algún estado. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Administrar o manipular datos

Estas son las acciones integradas para trabajar con salidas de datos y sus formatos:  

|   |   | 
|---|---| 
| ![Icono integrado][data-operations-icon]<br/>**Operaciones de datos** | Realiza operaciones con datos: <p>- **Redactar**: crea una única salida a partir de varias entradas con varios tipos. <br>- **Crear tabla CSV**: crea una tabla con valores separados por comas (CSV) a partir de una matriz con objetos JSON. <br>- **Crear tabla HTML**: crea una tabla HTML a partir de una matriz con objetos JSON. <br>- **Filtrar matriz**: crea una matriz a partir de elementos de otra matriz que cumplen los criterios. <br>- **Unirse**: crea una cadena con todos los elementos de una matriz y los separa con el delimitador especificado. <br>- **Analizar JSON**: crea tokens sencillos a partir de propiedades y sus valores en contenido JSON para que pueda usar esas propiedades en el flujo de trabajo. <br>- **Seleccionar**: crea una matriz con objetos JSON mediante la transformación de elementos o valores de otra matriz, y asignar esos elementos a propiedades especificadas. | 
| ![Icono integrado][date-time-icon]<br/>**Fecha y hora** | Realiza operaciones con marcas de tiempo: <p>- **Agregar a la hora**: agrega el número especificado de unidades a una marca de tiempo. <br>- **Convertir la zona horaria**: convierte una marca de tiempo de la zona horaria de origen a la zona horaria de destino. <br>- **Hora actual**: devuelve la marca de tiempo actual como una cadena. <br>- **Obtener la hora futura**: devuelve la marca de tiempo actual más las unidades de tiempo especificadas. <br>- **Obtener la hora pasada**: devuelve la marca de tiempo actual menos las unidades de tiempo especificadas. <br>- **Restar de la hora**: resta un número de unidades de tiempo de una marca de tiempo. |
| [![Icono integrado][variables-icon]<br/>**Variables**][variables-doc] | Realiza operaciones con variables: <p>- **Anexar a la variable de matriz**: inserta un valor como el último elemento en una matriz almacenada en una variable. <br>- **Anexar a la variable de cadena**: inserta un valor como el último carácter en una cadena almacenada en una variable. <br>- **Reducir variable**: reduce una variable en un valor constante. <br>- **Incrementar variable**: incrementa una variable en un valor constante. <br>- **Inicializar variable**: crea una variable y declara su tipo de datos y el valor inicial. <br>- **Establecer variable**: asigna un valor diferente a una variable existente. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Conectores de API administrados

Estos son los conectores más populares para automatizar tareas, procesos y flujos de trabajo con estos servicios o sistemas:

|   |   |   |   | 
|---|---|---|---| 
| [![Icono de API][azure-service-bus-icon]<br/>**Azure Service Bus**][azure-service-bus-doc] | Administre mensajes asincrónicos, sesiones y suscripciones a temas con el conector más usado en Logic Apps. | [![Icono de API][sql-server-icon]<br/>**SQL Server**][sql-server-doc] | Conéctese a SQL Server en el entorno local o a una base de datos de Azure SQL Database en la nube para poder administrar los registros, ejecutar procedimientos almacenados o realizar consultas. | 
| [![Icono de API][office-365-outlook-icon]<br/>**Office 365<br/>Outlook**][office-365-outlook-doc] | Conéctese a su cuenta de correo electrónico de Office 365 para poder crear y administrar mensajes de correo electrónico, tareas, eventos de calendario y reuniones, contactos, solicitudes y más. | [![Icono de API][azure-blob-storage-icon]<br/>**Azure Blob<br/>Storage**][azure-blob-storage-doc] | Conéctese a su cuenta de almacenamiento para poder crear y administrar el contenido en blobs. | 
| [![Icono de API][sftp-icon]<br/>**SFTP**][sftp-doc] | Conéctese a servidores SFTP accesibles desde internet para poder trabajar con archivos y carpetas. | [![Icono de API][sharepoint-online-icon]<br/>**SharePoint<br/>Online**][sharepoint-online-doc] | Conéctese a SharePoint Online para poder administrar archivos, datos adjuntos, carpetas y mucho más. | 
| [![Icono de API][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Conéctese a su cuenta de Dynamics 365 para poder crear y administrar registros, elementos y mucho más. | [![Icono de API][ftp-icon]<br/>**FTP**][ftp-doc] | Conéctese a servidores FTP accesibles desde internet para poder trabajar con archivos y carpetas. | 
| [![Icono de API][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Conéctese a su cuenta de Salesforce para poder crear y administrar elementos tales como registros, trabajos, objetos y mucho más. | [![Icono de API][twitter-icon]<br/>**Twitter**][twitter-doc] | Conéctese a su cuenta de Twitter para poder administrar tweets, seguidores, escalas de tiempo y mucho más. Guarde sus tweets en SharePoint, Excel o SQL. | 
| [![Icono de API][azure-event-hubs-icon]<br/>**Azure Event Hubs**][azure-event-hubs-doc] | Consuma y publique eventos en un centro de eventos. Por ejemplo, obtenga una salida de su aplicación lógica con Event Hubs y enviarla luego a un proveedor de análisis en tiempo real. | [![Icono de API][azure-event-grid-icon]<br/>**Azure Event**</br>**Grid**][azure-event-grid-doc] | Supervise los eventos publicados por Event Grid, por ejemplo, cuando cambian los recursos de Azure o los recursos de terceros. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Conectores locales 

Estos son algunos de los conectores más usados que proporcionan acceso a datos y recursos en sistemas locales. Para poder crear una conexión a un sistema local, primero debe [descargar, instalar y configurar una puerta de enlace de datos local][gateway-doc]. Esta puerta de enlace proporciona un canal de comunicación seguro sin tener que configurar la infraestructura de red necesaria. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![Icono de API][biztalk-server-icon]<br/>**BizTalk**</br> **Servidor** | [![Icono de API][file-system-icon]<br/>**File</br> System**][file-system-doc] | [![Icono de API][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![Icono de API][ibm-informix-icon]<br/>**IBM**</br> **Informix**][ibm-informix-doc] | ![Icono de API][mysql-icon]<br/>**MySQL** | 
| [![Icono de API][oracle-db-icon]<br/>**Oracle DB**][oracle-db-doc] | ![Icono de API][postgre-sql-icon]<br/>**PostgreSQL** | [![Icono de API][sharepoint-server-icon]<br/>**SharePoint</br> Server**][sharepoint-server-doc] | [![Icono de API][sql-server-icon]<br/>**SQL Server</br>**][sql-server-doc] | ![Icono de API][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Conectores de la cuenta de integración 

Estos son los conectores para crear soluciones de negocio a negocio (B2B) con sus aplicaciones lógicas cuando se crea y se paga por una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), que está disponible en Enterprise Integration Pack (EIP) en Azure. Con esta cuenta, puede crear y almacenar artefactos B2B, tales como asociados comerciales, acuerdos, mapas, esquemas, certificados, etc. Para usar estos artefactos, asocie las aplicaciones lógicas con la cuenta de integración. Si actualmente usa BizTalk Server, estos conectores le resultará familiares ya.

|   |   |   |   | 
|---|---|---|---| 
| [![Icono de API][as2-icon]<br/>**Descodificación de</br> AS2**][as2-decode-doc] | [![Icono de API][as2-icon]<br/>**Codificación de</br> AS2**][as2-encode-doc] | [![Icono de API][edifact-icon]<br/>**Descodificación de</br> EDIFACT**][edifact-decode-doc] | [![Icono de API][edifact-icon]<br/>**Codificación de</br> EDIFACT**][edifact-encode-doc] | 
| [![Icono de API][flat-file-decode-icon]<br/>**Descodificación de</br> archivos planos**][flat-file-decode-doc] | [![Icono de API][flat-file-encode-icon]<br/>**Codificación de</br> archivos planos**][flat-file-encode-doc] | [![Icono de API][integration-account-icon]<br/>**Cuenta de<br/>integración**][integration-account-doc] | [![Icono de la API][liquid-icon]<br/>**Transformaciones de**</br>**Liquid**][json-liquid-transform-doc] | 
| [![Icono de API][x12-icon]<br/>**Descodificación de</br> X12**][x12-decode-doc] | [![Icono de API][x12-icon]<br/>**Codificación de</br> X12**][x12-encode-doc] | [![Icono de la API][xml-transform-icon]<br/>**Transformaciones de**</br>**XML**][xml-transform-doc] | [![Icono de API][xml-validate-icon]<br/>**Validación de <br/>XML**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Conectores de empresa

Las aplicaciones lógicas pueden acceder a los sistemas empresariales, tales como SAP y IBM MQ:

|   |   | 
|---|---| 
| [![Icono de API][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![Icono de API][sap-icon]<br/>**SAP**][sap-connector-doc] |
||| 

## <a name="more-about-triggers-and-actions"></a>Más información acerca de acciones y desencadenadores

Algunos conectores proporcionan *desencadenadores* que envían una notificación a la aplicación lógica cuando se producen determinados eventos. Cuando se producen estos eventos, el desencadenador crea y ejecuta una instancia de la aplicación lógica. Por ejemplo, el conector FTP tiene el desencadenador "Cuando se agrega o se modifica un archivo", que inicia la aplicación lógica cuando se actualiza un archivo. 

Logic Apps proporciona estos tipos de desencadenadores:  

* *Desencadenadores de sondeo*: estos desencadenadores sondean el servicio que proceda a una frecuencia especificada para comprobar si hay nuevos datos. 

  Cuando hay nuevos datos, se crea una nueva instancia de la aplicación lógica y se ejecuta con los datos pasados como entrada. 

* *Desencadenadores push*: estos desencadenadores escuchan datos nuevos en un punto de conexión o esperan a que se produzca un evento, que desencadena y ejecuta una nueva instancia de su aplicación lógica.

* *Desencadenador Periodicidad*: este desencadenador crea una instancia de la aplicación lógica según una programación especificada.

Los conectores también proporcionan *acciones* que realizan tareas en el flujo de trabajo de la aplicación lógica. Por ejemplo, la aplicación lógica puede leer datos y, después, usarlos en pasos posteriores de la aplicación lógica. Más concretamente, la aplicación lógica puede buscar los datos del cliente en una base de datos SQL y procesar estos datos más adelante en el flujo de trabajo de la aplicación lógica. 

Para más información sobre los desencadenadores y las acciones, consulte [Introducción a los conectores](connectors-overview.md). 

## <a name="custom-apis-and-connectors"></a>Conectores y API personalizadas 

Para llamar a las API que ejecutan código personalizado o que no están disponibles como conectores, puede extender la plataforma de Logic Apps mediante la [creación de aplicaciones de API personalizadas](../logic-apps/logic-apps-create-api-app.md). También puede [crear conectores personalizados](../logic-apps/custom-connector-overview.md) para *cualquier* API REST o SOAP, para que esas API estén disponibles para cualquier aplicación lógica en su suscripción de Azure.
Para que las aplicaciones de API o los conectores personalizados sean públicos y cualquier persona pueda usarlos en Azure, [envíe los conectores para que Microsoft los certifique](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Vote las ideas que encontrará en el [sitio de comentarios de usuario de Azure Logic Apps](http://aka.ms/logicapps-wish) o envíe ideas nuevas.

* ¿En los documentos faltan artículos o datos que considera importantes? Si es así, puede ayudar aportando a los artículos existentes o creando los suyos propios. La documentación es de código abierto y se hospeda en GitHub. Empiece a trabajar en el [repositorio de GitHub](https://github.com/Microsoft/azure-docs) de la documentación de Azure. 

## <a name="next-steps"></a>Pasos siguientes

* Búsqueda de la [lista completa de conectores](https://docs.microsoft.com/connectors)
* [Creación de una nueva aplicación lógica mediante la conexión de servicios de SaaS](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Creación de conectores personalizados para aplicaciones lógicas](https://docs.microsoft.com/connectors/custom-connectors/)
* [Creación de API personalizadas para las aplicaciones lógicas](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Conexión a orígenes de datos locales desde aplicaciones lógicas con una puerta de enlace de datos local"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Integración de aplicaciones lógicas con Azure Functions"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Creación de una instancia del servicio Azure API Management para administrar y publicar sus API"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integrar aplicaciones lógicas con App Service API Apps"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Envío de mensajes desde los temas y las colas de Service Bus y recepción de mensajes desde suscripciones y colas de Service Bus"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Procesamiento de mensajes en grupos o lotes"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Evaluar una condición y ejecutar acciones diferentes según si la condición es true o false"
[delay-doc]: ./connectors-native-delay.md "Realizar acciones diferidas"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Realizar las mismas acciones en todos los elementos de una matriz"
[http-doc]: ./connectors-native-http.md "Realizar llamadas HTTP con el conector HTTP"
[http-request-doc]: ./connectors-native-reqres.md "Agregar acciones para las solicitudes HTTP y respuestas a las aplicaciones lógicas"
[http-response-doc]: ./connectors-native-reqres.md "Agregar acciones para las solicitudes HTTP y respuestas a las aplicaciones lógicas"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Realizar llamadas HTTP con el conector HTTP y Swagger"
[http-webook-doc]: ./connectors-native-webhook.md "Agregar desencadenadores y acciones de webhook HTTP para las aplicaciones lógicas"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Integrar aplicaciones lógicas con flujos de trabajo anidados"
[query-doc]: ./connectors-native-query.md "Seleccionar y filtrar matrices con la acción Consulta"
[recurrence-doc]:  ./connectors-native-recurrence.md "Desencadenar acciones que se repiten para aplicaciones lógicas"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Organizar las acciones en grupos, que obtienen su propio estado después de que las acciones del grupo terminen de ejecutarse" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organizar las acciones en los casos, a los que se asignan valores únicos. Ejecutar solo el caso cuyo valor asignado coincida con el resultado de una expresión, objeto o token. Si no hay coincidencias, ejecutar el caso predeterminado"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Detener o cancelar un flujo de trabajo que se está ejecutando activamente para la aplicación lógica"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Repetir las acciones hasta que la condición especificada sea true o cambie algún estado"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Realizar operaciones con variables, por ejemplo, inicializar, establecer, incrementar, reducir y anexar a la variable de cadena o matriz"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Administrar archivos del contenedor de blobs con el conector de Azure Blob Storage"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Supervisar los eventos publicados por Event Grid, por ejemplo, cuando cambian los recursos de Azure o los recursos de terceros"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Conexión a Azure Event Hubs. Recibir y enviar eventos entre instancias de Logic Apps y Event Hubs"
[box-doc]: ./connectors-create-api-box.md "Conectar con Box. Cargar, obtener, eliminar y enumerar archivos, entre otras operaciones."
[dropbox-doc]: ./connectors-create-api-dropbox.md "Conectar con Dropbox. Cargar, obtener, eliminar y enumerar archivos, entre otras operaciones."
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Conectar con Dynamics CRM Online para trabajar con datos de CRM Online"
[facebook-doc]: ./connectors-create-api-facebook.md "Conectar con Facebook. Publicar en una línea de tiempo, obtener una fuente de páginas y más."
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Conectar con un sistema de archivos local"
[ftp-doc]: ./connectors-create-api-ftp.md "Conectar con un servidor FTP/FTPS para tareas de FTP, incluidas la carga, la obtención y la eliminación de archivos, entre otras"
[github-doc]: ./connectors-create-api-github.md "Conectar con GitHub y realizar un seguimiento de los problemas"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Conecta con Google Calendar y puede administrar el calendario."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Conectar con Google Drive para trabajar con sus datos"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Conectar con Google Sheets para poder modificar sus hojas"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Conectar con Google Tasks para administrar las tareas"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Conectar con una instancia de IBM DB2 en la nube o local. Actualizar una fila, obtener una tabla, etc."
[ibm-informix-doc]: ./connectors-create-api-informix.md "Conectar con una instancia de Informix en la nube o local. Leer una fila, enumerar las tablas, etc."
[ibm-mq-doc]: ./connectors-create-api-mq.md "Conectarse a IBM MQ local o en Azure y enviar y recibir mensajes"
[instagram-doc]: ./connectors-create-api-instagram.md "Conectar con Instagram. Desencadenar eventos o realizar acciones en ellos"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Conectar con su cuenta de MailChimp. Administrar y automatizar los mensajes de correo"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Conectar con Mandrill para realizar comunicaciones"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Conectar con Microsoft Translator. Traducir texto, detectar idiomas, etc." 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Conectar con su cuenta de Office 365. Enviar y recibir correos electrónicos, administrar su calendario y los contactos, y más"
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Obtener información de vídeo, listas de vídeo y canales, y direcciones URL de reproducción de vídeos de Office 365"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Conectar con su instancia personal de Microsoft OneDrive. Cargar, eliminar y enumerar archivos, entre otras tareas"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Conectar con su instancia de empresa de Microsoft OneDrive. Cargar, eliminar y enumerar archivos, entre otras tareas"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Conectarse a una base de datos de Oracle para agregar, insertar, eliminar filas y muchas otras acciones"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Conectar con el buzón de Outlook. Administrar el correo electrónico, los calendarios y los contactos, entre otras tareas"
[project-online-doc]: ./connectors-create-api-projectonline.md "Conectar con Microsoft Project Online. Administrar proyectos, tareas, recursos etc."
[rss-doc]: ./connectors-create-api-rss.md "Publicar y recuperar elementos de fuente, desencadenar operaciones cuando se publica un nuevo elemento en una fuente RSS."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Conectar con su cuenta de Salesforce. Administrar cuentas, clientes potenciales, oportunidades, etc."
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Conectar con un servidor SAP local"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Conectar con SendGrid. Enviar correo electrónico y administrar listas de destinatarios"
[sftp-doc]: ./connectors-create-api-sftp.md "Conectar con su cuenta SFTP. Cargar, obtener y eliminar archivos, entre otras operaciones."
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Conectarse al servidor local de SharePoint. Administrar documentos, elementos de lista, etc."
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Conectar con SharePoint Online. Administrar documentos, elementos de lista, etc."
[slack-doc]: ./connectors-create-api-slack.md "Conectar con Slack y publicar mensajes en los canales de Slack"
[smtp-doc]: ./connectors-create-api-smtp.md "Conectar con un servidor SMTP y enviar correo con datos adjuntos"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Conectar con SparkPost para la comunicación"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Conectar con Azure SQL Database o SQL Server. Crear, actualizar, obtener y eliminar entradas en una tabla de base de datos SQL."
[trello-doc]: ./connectors-create-api-trello.md "Conectar con Trello. Administrar proyectos y organizar todo con todos"
[twilio-doc]: ./connectors-create-api-twilio.md "Conectar con Twilio. Enviar y recibir mensajes, obtener los números disponibles, administrar los números de teléfono entrantes y mucho más."
[twitter-doc]: ./connectors-create-api-twitter.md "Conectar con Twitter. Obtener líneas de tiempo, publicar twits, etc."
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Conectar con Wunderlist. Administrar tareas y listas de tareas pendientes, mantenerse sincronizado, y mucho más"
[yammer-doc]: ./connectors-create-api-yammer.md "Conectar con Yammer. Publicar mensajes, obtener nuevos mensajes, etc."
[youtube-doc]: ./connectors-create-api-youtube.md "Conectar con YouTube. Administrar sus vídeos y canales"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Más información sobre AS2 para integración empresarial."
[as2-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Más información sobre la descodificación AS2 para integración empresarial"
[as2-encode-doc]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Más información sobre la codificación AS2 para integración empresarial"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Más información sobre la descodificación EDIFACT para integración empresarial"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Más información sobre la codificación EDIFACT para integración empresarial"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Más información sobre archivos planos para integración empresarial."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Más información sobre archivos planos para integración empresarial."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Búsqueda de esquemas, mapas o asociados en su cuenta de integración"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Más información acerca de las transformaciones JSON con Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Más información sobre X12 para integración empresarial."
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Más información sobre la descodificación X12 para integración empresarial"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Más información sobre la codificación X12 para integración empresarial"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Más información sobre transformaciones para integración empresarial."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Más información sobre validación XML para integración empresarial."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png