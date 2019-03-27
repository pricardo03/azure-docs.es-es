---
title: Introducción a Azure Media Services v3 | Microsoft Docs
description: En este artículo se proporciona información general de alto nivel sobre Media Services y vínculos a artículos para obtener más información.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, transmisión, difusión, en vivo, sin conexión
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/14/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 018392db2ffb510d41385d8e0af19635c35678e6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58003421"
---
# <a name="what-is-azure-media-services-v3"></a>¿Qué es Azure Media Services v3?

Azure Media Services es una plataforma basada en la nube que le permite compilar soluciones que consiguen un streaming de vídeo con calidad de difusión, mejoran la accesibilidad y la distribución, analizan el contenido, etc. Tanto si es un desarrollador de aplicaciones, un centro de llamadas, una autoridad gubernamental o una empresa de entretenimiento, Media Services le ayuda a crear aplicaciones que ofrecen experiencias multimedia de alta calidad a grandes audiencias de los navegadores y dispositivos móviles más populares en la actualidad. 

## <a name="what-can-i-do-with-media-services"></a>¿Qué puedo hacer con Media Services?

Media Services le permite generar una variedad de flujos de trabajo multimedia en la nube. A continuación, se indican algunos ejemplos de lo que se puede lograr con Media Services.  

* Entregue vídeos en varios formatos para que se puedan reproducir en una amplia variedad de navegadores y dispositivos. Para la entrega de streaming en vivo y bajo petición a varios clientes (dispositivos móviles, televisión, equipos, etc.), el contenido de audio y vídeo debe codificarse y comprimirse adecuadamente. Para ver cómo entregar y hacer streaming de este tipo de contenido, consulte [Quickstart: Encode and stream files](stream-files-dotnet-quickstart.md) (Inicio rápido: codificación y streaming de archivos).
* Haga streaming de eventos deportivos para una gran audiencia en línea, como fútbol, béisbol, deportes universitarios o de instituto, etc. 
* Retransmita reuniones y eventos públicos, como plenos de ayuntamientos, juntas municipales y reuniones de órganos legislativos.
* Analice contenido de audio o vídeos grabados. Por ejemplo, para lograr una mayor satisfacción del cliente, las organizaciones pueden extraer texto a voz y crear paneles e índices de búsqueda. A continuación, pueden extraer inteligencia de las quejas habituales, los orígenes de las quejas y otros datos relevantes.
* Cree un servicio de suscripción de vídeo y haga streaming de contenido protegido por DRM cuando un cliente (por ejemplo, un estudio cinematográfico) necesite restringir el acceso y el uso de una obra con derechos de autor.
* Entregue contenido sin conexión para su reproducción en aviones, trenes y automóviles. Es posible que un cliente tenga que descargar el contenido en su teléfono o tableta para reproducirlo cuando tenga previsto desconectarse de la red.
* Implemente una plataforma de vídeo educativa con Azure Media Services y [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) para subtítulos de voz a texto, traducción a varios idiomas, etc. 
* Use Azure Media Services en conjunción con [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) para agregar subtítulos y leyendas a los vídeos. con el fin de llegar a más público (por ejemplo, personas con discapacidades auditivas o usuarios que quieren leer el contenido en un idioma diferente).
* Habilite Azure CDN para lograr un gran escalado y mejorar el control de picos de carga puntuales (por ejemplo, al inicio de un evento de lanzamiento de un producto). 

## <a name="v3-capabilities"></a>Funcionalidades de v3

v3 se basa en una superficie de API unificada que expone la funcionalidad de administración y operaciones creada en Azure Resource Manager. 

Esta versión ofrece las siguientes funcionalidades:  

