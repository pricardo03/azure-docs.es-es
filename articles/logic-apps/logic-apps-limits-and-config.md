---
title: Límites y configuración - Azure Logic Apps | Microsoft Docs
description: Valores de límites y configuración del servicio para Azure Logic Apps
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 8baca0fc46489a22d587ba6e742615b1da79c19a
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970234"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Información de límites y configuración para Azure Logic Apps

En este artículo, se describen los límites y los detalles de configuración para crear y ejecutar flujos de trabajo automatizados con Azure Logic Apps. Para Microsoft Flow, consulte [Límites y configuración en Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Límites de definición

Estos son los límites de una definición de aplicación lógica:

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| Acciones por flujo de trabajo | 500 | Para ampliar este límite, puede agregar flujos de trabajo anidados según sea necesario. |
| Niveles de anidamiento permitidos para acciones | 8 | Para ampliar este límite, puede agregar flujos de trabajo anidados según sea necesario. | 
| Flujos de trabajo por región y suscripción | 1000 | | 
| Desencadenadores por flujo de trabajo | 10 | Al trabajar en la vista Código, no en el diseñador | 
| Límite de cambio de ámbito de casos | 25 | | 
| Variables por flujo de trabajo | 250 | | 
| Caracteres por expresión | 8192 | | 
| Tamaño máximo de `trackedProperties` | 16 000 caracteres | 
| Nombre de `action` o `trigger` | 80 caracteres | | 
| Longitud de `description` | 256 caracteres | | 
| Máximo de `parameters` | 50 | | 
| Máximo de `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Límites de retención y duración de ejecución

Estos son los límites de ejecución de una única aplicación lógica:

| NOMBRE | Límite | Notas | 
|------|-------|-------| 
| Duración de la ejecución | 90 días | Para cambiar este límite, consulte [Cambio de la duración de ejecución](#change-duration). | 
| Retención de almacenamiento | 90 días a partir de la hora de inicio de la ejecución | Para cambiar este límite, consulte [Cambio de la retención de almacenamiento](#change-retention). | 
| Intervalo de periodicidad mínima | 1 segundo | | 
| Intervalo de periodicidad máxima | 500 días | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Cambio de la duración de ejecución y la retención de almacenamiento

Puede cambiar este límite a un valor de entre 7 y 90 días. En cambio, para superar el límite máximo, [póngase en contacto con el equipo de Logic Apps](mailto://logicappsemail@microsoft.com) para obtener ayuda con sus requisitos.

1. En Azure Portal, en el menú de la aplicación lógica, elija **Configuración del flujo de trabajo**. 

2. En **Opciones del entorno de ejecución**, en la lista **Retención del historial de ejecución en días** elija **Personalizado**. 

3. Escriba o arrastre el control deslizante hasta el número de días que quiera.

<a name="looping-debatching-limits"></a>

## <a name="looping-and-debatching-limits"></a>Límites de bucle y anulación de procesamiento por lotes

Estos son los límites de ejecución de una única aplicación lógica:

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| Iteraciones Until | 5.000 | | 
| Elementos ForEach | 100 000 | Puede usar la [acción de consulta](../connectors/connectors-native-query.md) para filtrar matrices más grandes, según sea necesario. | 
| Paralelismo de ForEach | 50 | El valor predeterminado es 20. <p>Para cambiar este nivel predeterminado en un bucle ForEach, establezca la propiedad `runtimeConfiguration` en la acción `foreach`. <p>Para ejecutar secuencialmente un bucle ForEach, establezca la propiedad `operationOptions` en "Sequential" en la acción `foreach`. | 
| Elementos SplitOn | 100 000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Límites de rendimiento

Estos son los límites de ejecución de una única aplicación lógica:

| NOMBRE | Límite | Notas | 
| ----- | ----- | ----- | 
| Ejecuciones de acciones cada 5 minutos | 100 000 | Para aumentar el límite a 300 000, puede ejecutar una aplicación de lógica en modo `High Throughput`. Para configurar el modo de alto rendimiento, en `runtimeConfiguration` del recurso de flujo de trabajo, establezca la propiedad `operationOptions` en `OptimizedForHighThroughput`. <p>**Nota**: El modo de alto rendimiento se encuentra en versión preliminar. Además, puede distribuir una carga de trabajo entre varias aplicaciones, según sea necesario. | 
| Llamadas salientes simultáneas de acciones | Aproximadamente 2500 | Reduzca el número de solicitudes simultáneas o reduzca la duración según sea necesario. | 
| Punto de conexión en tiempo de ejecución: llamadas entrantes simultáneas | ~1000 | Reduzca el número de solicitudes simultáneas o reduzca la duración según sea necesario. | 
| Punto de conexión en tiempo de ejecución: llamadas de lectura por 5 minutos  | 60 000 | Puede distribuir una carga de trabajo entre varias aplicaciones, según sea necesario. | 
| Punto de conexión en tiempo de ejecución: llamadas de invocación por 5 minutos| 45 000 | Puede distribuir una carga de trabajo entre varias aplicaciones, según sea necesario. | 
|||| 

Si quiere superar estos límites en el procesamiento normal, o ejecutar pruebas de carga que puedan superar estos límites, [póngase en contacto con el equipo de Logic Apps](mailto://logicappsemail@microsoft.com) para que podamos ayudarle con sus solicitudes.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>Límites de solicitudes HTTP

Estos son los límites de una única solicitud HTTP o llamada de conector sincrónica:

#### <a name="timeout"></a>Tiempo de espera

Algunas operaciones de conector realizan llamadas asincrónicas o escuchan las solicitudes de webhook, por lo que el tiempo de expiración de estas operaciones puede ser superior a estos límites. Para obtener más información, vea los detalles técnicos del conector concreto y también [Acciones y desencadenadores de flujos de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| Solicitud saliente | 120 segundos | Para las operaciones de ejecución más largas, use un [patrón de sondeo asincrónico](../logic-apps/logic-apps-create-api-app.md#async-pattern) o un [bucle Until](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Respuesta sincrónica | 120 segundos | Para que la solicitud original obtenga la respuesta, todos los pasos de la respuesta deben terminar dentro del límite, a menos que llame a otra aplicación lógica como un flujo de trabajo anidado. Para obtener más información, consulte [Llamada, desencadenamiento o anidación de aplicaciones lógicas](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>Tamaño del mensaje

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| Tamaño del mensaje | 100 MB | Para evitar este límite, consulte [Handle large messages with chunking](../logic-apps/logic-apps-handle-large-messages.md) (Controlar mensajes grandes con fragmentación). En cambio, puede que algunos conectores y API no admitan la fragmentación ni el límite predeterminado. | 
| Tamaño del mensaje con fragmentación | 1 GB | Este límite se aplica a las acciones que admiten la fragmentación de forma nativa o que le permiten habilitar la opción de fragmentación en la configuración del entorno de ejecución. Para obtener más información, consulte [Handle large messages with chunking](../logic-apps/logic-apps-handle-large-messages.md) (Controlar mensajes grandes con fragmentación). | 
| Límite de evaluación de expresiones | 131 072 caracteres. | Las expresiones `@concat()`, `@base64()` y `@string()` no pueden superar este límite. | 
|||| 

#### <a name="retry-policy"></a>Directiva de reintentos

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| Número de reintentos | 90 | El valor predeterminado es 4. Para cambiar el valor predeterminado, use el [parámetro de directiva de reintentos](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Retraso máximo entre reintentos | 1 día | Para cambiar el valor predeterminado, use el [parámetro de directiva de reintentos](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Retraso mínimo entre reintentos | 5 segundos | Para cambiar el valor predeterminado, use el [parámetro de directiva de reintentos](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Límites de conector personalizado

Estos son los límites para los conectores personalizados que puede crear a partir de las API web.

| NOMBRE | Límite | 
| ---- | ----- | 
| Número de conectores personalizados | 1000 por cada suscripción de Azure | 
| Número de solicitudes por minuto para cada conexión creada por un conector personalizado | 500 solicitudes por conexión |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Límites de cuenta de integración

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Límites de artefacto por cuenta de integración

Estos son los límites en cuanto al número de artefactos de cada cuenta de integración. Para obtener más información, consulte [Precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). 

*Nivel Gratis*

Use el nivel gratuito solo en escenarios de exploración, y no en escenarios de producción. Este nivel restringe el uso y el rendimiento, y no tiene ningún acuerdo de nivel de servicio (SLA).

| Artefacto | Límite | Notas | 
|----------|-------|-------| 
| Socios comerciales de EDI | 25 | | 
| Acuerdos comerciales de EDI | 10 | | 
| Mapas | 25 | | 
| Esquemas | 25 | 
| Ensamblados | 10 | | 
| Configuraciones por lotes | 5 | 
| Certificados | 25 | | 
|||| 

*Nivel básico*

| Artefacto | Límite | Notas | 
|----------|-------|-------| 
| Socios comerciales de EDI | 2 | | 
| Acuerdos comerciales de EDI | 1 | | 
| Mapas | 500 | | 
| Esquemas | 500 | 
| Ensamblados | 25 | | 
| Configuraciones por lotes | 1 | | 
| Certificados | 2 | | 
|||| 

*Nivel estándar*

| Artefacto | Límite | Notas | 
|----------|-------|-------| 
| Socios comerciales de EDI | 500 | | 
| Acuerdos comerciales de EDI | 500 | | 
| Mapas | 500 | | 
| Esquemas | 500 | 
| Ensamblados | 50 | | 
| Configuraciones por lotes | 5 |  
| Certificados | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Límites de capacidad de artefactos

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| Esquema | 8 MB | Para cargar archivos mayores de 2 MB, use el [URI del blob](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Asignación (archivo XSLT) | 2 MB | | 
| Punto de conexión en tiempo de ejecución: llamadas de lectura por 5 minutos | 60 000 | Puede distribuir la carga de trabajo entre varias cuentas según sea necesario. | 
| Punto de conexión en tiempo de ejecución: llamadas de invocación por 5 minutos | 45 000 | Puede distribuir la carga de trabajo entre varias cuentas según sea necesario. | 
| Punto de conexión en tiempo de ejecución: llamadas de seguimiento por 5 minutos | 45 000 | Puede distribuir la carga de trabajo entre varias cuentas según sea necesario. | 
| Punto de conexión en tiempo de ejecución: llamadas simultáneas de bloqueo | ~1000 | Puede reducir el número de solicitudes simultáneas o la duración según sea necesario. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Tamaño de mensaje de protocolo B2B (AS2, X12 y EDIFACT)

Estos son los límites que se aplican a los protocolos B2B:

| NOMBRE | Límite | Notas | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Se aplica a la decodificación y la codificación | 
| X12 | 50 MB | Se aplica a la decodificación y la codificación | 
| EDIFACT | 50 MB | Se aplica a la decodificación y la codificación | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Configuración: direcciones IP

### <a name="azure-logic-apps-service"></a>Servicio Azure Logic Apps

Todas las aplicaciones lógicas de una región usan el mismo intervalo de direcciones IP. Para admitir las llamadas que las aplicaciones lógicas realizan directamente con [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) y otras solicitudes HTTP, establezca las opciones de configuración del firewall para que incluyan estas direcciones IP entrantes y salientes, en función del lugar donde estén las aplicaciones lógicas:

| Región de Logic Apps | IP de salida |
|-------------------|-------------|
| Este de Australia | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Sudeste de Australia | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
| Sur de Brasil | 191.235.82.221, 191.235.91.7, 191.234.182.26 |
| Centro de Canadá | 52.233.29.92, 52.228.39.241, 52.228.39.244 |
| Este de Canadá | 52.232.128.155, 52.229.120.45, 52.229.126.25 |
| India Central | 52.172.154.168, 52.172.186.159, 52.172.185.79 |
| Centro de EE. UU. | 13.67.236.125, 104.208.25.27, 40.122.170.198 |
| Asia oriental | 13.75.94.173, 40.83.127.19, 52.175.33.254 |
| Este de EE. UU | 13.92.98.111, 40.121.91.41, 40.114.82.191 |
| Este de EE. UU. 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174 |
| Este de Japón | 13.71.158.3, 13.73.4.207, 13.71.158.120 |
| Oeste de Japón | 40.74.140.4, 104.214.137.243, 138.91.26.45 |
| Centro-Norte de EE. UU | 168.62.248.37, 157.55.210.61, 157.55.212.238 |
| Europa del Norte | 40.113.12.95, 52.178.165.215, 52.178.166.21 |
| Centro-Sur de EE. UU | 104.210.144.48, 13.65.82.17, 13.66.52.232 |
| Sur de la India | 52.172.50.24, 52.172.55.231, 52.172.52.0 |
| Sudeste asiático | 13.76.133.155, 52.163.228.93, 52.163.230.166 |
| Centro occidental de EE.UU. | 52.161.27.190, 52.161.18.218, 52.161.9.108 |
| Europa occidental | 40.68.222.65, 40.68.209.23, 13.95.147.65 |
| Oeste de la India | 104.211.164.80, 104.211.162.205, 104.211.164.136 |
| Oeste de EE. UU. | 52.160.92.112, 40.118.244.241, 40.118.241.243 |
| Oeste de EE. UU. 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132 |
| Sur de Reino Unido 2 | 51.140.74.14, 51.140.73.85, 51.140.78.44 |
| Oeste de Reino Unido | 51.141.54.185, 51.141.45.238, 51.141.47.136 |
| | |

| Región de Logic Apps | IP entrante |
|-------------------|------------|
| Este de Australia | 3.75.153.66, 104.210.89.222, 104.210.89.244 |
| Sudeste de Australia | 13.73.115.153, 40.115.78.70, 40.115.78.237 |
| Sur de Brasil | 191.235.86.199, 191.235.95.229, 191.235.94.220 |
| Centro de Canadá | 13.88.249.209, 52.233.30.218, 52.233.29.79 |
| Este de Canadá | 52.232.129.143, 52.229.125.57, 52.232.133.109 |
| India Central | 52.172.157.194, 52.172.184.192, 52.172.191.194 |
| Centro de EE. UU. | 13.67.236.76, 40.77.111.254, 40.77.31.87 |
| Asia oriental | 168.63.200.173, 13.75.89.159, 23.97.68.172 |
| Este de EE. UU | 137.135.106.54, 40.117.99.79, 40.117.100.228 |
| Este de EE. UU. 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136 |
| Este de Japón | 13.71.146.140, 13.78.84.187, 13.78.62.130 |
| Oeste de Japón | 40.74.140.173, 40.74.81.13, 40.74.85.215 |
| Centro-Norte de EE. UU | 168.62.249.81, 157.56.12.202, 65.52.211.164 |
| Europa del Norte | 13.79.173.49, 52.169.218.253, 52.169.220.174 |
| Centro-Sur de EE. UU | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Sur de la India | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Sudeste asiático | 52.163.93.214, 52.187.65.81, 52.187.65.155 |
| Centro occidental de EE.UU. | 52.161.26.172, 52.161.8.128, 52.161.19.82 |
| Europa occidental | 13.95.155.53, 52.174.54.218, 52.174.49.6 |
| Oeste de la India | 104.211.164.112, 104.211.165.81, 104.211.164.25 |
| Oeste de EE. UU. | 52.160.90.237, 138.91.188.137, 13.91.252.184 |
| Oeste de EE. UU. 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| Sur de Reino Unido 2 | 51.140.79.109, 51.140.78.71, 51.140.84.39 |
| Oeste de Reino Unido | 51.141.48.98, 51.141.51.145, 51.141.53.164 |
| | |

### <a name="connectors"></a>Conectores

Para admitir las llamadas que realizan los [conectores](../connectors/apis-list.md), establezca las opciones de configuración del firewall para que incluyan estas direcciones IP salientes, en función de las regiones donde estén las aplicaciones lógicas.

> [!IMPORTANT]
>
> Si ya tiene algunas opciones de configuración, actualícelas **tan pronto como le sea posible antes del 1 de septiembre de 2018** para que incluyan y coincidan con las direcciones IP de esta lista de las regiones donde están las aplicaciones lógicas. 

| Región de Logic Apps | IP de salida | 
|-------------------|-------------|  
| Este de Australia | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213 | 
| Sudeste de Australia | 13.77.50.240 - 13.77.50.255, 13.70.136.174, 40.127.80.34 | 
| Sur de Brasil | 191.233.203.192 - 191.233.203.207, 104.41.59.51, 191.232.38.129 | 
| Centro de Canadá | 13.71.170.208 - 13.71.170.223, 13.71.170.224 - 13.71.170.239, 52.237.24.126, 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13 | 
| Este de Canadá | 40.69.106.240 - 40.69.106.255, 52.242.35.152, 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52 | 
| India Central | 104.211.81.192 - 104.211.81.207, 52.172.211.12, 104.211.98.164 | 
| Centro de EE. UU. | 13.89.171.80 - 13.89.171.95, 52.173.245.164, 40.122.49.51 | 
| Asia oriental | 13.75.36.64 - 13.75.36.79, 52.175.23.169, 23.99.116.181 | 
| Este de EE. UU | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 191.237.41.52 | 
| Este de EE. UU. 2 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100 | 
| Este de Japón | 13.78.108.0 - 13.78.108.15, 13.71.153.19, 40.115.186.96 | 
| Oeste de Japón | 40.74.100.224 - 40.74.100.239, 104.215.61.248, 40.74.130.77 | 
| Centro-Norte de EE. UU | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230 | 
| Europa del Norte | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9 | 
| Centro-Sur de EE. UU | 104.214.19.48 - 104.214.19.63, 13.65.86.57, 104.214.70.191 | 
| Sur de la India | 40.78.194.240 - 40.78.194.255, 13.71.125.22, 104.211.227.225 | 
| Sudeste asiático | 13.67.8.240 - 13.67.8.255, 52.187.68.19, 13.76.231.68 | 
| Centro occidental de EE.UU. | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 52.161.27.108, 52.161.30.5, 52.161.29.35, 52.161.26.212 | 
| Europa occidental | 13.69.64.208 - 13.69.64.223, 52.174.88.118, 40.115.50.13 | 
| Oeste de la India | 104.211.146.224 - 104.211.146.239, 104.211.189.218, 104.211.161.203 | 
| Oeste de EE. UU. | 40.112.243.160 - 40.112.243.175, 104.42.122.49, 104.40.51.248 | 
| Oeste de EE. UU. 2 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 13.66.225.219, 13.66.218.78, 13.66.220.135, 13.66.219.14 | 
| Sur de Reino Unido 2 | 51.140.148.0 - 51.140.148.15, 51.140.80.51, 51.140.80.51 | 
| Oeste de Reino Unido | 51.140.211.0 - 51.140.211.15, 51.141.47.105, 51.141.47.105 | 
| | | 

## <a name="next-steps"></a>Pasos siguientes  

* Aprenda a [crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Obtenga información acerca de [ejemplos y escenarios comunes](../logic-apps/logic-apps-examples-and-scenarios.md)
