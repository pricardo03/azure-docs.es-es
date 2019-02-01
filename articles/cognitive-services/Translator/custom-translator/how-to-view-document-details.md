---
title: 'Detalles de los documentos: Custom Translator'
titleSuffix: Azure Cognitive Services
description: En la página de la lista de documentos se muestran los 10 primeros documentos en el área de trabajo. Para cada uno de los documentos, se muestra el nombre, emparejamiento, tipo, idioma, marca de tiempo de carga y la dirección de correo electrónico del usuario que ha cargado el documento.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: how to edit a model
ms.openlocfilehash: e656c6dd7050851eb9885fe253ab29f4e3675216
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219811"
---
# <a name="view-document-details"></a>Ver detalles del documento

En la página de la lista de documentos se muestran los 10 primeros documentos en el área de trabajo. Para cada uno de los documentos, se muestra el nombre, emparejamiento, tipo, idioma, marca de tiempo de carga y la dirección de correo electrónico del usuario que ha cargado el documento.

Haga clic en un documento individual para ver la página de detalles del documento. La página de detalles del documento muestra la lista de las oraciones extraídas del documento.

- De forma predeterminada, está seleccionado el idioma de "origen" en el campo con el menú desplegable, pero puede cambiar la selección para ver las oraciones del idioma de destino. 
- Se muestran 20 oraciones por página de forma predeterminada. Puede usar el control de paginación para desplazarse entre las páginas.

![detalles del documento](media/how-to/how-to-view-document-details.png)

## <a name="delete-a-document"></a>Eliminar un documento

El usuario debe ser propietario del área de trabajo para eliminar un documento. Además, si un modelo está usando el documento, ya sea durante el proceso de entrenamiento o durante el proceso de implementación, dicho documento no podrá eliminarse.

1. Vaya a la página del documento
2.  Mantenga el mouse sobre cualquier registro de documento y haga clic en el icono de papelera.

    ![Eliminación de un documento](media/how-to/how-to-delete-document-1.png)

3.  Confirme la eliminación.

    ![Confirmación de la eliminación](media/how-to/how-to-delete-document-confirm.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [cómo entrenar un modelo](how-to-train-model.md).
