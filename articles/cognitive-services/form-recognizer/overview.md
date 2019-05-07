---
title: ¿Qué es Form Recognizer?
titleSuffix: Azure Cognitive Services
description: Aprenda a utilizar Form Recognizer para analizar datos de formularios y tablas.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: 2a120a59a58eb8d7a017cce0dd85c21038bdcf51
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143211"
---
# <a name="what-is-form-recognizer"></a>¿Qué es Form Recognizer?

Azure Form Recognizer es un servicio cognitivo que utiliza tecnología de aprendizaje automático para identificar y extraer pares clave-valor, así como datos de tablas de los documentos de formularios. A continuación, genera datos estructurados que incluyen las relaciones del fichero original. Puede llamar al modelo personalizado de Form Recognizer mediante una simple API REST para reducir la complejidad e integrarla fácilmente en su flujo de trabajo o aplicación. Solo necesita cinco documentos de formulario o un formulario vacío del mismo tipo que el material de entrada para empezar. Puede obtener resultados de forma rápida, precisa y adaptada a su contenido específico sin necesidad de una pesada intervención manual o una amplia experiencia en ciencia de datos.

## <a name="request-access"></a>Solicitar acceso
Form Recognizer está disponible como versión preliminar de acceso limitado. Para acceder a la versión preliminar, rellene y envíe el formulario [Solicitud de acceso de Form Recognizer de Cognitive Services](https://aka.ms/FormRecognizerRequestAccess). El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar Form Recognizer. Si la solicitud la aprueba el equipo de Azure Cognitive Services, recibirá un correo electrónico con instrucciones sobre cómo acceder al servicio.

## <a name="what-it-does"></a>Qué hace

Cuando se envían los datos de entrada, el algoritmo se entrena, agrupa los formularios por tipos, descubre qué claves y tablas están presentes y aprende a asociar valores a las claves y entradas a las tablas. El aprendizaje sin supervisión permite que el modelo comprenda el diseño y las relaciones entre los campos y las entradas sin necesidad de un etiquetado de datos manual o de una codificación y un mantenimiento intensivos. Por el contrario, los modelos de aprendizaje automático entrenados previamente requieren datos estandarizados y son menos precisos con el material de entrada que se desvía de los formatos tradicionales, como los formularios específicos del sector.

Cuando se entrena el modelo, puede probarlo, volver a entrenarlo y finalmente usarlo para extraer datos de forma confiable de más formularios en función de las propias necesidades.

## <a name="what-it-includes"></a>Qué incluye

Form Recognizer está disponible como una API REST. Puede crear, entrenar y calificar un modelo mediante la invocación a la API y, opcionalmente, puede ejecutar el modelo en un contenedor de Docker local.

## <a name="input-requirements"></a>Requisitos de entrada

Form Recognizer trabaja con documentos de entrada que cumplen los siguientes requisitos:

* formato PDF, PNG o JPG (texto o digitalizado). Son preferibles los PDF insertados de texto porque no hay posibilidad de error en la extracción de caracteres y en la ubicación.
* El tamaño del archivo debe ser inferior a 4 megabytes (MB).
* Para las imágenes, las dimensiones deben estar entre 50 x 50 y 4200 x 4200 píxeles.
* Si se digitalizan desde documentos en papel, los formularios deben ser digitalizaciones de alta calidad.
* Debe usarse el alfabeto latino.
* Datos impresos (no escritos a mano).
* Debe contener claves y valores.
* Las claves pueden aparecer arriba o a la izquierda de los valores, pero no abajo o a la derecha.

Además, Form Recognizer todavía no admite los siguientes tipos de datos de entrada:

* Tablas complejas (tablas anidadas, celdas o encabezados combinados, etc.) 
* Casillas o botones de radio
* Documentos PDF de más de 50 páginas

## <a name="where-do-i-start"></a>¿Por dónde empiezo?

**Paso 1:** Cree un recurso de Form Recognizer en Azure Portal.

**Paso 2:** Pruebe un inicio rápido para obtener experiencia práctica:
* [Inicio rápido: Train a Form Recognizer model and extract form data using REST API with cURL](quickstarts/curl-train-extract.md) (Entrenamiento de un modelo de Form Recognizer y extracción de los datos del formulario mediante la API REST con cURL)
* [Inicio rápido: Train a Form Recognizer model and extract form data using REST API with Python](quickstarts/python-train-extract.md) (Entrenamiento de un modelo de Form Recognizer y extracción de datos del formulario mediante la API REST con Python)

Para fines de aprendizaje se recomienda el servicio Gratis, pero tenga en cuenta que el número de páginas gratis se limita a 500 páginas al mes.

**Paso 3:** Revise el uso de la API REST. Utilice las API siguientes para entrenar y extraer datos estructurados de formularios.

| API DE REST | DESCRIPCIÓN |
|-----|-------------|
| Train | Entrene un nuevo modelo para analizar los formularios usando cinco formularios del mismo tipo o un formulario vacío.  |
| Análisis  |Analice un documento único pasado como una secuencia para extraer pares clave-valor y tablas del formulario con su modelo personalizado.  |

Explore el [documento de referencia de la API REST](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

El servicio se ofrece como [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) de un servicio de Azure bajo los [términos de los servicios en línea](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Form Recognizer deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Siga un [inicio rápido](quickstarts/curl-train-extract.md) para empezar a usar las [Form Recognizer API](https://aka.ms/form-recognizer/api).
