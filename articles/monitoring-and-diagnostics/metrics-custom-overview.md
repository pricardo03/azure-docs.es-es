---
title: Métricas personalizadas en Azure Monitor
description: Obtenga información sobre las métricas personalizadas en Azure Monitor y cómo se modelan.
author: ancav
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 1bdf1e1f5e58ecb0939d5876e0cef349e32de517
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344765"
---
# <a name="custom-metrics-in-azure-monitor"></a>Métricas personalizadas en Azure Monitor

A medida que implementa recursos y aplicaciones en Azure, querrá empezar a recopilar datos de telemetría para obtener conclusiones detalladas sobre su rendimiento y mantenimiento. Azure pone algunas métricas a su disposición de manera estándar a medida que implementa recursos. Se denominan métricas estándar o de plataforma. Sin embargo, estas métricas son limitadas por naturaleza. Es posible que quiera recopilar algunos indicadores de rendimiento personalizados o métricas específicas del negocio para obtener conclusiones más detalladas.
Estas métricas "personalizadas" pueden recopilarse a través de los datos de telemetría de aplicación, un agente que se ejecuta en sus recursos de Azure o, incluso, fuera del sistema de supervisión y enviarse directamente a Azure Monitor. Una vez publicado en Azure Monitor, puede examinar, consultar y generar una alerta sobre métricas personalizadas para sus recursos y aplicaciones de Azure en paralelo con las métricas estándar emitidas por Azure.

## <a name="send-custom-metrics"></a>Envío de métricas personalizadas
Las métricas personalizadas pueden enviarse a Azure Monitor a través de distintos métodos.
- Instrumente la aplicación con el SDK de Application Insights y envíe datos de telemetría personalizados a Azure Monitor. 
- Instalar la extensión de diagnóstico de Windows en su [VM de Azure](metrics-store-custom-guestos-resource-manager-vm.md), [conjunto de escalado de máquinas virtuales](metrics-store-custom-guestos-resource-manager-vmss.md), [VM clásica](metrics-store-custom-guestos-classic-vm.md) o [servicio en la nube clásico](metrics-store-custom-guestos-classic-cloud-service.md) y envíe los contadores de rendimiento a Azure Monitor 
- Instale el [agente InfluxDB Telegraf](metrics-store-custom-linux-telegraf.md) en su VM Linux de Azure y envíe las métricas mediante el complemento de salida de Azure Monitor.
- Envíe las métricas personalizadas [directamente a la API REST de Azure Monitor](metrics-store-custom-rest-api.md) https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics.

Al enviar las métricas personalizadas a Azure Monitor, cada punto de datos (o valor) notificado debe incluir la siguiente información:

### <a name="authentication"></a>Autenticación
Para enviar métricas personalizadas a Azure Monitor, la entidad que envía la métrica debe tener un token de Azure Active Directory válido en el encabezado de "portador" de la solicitud. Existen algunas formas admitidas para adquirir un token de portador válido:
1. [Identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview): proporciona una identidad a un recurso de Azure (por ejemplo, una máquina virtual). MSI se ha diseñado para otorgar permisos a los recursos para llevar a cabo ciertas operaciones, por ejemplo, permitir a un recurso emitir métricas sobre sí mismo. Un recurso (o su MSI) puede obtener permisos de "Supervisión del publicador de métricas" en otro recurso, lo que permite a MSI emitir métricas para los otros recursos también.
2. [Entidad de servicio de AAD](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals): el escenario aquí es que pueden asignarse permisos a una aplicación (servicio) de AAD para que emita métricas sobre un recurso de Azure.
Para autenticar la solicitud, Azure Monitor valida el token de aplicación mediante las claves públicas de AAD. El rol "Supervisión del publicador de métricas" existente ya tiene este permiso, que está disponible en Azure Portal. La entidad de servicio, según para qué recursos emitirá métricas personalizadas, puede recibir el rol "Supervisión del publicador de métricas" en el ámbito necesario (suscripción, grupo de recursos o recurso específico).

> [!NOTE]
> Al solicitar un token de AAD para emitir métricas personalizadas, asegúrese de que el público o el recurso para el que se solicita el token sea https://monitoring.azure.com/ (no olvide incluir la barra final "/").

### <a name="subject"></a>Asunto
Esta propiedad captura para cuál identificador de recurso de Azure se notifica la métrica personalizada. Esta información se codificarán en la dirección URL de la llamada API que se realiza. Cada API solo puede enviar los valores de métrica para un único recurso de Azure.

> [!NOTE]
> No se pueden emitir métricas personalizadas frente al identificador de recurso de un grupo de recursos o suscripción.
>
>

### <a name="region"></a>Region
Esta propiedad captura en qué región de Azure se implementa el recurso para el que se van a emitir las métricas. Las métricas se deben emitir al mismo punto de conexión regional de Azure Monitor que la región donde se implementa el recurso. Por ejemplo, las métricas personalizadas para una VM que se implementa en el Oeste de EE. UU. deben enviarse al punto de conexión de Azure Monitor regional WestUS. La información de región también está codificada en la dirección URL de la llamada API.

