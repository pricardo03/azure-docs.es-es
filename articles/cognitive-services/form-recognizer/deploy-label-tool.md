---
title: Implementación de la herramienta de etiquetado de ejemplo de Form Recognizer
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre las distintas formas en que puede implementar la herramienta de etiquetado de ejemplo de Form Recognizer para que lo ayude con el aprendizaje supervisado.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: pafarley
ms.openlocfilehash: fa419d7dd9668ac2ce8f2b0eb904117c7e22692d
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2020
ms.locfileid: "78207834"
---
# <a name="deploy-the-sample-labeling-tool"></a>Implementación de la herramienta de etiquetado de ejemplo

La herramienta de etiquetado de ejemplo de Form Recognizer es una aplicación que se ejecuta en un contenedor de Docker. Proporciona una interfaz de usuario útil que se puede usar para etiquetar manualmente documentos de formulario con fines de aprendizaje supervisado. En el inicio rápido [Entrenamiento con etiquetas](./quickstarts/label-tool.md) se muestra cómo ejecutar la herramienta en el equipo local, que es el escenario más común. 

En esta guía se explican otras formas de implementar y ejecutar la herramienta de etiquetado de ejemplo. 

## <a name="deploy-with-azure-container-instances"></a>Implementación con Azure Container Instances

Puede ejecutar la herramienta de etiquetado en un contenedor de aplicación web de Docker. En primer lugar, [cree un recurso de aplicación web](https://ms.portal.azure.com/#create/Microsoft.WebSite) en Azure Portal. Rellene el formulario con los detalles de la suscripción y del grupo de recursos. Escriba la siguiente información en los campos obligatorios:
* **Publicar**: Contenedor de Docker
* Sistema **operativo**: Linux

En la página siguiente, rellene los campos siguientes para la configuración del contenedor de Docker:

* **Opciones**: Contenedor único
* **Origen de la imagen**: Azure Container Registry
* **Tipo de acceso**: público
* **Imagen y etiqueta**: mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest

Los pasos siguientes son opcionales. Una vez finalizada la implementación de la aplicación, puede ejecutarla y acceder a la herramienta de etiquetado en línea.

### <a name="connect-to-azure-ad-for-authorization"></a>Conexión a Azure AD para la autorización

Se recomienda conectar la aplicación web a Azure Active Directory (AAD) para que solo un usuario con sus credenciales pueda iniciar sesión y usar la aplicación. Siga las instrucciones sobre cómo [configurar una aplicación de App Service](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) para conectarse a AAD.

## <a name="next-steps"></a>Pasos siguientes

Vuelva al inicio rápido [Entrenamiento con etiquetas](./quickstarts/label-tool.md) para aprender a usar la herramienta para etiquetar manualmente los datos de entrenamiento y realizar el aprendizaje supervisado.
