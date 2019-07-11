---
title: ¿Qué es Form Recognizer?
titleSuffix: Azure Cognitive Services
description: Aprenda a utilizar Form Recognizer para analizar datos de formularios y tablas.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: e064faf3017b95cb3a5f3d9b89f178fb7f846766
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592619"
---
# <a name="what-is-form-recognizer"></a>¿Qué es Form Recognizer?

Azure Form Recognizer es un servicio cognitivo que utiliza tecnología de aprendizaje automático para identificar y extraer pares clave-valor, así como datos de tablas de los documentos de formularios. A continuación, genera datos estructurados que incluyen las relaciones del fichero original. Puede llamar al modelo personalizado de Form Recognizer mediante una simple API REST para reducir la complejidad e integrarla fácilmente en su flujo de trabajo o aplicación. Para comenzar, solo necesita cinco documentos de formulario rellenos o dos formularios rellenos además de un formulario vacío del mismo tipo que el material de entrada. Obtendrá rápidamente resultados precisos a la medida de su contenido específico sin necesidad de una intervención manual pesada o una amplia experiencia en ciencia de datos.

## <a name="custom-models"></a>Modelos personalizados

Form Recognizer entrena modelos personalizados modelo para sus propios datos y solo necesitará cinco formularios de entrada de ejemplo para comenzar. Cuando se envían los datos de entrada, el algoritmo agrupa los formularios por tipos, descubre qué claves y tablas están presentes y asocia los valores a las claves y entradas a las tablas. A continuación, genera datos estructurados que incluyen las relaciones del fichero original. Después de entrenar el modelo, puede probarlo, volver a entrenarlo y finalmente usarlo para extraer datos de forma confiable de más formularios en función de las propias necesidades.

El aprendizaje sin supervisión permite que el modelo comprenda el diseño y las relaciones entre los campos y las entradas sin necesidad de un etiquetado de datos manual o de una codificación y un mantenimiento intensivos. Por el contrario, los modelos de aprendizaje automático entrenados previamente requieren datos estandarizados. Son menos precisos con el material de entrada que procede de los formatos tradicionales, como formularios específicos del sector.

## <a name="pre-built-receipt-model"></a>Modelo de recibo pregenerado

Form Recognizer también incluye un modelo para leer los recibos de ventas. Este modelo extrae información clave como la hora y fecha de la transacción, información del comerciante, cantidades de impuestos, totales y mucho más. Además, el modelo de recibos pregenerado se entrena para reconocer y devolver todo el texto en un recibo.

## <a name="what-it-includes"></a>Qué incluye

Form Recognizer está disponible como una API REST. Puede crear, entrenar y puntuar un modelo personalizado o acceder al modelo pregenerado mediante la invocación de estas API. Si lo desea, puede entrenar y ejecutar modelos personalizados en un contenedor de Docker local.

## <a name="input-requirements-custom-model"></a>Requisitos de entrada (modelo personalizado)

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="request-access"></a>Solicitar acceso

Form Recognizer está disponible como versión preliminar de acceso limitado. Para acceder a la versión preliminar, rellene y envíe el formulario [Solicitud de acceso de Form Recognizer](https://aka.ms/FormRecognizerRequestAccess). El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar Form Recognizer. Si el equipo de Azure Cognitive Services aprueba la solicitud, recibirá un correo electrónico con instrucciones para acceder al servicio.

## <a name="where-do-i-start"></a>¿Por dónde empiezo?

**Paso 1:** Cree un recurso de Form Recognizer en Azure Portal.

**Paso 2:** Siga un inicio rápido para usar la API REST:
* [Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de los datos del formulario mediante la API REST con cURL](quickstarts/curl-train-extract.md)
* [Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de datos del formulario mediante la API REST con Python](quickstarts/python-train-extract.md)
* [Inicio rápido: Extracción de datos de recibos con cURL](quickstarts/curl-receipts.md)
* [Inicio rápido: Extracción de datos de recibos con Python](quickstarts/python-receipts.md)

Se recomienda usar el servicio gratuito cuando se está aprendiendo la tecnología. Tenga en cuenta que el número de páginas libres se limita a 500 al mes.

**Paso 3:** Revisión de las API REST

Use las siguientes API para entrenar y extraer datos estructurados de los formularios.

|||
|---|---|
| Train Model (Entrenar modelo)| Entrene un nuevo modelo para analizar los formularios con cinco formularios del mismo tipo. O bien, entrene con un formulario vacío y dos formularios prerrellenados.  |
| Análisis del formulario |Analice un documento único pasado como una secuencia para extraer pares clave-valor y tablas del formulario con su modelo personalizado.  |
| Análisis del recibo |Analice un documento de entrada individual para extraer la información clave y cualquier otro texto del recibo.|

Explore la [documentación de referencia de API REST](https://aka.ms/form-recognizer/api) para más información. 

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

El servicio se ofrece como [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) de un servicio de Azure bajo los [términos de los servicios en línea](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Form Recognizer deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Realice un [inicio rápido](quickstarts/curl-train-extract.md) para empezar a usar las [API de Form Recognizer](https://aka.ms/form-recognizer/api).
