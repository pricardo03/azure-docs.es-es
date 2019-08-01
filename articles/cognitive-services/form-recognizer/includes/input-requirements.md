---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 73a7795629a94395b43bfca191cec7b2c4773611
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68481558"
---
Form Recognizer trabaja con documentos de entrada que cumplen estos requisitos:

* El formato debe ser JPG, PNG o PDF (texto o digitalizado). Los PDF insertados de texto son mejores porque no hay posibilidad de error en la extracción de caracteres y en la ubicación.
* Si los archivos PDF están bloqueados con contraseña, debe quitar el bloqueo antes de enviarlos.
* El tamaño del archivo debe ser inferior a 4 megabytes (MB).
* Para las imágenes, las dimensiones deben tener entre 50 x 50 píxeles y 4200 x 4200 píxeles.
* Si se digitalizan desde documentos en papel, los formularios deben ser digitalizaciones de alta calidad.
* El texto debe usar el alfabeto latino (caracteres ingleses).
* Los datos deben contener claves y valores.
* Las claves pueden aparecer arriba o a la izquierda de los valores, pero no abajo o a la derecha.

Form Recognizer no admite actualmente estos tipos de datos de entrada:

* Tablas complejas (tablas anidadas, encabezados combinados o celdas, etc.)
* Casillas o botones de radio
* Documentos PDF de más de 50 páginas