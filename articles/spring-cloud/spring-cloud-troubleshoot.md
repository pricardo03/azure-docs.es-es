---
title: Guía para la solución de problemas de Azure Spring Cloud | Microsoft Docs
description: Guía para la solución de problemas de Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: ebb960085691206b096090813636ef56366e6536
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038364"
---
# <a name="troubleshooting-guide-for-common-problems"></a>Guía para la solución de problemas comunes

En este artículo se especifican algunos problemas comunes y los pasos que los desarrolladores que trabajan en Azure Spring Cloud deben dar para solucionarlos. También es aconsejable leer el [artículo de preguntas frecuentes](spring-cloud-faq.md).

## <a name="availability-performance-and-application-issues"></a>Incidencias relacionadas con la disponibilidad, el rendimiento y las aplicaciones

### <a name="my-application-cannot-start-for-example-the-endpoint-cannot-be-connected-or-returns-502-after-few-retries"></a>No se puede iniciar la aplicación (por ejemplo, el punto de conexión no se puede conectar o devuelve el error 502 a los pocos reintentos)

Exporte los registros a _Azure Log Analytics_. La tabla de los registros de aplicaciones de Spring se denomina `AppPlatformLogsforSpring`. Para más información, visite [Análisis de registros y métricas con la configuración de diagnóstico](diagnostic-services.md)

Si el siguiente error se encuentra en sus registros es muy probable que tenga uno de estos dos problemas:

`org.springframework.context.ApplicationContextException: Unable to start web server`

* Falta uno de los beans o una de sus dependencias.
* Falta una de las propiedades de bean o no es válida. En ese caso, es probable que vea `java.lang.IllegalArgumentException`.

Los enlaces de servicios también pueden provocar errores al iniciarse la aplicación. Use palabras clave relacionadas con los servicios enlazados para consultar los registros.  Por ejemplo, suponga que una aplicación tiene un enlace a una instancia de MySQL establecida en la hora del sistema local. Si la aplicación no se inicia, puede aparecer el siguiente error en el registro:

`java.sql.SQLException: The server time zone value 'Coordinated Universal Time' is unrecognized or represents more than one time zone.`

Para solucionar el error, vaya a la sección `server parameters` de la instancia de MySQL y cambie `time_zone` de `SYSTEM` a `+0:00`.


### <a name="my-application-crashes-or-throws-an-unexpected-error"></a>La aplicación se bloquea o genera un error inesperado

Al depurar los bloqueos de la aplicación, empiece por comprobar el estado de la ejecución y de detección de la aplicación. Vaya a _Administración de aplicaciones_ en Azure Portal para asegurarse de que todas las aplicaciones están _en ejecución_ y _activas_.

