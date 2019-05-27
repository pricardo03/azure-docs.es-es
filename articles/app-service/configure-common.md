---
title: 'Configuración de aplicaciones en el portal: Azure App Service'
description: Aprenda a configurar la configuración común para una aplicación de App Service en Azure portal.
keywords: servicio de aplicación de Azure, aplicación web, configuración de la aplicación, las variables de entorno
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: bcc970375120f76e4ec8a90f487d251296f92dba
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957916"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>Configurar una aplicación de App Service en Azure portal

En este tema se explica cómo configurar las opciones comunes para aplicaciones web, back-end móvil o aplicación de API mediante el [Azure Portal].

## <a name="configure-app-settings"></a>Configuración de aplicaciones

En App Service, use la configuración de la aplicación como variables de entorno. En el [Azure Portal], vaya a la página de administración de la aplicación. En el menú izquierdo de la aplicación, haga clic en **configuración** > **configuración de la aplicación**.

![Configuración de la aplicación](./media/configure-common/open-ui.png)

Para los desarrolladores ASP.NET y ASP.NET Core, configuración de la aplicación de configuración de App Service es como configurarlos en `<appSettings>` en *Web.config*, pero los valores de App Service invalidan la de *Web.config*. Puede mantener la configuración de desarrollo (por ejemplo, contraseña de MySQL local) en *Web.config*, pero los secretos de producción (por ejemplo, contraseña de base de datos de Azure para MySQL) seguros en App Service. El mismo código usa la configuración de desarrollo cuando se depura localmente, y utiliza los secretos de producción cuando se implementa en Azure.

