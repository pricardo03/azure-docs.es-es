---
title: 'Inicio rápido: Creación de un comando personalizado (versión preliminar) - servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará y probará una aplicación de comandos personalizados hospedada.
services: cognitive-services
author: donkim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 5c90c91791af8a9a16039e9650765bd24433ff38
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815817"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Inicio rápido: Creación de un comando personalizado (versión preliminar)

En este artículo, obtendrá información sobre cómo crear y probar una aplicación de comandos personalizados hospedada.
La aplicación reconocerá una expresión como "enciende el televisor" y responderá con un mensaje sencillo como "de acuerdo, encenderé el televisor".

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Voz. [Prueba gratuita del servicio Voz](~/articles/cognitive-services/speech-service/get-started.md).

  > [!NOTE]
  > Durante la versión preliminar, solo se admite la región westus2 para las claves de suscripción.

- Una clave de creación de [Language Understanding](https://www.luis.ai/home) (LUIS):
  1. Abra un explorador web y vaya a [Azure Portal](https://portal.azure.com).
  1. Seleccione Crear un recurso.
  1. Busque y seleccione [Language Understanding](https://aka.ms/sc-luis-all).
  1. Seleccionar Creación en las opciones de creación.
  1. Una vez implementado el recurso, diríjase a él y copie la clave de la sección Inicio rápido o Claves.

      > [!div class="mx-imgBorder"]
      > ![Creación de un recurso de creación](media/custom-speech-commands/resources-lu-authoring.png)

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Navegación a Speech Studio para los comandos personalizados

1. Abra el explorador web y navegue a [Speech Studio](https://speech.microsoft.com/).
1. Introduzca sus credenciales para iniciar sesión en el portal.

   - La vista predeterminada es la lista de suscripciones de Voz.
     > [!NOTE]
     > Si no ve la página para seleccionar una suscripción, puede navegar a ella al seleccionar "Recursos de voz" en el menú de configuración de la barra superior.

1. Seleccione su suscripción a voz y, a continuación, seleccione **Ir a Studio**.
1. Seleccione **Comandos personalizados (versión preliminar)** .

La vista predeterminada es una lista de las aplicaciones de comandos personalizados que ha creado.

## <a name="create-a-custom-commands-project"></a>Creación de un proyecto de comandos personalizados

1. Seleccione **Nuevo proyecto** para crear un nuevo proyecto.

   > [!div class="mx-imgBorder"]
   > ![Crear proyecto](media/custom-speech-commands/create-new-project.png)

1. Escriba el nombre del proyecto y el idioma y, a continuación, seleccione **Siguiente** para continuar.
1. Escriba su clave de creación de LUIS.
1. Una vez creada, seleccione el proyecto.

La vista ahora debe mostrar una visión general de la aplicación de comandos personalizados.

## <a name="create-a-new-command"></a>Creación de un nuevo comando

Ahora puede crear un comando. Vamos a usar un ejemplo que tomará una sola expresión (`turn on the tv`) y responderá con el mensaje `Ok, turning on the TV`.

1. Cree un nuevo comando. Para ello, seleccione el icono de `+` junto a comandos y asígnele el nombre `TurnOn`.
1. Seleccione **Guardar**.

> [!div class="mx-imgBorder"]
> ![Creación de un comando](media/custom-speech-commands/create-add-command.png)

Un comando es un conjunto de:

| Grupo            | DESCRIPCIÓN                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Oraciones de ejemplo | Expresiones de ejemplo que el usuario puede decir para desencadenar este comando.                                                                 |
| Parámetros       | Información necesaria para completar el comando.                                                                                |
| Reglas de finalización | Acciones que se van a realizar para finalizar el comando. Por ejemplo, para responder al usuario o comunicarse con otro servicio web. |
| Reglas avanzadas   | Reglas adicionales para controlar situaciones más específicas o complejas.                                                              |

### <a name="add-a-sample-sentence"></a>Adición de una oración de ejemplo

Se comienza con las oraciones de ejemplo y se proporciona un ejemplo de lo que el usuario puede decir:

```
turn on the tv
```

Por ahora, no hay ningún parámetro, así que es posible continuar con las reglas de finalización.

### <a name="add-a-completion-rule"></a>Adición de una regla de finalización

Ahora, agregue una regla de finalización para responder al usuario que una acción se está llevando a cabo.

> [!div class="mx-imgBorder"]
> ![Creación de una regla de finalización](media/custom-speech-commands/create-basic-completion-response-rule.png)

| Configuración    | Valor sugerido                        | DESCRIPCIÓN                                        |
| ---------- | -------------------------------------- | -------------------------------------------------- |
| Nombre de la regla  | "ConfirmationResponse"                 | Nombre que describe el propósito de la regla.          |
| Condiciones | None                                   | Condiciones que determinan cuándo se puede ejecutar la regla.    |
| Acciones    | SpeechResponse "Ok, turning on the TV" (de acuerdo, encenderé el televisor) | Acción que se realizará cuando la condición de la regla sea true. |

## <a name="try-it-out"></a>Prueba

Pruebe el comportamiento mediante el panel de conversación de prueba.

> [!div class="mx-imgBorder"]
> ![Prueba con webchat](media/custom-speech-commands/create-basic-test-chat.png)

- Si escribe: "turn on the tv" (enciende el televisor)
- La respuesta esperada: "Ok, turning on the TV" (de acuerdo, encenderé el televisor)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
