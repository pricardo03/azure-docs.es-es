---
title: Escalado de streaming con CDN
titleSuffix: Azure Media Services
description: Obtenga información sobre el servicio de streaming que entrega contenido directamente a una aplicación de reproducción cliente o a una instancia de Content Delivery Network (CDN).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2020
ms.author: juliako
ms.openlocfilehash: 90fa3b06e2696e9b45c333c75c8a8e117d5c0c96
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562763"
---
# <a name="scaling-streaming-with-cdn"></a>Escalado de streaming con CDN

Azure Content Delivery Network (CDN) ofrece a los desarrolladores una solución global para la entrega rápida de contenido con alto ancho de banda a los usuarios mediante el almacenamiento en caché del contenido en nodos físicos estratégicamente situados en todo el mundo.  

CDN almacena en caché el contenido transmitido desde [punto de conexión de streaming (origen)](streaming-endpoint-concept.md) de Media Services por códec, por protocolo de streaming, por velocidad de bits, por formato de contenedor y por cifrado/DRM. Para cada combinación de formato-velocidad de bits-cifrado protocolo-contenedor códec-streaming, habrá una caché de CDN independiente. 

El contenido popular se servirá directamente desde la caché de CDN, siempre que el fragmento de vídeo esté almacenado en caché. Es probable que el contenido en vivo se almacene en caché porque suele haber muchas personas mirando exactamente lo mismo. El contenido a petición puede ser un poco más complejo porque podría haber parte del contenido que fuera popular y otra que no. Si hay millones de recursos de vídeo que no son populares (solo uno o dos visores a la semana), pero hay miles de personas que miran vídeos diferentes, la red CDN pasa a ser mucho menos efectiva. 

También debe tener en cuenta cómo funciona el streaming adaptable. Cada fragmento de vídeo individual se almacena en caché como entidad propia. Por ejemplo, imagine la primera vez que se mira un vídeo determinado. Si el visor omite parte y mira solo unos segundos aquí y allá, solo los fragmentos de vídeo asociados con lo que miró esa persona se almacenan en la caché de CDN. Con el streaming adaptable, se suelen tener de 5 a 7 velocidades de bits de vídeo distintas. Si una persona está mirando a una velocidad de bits y otra persona a una velocidad de bits diferente, se almacenan en caché cada uno por separado en la red CDN. Incluso si dos personas están mirando a la misma velocidad de bits podrían hacer streaming a través de protocolos diferentes. Cada protocolo (HLS, MPEG-DASH, Smooth Streaming) se almacena en caché por separado. Por lo tanto, cada velocidad de bits y protocolo se almacenan en caché por separado y solo se almacenan en caché los fragmentos de vídeo que se han solicitado.

A la hora de decidir si habilitar CDN en el [punto de conexión de streaming](streaming-endpoint-concept.md) de Media Services, tenga en cuenta el número de espectadores previstos. CDN solo resulta útil si se prevén muchos espectadores para el contenido. Si la simultaneidad máxima es inferior a 500 espectadores, se recomienda deshabilitar CDN, ya que esta red se escala mejor con simultaneidad. 

