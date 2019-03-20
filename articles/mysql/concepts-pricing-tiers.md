---
title: Planes de tarifa para Azure Database for MySQL
description: En este artículo se describen los planes de tarifa para Azure Database for MySQL.
author: jan-eng
ms.author: janeng
ms.service: mysql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 21ddeb9cb0621f0f11ca23a7b12a82df0b7aa8ca
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57546681"
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Planes de tarifa de Azure Database for MySQL

Puede crear un servidor de Azure Database for MySQL en tres planes de tarifa diferentes: Básico, De uso general y Optimizado para memoria. Los planes de tarifa se diferencian por la cantidad de proceso en núcleos virtuales que se puede aprovisionar, la cantidad de memoria por núcleo virtual y la tecnología de almacenamiento usada para almacenar los datos. Todos los recursos se aprovisionan en el nivel de servidor MySQL. Un servidor puede tener una o varias bases de datos.

|    | **Básico** | **Uso general** | **Memoria optimizada** |
|:---|:----------|:--------------------|:---------------------|
| Generación de procesos | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| Núcleos virtuales | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Memoria por núcleo virtual | 2 GB | 5 GB | 10 GB |
| Tamaño de almacenamiento | De 5 GB a 1 TB | De 5 GB a 4 TB | De 5 GB a 4 TB |
| Tipo de almacenamiento | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Período de retención de copias de seguridad de base de datos | De 7 a 35 días | De 7 a 35 días | De 7 a 35 días |

Para elegir un plan de tarifa, use la siguiente tabla como punto de partida.

| Plan de tarifa | Carga de trabajo objetivo |
|:-------------|:-----------------|
| Básica | Cargas de trabajo que requieren proceso y rendimiento de E/S ligeros. Algunos ejemplos son los servidores que se usan para desarrollo o prueba, o bien las aplicaciones a pequeña escala que se usan con poca frecuencia. |
| Uso general | La mayoría de las cargas de trabajo de empresa que requieren un equilibrio entre proceso y memoria con rendimiento de E/S escalable. Por ejemplo, servidores para hospedar aplicaciones web y móviles, y otras aplicaciones empresariales.|
| Memoria optimizada | Cargas de trabajo de base de datos de alto rendimiento que requieren rendimiento en memoria para un procesamiento de transacciones más rápido y una mayor simultaneidad. Por ejemplo, servidores para procesar datos en tiempo real y aplicaciones de análisis y transacciones de alto rendimiento.|

