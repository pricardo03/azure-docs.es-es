---
title: Inicio de sesión en aplicaciones MSAL | Plataforma de identidad de Microsoft
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
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dad8a276cd40b1ff04bbced833b5d70cec4fc87
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268583"
---
# <a name="logging-in-msal-applications"></a>Inicio de sesión en aplicaciones de MSAL

Las aplicaciones de la biblioteca de autenticación de Microsoft (MSAL) generan mensajes de registro que pueden ayudar a diagnosticar problemas. Una aplicación puede configurar el registro con unas cuentas líneas de código y tener un control personalizado sobre el nivel de detalle y si se registran o no datos de la organización y personales. Se recomienda establecer una devolución de llamada del registro de MSAL y proporcionar una forma de que los usuarios envíen registros cuando tengan problemas de autenticación.

## <a name="logging-levels"></a>Niveles de registro

MSAL ofrece varios niveles de detalle de registro:

- Error: indica que algo no ha funcionado bien y se ha generado un error. Se usa para la depuración y la identificación de problemas.
- Advertencia: No se ha producido necesariamente un error, pero es necesario realizar un diagnóstico e identificar los problemas.
- Info: MSAL registrará los eventos de carácter informativo, no relacionados necesariamente con la depuración.
- Verbose: Predeterminada. MSAL registra todos los detalles del comportamiento de la biblioteca.

## <a name="personal-and-organizational-data"></a>Datos personales y organizativos

De forma predeterminada, el registrador de MSAL no captura datos personales u organizativos sumamente confidenciales. La biblioteca ofrece la opción de habilitar el registro de datos personales y organizativos si decide hacerlo.

## <a name="logging-in-msalnet"></a>Registro en MSAL.NET

 > [!NOTE]
 > Para más información sobre MSAL.NET, consulte la [wiki de MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki). Obtenga ejemplos de registro de MSAL.NET y mucho más.
 
En MSAL 3.x, el registro se establece por aplicación en el momento de crear la aplicación mediante el modificador de generador `.WithLogging`. Este método toma parámetros opcionales:

- *Level* le permite decidir el nivel de registro deseado. Si se establece en Errors solo se reciben errores.
- *PiiLoggingEnabled* permite registrar los datos personales y organizativos si establece en true. De forma predeterminada, se establece en false para que la aplicación no registre datos personales.
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

- `localCallback`: una instancia de devolución de llamada que el desarrollador puede proporcionar para consumir y publicar registros de una manera personalizada. Implemente el método localCallback según cómo quiera redirigir los registros.

- `level` (opcional): el nivel de registro configurable. Los niveles admitidos son: Error, Warning, Info, Verbose. El valor predeterminado es Info.

- `piiLoggingEnabled` (opcional): permite registrar datos personales y organizativos si se establece en true. De forma predeterminada, se establece en false para que la aplicación no registre datos personales. Los registros de datos personales nunca se escriben en salidas predeterminadas, como consola, Logcat o NSLog. El valor predeterminado se establece en false.

- `correlationId` (opcional): un identificador único, que se usa para asignar la solicitud con la respuesta para fines de depuración. El valor predeterminado es RFC4122 version 4 guid (128 bits).

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

## <a name="logging-in-msal-for-ios-and-macos"></a>Registro en MSAL para iOS y macOS

Establezca una devolución de llamada para capturar el registro de MSAL e incorporarlo al registro de su propia aplicación. La firma de la devolución de llamada tiene el siguiente aspecto:

```objc
/*!
    The LogCallback block for the MSAL logger
 
    @param  level           The level of the log message
    @param  message         The message being logged
    @param  containsPII     If the message might contain Personally Identifiable Information (PII)
                            this will be true. Log messages possibly containing PII will not be
                            sent to the callback unless PIllLoggingEnabled is set to YES on the
                            logger.

 */
typedef void (^MSALLogCallback)(MSALLogLevel level, NSString *message, BOOL containsPII);
```

Por ejemplo:

Objective-C
```objc
[MSALGlobalConfig.loggerConfig setLogCallback:^(MSALLogLevel level, NSString *message, BOOL containsPII)
    {
        if (!containsPII)
        {
#if DEBUG
            // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
            NSLog(@"MSAL log: %@", message);
#endif
        }
    }];
```

Swift
```swift
MSALGlobalConfig.loggerConfig.setLogCallback { (level, message, containsPII) in
    if let message = message, !containsPII
    {
#if DEBUG
    // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
    print("MSAL log: \(message)")
#endif
    }
}
```

### <a name="personal-identifiable-information-pii"></a>Información de identificación personal

De forma predeterminada, MSAL no captura ni registra información de identificación personal. La biblioteca permite a los desarrolladores de aplicaciones activar esta opción una propiedad en la clase MSALLogger. Al activar la información de identificación personal, la aplicación tiene la responsabilidad de controlar la información confidencial y cumplir con los requisitos normativos de forma segura.

Objective-C
```objc
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = YES;

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = NO;
```

Swift
```swift
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = true

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = false
```

### <a name="logging-levels"></a>Niveles de registro

Para establecer el nivel de registro al iniciar sesión con MSAL para iOS y macOS, use uno de los siguientes valores:

|Nivel  |DESCRIPCIÓN |
|---------|---------|
| `MSALLogLevelNothing`| Deshabilitar todos los registros |
| `MSALLogLevelError` | Nivel predeterminado; imprime información solo cuando se producen errores. |
| `MSALLogLevelWarning` | Advertencias |
| `MSALLogLevelInfo` |  Puntos de entrada de la biblioteca, con parámetros y varias operaciones de llavero. |
|`MSALLogLevelVerbose`     |  Seguimiento de API       |

Por ejemplo:

Objective-C
```objc
MSALGlobalConfig.loggerConfig.logLevel = MSALLogLevelVerbose;
 ```
 
 Swift
```swift
MSALGlobalConfig.loggerConfig.logLevel = .verbose
 ```

### <a name="log-message-format"></a>Formato de los mensajes de registro

En los mensajes de registro de MSAL, la parte del mensaje tiene el formato `TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message`.

Por ejemplo:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Proporcionar los identificadores de correlación y las marcas de tiempo son útiles para el seguimiento de problemas. La información del identificador de correlación y las marcas de tiempo está disponible en el mensaje de registro. El único lugar confiable para recuperarlos es en los mensajes de registro de MSAL.