En este tema se describe la habilitación de la [integración de CDN](#enable-azure-cdn-integration). También se explica la captura previa (almacenamiento en caché activo) y el concepto [Origin-Assist CDN-Prefetch](#origin-assist-cdn-prefetch).

## <a name="considerations"></a>Consideraciones

* El [punto de conexión de streaming](streaming-endpoint-concept.md) `hostname` y la dirección URL de streaming se mantienen independientemente de si habilita CDN.
* Si necesita poder probar el contenido con o sin CDN, cree otro punto de conexión de streaming que no tenga la red CDN habilitada.

## <a name="enable-azure-cdn-integration"></a>Habilitación de la integración de Azure CDN

> [!IMPORTANT]
> No puede habilitar CDN con cuentas de prueba o cuentas educativas de Azure.
>
> La integración de la red CDN está habilitada en todos los centros de datos de Azure excepto para las regiones Gobierno Federal y China.

Después de aprovisionar un punto de conexión de streaming con la red CDN habilitada, hay un tiempo de espera definido en Media Services antes de realizar la actualización de DNS para asignar el punto de conexión de streaming al punto de conexión de CDN.

Si más adelante desea volver a habilitar o deshabilitar la red CDN, punto de conexión de streaming debe estar en estado **stopped** (detenido). Es posible que transcurran hasta dos horas hasta que la integración de Azure CDN se habilite y los cambios se activen en todos los POP de la red CDN. Sin embargo, puede iniciar el punto de conexión de streaming y transmitir sin interrupciones desde ahí y, una vez que la integración esté completa, la transmisión se efectúa desde la red CDN. Durante el período de aprovisionamiento, el punto de conexión de streaming estará en estado **iniciando** y es posible que note una reducción en el rendimiento.

Cuando se crea el punto de conexión de streaming Estándar, se configura de forma predeterminada con Standard Verizon. Puede configurar los proveedores Premium Verizon o Standard Akamai mediante las API REST.

La integración de Azure Media Services con Azure CDN se implementa en **Azure CDN de Verizon** para puntos de conexión de streaming estándar. Los puntos de conexión de streaming premium pueden configurarse con todos los **proveedores y planes de tarifa de Azure CDN**. 

> [!NOTE]
> Para más información sobre Azure CDN, consulte [Información general de la red CDN](../../cdn/cdn-overview.md).

## <a name="determine-if-a-dns-change-was-made"></a>Determinar si se realizó el cambio de DNS

Puede determinar si se ha realizado el cambio de DNS en un punto de conexión de streaming (el tráfico se dirige a la red Azure CDN) mediante https://www.digwebinterface.com. Si los resultados tienen los nombres de dominio azureedge.net, el tráfico se dirige ahora a la red CDN.

## <a name="origin-assist-cdn-prefetch"></a>Origin-Assist CDN-Prefetch

El almacenamiento en caché de CDN es un proceso reactivo. Si CDN puede predecir el siguiente objeto que se va a solicitar, también puede solicitar y almacenar en caché el siguiente objeto de forma proactiva. Con este proceso, se puede lograr un acierto de caché para todos los objetos (o la mayoría de ellos), lo que mejora el rendimiento.

El concepto de captura previa se esfuerza por colocar los objetos en el "perímetro de Internet" en previsión de que el reproductor lo solicitará de manera inminente, lo que reduce el tiempo de entrega del objeto al reproductor.

Para lograr este objetivo, un punto de conexión de streaming (origen) y CDN deben funcionar conjuntamente: 

- El origen de Media Services debe tener la "inteligencia" (Origin-Assist) para informar a CDN del siguiente objeto que se va a capturar previamente, y 
- CDN realiza la captura previa y el almacenamiento en caché (parte CDN-Prefetch). CDN también debe tener la "inteligencia" para notificar al origen si se trata de una captura previa o una captura normal, así como controlar las respuestas 404 y la manera de evitar un bucle de captura previa sin fin.

### <a name="benefits"></a>Ventajas

Las ventajas de la característica *Origin-Assist CDN-Prefetch* incluyen:

- La captura previa mejora la calidad de la reproducción de vídeo: coloca previamente los segmentos de vídeo previstos en el perímetro durante la reproducción, lo que reduce la latencia para el espectador y mejora los tiempos de descarga de segmentos de vídeo. El resultado es un tiempo de inicio del vídeo más rápido y menos repeticiones de almacenamiento en búfer.
- Este concepto es aplicable al escenario general de CDN-Origin, no se limita a los elementos multimedia.
- Akamai ha agregado esta característica a [Akamai Cloud Embed (ACE)](https://learn.akamai.com/en-us/products/media_delivery/cloud_embed.html).

> [!NOTE]
> Esta característica todavía no es aplicable a la red CDN de Akamai integrada con el punto de conexión de streaming de Media Services. Sin embargo, está disponible para los clientes de Media Services que tienen un contrato de Akamai existente y requieren una integración personalizada entre la red CDN de Akamai y el origen de Media Services.

### <a name="how-it-works"></a>Funcionamiento

La compatibilidad de CDN con los encabezados *Origin-Assist CDN-Prefetch* (para el streaming a petición tanto en vivo como en vídeo) está disponible para los clientes que tienen un contrato directo con CDN de Akamai. La característica supone los siguientes intercambios de encabezados HTTP entre CDN de Akamai y el origen de Media Services:

|Encabezado HTTP|Valores|Remitente|Receptor|Propósito|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-Assist-Prefetch-Enabled | 1 (valor predeterminado) o 0 |CDN|Origen|Para indicar que la red CDN está habilitada para la captura previa.|
|CDN-Origin-Assist-Prefetch-Path| Ejemplo: <br/>Fragments(video=1400000000,format=mpd-time-cmaf)|Origen|CDN|Para proporcionar la ruta de acceso de captura previa a la red CDN.|
|CDN-Origin-Assist-Prefetch-Request|1 (solicitud de captura previa) o 0 (solicitud normal)|CDN|Origen|Para indicar que la solicitud de CDN es una captura previa.|

Para ver en acción parte del intercambio de encabezados, puede probar los pasos siguientes:

1. Use Postman o cURL para emitir una solicitud de un segmento o fragmento de audio o vídeo al origen de Media Services. Asegúrese de agregar el encabezado CDN-Origin-Assist-Prefetch-Enabled: 1 en la solicitud.
2. En la respuesta, debería ver el encabezado CDN-Origin-Assist-Prefetch-Path con una ruta de acceso relativa como su valor.

### <a name="supported-streaming-protocols"></a>Protocolos de streaming admitidos 

La característica *Origin-Assist CDN-Prefetch* admite los siguientes protocolos de streaming para el streaming en vivo y a petición:

* HLS v3
* HLS v4
* HLS CMAF
* DASH (CSF)
* DASH (CMAF)
* Streaming con velocidad de transmisión adaptable

### <a name="faqs"></a>Preguntas más frecuentes

* ¿Qué ocurre si una dirección URL de ruta de acceso de captura previa no es válida y la captura previa de CDN obtiene una respuesta 404? 

    CDN solo almacena en caché una respuesta 404 durante 10 segundos (u otro valor configurado).
* Supongamos que tiene un vídeo a petición. Si la captura previa de CDN está habilitada, ¿esta característica implica que una vez que un cliente solicite el primer segmento de vídeo, la captura previa iniciará un bucle para capturar previamente todos los segmentos de vídeo posteriores con la misma velocidad de bits? 

    No, CDN-Prefetch solo se ejecuta después de una solicitud/respuesta iniciada por el cliente. Para evitar un bucle de captura previa, CDN-Prefetch nunca se desencadena con una captura previa. 
* ¿La característica Origin-Assist CDN-Prefetch está siempre activada? ¿Cómo se puede activar o desactivar? 

    Esta característica está desactivada de manera predeterminada. Los clientes deben activarla a través de la API de Akamai.
* En el caso del streaming en vivo, ¿qué pasaría con Origin-Assist si el siguiente segmento o fragmento no está disponible todavía? 

    En este caso, el origen de Media Services no proporcionará el encabezado CDN-Origin-Assist-Prefetch-Path y no se ejecutará CDN-Prefetch.
* ¿Cómo funciona *Origin-Assist CDN-Prefetch* con los filtros de manifiesto dinámico? 

    Esta característica funciona independientemente del filtro del manifiesto. Cuando el siguiente fragmento está fuera de una ventana de filtro, su dirección URL se podrá encontrar en el manifiesto del cliente sin formato y, a continuación, se devolverá como el encabezado de respuesta de la captura previa de CDN. Esto significa que CDN obtendrá la dirección URL de un fragmento filtrado del manifiesto DASH/HLS/Smooth. Sin embargo, el reproductor nunca realizará una solicitud GET a CDN para capturar ese fragmento, ya que dicho fragmento no se incluye en el manifiesto DASH/HLS/Smooth del reproductor (el reproductor no conoce la existencia de ese fragmento).
* ¿Se puede realizar una captura previa de DASH MPD, la lista de reproducción de HLS o el manifiesto Smooth?

    No, DASH MPD, la lista de reproducción maestra de HLS, la lista de reproducción de la variante HLS o la dirección URL del manifiesto Smooth no se agregan al encabezado de captura previa.
* ¿Las direcciones URL de captura previa son relativas o absolutas? 

    Mientras que CDN de Akamai admite ambas opciones, el origen de Media Services solo proporciona direcciones URL relativas para la ruta de acceso de captura previa porque el uso de direcciones URL absolutas no presenta ninguna ventaja aparente.
* ¿Esta característica funciona con el contenido protegido con DRM?

    Sí, puesto que esta característica funciona en el nivel HTTP, ni descodifica ni analiza ningún segmento o fragmento. No tiene en cuenta si el contenido está cifrado o no.
* ¿Esta característica funciona con la inserción de anuncios del lado del servidor (SSAI)?
    
    Para el contenido original o principal (el contenido del vídeo original antes de la inserción de anuncios) funciona, ya que SSAI no cambia la marca de tiempo del contenido de origen indicada en el origen de Media Services. Si esta característica funciona con el contenido del anuncio, depende de si el origen del anuncio es compatible con Origin-Assist. Por ejemplo, si el contenido del anuncio también se hospeda en Azure Media Services (en el mismo origen o uno diferente), también se realizará la captura previa del contenido del anuncio.
* ¿Esta característica funciona con el contenido UHD/HEVC?

    Sí.

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

En el ejemplo [de este repositorio](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) se muestra cómo iniciar el punto de conexión de streaming predeterminado con .NET.