> [!NOTE]
> Durante la versión preliminar pública, las métricas personalizadas solo están disponibles en un subconjunto de las regiones de Azure. En una sección posterior de este artículo se incluye una lista de regiones admitidas.
>
>

### <a name="timestamp"></a>Timestamp
Cada punto de datos que se envía a Azure Monitor debe marcarse con una marca de tiempo. Esta marca de tiempo captura la fecha y hora en que se midió o recopiló el valor de la métrica. Azure Monitor aceptará datos de métrica con marcas de tiempo de hasta 20 minutos en el pasado y hasta 5 minutos en el futuro.

### <a name="namespace"></a>Espacio de nombres
Los espacios de nombres son una manera de clasificar o agrupar las métricas similares. Los espacios de nombres permiten conseguir el aislamiento entre los grupos de métricas que puedan estar recopilando diferentes conclusiones o indicadores de rendimiento. Por ejemplo, podría tener un espacio de nombres denominado *ContosoMemoryMetrics* que se use para un seguimiento de las métricas de uso de memoria que generan perfiles de la aplicación, y otro espacio de nombres denominado *ContosoAppTransaction* que realiza el seguimiento de todas las métricas sobre las transacciones de usuario en la aplicación.

### <a name="name"></a>NOMBRE
El nombre de la métrica que se está informando. Normalmente, el nombre es lo suficientemente descriptivo como para ayudar a identificar lo que se está midiendo. Por ejemplo, una métrica que mide el número de bytes de memoria que se utilizan en una VM determinada podría tener un nombre de métrica como "Bytes de memoria en uso".

### <a name="dimension-keys"></a>Claves de dimensión
Una dimensión es un par clave/valor que ayuda a describir las características adicionales sobre la métrica que se recopila. Las características adicionales permiten recopilar más información acerca de la métrica, lo que permite obtener conclusiones más detalladas. Por ejemplo, la métrica "Bytes de memoria en uso" podría tener una clave de dimensión denominada "Proceso", que capture el número de bytes de memoria que consume cada proceso en una VM. Esto le permite filtrar la métrica para ver cuánta memoria usan procesos específicos, o para identificar los 5 procesos principales por uso de memoria.
Cada métrica personalizada puede tener hasta 10 dimensiones.

### <a name="dimension-values"></a>Valores de dimensión
Al informar de un punto de datos de métrica, para cada clave de dimensión en la métrica notificada, hay un valor de dimensión correspondiente. Por ejemplo, si quisiera  informar de la memoria utilizada por ContosoApp en la VM:

* El nombre de la métrica sería *Bytes de memoria en uso*.
* La clave de dimensión sería *Proceso*.
* El valor de la dimensión sería *ContosoApp.exe*.

Al publicar un valor de métrica, solo puede especificar un único valor de dimensión por clave de dimensión. Si recopila el mismo uso de memoria para varios procesos en la VM, podría informar sobre varios valores de métricas para esa marca de tiempo. Cada valor de métrica especificaría un valor de dimensión diferente para la clave de dimensión Proceso.

### <a name="metric-values"></a>Valores de métrica
Azure Monitor almacena todas las métricas a intervalos de granularidad de un minuto. Somos conscientes de que una métrica puede tener que muestrearse varias veces (p. ej. Uso de CPU) o medirse para muchos eventos discretos (p. ej. latencias de transacción de inicio de sesión) durante un minuto determinado. Para limitar el número de valores sin procesar que tenga que emitir y pagar en Azure Monitor, puede agregar previamente de manera local y emitir los valores:

* Mínimo: el mínimo valor observado de todas las muestras/medidas durante el minuto.
* Máximo: el máximo valor observado de todas las muestras/medidas durante el minuto.
* Suma: el sumatorio de todos los valores observados de todas las muestras/medidas durante el minuto.
* Recuento: El número de muestras/mediciones tomadas durante el minuto.

Por ejemplo, si hubo 4 transacciones de inicio de sesión a la aplicación durante un minuto dado y las latencias medidas resultantes para cada una fueron:

|Transacción 1|Transacción 2|Transacción 3|Transacción 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

La publicación de métricas resultante en Azure Monitor sería:
* Mínimo: 4
* Máximo: 16
* Suma: 40
* Recuento: 4

Si la aplicación no puede agregar previamente de manea local y debe emitir cada muestra o evento discretos inmediatamente después de la recopilación, puede emitir los valores de medida sin procesar.
Por ejemplo, cada vez que se produjera una transacción de inicio de sesión en la aplicación, publicaría una métrica en Azure Monitor con solo una única medida. Por lo tanto, para una transacción de inicio de sesión que llevó 12 ms, la publicación de la métrica sería:
* Mínimo: 12
* Máximo: 12
* Suma: 12
* Recuento: 1

Este proceso le permite emitir varios valores para la misma combinación de métrica+dimensión durante un minuto dado. Azure Monitor luego tomará todos los valores sin procesar emitidos para un minuto dado y los agregará.

