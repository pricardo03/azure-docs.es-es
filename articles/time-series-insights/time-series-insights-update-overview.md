---
title: Introducción a Azure Time Series Insights | Microsoft Docs
description: Introducción a Azure Time Series Insights
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 12/03/2018
ms.openlocfilehash: 83513ab9f7bee5c51fc909e5f4246105b6f145ca
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876138"
---
# <a name="azure-time-series-insights-overview"></a>Introducción a Azure Time Series Insights

Azure Time Series Insights (TSI) es una plataforma como servicio global para ingerir, procesar, almacenar y consultar rápidamente datos de escala de IoT muy contextualizados y optimizados para series temporales. Por lo tanto, Azure TSI es ideal para la exploración de datos ad hoc, así como para el análisis operativo. TSI es una oferta de servicio de extensibilidad única, personalizado, que satisface las necesidades de las implementaciones de IoT industriales.

## <a name="what-is-iot-data"></a>¿Qué son los datos de IoT?

Los datos de IoT hacen referencia a cualquier dato "industrial" disponible en organizaciones con uso intensivo de los recursos. Los datos de IoT están frecuentemente muy desestructurados ya que se envían desde una amplia gama de recursos que registran mediciones con mucho ruido como, por ejemplo, temperatura, movimiento y humedad. Además, estos flujos de datos se caracterizan con frecuencia por incluir vacíos significativos, mensajes dañados y lecturas falsas. Los datos que proceden de estos flujos se deben limpiar antes de que se produzca el análisis. Los datos de IoT a menudo son solo significativos en el contexto de entradas adicionales de datos procedentes de orígenes de datos propietarios (como CRM o ERP) o de terceros (como, por ejemplo, información meteorológica o ubicación).

Como resultado, solo una fracción insignificante de esos datos se usa con fines operativos y empresariales. Estos datos proporcionan una información coherente, completa, actualizada y correcta que resulta necesaria para los informes y los análisis empresariales. Convertir los datos de IoT recopilados en información procesable requiere varias funcionalidades clave:

* Procesamiento de datos para limpiar, filtrar, interpolar, transformar y preparar los datos para su análisis
* Estructuración para poder explorar y comprender los datos (para normalizarlos y contextualizarlos)
* Almacenamiento rentable para una retención duradera durante décadas, o infinita, de datos valiosos procesados (derivados) o sin procesar

