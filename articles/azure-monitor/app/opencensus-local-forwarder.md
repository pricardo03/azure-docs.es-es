---
title: Reenviador local OpenCensus de Azure Application Insights (versión preliminar)
description: Reenvío de seguimientos e intervalos distribuidos desde lenguajes como Python y Go a Azure Application Insights
ms.topic: conceptual
ms.date: 09/18/2018
ms.reviewer: nimolnar
ms.openlocfilehash: bcf7ba495897eb1c9b40c78f00825e863390b5d1
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77669971"
---
# <a name="local-forwarder-preview"></a>Reenviador local (versión preliminar)

Un reenviador local es un agente que recopila datos de telemetría de Application Insights o [OpenCensus](https://opencensus.io/) procedentes de diversos SDK y los enruta a Application Insights. Se puede ejecutar en Windows y Linux. También puede ejecutarlo en macOS, pero eso no está oficialmente admitido en este momento.

## <a name="running-local-forwarder"></a>Ejecución del reenviador local

El reenviador local es un [proyecto de código abierto en GitHub](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/releases). Hay diferentes formas de ejecutar el reenviador local en varias plataformas.

### <a name="windows"></a>Windows

#### <a name="windows-service"></a>Servicio de Windows

La manera más fácil de ejecutar el reenviador local en Windows es mediante su instalación como un servicio de Windows. La versión va acompañada de un archivo ejecutable de un servicio de Windows (*WindowsServiceHost/Microsoft.LocalForwarder.WindowsServiceHost.exe*) que se puede registrar fácilmente con el sistema operativo.

> [!NOTE]
> El servicio del reenviador local requiere .NET Framework 4.7 como mínimo. Si no tiene .NET Framework 4.7 el servicio se instalará pero no se iniciará. Para acceder a la versión más reciente de .NET Framework **[visite la página descarga de .NET Framework](
https://www.microsoft.com/net/download/dotnet-framework-runtime/net472?utm_source=getdotnet&utm_medium=referral)** .

1. Descargue el archivo LF.WindowsServiceHost.zip file de la [página de la versión del reenviador local](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/releases) de GitHub.

    ![Captura de pantalla de la página de descarga de la versión del reenviador local](./media/opencensus-local-forwarder/001-local-forwarder-windows-service-host-zip.png)

2. En este ejemplo, y solo para facilitar la demostración, vamos a extraer solo el archivo .zip en la ruta `C:\LF-WindowsServiceHost`.

    Para registrar el servicio y configurarlo para que se inicie al arrancar el sistema ejecute lo siguiente desde la línea de comandos como administrador:

    ```
    sc create "Local Forwarder" binpath="C:\LF-WindowsServiceHost\Microsoft.LocalForwarder.WindowsServiceHost.exe" start=auto
    ```
    
    Debería recibir esta respuesta:
    
    `[SC] CreateService SUCCESS`
    
    Para examinar el nuevo servicio mediante la interfaz gráfica de usuario de los servicios, escriba ``services.msc``
        
     ![Captura de pantalla del servicio de reenviador local](./media/opencensus-local-forwarder/002-services.png)

3. **Haga clic con el botón derecho** en el reenviador local nuevo y seleccione **Iniciar**. El servicio empezará ahora a ejecutarse.

4. De forma predeterminada, el servicio se crea sin ninguna acción de recuperación. Puede **hacer clic con el botón derecho** y seleccionar **Propiedades** > **Recuperación** para configurar respuestas automáticas ante un error del servicio.

    O bien, si lo prefiere, puede establecer opciones de recuperación automática mediante programación para cuando se produzcan los errores. Para ello, puede usar:

    ```
    sc failure "Local Forwarder" reset= 432000 actions= restart/1000/restart/1000/restart/1000
    ```

5. En la misma ubicación que su archivo ``Microsoft.LocalForwarder.WindowsServiceHost.exe`` que, en este ejemplo es ``C:\LF-WindowsServiceHost``, hay un archivo llamado ``LocalForwarder.config``. Se trata de un archivo basado en xml que le permite ajustar la configuración del reenviador local y especificar la clave de instrumentación del recurso de Application Insights del que desea reenviar los datos de seguimiento distribuidos. 

    Después de editar el archivo ``LocalForwarder.config`` para agregar la clave de instrumentación, no olvide reiniciar el **servicio de reenviador local** para permitir que los cambios surtan efecto.
    
6. Para confirmar que la configuración que desea está en vigor y que el reenviador local está escuchando los datos de seguimiento de la forma esperada, compruebe el archivo ``LocalForwarder.log``. Debería ver unos resultados parecidos a los que se pueden ver en la siguiente imagen de la parte inferior del archivo:

    ![Captura de pantalla del archivo LocalForwarder.log](./media/opencensus-local-forwarder/003-log-file.png)

#### <a name="console-application"></a>Aplicación de consola

Para ciertos casos de uso, podría ser útil ejecutar el reenviador local como una aplicación de consola. La versión incluye las siguientes versiones del archivo ejecutable del host de consola:
* un binario de .NET Core dependiente de la plataforma */ConsoleHost/publish/Microsoft.LocalForwarder.ConsoleHost.dll*. La ejecución de este binario requiere la instalación de un entorno en tiempo de ejecución de .NET Core. Consulte esta [página](https://www.microsoft.com/net/download/dotnet-core/2.1) de descarga para más información.
  ```batchfile
  E:\uncdrop\ConsoleHost\publish>dotnet Microsoft.LocalForwarder.ConsoleHost.dll
  ```
* un conjunto independiente de archivos binarios de .NET Core para las plataformas x86 y x64. Estos no requieren un entorno en tiempo de ejecución de .NET Core para ejecutarse. */ConsoleHost/win-x86/publish/Microsoft.LocalForwarder.ConsoleHost.exe*, */ConsoleHost/win-x64/publish/Microsoft.LocalForwarder.ConsoleHost.exe*.
  ```batchfile
  E:\uncdrop\ConsoleHost\win-x86\publish>Microsoft.LocalForwarder.ConsoleHost.exe
  E:\uncdrop\ConsoleHost\win-x64\publish>Microsoft.LocalForwarder.ConsoleHost.exe
  ```

### <a name="linux"></a>Linux

Al igual que pasaba con Windows, la versión incluye las siguientes versiones del archivo ejecutable del host de consola:
* un binario de .NET Core dependiente de la plataforma */ConsoleHost/publish/Microsoft.LocalForwarder.ConsoleHost.dll*. La ejecución de este binario requiere la instalación de un entorno en tiempo de ejecución de .NET Core. Consulte esta [página](https://www.microsoft.com/net/download/dotnet-core/2.1) de descarga para más información.

```batchfile
dotnet Microsoft.LocalForwarder.ConsoleHost.dll
```

* un conjunto independiente de archivos binarios de .NET Core para linux-64. Este no requiere un entorno en tiempo de ejecución de .NET Core para ejecutarse. */ConsoleHost/linux-x64/publish/Microsoft.LocalForwarder.ConsoleHost*.

```batchfile
user@machine:~/ConsoleHost/linux-x64/publish$ sudo chmod +x Microsoft.LocalForwarder.ConsoleHost
user@machine:~/ConsoleHost/linux-x64/publish$ ./Microsoft.LocalForwarder.ConsoleHost
```

Es posible que muchos de los usuarios de Linux deseen ejecutar el reenviador local como un demonio. Los sistemas Linux incluyen varias soluciones de administración de servicios como Upstart, sysv o systemd. Sea cual sea la versión específica, puede usarla para ejecutar el reenviador local de la forma más adecuada para su escenario.

Por ejemplo, vamos a crear un servicio de demonio mediante systemd. Vamos a usar la versión dependiente de la plataforma, pero se puede hacer lo mismo para una versión independiente.

* Cree el siguiente archivo de servicio denominado *localforwarder.service* y colóquelo en */lib/systemd/system*.
En este ejemplo se da por supuesto que su nombre de usuario es USUARIO_EJEMPLO y que copió los archivos binarios dependientes de la plataforma del reenviador local (desde */ConsoleHost/publicación*) a */home/USUARIO_EJEMPLO/LOCALFORWARDER_DIR*.

```
# localforwarder.service
# Place this file into /lib/systemd/system/
# Use 'systemctl enable localforwarder' to start the service automatically on each boot
# Use 'systemctl start localforwarder' to start the service immediately

[Unit]
Description=Local Forwarder service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=SAMPLE_USER
WorkingDirectory=/home/SAMPLE_USER/LOCALFORWARDER_DIR
ExecStart=/usr/bin/env dotnet /home/SAMPLE_USER/LOCALFORWARDER_DIR/Microsoft.LocalForwarder.ConsoleHost.dll noninteractive

[Install]
WantedBy=multi-user.target
```

* Ejecute el siguiente comando para indicar a systemd que debe iniciar el reenviador local en cada arranque

```
systemctl enable localforwarder
```

* Ejecute el siguiente comando para indicar a systemd que debe iniciar el reenviador local de forma inmediata

```
systemctl start localforwarder
```

* Para supervisar el servicio, inspeccione los archivos * *.log* del directorio /home/USUARIO_EJEMPLO/LOCALFORWARDER_DIR.

### <a name="mac"></a>Mac
El reenviador local puede funcionar con macOS, pero, en la actualidad, no está oficialmente admitido.

### <a name="self-hosting"></a>Autohospedaje
El reenviador local también se distribuye como un paquete NuGet de .NET Standard, lo que le permite hospedarlo dentro de su propia aplicación .NET.

```csharp
using Library;
...
Host host = new Host();

// see section below on configuring local forwarder
string configuration = ...;
    
host.Run(config, TimeSpan.FromSeconds(5));
...
host.Stop();
```

## <a name="configuring-local-forwarder"></a>Configuración del reenviador local

* Si ejecuta uno de los hosts propios del reenviador local (el host de consola o el host de servicio de Windows), verá que el archivo **LocalForwarder.config** se encuentra junto al archivo binario.
* Cuando se autohospeda el paquete NuGet del reenviador local, se debe proporcionar la configuración del mismo formato en el código (consulte la sección sobre autohospedaje). Para la sintaxis de configuración, compruebe el archivo [LocalForwarder.config](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/blob/master/src/ConsoleHost/LocalForwarder.config) en el repositorio de GitHub. 

> [!NOTE]
> La configuración puede cambiar según la versión, así que preste atención a qué versión está usando.

## <a name="monitoring-local-forwarder"></a>Supervisión del reenviador local

Los seguimientos se escriben en el sistema de archivos junto al ejecutable que ejecuta el reenviador local (busque archivos * *.log*). Puede colocar un archivo con el nombre de *NLog.config* junto al archivo ejecutable para proporcionar su propia configuración en lugar del predeterminado. Consulte la [documentación](https://github.com/NLog/NLog/wiki/Configuration-file#configuration-file-format) para obtener la descripción del formato.

Si no se proporciona ningún archivo de configuración (que es el valor predeterminado), el reenviador local usará la configuración predeterminada, que puede encontrar [aquí](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/blob/master/src/Common/NLog.config).

## <a name="next-steps"></a>Pasos siguientes

* [OpenCensus](https://opencensus.io/)