* **Transformaciones** que le ayudan a definir flujos de trabajo básicos de tareas de análisis o procesamiento multimedia. La transformación es una receta para procesar archivos de audio y vídeo. A continuación, puede aplicarla varias veces para procesar todos los archivos en la biblioteca de contenido si envía trabajos a la transformación.
* **Trabajos** para procesar (codificar o analizar) vídeos. Se puede especificar contenido de entrada en un trabajo con direcciones URL de HTTPS, direcciones URL de SAS o rutas de acceso a archivos ubicados en Azure Blob Storage. Actualmente, AMS v3 no admite la codificación de transferencia fragmentada a través de direcciones URL HTTPS.
* **Notificaciones** que supervisan el progreso o estado del trabajo y eventos de error, inicio o detención de los eventos en directo. Las notificaciones están integradas en el sistema de notificaciones de Azure Event Grid. Puede suscribirse fácilmente a eventos de varios recursos en Azure Media Services. 
* Las plantillas de **Administración de recursos de Azure** se pueden usar para crear e implementar transformaciones, puntos de conexión de streaming, Eventos en directo, etc.
* **El control de acceso basado en rol** se puede establecer en recursos, lo que le permite bloquear el acceso a recursos específicos, como transformaciones, eventos en directo, etc.
* **SDK de cliente** en varios lenguajes: .NET, .NET Core, Python, Go, Java y Node.js.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Los nombres de recursos de Azure Media Services v3 (por ejemplo, recursos, trabajos y transformaciones) están sujetos a las restricciones de nomenclatura de Azure Resource Manager. De acuerdo con Azure Resource Manager, los nombres de los recursos siempre son únicos. Por lo tanto, puede usar cualquier cadena de identificador único (por ejemplo, GUID) para los nombres de recursos. 

Los nombres de recursos de Media Services no pueden incluir: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', el carácter de comilla simple o cualquier carácter de control. Todos los demás caracteres se permiten. La longitud máxima de un nombre de recurso es de 260 caracteres. 

Para más información sobre la nomenclatura de Azure Resource Manager, consulte: [Requisitos de nomenclatura](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) y [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles"></a>Principios de diseño de la versión v3 de la API

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. Las API de la versión v3 no devuelven secretos o credenciales en una operación **Get** o **List**. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. Debe llamar a un método de acción independiente para obtener los secretos o las credenciales. Acciones independientes le permiten establecer distintos permisos de seguridad RBAC en caso de que algunas API recuperen o muestren secretos y otras no. Para más información acerca de cómo administrar el acceso mediante RBAC, consulte [Uso de RBAC para administrar el acceso](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Estos son algunos ejemplos 

* no devolver los valores de ContentKey en la operación Get del objeto StreamingLocator, 
* no devolver las claves de restricción en la operación Get de ContentKeyPolicy, 
* no devolver la parte de la cadena de consulta de la dirección URL (para quitar la firma) de direcciones URL de entrada HTTP de los trabajos.

Consulte el ejemplo de [Get content key policy: .NET](get-content-key-policy-dotnet-howto.md).

## <a name="how-can-i-get-started-with-v3"></a>¿Cómo puedo comenzar con v3?

Para obtener información acerca de cómo empezar a desarrollar con la API de Media Services v3 mediante varias herramientas y SDK, consulte el artículo sobre cómo se [empieza a desarrollar](developers-guide.md).

## <a name="v3-content-map"></a>Mapa de contenido de v3

El contenido de Media Services v3 se organiza según la siguiente estructura (que también se refleja en la tabla de contenido):

|Secciones| DESCRIPCIÓN|
|---|---|
| Información general | Explica la funcionalidad de Media Services y lo que puede hacer con el servicio.|
| Guías de inicio rápido | Muestra instrucciones básicas del primer día para que los nuevos clientes prueben rápidamente Media Services.|
| Tutoriales | Muestran procedimientos basados en un escenario para algunas de las principales tareas de Media Services.|
| Ejemplos | Vínculos a ejemplos de código. |
| Conceptos | Contiene una explicación detallada de los [conceptos y la funcionalidad de Media Services v3](concepts-overview.md). Deben revisarse los conceptos fundamentales que se describen en estos temas antes de comenzar a desarrollar.<br/><br/>* Carga y almacenamiento en la nube<br/>* Codificación<br/>* Análisis multimedia<br/>* Empaquetado, entrega, protección<br/>* Streaming en vivo<br/>* Supervisión<br/>* Clientes de Player<br/><br/>Y mucho más... |
| Guías de procedimientos | Muestra cómo completar una tarea.|

## <a name="next-steps"></a>Pasos siguientes

Para ver lo fácil que es empezar a codificar y hacer streaming de archivos de vídeo, consulte [Stream files](stream-files-dotnet-quickstart.md) (Streaming de vídeos). 