### <a name="sample-custom-metric-publication"></a>Publicación de métricas personalizadas de ejemplo
En el ejemplo siguiente, creará una métrica personalizada llamada "Memoria de bytes en uso", en el espacio de nombres de métrica "Perfil de memoria" para una máquina Virtual. La métrica tiene una única dimensión denominada "Proceso". Para la marca de tiempo dada, se emitirán valores de métricas para dos procesos diferentes:

```json
{
    "time": "2018-08-20T11:25:20-7:00",
    "data": {

      "baseData": {

        "metric": "Memory Bytes in Use",
        "namespace": "Memory Profile",
        "dimNames": [
          "Process"        ],
        "series": [
          {
            "dimValues": [
              "ContosoApp.exe"
            ],
            "min": 10,
            "max": 89,
            "sum": 190,
            "count": 4
          },
          {
            "dimValues": [
              "SalesApp.exe"
            ],
            "min": 10,
            "max": 23,
            "sum": 86,
            "count": 4
          }
        ]
      }
    }
  }
```
> [!NOTE]
> Application Insights, la extensión Microsoft Azure Diagnostics, y el agente InfluxData Telegraf ya están configurados para emitir los valores de métrica frente al punto de conexión regional correcto y transportar todas las propiedades anteriores en cada emisión.
>
>

## <a name="custom-metric-definitions"></a>Definiciones de métricas personalizadas
No hay ninguna necesidad de definir previamente una métrica personalizada en Azure Monitor antes de emitirla. Puesto que cada punto de datos de métrica publicado contiene la información de espacio de nombres, nombre y dimensión, la primera vez que se emite una métrica personalizada a Azure Monitor se crea automáticamente una definición de métrica. Esta definición de métricas luego puede detectarse en cualquier recurso frente al que se emitió la métrica a través de las definiciones de métricas.

> [!NOTE]
> Azure Monitor todavía no admite la definición de "Unidades" para una métrica personalizada.

## <a name="using-custom-metrics"></a>Uso de métricas personalizadas
Una vez que las métricas personalizadas se envían a Azure Monitor, puede examinarlas a través de Azure Portal, consultarlas a través de las API REST de Azure Monitor o crear alertas para ellas, de modo que pueda recibir una notificación cuando se cumplan ciertas condiciones.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Exploración de las métricas personalizadas a través de Azure Portal
1.  Vaya a [Azure Portal](https://portal.azure.com).
2.  Seleccione la hoja Supervisión.
3.  Haga clic en Métrica.
4.  Seleccione un recurso frente al cual ha emitido métricas personalizadas.
5.  Seleccione el espacio de nombres de métrica para la métrica personalizada.
6.  Seleccione la métrica personalizada.

## <a name="supported-regions"></a>Regiones admitidas
Durante la versión preliminar pública, la capacidad de publicar métricas personalizadas solo está disponible en un subconjunto de las regiones de Azure. Esto significa que las métricas solo pueden publicarse para recursos de una de las regiones admitidas. En la tabla siguiente se muestra el conjunto de regiones de Azure admitidas para las métricas personalizadas y el punto de conexión correspondiente donde deben publicarse las métricas para los recursos en estas regiones.

|Región de Azure|Prefijo del punto de conexión regional|
|---|---|
|Este de EE. UU|https://eastus.monitoring.azure.com/|
|Centro-Sur de EE. UU|https://southcentralus.monitoring.azure.com/|
|Centro occidental de EE.UU.|https://westcentralus.monitoring.azure.com/|
|Oeste de EE. UU. 2|https://westus2.monitoring.azure.com/|
|Sudeste asiático|https://southeastasia.monitoring.azure.com/|
|Europa del Norte|https://northeurope.monitoring.azure.com/|
|Europa occidental|https://westeurope.monitoring.azure.com/|

## <a name="quotas-and-limits"></a>Cuotas y límites
Azure Monitor impone los siguientes límites de uso a las métricas personalizadas.

|Categoría|Límite|
|---|---|
|Serie temporal activa/suscripciones/región|50.000|
|Claves de dimensión por métrica|10|
|Longitud de cadena de espacios de nombres de métricas, nombres de métricas, claves de dimensión y valores de dimensión|256 caracteres|
Una serie temporal activa se define como cualquier combinación única de métrica, clave de dimensión, valor de dimensión para la que se han publicado valores de métrica en las últimas 12 horas.

## <a name="next-steps"></a>Pasos siguientes
Usar métricas personalizadas de distintos servicios 
 - [Máquina virtual](metrics-store-custom-guestos-resource-manager-vm.md)
 - [Conjunto de escalado de máquinas virtuales](metrics-store-custom-guestos-resource-manager-vmss.md)
 - [Máquina virtual (clásica)](metrics-store-custom-guestos-classic-vm.md)
 - [Máquina virtual Linux que usa el agente Telegraf](metrics-store-custom-linux-telegraf.md)
 - [API DE REST](metrics-store-custom-rest-api.md)
 - [Servicio en la nube (clásico)](metrics-store-custom-guestos-classic-cloud-service.md)
 