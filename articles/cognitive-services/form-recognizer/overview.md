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
ms.openlocfilehash: 8fb382227c71fce7ebe062057adf5edfb90a1a92
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65601626"
---
# <a name="what-is-form-recognizer"></a>¿Qué es Form Recognizer?

Azure Form Recognizer es un servicio cognitivo que utiliza tecnología de aprendizaje automático para identificar y extraer pares clave-valor, así como datos de tablas de los documentos de formularios. A continuación, genera datos estructurados que incluyen las relaciones del fichero original. Puede llamar al modelo personalizado de Form Recognizer mediante una simple API REST para reducir la complejidad e integrarla fácilmente en su flujo de trabajo o aplicación. Para comenzar, solo necesita cinco documentos de formulario o un formulario vacío del mismo tipo que el material de entrada. Obtendrá rápidamente resultados precisos a la medida de su contenido específico sin necesidad de una intervención manual pesada o una amplia experiencia en ciencia de datos.

## <a name="request-access"></a>Solicitar acceso
Form Recognizer está disponible como versión preliminar de acceso limitado. Para acceder a la versión preliminar, rellene y envíe el formulario [Solicitud de acceso de Form Recognizer](https://aka.ms/FormRecognizerRequestAccess). El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar Form Recognizer. Si el equipo de Azure Cognitive Services aprueba la solicitud, recibirá un correo electrónico con instrucciones para acceder al servicio.

## <a name="what-it-does"></a>Qué hace

Cuando se envían los datos de entrada, el algoritmo se entrena, agrupa los formularios por tipos, descubre qué claves y tablas están presentes y aprende a asociar valores a las claves y entradas a las tablas. El aprendizaje sin supervisión permite que el modelo comprenda el diseño y las relaciones entre los campos y las entradas sin necesidad de un etiquetado de datos manual o de una codificación y un mantenimiento intensivos. Por el contrario, los modelos de aprendizaje automático entrenados previamente requieren datos estandarizados y son menos precisos cuando se usan con material de entrada que se desvía de los formatos tradicionales, como los formularios específicos del sector.

Después de entrenar el modelo, puede probarlo, volver a entrenarlo y finalmente usarlo para extraer datos de forma confiable de más formularios en función de las propias necesidades.

## <a name="what-it-includes"></a>Qué incluye

Form Recognizer está disponible como una API REST. Puede crear, entrenar y puntuar un modelo mediante la invocación de la API. Si quiere, puede ejecutar el modelo en un contenedor de Docker local.

## <a name="input-requirements"></a>Requisitos de entrada

Form Recognizer trabaja con documentos de entrada que cumplen estos requisitos:

* El formato debe ser JPG, PNG o PDF (texto o digitalizado). Los PDF insertados de texto son mejores porque no hay posibilidad de error en la extracción de caracteres y en la ubicación.
* El tamaño del archivo debe ser inferior a 4 megabytes (MB).
* Para las imágenes, las dimensiones deben tener entre 50 x 50 píxeles y 4200 x 4200 píxeles.
* Si se digitalizan desde documentos en papel, los formularios deben ser digitalizaciones de alta calidad.
* El texto debe usar el alfabeto latino (caracteres ingleses).
* Los datos deben estar impresos (no escritos a mano).
* Los datos deben contener claves y valores.
* Las claves pueden aparecer arriba o a la izquierda de los valores, pero no abajo o a la derecha.

Form Recognizer no admite actualmente estos tipos de datos de entrada:

* Tablas complejas (tablas anidadas, encabezados combinados o celdas, etc.)
* Casillas o botones de radio
* Documentos PDF de más de 50 páginas

## <a name="where-do-i-start"></a>¿Por dónde empiezo?

**Paso 1:** Cree un recurso de Form Recognizer en Azure Portal.

**Paso 2:** Pruebe un inicio rápido para obtener experiencia práctica:
* [Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de los datos del formulario mediante la API REST con cURL](quickstarts/curl-train-extract.md)
* [Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de datos del formulario mediante la API REST con Python](quickstarts/python-train-extract.md)

Se recomienda que use el servicio gratuito cuando esté aprendiendo la tecnología, pero tenga en cuenta que el número de páginas gratuitas se limita a 500 al mes.

**Paso 3:** Revisión de las API REST

Use las siguientes API REST para entrenar y extraer datos estructurados de los formularios.

| API DE REST | DESCRIPCIÓN |
|-----|-------------|
| Train | Entrene un nuevo modelo para analizar los formularios usando cinco formularios del mismo tipo o un formulario vacío.  |
| Análisis  |Analice un documento único pasado como una secuencia para extraer pares clave-valor y tablas del formulario con su modelo personalizado.  |

Explore el [documento de referencia de la API REST](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

El servicio se ofrece como [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) de un servicio de Azure bajo los [términos de los servicios en línea](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Form Recognizer deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Realice un [inicio rápido](quickstarts/curl-train-extract.md) para empezar a usar las [API de Form Recognizer](https://aka.ms/form-recognizer/api).
