---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 4079b0cab0126a9750bbb91ca7046aa18dbe9ece
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657869"
---
Form Recognizer trabaja con documentos de entrada que cumplen estos requisitos:

* El formato debe ser JPG, PNG o PDF (texto o digitalizado). Los PDF insertados de texto son mejores porque no hay posibilidad de error en la extracción de caracteres y en la ubicación.
* Si los archivos PDF están bloqueados con contraseña, debe quitar el bloqueo antes de enviarlos.
* El tamaño del archivo debe ser inferior a 4 MB.
* Para las imágenes, las dimensiones deben tener entre 600 x 100 píxeles y 4200 x 4200 píxeles.
* Si se digitalizan desde documentos en papel, los formularios deben ser digitalizaciones de alta calidad.
* El texto debe usar el alfabeto latino (caracteres ingleses).
* Los datos deben contener claves y valores.
* Las claves pueden aparecer arriba o a la izquierda de los valores, pero no abajo o a la derecha.

Form Recognizer no admite actualmente estos tipos de datos de entrada:

* Tablas complejas (tablas anidadas, encabezados combinados o celdas, etc.)
* Casillas o botones de radio
* Documentos PDF de más de 50 páginas