Después de crear un servidor, el número de núcleos virtuales, la generación de hardware y el plan de tarifa (excepto hacia y desde Básico) se puede aumentar o reducir en cuestión de segundos. También puede ajustar de forma independiente la cantidad de almacenamiento y aumentar o reducir el período de retención sin que las aplicaciones experimenten tiempo de inactividad. No puede cambiar el tipo de almacenamiento de copia de seguridad. Para más información, consulte la sección [Escalado de recursos](#scale-resources).

## <a name="compute-generations-and-vcores"></a>Procesar generaciones y núcleos virtuales

Los recursos de proceso se proporcionan como núcleos virtuales, que representan la CPU lógica del hardware subyacente. Actualmente, puede elegir entre dos generaciones de proceso, Gen 4 y Gen 5. Las CPU lógicas Gen 4 se basan en los procesadores Intel E5-2673 v3 (Haswell) de 2.4 GHz. Las CPU lógicas Gen 5 se basan en los procesadores Intel E5-2673 v4 (Broadwell) de 2.3 GHz. Las CPU lógicas Gen 4 y Gen 5 están disponibles en las siguientes regiones ("X" indica disponible).

> [!IMPORTANT]
> A partir del 12 de diciembre de 2018, los clientes nuevos no podrán aprovisionar servidores de proceso de cuarta generación en estas regiones: Sur de Brasil, Centro de Canadá, Este de Canadá, Asia Oriental, Este de EE. UU. 2, India central, India occidental, Oeste de la India, Centro-norte de EE. UU., Oeste de EE. UU. Los servidores de proceso de la cuarta generación anteriormente creados se migrarán a los de la quinta generación a partir del 1 de febrero de 2019 en estas regiones.
>
> [!IMPORTANT]
> A partir del 15 de febrero de 2019 nuevos clientes no podrán realizar aprovisionar servidores de proceso generación 4 en EE. UU., este de Estados Unidos, Japón oriental, Europa del Norte, sur de EE. UU, sudeste asiático, Europa occidental. Generación de procesos 4 servidores se migrarán para calcular la generación de 5 a partir del 1 de abril de 2019 en estas regiones se creó anteriormente.

| **Región de Azure** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| Centro de EE. UU. |  | X |
| Este de EE. UU |  | X |
| Este de EE. UU. 2 |  | X |
| Centro-Norte de EE. UU |  | X |
| Centro-Sur de EE. UU | X | X |
| Oeste de EE. UU. |  | X |
| Oeste de EE. UU. 2 |  | X |
| Sur de Brasil |  | X |
| Centro de Canadá |  | X |
| Este de Canadá |  | X |
| Europa del Norte | X | X |
| Europa occidental |  | X |
| Centro de Francia |  | X |
| Sur de Reino Unido 2 |  | X |
| Oeste de Reino Unido |  | X |
| Asia oriental |  | X |
| Sudeste asiático | X | X |
| Este de Australia |  | X |
| Centro de Australia |  | X |
| Centro de Australia 2 |  | X |
| Sudeste de Australia |  | X |
| India Central |  | X |
| Sur de la India |  | X |
| Oeste de la India |  | X |
| Este de Japón | X | X |
| Oeste de Japón |  | X |
| Corea Central |  | X |
| Corea del Sur |  | X |
| Este de China 1 | X |  |
| Este de China 2 |  | X |
| Norte de China 1 | X |  |
| Norte de China 2 |  | X |
| Centro de Alemania |  | X |
| Departamento de Defensa de EE. UU. Centro  | X |  |
| Departamento de Defensa de EE. UU. Este  | X |  |
| Gobierno de EE. UU.: Arizona |  | X |
| Gobierno de EE. UU.: Texas |  | X |
| Gobierno de EE. UU. - Virginia |  | X |

## <a name="storage"></a>Almacenamiento

El almacenamiento que se aprovisiona es la cantidad de capacidad de almacenamiento disponible para el servidor de Azure Database for MySQL. El almacenamiento se usa para los archivos de base de datos, los archivos temporales, los registros de transacciones y los registros del servidor MySQL. La cantidad total de almacenamiento que se aprovisiona también define la capacidad de E/S disponible para su servidor.

|    | **Básico** | **Uso general** | **Memoria optimizada** |
|:---|:----------|:--------------------|:---------------------|
| Tipo de almacenamiento | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Tamaño de almacenamiento | De 5 GB a 1 TB | De 5 GB a 4 TB | De 5 GB a 4 TB |
| Tamaño de incremento de almacenamiento | 1 GB | 1 GB | 1 GB |
| E/S | Variable |3 IOPS/GB<br/>100 IOPS mín.<br/>6000 IOPS máx. | 3 IOPS/GB<br/>100 IOPS mín.<br/>6000 IOPS máx. |

Puede agregar capacidad de almacenamiento adicional durante y después de la creación del servidor. El plan Básico no proporciona una garantía de IOPS. En los planes de tarifa Uso general y Memoria optimizada, el valor de IOPS se escala con el tamaño de almacenamiento aprovisionado en una proporción 3:1.

Puede supervisar el consumo de E/S en Azure Portal o mediante los comandos de la CLI de Azure. Las métricas pertinentes que se deben supervisar son el [límite de almacenamiento, el porcentaje de almacenamiento, el almacenamiento usado y el porcentaje de E/S](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Alcance del límite de almacenamiento

El servidor se marca como de solo lectura cuando el almacenamiento disponible se vuelve menor que 5 GB o, de ser inferior, el 5 % del almacenamiento aprovisionado. Por ejemplo, si ha aprovisionado 100 GB de almacenamiento, y el uso real supera los 95 GB, el servidor se marca como de solo lectura. O bien, si ha aprovisionado 5 GB de almacenamiento, el servidor se marca como de solo lectura cuando el almacenamiento disponible se vuelva inferior a 250 MB.  

Mientras el servicio intenta hacer que el servidor sea de solo lectura, se bloquean todas las nuevas solicitudes de transacción de escritura, y las transacciones activas existentes continuarán ejecutándose. Cuando el servidor se establece en solo lectura, todas las operaciones de escritura y confirmaciones de transacción posteriores generarán errores. Las consultas de lectura seguirán funcionando sin interrupciones. Después de aumentar el almacenamiento aprovisionado, el servidor estará listo para aceptar las transacciones de escritura de nuevo.

Recomendamos que configure una alerta que le envíe una notificación cada vez que su almacenamiento en servidor esté cerca del umbral para que pueda evitar entrar en el estado de solo lectura. Para obtener más información, consulte la documentación sobre [cómo configurar una alerta](howto-alert-on-metric.md).

## <a name="backup"></a>Copia de seguridad

El servicio realiza automáticamente copias de seguridad del servidor. El período mínimo de retención de las copias de seguridad es siete días. Puede establecer un período de retención de hasta 35 días. La retención se puede ajustar en cualquier momento de la vigencia del servidor. Puede elegir entre copia de seguridad con redundancia geográfica y con redundancia local. Las copias de seguridad con redundancia geográfica también se almacenan en la [región emparejada geográficamente](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) de la región en la que se crea el servidor. Esta redundancia proporciona un nivel de protección en caso de desastre. También ofrece la posibilidad de restaurar el servidor en cualquier otra región de Azure en la que el servicio está disponible con las copias de seguridad con redundancia geográfica. Una vez que se crea el servidor, no es posible cambiar entre las dos opciones de almacenamiento de copia de seguridad.

## <a name="scale-resources"></a>Escalado de recursos

Después de crear el servidor, puede cambiar los núcleos virtuales, la generación de hardware, el plan de tarifa (excepto hacia y desde Básico), la cantidad de almacenamiento y el período de retención de copia de seguridad de manera independiente. No puede cambiar el tipo de almacenamiento de copia de seguridad. El número de núcleos virtuales se pueden escalar o reducir verticalmente. El período de retención de copia de seguridad se puede escalar o reducir verticalmente de 7 a 35 días. El tamaño de almacenamiento solo se puede aumentar. El escalado de los recursos puede realizarse a través del portal o la CLI de Azure. Para ver un ejemplo de escalado con la CLI de Azure, consulte [Supervisión y escalado de un servidor de Azure Database for MySQL mediante la CLI de Azure](scripts/sample-scale-server.md).

Al cambiar el número de núcleos virtuales, la generación de hardware o el plan de tarifa, se crea una copia del servidor original con la nueva asignación de recursos de proceso. Una vez que el nuevo servidor está en funcionamiento, las conexiones se transfieren a él. Durante el breve espacio de tiempo en que el sistema cambia al nuevo servidor, no se puede establecer ninguna nueva conexión y todas las transacciones no confirmadas se revierten. Este intervalo de tiempo varía, pero en la mayoría de los casos es inferior a un minuto.

El escalado del almacenamiento y el cambio del período de retención de copia de seguridad son operaciones verdaderamente en línea. No hay ningún tiempo de inactividad y la aplicación no se ve afectada. A medida que el valor de IOPS se escala con el tamaño del almacenamiento aprovisionado, puede aumentar el número de IOPS disponibles para el servidor mediante el escalado vertical del almacenamiento.

## <a name="pricing"></a>Precios

Para conocer la información más actualizada sobre precios, consulte la [página de precios](https://azure.microsoft.com/pricing/details/mysql/) del servicio. Para ver el costo de la configuración deseada, en [Azure Portal](https://portal.azure.com/#create/Microsoft.MySQLServer) se muestra el costo mensual en la pestaña **Plan de tarifa** según las opciones que seleccione. Si no tiene una suscripción de Azure, puede usar la calculadora de precios de Azure para obtener un precio estimado. En el sitio web [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/), seleccione **Agregar elementos**, expanda la categoría **Bases de datos** y elija **Azure Database for MySQL** para personalizar las opciones.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda cómo [crear un servidor MySQL en el portal](howto-create-manage-server-portal.md).
- Obtenga información acerca de los [límites de servicio](concepts-limits.md).
- Obtenga información sobre cómo [escalar horizontalmente con réplicas de lectura](howto-read-replicas-portal.md).