Otras pilas de lenguaje, del mismo modo, obtención la configuración de la aplicación como variables de entorno en tiempo de ejecución. Para obtener pasos específicos de la pila de lenguaje, consulte:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Contenedores personalizados](containers/configure-custom-container.md#configure-environment-variables)

La configuración de la aplicación siempre se cifra cuando se almacena (cifrado en reposo).

> [!NOTE]
> Configuración de la aplicación también se puede resolver desde [Key Vault](/azure/key-vault/) mediante [hace referencia Key Vault](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Mostrar los valores ocultos

De forma predeterminada, los valores de configuración de la aplicación se ocultan en el portal para la seguridad. Para ver un valor oculto de una configuración de aplicación, haga clic en el **valor** campo de dicha configuración. Para ver los valores de todas las configuraciones de aplicación, haga clic en el **Mostrar valor** botón.

### <a name="add-or-edit"></a>Agregar o editar

Para agregar una nueva configuración de aplicación, haga clic en **nueva configuración de la aplicación**. En el cuadro de diálogo, puede [seguir la configuración a la ranura actual](deploy-staging-slots.md#which-settings-are-swapped).

Para editar una configuración, haga clic en el **editar** situado a la derecha.

Cuando termine, haga clic en **actualización**. No olvide hacer clic en **guardar** en el **configuración** página.

> [!NOTE]
> En un contenedor de Linux personalizado o en un contenedor de Linux de forma predeterminada, al igual que cualquier estructura de clave de JSON anidado en el nombre de la configuración de aplicación `ApplicationInsights:InstrumentationKey` debe configurarse en App Service como `ApplicationInsights__InstrumentationKey` para el nombre de clave. En otras palabras, cualquier `:` deben reemplazarse por `__` (subrayado doble).
>

### <a name="edit-in-bulk"></a>Editar en masa

Para agregar o editar la configuración de la aplicación en masa, haga clic en el **Advanced edit** botón. Cuando termine, haga clic en **actualización**. No olvide hacer clic en **guardar** en el **configuración** página.

Configuración de la aplicación tiene el formato JSON siguiente:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## <a name="configure-connection-strings"></a>Configurar cadenas de conexión

En el [Azure Portal], vaya a la página de administración de la aplicación. En el menú izquierdo de la aplicación, haga clic en **configuración** > **configuración de la aplicación**.

![Configuración de la aplicación](./media/configure-common/open-ui.png)

Para los desarrolladores ASP.NET y ASP.NET Core, las cadenas de conexión de configuración en App Service son como configurarlos en `<connectionStrings>` en *Web.config*, pero los valores establecidos en App Service invalidan la de *Web.config*. Puede mantener la configuración de desarrollo (por ejemplo, un archivo de base de datos) en *Web.config* y secretos de producción (por ejemplo, credenciales de base de datos SQL) seguros en App Service. El mismo código usa la configuración de desarrollo cuando se depura localmente, y utiliza los secretos de producción cuando se implementa en Azure.

Para otras pilas de idioma, es mejor usar [configuración de la aplicación](#configure-app-settings) en su lugar, dado que las cadenas de conexión requieren un formato especial en las claves de la variable a fin de acceder a los valores. Aquí es una excepción, sin embargo: determinados tipos de base de datos de Azure son una copia de seguridad junto con la aplicación si configura sus cadenas de conexión en la aplicación. Para obtener más información, consulte [lo que obtiene una copia de seguridad](manage-backup.md#what-gets-backed-up). Si no necesita esta copia de seguridad automatizada, a continuación, use la configuración de la aplicación.

En tiempo de ejecución, las cadenas de conexión están disponibles como variables de entorno, el prefijo con los siguientes tipos de conexión:

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL Database: `SQLAZURECONNSTR_`
* Personalizado: `CUSTOMCONNSTR_`

Por ejemplo, una cadena de conexión de MySql llamado *connectionstring1* posible tener acceso a la variable de entorno `MYSQLCONNSTR_connectionString1`. Para obtener pasos específicos de la pila de lenguaje, consulte:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Contenedores personalizados](containers/configure-custom-container.md#configure-environment-variables)

Las cadenas de conexión siempre se cifran cuando se almacenan (cifrado en reposo).

> [!NOTE]
> Las cadenas de conexión también se pueden resolver desde [Key Vault](/azure/key-vault/) mediante [hace referencia Key Vault](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Mostrar los valores ocultos

De forma predeterminada, los valores para las cadenas de conexión están ocultos en el portal para la seguridad. Para ver un valor de una cadena de conexión oculto, simplemente haga clic en el **valor** campo de esa cadena. Para ver los valores de todas las cadenas de conexión, haga clic en el **Mostrar valor** botón.

### <a name="add-or-edit"></a>Agregar o editar

Para agregar una nueva cadena de conexión, haga clic en **nueva cadena de conexión**. En el cuadro de diálogo, puede [ajustarse la cadena de conexión a la ranura actual](deploy-staging-slots.md#which-settings-are-swapped).

Para editar una configuración, haga clic en el **editar** situado a la derecha.

Cuando termine, haga clic en **actualización**. No olvide hacer clic en **guardar** en el **configuración** página.

### <a name="edit-in-bulk"></a>Editar en masa

Para agregar o editar las cadenas de conexión de forma masiva, haga clic en el **Advanced edit** botón. Cuando termine, haga clic en **actualización**. No olvide hacer clic en **guardar** en el **configuración** página.

Las cadenas de conexión tienen el formato JSON siguiente:

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>Configurar las opciones generales

En el [Azure Portal], vaya a la página de administración de la aplicación. En el menú izquierdo de la aplicación, haga clic en **configuración** > **configuración de la aplicación**.

![Configuración general](./media/configure-common/open-general.png)

En este caso, puede configurar algunas opciones comunes para la aplicación. Algunas configuraciones requieren le [escala hasta los planes de tarifa superior](web-sites-scale.md).

- **Configuración de la pila**: La pila de software para ejecutar la aplicación, incluidos el idioma y las versiones del SDK. Para aplicaciones de Linux y aplicaciones de contenedor personalizado, también puede establecer un archivo o un comando de inicio opcional.
- **Configuración de plataforma**: Le permite configurar opciones para la plataforma de hospedaje, incluidos:
    - **Valor de bits**: 32 bits o 64 bits.
    - **Protocolo WebSocket**: Para [ASP.NET SignalR] o [socket.io](https://socket.io/), por ejemplo.
    - **AlwaysOn**: Mantenga la aplicación cargada incluso cuando no hay ningún tráfico. Deberá habilitarlo para WebJobs continuos o WebJobs que se desencadenan mediante una expresión CRON.
    - **Versión de canalización administrada**: IIS [modo de canalización]. Establézcalo en **clásico** si tiene una aplicación heredada que requiere una versión anterior de IIS.
    - **Versión HTTP**: Establézcala en **2.0** para habilitar la compatibilidad con el protocolo [HTTPS/2](https://wikipedia.org/wiki/HTTP/2).
    > [!NOTE]
    > Los exploradores más modernos admiten el protocolo HTTP/2 sobre TLS únicamente, mientras que el tráfico no cifrado sigue usando HTTP/1.1. Para asegurarse de que el cliente exploradores conexión a la aplicación con HTTP/2, ya sea [comprar un certificado de App Service](web-sites-purchase-ssl-web-site.md) para el dominio personalizado de la aplicación o [enlazar un certificado de terceros](app-service-web-tutorial-custom-ssl.md).
    - **Afinidad ARR**: En una implementación de instancias múltiples, asegúrese de que el cliente se enrute a la misma instancia de la vida de la sesión. Puede establecer esta opción en **desactivar** para las aplicaciones sin estado.
- **Depuración**: Habilitar la depuración remota para [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug), [ASP.NET Core](/visualstudio/debugger/remote-debugging-azure), o [Node.js](containers/configure-language-nodejs.md#debug-remotely) aplicaciones. Esta opción se desactiva automáticamente después de 48 horas.
- **Certificados de cliente entrantes**: requerir certificados de cliente en [autenticación mutua](app-service-web-configure-tls-mutual-auth.md).

## <a name="configure-default-documents"></a>Configurar los documentos predeterminados

Esta configuración es solo para las aplicaciones de Windows.

En el [Azure Portal], vaya a la página de administración de la aplicación. En el menú izquierdo de la aplicación, haga clic en **configuración** > **documentos predeterminados**.

![Configuración general](./media/configure-common/open-documents.png)

El documento predeterminado es la página web que se muestra en la dirección URL raíz de un sitio Web. Se usa el primer archivo coincidente en la lista. Para agregar un nuevo documento predeterminado, haga clic en **nuevo documento**. No olvide hacer clic en **guardar**.

Si la aplicación usa módulos que enruten basados en dirección URL en lugar de usar contenido estático, no hay ninguna necesidad de documentos predeterminados.

## <a name="configure-path-mappings"></a>Configurar asignaciones de ruta de acceso

En el [Azure Portal], vaya a la página de administración de la aplicación. En el menú izquierdo de la aplicación, haga clic en **configuración** > **asignaciones de ruta**.

![Configuración general](./media/configure-common/open-path.png)

El **asignaciones de ruta** página muestra diferentes significados según el tipo de sistema operativo.

### <a name="windows-apps-uncontainerized"></a>Aplicaciones de Windows (uncontainerized)

Para las aplicaciones de Windows, puede personalizar las asignaciones de controlador IIS y las aplicaciones virtuales y directorios.

Asignaciones de controlador permiten agregar procesadores de script personalizado para controlar las solicitudes de determinadas extensiones de archivo. Para agregar un controlador personalizado, haga clic en **nuevo controlador**. Configure el controlador de la manera siguiente:

- **Extensión**. La extensión de archivo que desee controlar, como  *\*.php* o *handler.fcgi*.
- **Procesador de script**. La ruta de acceso absoluta del procesador de script para usted. Se procesan las solicitudes de archivos que coinciden con la extensión de archivo mediante el procesador de script. Utilice la ruta de acceso `D:\home\site\wwwroot` para hacer referencia al directorio raíz de la aplicación.
- **Argumentos**. Argumentos de línea de comandos opcionales para el procesador de script.

Cada aplicación tiene la ruta de acceso predeterminada (`/`) asignado a `D:\home\site\wwwroot`, donde el código se implementa de forma predeterminada. Si es la raíz de la aplicación en una carpeta diferente, o si el repositorio tiene más de una aplicación, puede editar o agregar aplicaciones virtuales y directorios aquí. Haga clic en **nueva aplicación o directorio virtual**.

Para configurar aplicaciones virtuales y directorios, especifique cada directorio virtual y su correspondiente ruta física relativa a la raíz del sitio Web (`D:\home`). De manera opcional, puede activar la casilla **Aplicación** para marcar un directorio virtual como una aplicación.

### <a name="containerized-apps"></a>Aplicaciones en contenedores

También puede [agregar almacenamiento personalizado para la aplicación en contenedor](containers/how-to-serve-content-from-azure-storage.md). Aplicaciones en contenedores incluyen todas las aplicaciones de Linux y también los contenedores personalizados de Windows y Linux que se ejecuta en App Service. Haga clic en **nuevo almacenamiento de Azure montar** y configure el almacenamiento personalizado como sigue:

- **Nombre**: El nombre para mostrar.
- **Las opciones de configuración**: **Básico** o **avanzada**.
- **Cuentas de almacenamiento**: La cuenta de almacenamiento con el contenedor que desee.
- **Storage type** (Tipo de almacenamiento): **Los Blobs de Azure** o **archivos Azure**.
  > [!NOTE]
  > Aplicaciones de contenedor de Windows solo admiten archivos de Azure.
- **Contenedor de almacenamiento**: Configuración básica, el contenedor que desee.
- **Nombre del recurso compartido**: Para la configuración avanzada, el archivo comparten el nombre.
- **Clave de acceso**: Para la configuración avanzada, la clave de acceso.
- **Ruta de acceso de montaje**: La ruta de acceso absoluta en el contenedor para montar el almacenamiento personalizado.

Para obtener más información, consulte [servir contenido desde el almacenamiento de Azure en App Service en Linux](containers/how-to-serve-content-from-azure-storage.md).

## <a name="configure-language-stack-settings"></a>Configurar la configuración de idioma de pila

Las aplicaciones de Linux, consulte:

- [ASP.NET Core](containers/configure-language-dotnetcore.md)
- [Node.js](containers/configure-language-nodejs.md)
- [PHP](containers/configure-language-php.md)
- [Python](containers/how-to-configure-python.md)
- [Java](containers/configure-language-java.md)
- [Ruby](containers/configure-language-ruby.md)

## <a name="configure-custom-containers"></a>Configurar contenedores personalizados

Consulte [configurar un contenedor de Linux personalizado para Azure App Service](containers/configure-custom-container.md)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de un nombre de dominio personalizado en Azure App Service]
- [Configurar entornos de ensayo en Azure App Service]
- [Habilitación de HTTPS para una aplicación en Azure App Service]
- [Habilitar los registros de diagnóstico](troubleshoot-diagnostic-logs.md)
- [Escalado de una aplicación en Azure App Service]
- [Aspectos básicos de supervisión en Azure App Service]
- [Cambiar la configuración applicationHost.config con applicationHost.xdt](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Configuración de un nombre de dominio personalizado en Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Configurar entornos de ensayo en Azure App Service]: ./deploy-staging-slots.md
[Habilitación de HTTPS para una aplicación en Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[How to: Monitor web endpoint status]: https://go.microsoft.com/fwLink/?LinkID=279906
[Aspectos básicos de supervisión en Azure App Service]: ./web-sites-monitor.md
[modo de canalización]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Escalado de una aplicación en Azure App Service]: ./web-sites-scale.md