El flujo de datos típico de IoT se puede describir como sigue:

  ![Flujo de datos de IoT][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Azure Time Series Insights para IoT industrial

El panorama actual de IoT es variado. Incluye clientes que pertenecen a diversos sectores como el de la fabricación, automoción, petróleo y gas, suministro de electricidad, edificios inteligentes y consultoría. Entre los escenarios se incluye la exploración de datos ad-hoc en los que la forma de los datos resulta desconocida, así como el análisis operativo de datos esquematizados (modelados explícitamente) para aumentar la eficacia operativa. Estos escenarios se dan normalmente de forma conjunta y admiten distintos casos de uso. Las funcionalidades de plataforma como el almacenamiento en varias capas (datos activos e inactivos), la posibilidad de almacenar décadas de datos de series temporales y la posibilidad de modelar y optimizar explícitamente las consultas para una inteligencia operativa basada en recursos se están convirtiendo en claves que contribuyen al éxito de las empresas de IoT industriales y a una revolución digital.

Azure TSI es una oferta de plataforma como servicio completa y global para la exploración de datos de IoT así como para la obtención de información operativa. Time Series Insights ofrece un servicio en la nube totalmente administrado para analizar datos de series temporales a escala de IoT.

Los clientes pueden almacenar datos sin procesar en un almacén en memoria sin esquemas y realizar consultas ad hoc interactivas mediante un motor y una API de consultas distribuidas así como aprovechar nuestra rica experiencia de usuario para visualizar miles de millones de eventos en cuestión de segundos. Aquí puede encontrar más información acerca de nuestras [funcionalidades de exploración de datos](./time-series-insights-overview.md).

TSI también ofrece funcionalidades de obtención de información operativa que actualmente se encuentran en versión preliminar pública. Además de la exploración de datos interactiva y la inteligencia operativa, Time Series Insights permite a los clientes obtener más valor de los datos recopilados de los recursos de IoT. En concreto, la oferta de la versión preliminar pública admite las siguientes funcionalidades clave:

* Un almacén de datos escalable y con un costo y un rendimiento optimizados, que permite a una solución de IoT basada en la nube analizar tendencias de años de datos de series temporales en cuestión de segundos.
* Compatibilidad con el modelo semántico para describir el dominio y los metadatos asociados con las señales derivadas y no derivadas de los recursos y los dispositivos.
* Una experiencia de usuario mejorada que combina la obtención de información a partir de datos basados en recursos con un análisis enriquecido de datos ad hoc que fomenta la inteligencia empresarial y operativa.
* Integración con herramientas avanzadas de aprendizaje automático y de análisis como Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter Notebooks, Power BI, etc, que ayuda a los clientes a afrontar los desafíos de los datos de series temporales y favorece la eficacia operativa.

La exploración de los datos y la obtención de información operativa se ofrece a través de un sencillo modelo de precios de pago por uso para el procesamiento de datos, el almacenamiento y la consulta de estos, lo cual proporciona a los clientes un modelo mucho más escalable que se ajusta a las cambiantes necesidades del negocio.

A continuación puede ver un diagrama de flujo de datos de alto nivel que describe las funcionalidades actualizadas:

  ![Principales capacidades][2]

Gracias a la introducción de estas funcionalidades de IoT industriales clave, Azure TSI ofrece las siguientes ventajas.

* Almacenamiento en varias capas para datos de series temporales a escala de IoT

  * Con una canalización de procesamiento de datos habitual para ingerir datos, los clientes tienen la posibilidad de almacenar datos en un almacenamiento intermedio para la realización de consultas interactivas, o en un almacenamiento en frío para poder almacenar grandes volúmenes de datos y aprovechar las consultas de alto rendimiento basadas en recursos.

  * El enrutamiento dinámico entre las capas de almacenamiento estará disponible próximamente.

* Modelo de serie temporal para contextualizar datos de telemetría sin procesar y derivar información basada en activos

  * Los clientes pueden contextualizar los datos de telemetría sin procesar con el modelo de serie temporal descriptivo y derivar una inteligencia operativa enriquecida, con consultas de alto rendimiento, rentables y basadas en dispositivos.

* Integración perfecta con otras soluciones de datos
  
  * Dado que los datos de Azure Time Series Insights se almacenan en archivos de código abierto de Apache Parquet, los clientes se pueden integrar fácilmente con otras soluciones de datos (propietarias o de terceros) para escenarios globales que incluyen inteligencia empresarial, aprendizaje automático avanzado, análisis predictivo, etc.

* Exploración de datos casi en tiempo real

  * La experiencia de usuario del explorador de Azure Time Series Insights proporciona la visualización de todos los flujos de datos de la canalización de ingesta. Poco después de conectarse a un origen del evento, los clientes pueden ver, explorar y consultar datos de eventos para validar si un dispositivo emite los datos según lo previsto y para supervisar el estado, la productividad y la eficacia global de un recurso de IoT.

* Análisis de las causas principales y detección de anomalías

  * Azure Time Series Insights es compatible con patrones y vistas de perspectiva para llevar a cabo y guardar análisis de las causas principales de varios pasos. En combinación con Azure Stream Analytics, los clientes pueden usar Time Series Insights para detectar alertas y anomalías casi en tiempo real.

* Compilación de aplicaciones personalizadas en la plataforma de Time Series Insights

  * Azure Time Series Insights es compatible con el SDK de JavaScript con controles enriquecidos y un acceso simplificado a las consultas para permitir a los clientes compilar aplicaciones personalizadas de IoT sobre la plataforma de Time Series Insights para satisfacer las necesidades de negocios individuales.
  * Los clientes también pueden usar las API de consulta de Time Series Insights directamente para dirigir datos a las aplicaciones de IoT personalizadas.

## <a name="next-steps"></a>Pasos siguientes

Está listo para empezar a trabajar con Azure TSI (versión preliminar):

> [!div class="nextstepaction"]
> [Lea la guía de inicio rápido](./time-series-insights-update-quickstart.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png