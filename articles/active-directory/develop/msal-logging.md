---
title: Registro en aplicaciones de MSAL | Azure
description: Aprenda a registrar aplicaciones de la biblioteca de autenticación de Microsoft (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/22/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02c093375ba2dc5c851a2deb35bdea28338ee982
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70135780"
---
# <a name="logging"></a>Registro
Aplicaciones de la biblioteca de autenticación de Microsoft (MSAL) para generar mensajes de registro que pueden ayudar a diagnosticar problemas y proporcionan detalles. Una aplicación puede configurar el registro con unas cuentas líneas de código y tener un control personalizado sobre el nivel de detalle y si se registran o no datos de la organización y personales. Se recomienda que establezca una devolución de llamada de registro de MSAL y que proporcione una forma de que los usuarios envíen registros cuando tengan problemas de autenticación.

## <a name="logging-levels"></a>Niveles de registro

El registrador de MSAL permite la captura de varios niveles de detalle:

- Error: indica que algo no ha funcionado bien y se ha generado un error. Se usa para la depuración y la identificación de problemas.
- Advertencia: eventos que son de pregunta y sobre los que la aplicación necesita más información. No se ha producido necesariamente un error, pero es necesario realizar un diagnóstico e identificar los problemas.
- Info: MSAL registrará los eventos de carácter informativo, no relacionados necesariamente con la depuración.
- Verbose: Predeterminada. MSAL registrará una gran cantidad de información y proporcionará detalles completos sobre el comportamiento de la biblioteca.

## <a name="personal-and-organizational-data"></a>Datos personales y organizativos
De forma predeterminada, el registrador de MSAL no captura datos personales u organizativos sumamente delicados. La biblioteca ofrece la opción de habilitar el registro de datos personales y organizativos si decide hacerlo.

## <a name="logging-in-msalnet"></a>Registro en MSAL.NET

 > [!NOTE]
 > Para más información sobre MSAL.NET, consulte la [wiki de MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki). Obtenga ejemplos de registro de MSAL.NET y mucho más. 
 
En MSAL 3.x, el registro se establece por aplicación en el momento de crear la aplicación mediante el modificador de generador `.WithLogging`. Este método toma parámetros opcionales:

- *Level* le permite decidir el nivel de registro deseado. Si se establece en Errors solo se reciben errores.
- *PiiLoggingEnabled* permite registrar los datos personales y organizativos si establece en true. De forma predeterminada, se establece en false, por lo que la aplicación no registra datos personales.
- *LogCallback* se establece en un delegado que realiza el registro. Si *PiiLoggingEnabled* es true, este método recibirá los mensajes de dos veces: una vez con el parámetro *containsPii* igual a false y el mensaje sin datos personales y una segunda vez con el parámetro *containsPii* igual a true y el mensaje posiblemente con datos personales. En algunos casos (cuando el mensaje no contiene datos personales), el mensaje será el mismo.
- *DefaultLoggingEnabled* habilita el registro predeterminado de la plataforma. De forma predeterminada, es "false". Si se establece en true se usa el seguimiento de eventos en aplicaciones de escritorio o UWP, NSLog en iOS y logcat en Android.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsync().Result;
  }
 }
 ```

 ## <a name="logging-in-msaljs"></a>Registro en MSAL.js

 Puede habilitar el registro en MSAL.js si pasa un objeto de registrador durante la configuración para crear una instancia de `UserAgentApplication`. El objeto de registrador tiene las siguientes propiedades:

- *localCallback*: una instancia de devolución de llamada que puede proporcionar el desarrollador para consumir y publicar registros de una manera personalizada. Implemente el método localCallback según cómo quiera redirigir los registros.

- *level* (opcional): el nivel de registro configurable. Los niveles admitidos son: Error, Warning, Info, Verbose. El valor predeterminado es Info.

- *piiLoggingEnabled* (opcional): permite registrar datos personales y organizativos si se establece en true. De forma predeterminada, se establece en false de modo que la aplicación no registra datos personales. Los registros de datos personales nunca se escriben en salidas predeterminadas, como consola, Logcat o NSLog. El valor predeterminado se establece en false.

- *correlationId* (opcional): un identificador único, que se usa para asignar la solicitud con la respuesta para fines de depuración. El valor predeterminado es RFC4122 version 4 guid (128 bits).

```javascript

function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”,
    },
     system: {
             logger: new Msal.Logger(
                                loggerCallback ,{
                                     level: Msal.LogLevel.Verbose,
                                     piiLoggingEnabled: false,
                                     correlationId: '1234'
                                }
                        )
     }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```