* Si el estado es _En ejecución_, pero el estado de detección no es _ACTIVO_, vaya a [My application cannot be registered](#my-application-cannot-be-registered) (Mi aplicación no se puede registrar).

* Si el estado de detección es _ACTIVO_, vaya a _Métricas_ para comprobar el estado de la aplicación. Compruebe las siguientes métricas:


  - `TomcatErrorCount` (_tomcat.global.error_): Aquí se contarán todas las excepciones de la aplicación Spring. Si el número es grande, vaya a _Azure Log Analytics_ para comprobar los registros de la aplicación.

  - `AppMemoryMax` (_jvm.memory.max_): La cantidad máxima de memoria disponible para la aplicación. Puede que no esté definida o que, si se ha definido, cambie con el tiempo. La cantidad de memoria usada y asignada siempre será menor o igual al valor máximo, si se definió. Sin embargo, se puede producir un error de asignación de memoria con `OutOfMemoryError` si intenta aumentar la memoria usada de modo que la memoria usada > memoria asignada, incluso si la memoria usada < = valor máximo. En tal caso, intente aumentar el tamaño máximo del montón mediante el parámetro `-Xmx`.

  - `AppMemoryUsed` (_jvm.memory.used_): La cantidad de memoria, en bytes, que se usa actualmente. En el caso de una aplicación Java de carga normal, esta serie de métricas forma un patrón de "sierra", donde el uso de memoria aumenta y disminuye continuamente con muchas pequeñas subidas y bajadas de repente y este patrón se repite. Esto se debe a la recolección de elementos no utilizados dentro de la máquina virtual Java, donde las acciones de recolección representan caídas en el patrón de "dientes de sierra".
    Esta métrica es importante para identificar incidencias en la memoria, como: * la explosión de memoria inicial, * un pico de asignación de memoria para una ruta de acceso lógica específica y * fugas de memoria graduales

  Para más información, visite [Métricas](spring-cloud-concept-metrics.md).

Vaya a [este artículo introductorio](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) para dar los primeros pasos con _Azure Log Analytics_.

### <a name="my-application-experiences-high-cpu-usage-or-high-memory-usage"></a>Mi aplicación experimenta un uso elevado de la CPU o la memoria

Si la aplicación experimenta un uso elevado de la CPU o de la memoria, sucede una de estas dos cosas:
* Todas las instancias de la aplicación experimentan un uso elevado de la CPU o de la memoria, o bien
* Algunas de las instancias de la aplicación experimentan un uso elevado de la CPU o de la memoria.

Para confirmar de qué situación se trata:

1. Vaya a _Métricas_ y seleccione `Service CPU Usage Percentage` o `Service Memory Used`,
2. Agregue un filtro `App=` para especificar qué aplicación quiere supervisar y
3. Divida las métricas por el valor de `Instance`.

Si todas las instancias experimentan un uso elevado de la CPU o memoria, debe escalar horizontalmente la aplicación o escalar verticalmente la CPU o la memoria. Para más detalles, vaya al artículo acerca del [escalado de aplicaciones](spring-cloud-tutorial-scale-manual.md)

Si algunas de las instancias experimentan un uso elevado de la CPU o de la memoria, compruebe el estado de las instancias y su estado de detección.

Para más información, visite [Métricas](spring-cloud-concept-metrics.md).

Si todas las instancias están en funcionamiento, vaya a _Azure Log Analytics_ para consultar los registros de la aplicación y examine la lógica del código para ver si alguna de ellas puede afectar al escalado de las particiones. Para más información, visite [Análisis de registros y métricas con la configuración de diagnóstico](diagnostic-services.md).

Vaya a [este artículo introductorio](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) para dar los primeros pasos con _Azure Log Analytics_. Consulte los registros mediante el [lenguaje de consulta Kusto](https://docs.microsoft.com/azure/kusto/query/).

### <a name="checklist-before-onboarding-your-spring-application-to-azure-spring-cloud"></a>Lista de comprobación antes de incorporar la aplicación Spring a Azure Spring Cloud

* La aplicación se puede ejecutar localmente con la versión del runtime de Java especificada.
* La configuración del entorno (CPU/RAM/instancias) cumple los requisitos mínimos que establece el proveedor de aplicaciones.
* Los elementos de configuración tienen los valores esperados. Para más información, consulte el artículo acerca de [Config Server](spring-cloud-tutorial-config-server.md).
* Las variables de entorno tienen los valores esperados.
* Los parámetros de JVM tienen los valores esperados.
* Se recomienda deshabilitar o quitar los componentes _Config Server_ y _Service Registry_ insertados del servicio Spring del paquete de aplicación.
* Si algún recurso de Azure se debe enlazar mediante _Enlaces de servicio_, asegúrese de que los recursos de destino estén en funcionamiento.

## <a name="configuration-and-management"></a>Configuración y administración

### <a name="i-encountered-a-problem-creating-an-azure-spring-cloud-service-instance"></a>Ha aparecido un problema al crear una instancia de servicio de Azure Spring Cloud

Al intentar aprovisionar una instancia de servicio de _Azure Spring Cloud_ desde el portal, Azure Spring Cloud realizará la validación automáticamente.

Sin embargo, si intenta aprovisionar la instancia de servicio de _Azure Spring Cloud_ mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) o una [plantilla de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/), compruebe lo siguiente:

* Que la suscripción está activa.
* Que _Azure Spring Cloud_ [admite](spring-cloud-faq.md) la ubicación.
* Que se ha creado el grupo de recursos de la instancia.
* Que el nombre del recurso se ajusta a la regla de nomenclatura (solo puede contener minúsculas, números y guiones. El primer carácter debe ser una letra. El último carácter debe ser una letra o un número. El valor debe tener entre 2 y 32 caracteres).

Si intenta aprovisionar la instancia de servicio de _Azure Spring Cloud_ mediante la plantilla de Resource Manager, vaya a https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates para comprobar la sintaxis de la plantilla.

El nombre de la instancia de servicio de _Azure Spring Cloud_ se usará para solicitar un nombre de subdominio en `azureapps.io`, por lo que se producirá un error en el aprovisionamiento si el nombre entra en conflicto con uno existente. Puede encontrar más detalles en los registros de actividad.

### <a name="i-cannot-deploy-a-jar-package"></a>No puedo implementar un paquete JAR

No se puede cargar el paquete JAR o de origen desde el portal ni desde la plantilla de Resource Manager.

Al implementar el paquete de aplicación mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), se sondeará periódicamente el progreso de la implementación y, al final, se mostrará el resultado de la implementación.

