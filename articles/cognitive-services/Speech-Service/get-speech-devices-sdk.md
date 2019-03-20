---
title: Obtener el SDK de dispositivos de voz
titleSuffix: Azure Cognitive Services
description: El servicio de Voz funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. En este artículo obtendrá información sobre cómo acceder al SDK de dispositivos de voz y empezar a desarrollar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 3c5874625ee9d1932c401238c1586ad89d5d206d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57856729"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Obtener el SDK de dispositivos de voz de Cognitive Services

El SDK de dispositivos de voz está en versión preliminar restringida y requiere que esté inscrito en el programa. En la actualidad, Microsoft prefiere a las grandes empresas como candidatos para el acceso a este producto.

## <a name="request-access"></a>Solicitar acceso

Obtener acceso al SDK de dispositivos de voz:

1. Vaya al [formulario de registro](https://aka.ms/sdsdk-signup) del SDK de dispositivos de voz de Microsoft.
1. Lea el [contrato de licencia](speech-devices-sdk-license.md).
1. Si acepta los términos del contrato de licencia, seleccione **Acepto**.
1. Responda a las preguntas del formulario.
1. Envíe el formulario.
1. Si su dirección de correo electrónico todavía no forma parte de Azure Active Directory (Azure AD), recibirá un correo electrónico de invitación como el siguiente cuando se apruebe su acceso. Si la dirección de correo electrónico ya está en Azure AD, recibirá un mensaje de correo electrónico del equipo de Microsoft Speech cuando se apruebe su acceso y podrá ir directamente a la [descarga del SDK de dispositivos de voz](#download-the-speech-devices-sdk).

## <a name="approval-e-mail"></a>Correo electrónico de aprobación

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com>
Subject: You're invited to the Microsoft organization
```

![Mensaje de correo electrónico](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Aceptación de acceso

Complete los pasos siguientes para unirse a Azure AD con la dirección de correo electrónico que proporcionó durante el registro. Este proceso concede acceso al [sitio de descarga](https://shares.datatransfer.microsoft.com/) del SDK de dispositivos de voz.

1. Haga clic en **Comenzar** en el mensaje de correo electrónico que recibió. Si la organización ya es un cliente de Office 365, se le pedirá que inicie sesión y puede ir directamente al paso 7.

2. En la ventana del explorador que se abrirá, seleccione **Siguiente**.

    ![Ventana de autenticación](media/speech-devices-sdk/get-sdk-2.png)

3. Cree una cuenta de Microsoft si aún no tiene ninguna. Escriba la misma dirección de correo electrónico en la que recibió el correo electrónico de invitación.

    ![Creación de una cuenta Microsoft](media/speech-devices-sdk/get-sdk-3.png)

4. Seleccione **Siguiente** para crear una contraseña.

5. Cuando se le solicite que compruebe su correo electrónico, use el código de verificación del correo electrónico de invitación que recibió.

7. Pegue o escriba el código de seguridad del mensaje de correo electrónico en el cuadro de diálogo. En este ejemplo, el código de seguridad es **8406**. Seleccione **Next** (Siguiente).

    ![Comprobar el correo electrónico](media/speech-devices-sdk/get-sdk-6.png)

8. Cuando aparezca la aplicación del panel de acceso en el explorador, habrá confirmado que la dirección de correo electrónico forma parte de Azure AD. Ahora tiene acceso al sitio de descarga del SDK de dispositivos de voz.

## <a name="download-the-speech-devices-sdk"></a>Descargar el SDK de dispositivos de voz

Vaya al [sitio de descarga del SDK de dispositivos de voz](https://shares.datatransfer.microsoft.com/). Inicie sesión en la cuenta de Microsoft que creó anteriormente.

![Sitio de descarga del SDK](media/speech-devices-sdk/get-sdk-7.png)

Para descargar el SDK de dispositivos de voz, el código de ejemplo asociado y el material de referencia:

1. Descargue e instale la herramienta Aspera Connect cuando el explorador se lo pida.

    ![Descargar Aspera Connect](media/speech-devices-sdk/get-sdk-8.png)

1. Haga clic en **Sí** para cambiar las aplicaciones a Aspera Connect.

    ![Cambiar a Aspera Connect](media/speech-devices-sdk/get-sdk-9.png)

1. Seleccione **Permitir** para confirmar la descarga de los archivos con Aspera Connect.

    ![Descargar mediante Aspera Connect](media/speech-devices-sdk/get-sdk-10.png)

1. Cierre la ventana de transferencias de Aspera Connect cuando se hayan descargado los archivos.

    ![Ventana de transferencias de Aspera Connect](media/speech-devices-sdk/get-sdk-11.png)

De forma predeterminada, los archivos se descargan en la carpeta **Descargas**. Ya puede cerrar la sesión en este sitio.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al SDK de dispositivos de voz](speech-devices-sdk-qsg.md)
