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
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 056dd4331d30335078ea68350f711e37a7b42070
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976628"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Inicio rápido: Creación de un comando personalizado (versión preliminar)

En este artículo, obtendrá información sobre cómo crear y probar una aplicación de comandos personalizados hospedada.
La aplicación reconocerá una expresión como "enciende el televisor" y responderá con un mensaje sencillo como "de acuerdo, encenderé el televisor".

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Voz. [Prueba gratuita del servicio Voz](~/articles/cognitive-services/speech-service/get-started.md).

  > [!NOTE]
  > Durante la versión preliminar, solo se admite la región westus2 para las claves de suscripción.

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
   > ![Creación de un proyecto](media/custom-speech-commands/create-new-project.png)

1. Escriba el nombre del proyecto y el lenguaje.
1. Seleccione un recurso de creación. Si no hay ninguno válido, créelo. Para ello, debe seleccionar **Crear nuevo recurso**.

   > [!div class="mx-imgBorder"]
   > ![Creación de un recurso](media/custom-speech-commands/create-new-resource.png)

   1. Escriba el nombre del recurso, el grupo, la ubicación y el plan de tarifa.

         > [!NOTE]
         > Para crear grupos de recursos, escriba el nombre del grupo de recursos deseado en el campo "Grupo de recursos". El grupo de recursos se creará cuando se seleccione **Crear**.

1. Haga clic en **Crear** para crear el proyecto.
1. Una vez que cree el proyecto, selecciónelo.

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

1. Cree una regla de finalización, para lo que debe seleccione el icono `+` que hay junto a las reglas de finalización.
1. Escriba el nombre de la regla.
1. Agregar una acción
   1. Cree una nueva acción de respuesta de voz, para lo que debe seleccionar el icono `+`, que se encuentra junto a las acciones, y, después, seleccionar `SpeechResponse`
   1. Escriba la respuesta.

   > [!NOTE]
   > El texto normal debe empezar por un guión. Para más información, consulte [aquí](https://aka.ms/sc-lg-format)

   > [!div class="mx-imgBorder"]
   > ![Creación de una respuesta de voz](media/custom-speech-commands/create-speech-response-action.png)

1. Haga clic en **Guardar** para guardar la regla

> [!div class="mx-imgBorder"]
> ![Creación de una regla de finalización](media/custom-speech-commands/create-basic-completion-response-rule.png)


| Configuración    | Valor sugerido                        | DESCRIPCIÓN                                        |
| ---------- | -------------------------------------- | -------------------------------------------------- |
| Nombre de la regla  | "ConfirmationResponse"                 | Nombre que describe el propósito de la regla.          |
| Condiciones | None                                   | Condiciones que determinan cuándo se puede ejecutar la regla.    |
| Acciones    | SpeechResponse "- Ok, turning on the TV" (De acuerdo, encenderé el televisor) | Acción que se realizará cuando la condición de la regla sea true. |

## <a name="try-it-out"></a>Prueba

Pruebe el comportamiento mediante el panel de conversación de prueba.

> [!div class="mx-imgBorder"]
> ![Prueba con chat en web](media/custom-speech-commands/create-basic-test-chat.png)

- Si escribe: "turn on the tv" (enciende el televisor)
- La respuesta esperada: "Ok, turning on the TV" (de acuerdo, encenderé el televisor)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
