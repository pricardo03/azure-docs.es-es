---
title: Registro de Speech SDK - servicios de voz
titleSuffix: Azure Cognitive Services
description: Habilitar el registro en el SDK de voz.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: amishu
ms.openlocfilehash: 75eaea22c4809eda78e54514961d13113b4a5f3a
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012453"
---
# <a name="enable-logging-in-the-speech-sdk"></a>Habilitar el registro en el SDK de voz

Registro en el archivo es una característica opcional para el SDK de voz. Durante el desarrollo de registro proporciona información adicional y el diagnóstico de los componentes principales de Speeck SDK. Se puede habilitar estableciendo la propiedad `Speech_LogFilename` en un objeto de configuración de voz para la ubicación y el nombre del archivo de registro. El registro se activará globalmente una vez creado un módulo de reconocimiento de que la configuración y no se puede deshabilitar posteriormente. No se puede cambiar el nombre de un archivo de registro durante un sesión de registro de ejecución.

> [!NOTE]
> El registro está disponible en todos los SDK de voz admitidos lenguajes de programación, con la excepción de JavaScript.

## <a name="sample"></a>Muestra

El nombre de archivo de registro se especifica en un objeto de configuración. Tomando el `SpeechConfig` como ejemplo y suponiendo que haya creado una instancia denominada `config`:

```csharp
config.SetProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```java
config.setProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```C++
config->SetProperty(PropertyId::Speech_LogFilename, "LogfilePathAndName");
```

```Python
config.set_property(speechsdk.PropertyId.Speech_LogFilename, "LogfilePathAndName")
```

```objc
[config setPropertyTo:@"LogfilePathAndName" byId:SPXSpeechLogFilename];
```

Puede crear un módulo de reconocimiento en el objeto de configuración. Esto permitirá que el registro de todos los reconocedores.

> [!NOTE]
> Si creas un `SpeechSynthesizer` desde el objeto de configuración, no se habilitará el registro. Si está habilitado el registro sin embargo, también recibirá los diagnósticos necesarios desde el `SpeechSynthesizer`.

## <a name="create-a-log-file-on-different-platforms"></a>Crear un archivo de registro en distintas plataformas

Para Windows o Linux, puede ser el archivo de registro en cualquier ruta de acceso que el usuario tiene permiso de escritura. Permisos de escritura en ubicaciones del sistema de archivos en otros sistemas operativos pueden ser limitados o restringidos de forma predeterminada.

### <a name="universal-windows-platform-uwp"></a>Plataforma universal de Windows (UWP)

Las aplicaciones de UWP requieren sitios de los archivos de registro en una de las ubicaciones de datos de aplicación (locales, móviles o temporales). Un archivo de registro se puede crear en la carpeta de aplicación local:

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile logFile = await storageFolder.CreateFileAsync("logfile.txt", CreationCollisionOption.ReplaceExisting);
config.SetProperty(PropertyId.Speech_LogFilename, logFile.Path);
```

Más sobre el acceso de archivo está disponible permiso para las aplicaciones de UWP [aquí](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

### <a name="android"></a>Android

Puede guardar un archivo de registro para el almacenamiento interno, almacenamiento externo o el directorio de caché. Archivos creados en el almacenamiento interno o el directorio de caché son privados para la aplicación. Es preferible crear un archivo de registro en almacenamiento externo.

```java
File dir = context.getExternalFilesDir(null);
File logFile = new File(dir, "logfile.txt");
config.setProperty(PropertyId.Speech_LogFilename, logFile.getAbsolutePath());
```

El código anterior guardará un archivo de registro en el almacenamiento externo en la raíz de un directorio específico de la aplicación. Un usuario puede tener acceso al archivo con el Administrador de archivos (normalmente en `Android/data/ApplicationName/logfile.txt`). El archivo se eliminará cuando se desinstala la aplicación.

También deberá solicitar `WRITE_EXTERNAL_STORAGE` permiso en el archivo de manifiesto:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="...">
  ...
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  ...
</manifest>
```

Más acerca de los datos y archivo de almacenamiento para las aplicaciones de Android está disponible [aquí](https://developer.android.com/guide/topics/data/data-storage.html).

#### <a name="ios"></a>iOS

Solo los directorios dentro del espacio aislado de la aplicación son accesibles. Archivos pueden crearse en los documentos, biblioteca y directorios temporales. Archivos en el directorio de documentos pueden estar disponibles para un usuario. El fragmento de código siguiente muestra la creación de un archivo de registro en el directorio de documentos de la aplicación:

```objc
NSString *filePath = [
  [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject]
    stringByAppendingPathComponent:@"logfile.txt"];
[speechConfig setPropertyTo:filePath byId:SPXSpeechLogFilename];
```

Para acceder a un archivo creado, agregue la siguientes propiedades para el `Info.plist` lista de propiedades de la aplicación:

```xml
<key>UIFileSharingEnabled</key>
<true/>
<key>LSSupportsOpeningDocumentsInPlace</key>
<true/>
```

Más sobre iOS está disponible del sistema de archivos [aquí](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar nuestros ejemplos en GitHub](https://aka.ms/csspeech/samples)