Si se interrumpe el sondeo, se puede seguir usando el siguiente comando para capturar los registros de implementación:

`az spring-cloud app show-deploy-log -n <app-name>`

Asegúrese de que la aplicación está empaquetada en el [formato jar ejecutable](https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html) correcto. En caso contrario, se mostrará un error similar al siguiente:

`Error: Invalid or corrupt jarfile /jar/38bc8ea1-a6bb-4736-8e93-e8f3b52c8714`

### <a name="i-cannot-deploy-a-source-package"></a>No puedo implementar un paquete de origen

No se puede cargar el paquete JAR o de origen desde el portal ni desde la plantilla de Resource Manager.

Al implementar el paquete de aplicación mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), se sondea periódicamente el progreso de la implementación y, al final, se muestra el resultado de la implementación.

Si se interrumpe el sondeo, se puede seguir usando el siguiente comando para capturar los registros de creación e implementación:

`az spring-cloud app show-deploy-log -n <app-name>`

Sin embargo, tenga en cuenta que una instancia de servicio de _Azure Spring Cloud_ no puede desencadenar más de un trabajo de compilación para un paquete de origen a la vez. Para más información, consulte [Implementación de una aplicación](spring-cloud-quickstart-launch-app-portal.md) y [Guía del entorno de ensayo](spring-cloud-howto-staging-environment.md).

### <a name="my-application-cannot-be-registered"></a>No se puede registrar mi aplicación

En la mayoría de los casos, esto sucede cuando las dependencias requeridas o la detección de servicios no están configuradas correctamente en el archivo POM. Una vez configuradas, el punto de conexión del servidor de Service Registry integrado se insertará como variable de entorno con la aplicación. A continuación, las aplicaciones se registrarán en el servidor de Service Registry y detectarán otros microservicios dependientes.

Espere al menos 2 minutos para que una instancia recién registrada empiece a recibir tráfico.

Si va a migrar a Azure una solución existente de Spring Cloud, asegúrese de que las instancias ad-hoc de _Service Registry_ y _Config Server_ se han quitado (o deshabilitado) para evitar conflictos con las instancias administradas proporcionadas por _Azure Spring Cloud_.

También puede consultar los registros de cliente de _Service Registry_ en _Azure Log Analytics_. Para más información, visite [Análisis de registros y métricas con la configuración de diagnóstico](diagnostic-services.md)

Vaya a [este artículo introductorio](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) para dar los primeros pasos con _Azure Log Analytics_. Consulte los registros mediante el [lenguaje de consulta Kusto](https://docs.microsoft.com/azure/kusto/query/).

### <a name="i-cannot-find-metrics-or-logs-for-my-application"></a>No encuentro las métricas ni los registros de la aplicación

Vaya a _Administración de la aplicación_ para asegurarse de que la aplicación está _en ejecución_ y _activa_.

Si puede ver las métricas de _JVM_, pero no las de _Tomcat_, compruebe si la dependencia `spring-boot-actuator` está habilitada en el paquete de aplicación y si arranca correctamente.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Si los registros de la aplicación se pueden archivar en una cuenta de almacenamiento, pero no se envían a _Azure Log Analytics_, compruebe si [configuró el área de trabajo correctamente](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace). Si usa un nivel gratis de _Azure Log Analytics_, tenga en cuenta que [este nivel no proporciona ningún Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_3/).