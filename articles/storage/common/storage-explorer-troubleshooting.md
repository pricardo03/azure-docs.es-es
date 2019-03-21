---
title: Guía de solución de problemas del Explorador de Azure Storage | Microsoft Docs
description: Información general sobre técnicas de depuración para el Explorador de Azure Storage
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 3e26365c4273611c81682a760695522575f3875d
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225049"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Guía de solución de problemas de Azure Storage Explorer

Explorador de Microsoft Azure Storage es una aplicación independiente que permite trabajar fácilmente con los datos de Azure Storage en Windows, macOS y Linux. La aplicación puede conectarse a las cuentas de Storage hospedadas en Azure, nubes nacionales y Azure Stack.

Esta guía resume las soluciones para problemas comunes en el Explorador de Storage.

## <a name="role-based-access-control-permission-issues"></a>Problemas de permisos de Control de acceso basado en roles

[Control de acceso basado en roles (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) proporciona administración de acceso específico de recursos de Azure mediante la combinación de conjuntos de permisos en _roles_. Estas son algunas sugerencias que puede usar para poder trabajar en el Explorador de almacenamiento RBAC.

### <a name="what-do-i-need-to-see-my-resources-in-storage-explorer"></a>¿Qué es necesario ver los recursos en el Explorador de almacenamiento?

Si tiene problemas para acceder a los recursos de almacenamiento con RBAC, es posible que no se han asignado los roles correspondientes. Las siguientes secciones describen los permisos que el Explorador de Storage actualmente se requiere para tener acceso a los recursos de almacenamiento.

Si no está seguro de que tener los roles adecuados o permisos, póngase en contacto con el Administrador de cuenta de Azure.

#### <a name="read-listget-storage-accounts"></a>Lectura: Obtener una lista de las cuentas de almacenamiento

Debe tener permiso para enumerar cuentas de almacenamiento. Puede obtener este permiso cuando se asigna el rol "Lector".

#### <a name="list-storage-account-keys"></a>Obtener una lista de las claves de cuentas de almacenamiento

Explorador de Storage también puede usar las claves de cuenta para autenticar las solicitudes. Puede obtener acceso a las claves con roles más eficaces, como el rol "Colaborador".

> [!NOTE]
> Las claves de acceso conceden permisos sin restricciones a cualquier persona que disponga de ellos. Por lo tanto, generalmente no se recomienda entregarse a los usuarios de la cuenta. Si necesita revocar las claves de acceso, puede volver a generar desde la [Portal de Azure](https://portal.azure.com/).

#### <a name="data-roles"></a>Roles de datos

Debe asignarse al menos un rol que concede acceso a lee los datos de recursos. Por ejemplo, si tiene que mostrar o descargar blobs, necesitará al menos el rol "Lector de datos de Blob de almacenamiento".

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>¿Por qué necesito un rol de la capa de administración para ver los recursos en el Explorador de almacenamiento?

Almacenamiento de Azure tiene dos capas de acceso: _administración_ y _datos_. Las suscripciones y cuentas de almacenamiento se accede a través de la capa de administración. Contenedores, blobs y otros recursos de datos se accede a través de la capa de datos. Por ejemplo, si desea obtener una lista de las cuentas de almacenamiento de Azure, envíe una solicitud al punto de conexión de administración. Si desea obtener una lista de contenedores de blobs en una cuenta, envíe una solicitud al extremo de servicio adecuado.

Roles de RBAC pueden contener los permisos de acceso de capa de datos o administración. El rol "Lector", por ejemplo, concede recursos de nivel de administración de acceso de solo lectura.

En realidad, el rol "Lector" no proporciona ningún permiso de capa de datos y no es necesario para tener acceso a la capa de datos.

Explorador de Storage facilita acceso a los recursos mediante la recopilación de la información necesaria para conectarse a los recursos de Azure para usted. Por ejemplo, para mostrar los contenedores de blobs, Explorador de Storage envía una solicitud de lista de contenedores para el extremo de blob service. Para obtener ese punto de conexión, el Explorador de almacenamiento busca en la lista de suscripciones y cuentas de almacenamiento tiene acceso a. Sin embargo, para buscar sus suscripciones y cuentas de almacenamiento, el Explorador de Storage también necesita tener acceso a la capa de administración.

Si no tiene un rol que otorgue permisos de nivel de cualquier administración, el Explorador de almacenamiento no se puede obtener la información que necesita para conectarse a la capa de datos.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>¿Qué ocurre si no se puede obtener la administración de permisos de nivel necesario de mi administrador?

Aún no contamos con una solución de RBAC en este momento. Como alternativa, puede solicitar un URI de SAS [adjuntar a su recurso](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#attach-a-service-by-using-a-shared-access-signature-sas).

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Error: Certificado autofirmado de cadena de certificados (y errores similares)

Los errores de certificado están causados por una de las dos situaciones siguientes:

1. La aplicación se conecta a través de un "proxy transparente", lo que significa que un servidor (por ejemplo, el servidor de la empresa) intercepta el tráfico HTTPS, lo descifra y, luego, lo cifra mediante un certificado autofirmado.
2. Ejecuta una aplicación que inserta un certificado SSL autofirmado en los mensajes HTTPS que recibe. Ejemplos de aplicaciones que insertan certificados incluyen software antivirus y de inspección de tráfico de red.

Cuando el Explorador de Storage ve un certificado autofirmado o que no es de confianza, ya no podrá saber si se ha modificado el mensaje HTTPS recibido. Si tiene una copia del certificado autofirmado, puede indicar que el Explorador de Storage confíe en él con los pasos siguientes:

1. Obtención de una copia X.509 (.cer) codificada en Base-64 del certificado
2. Haga clic en **Editar** > **Certificados SSL** > **Importar certificados** y, luego, utilice el selector de archivos para encontrar, seleccionar y abrir el archivo .cer.

Este problema también puede ser el resultado de varios certificados (raíz e intermedio). Ambos certificados deben agregarse para solucionar el error.

Si no está seguro de dónde procede el certificado, puede intentar estos pasos para encontrarlo:

1. Instale Open SSL.
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (cualquiera de las versiones ligeras debería ser suficiente).
    * Mac y Linux: debe estar incluido con el sistema operativo.
2. Ejecute Open SSL.
    * Windows: abra el directorio de instalación, haga clic en **/bin/** y, luego, haga doble clic en **openssl.exe**.
    * Mac y Linux: ejecute **openssl** desde un terminal.
3. Ejecute `s_client -showcerts -connect microsoft.com:443`
4. Busque certificados autofirmados. Si no está seguro de qué certificados son autofirmados, busque cualquier lugar en el asunto `("s:")` y el emisor `("i:")` son los mismos.
5. Cuando haya encontrado cualquier certificado autofirmado, para cada uno, copie y pegue todo el contenido desde **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---** (inclusive) a un archivo .cer nuevo.
6. Abra el Explorador de Storage, haga clic en **Editar** > **Certificados SSL** > **Importar certificados** y, luego, utilice el selector de archivos para encontrar, seleccionar y abrir los archivos .cer que ha creado.

Si no se encuentra ningún certificado autofirmado siguiendo los pasos anteriores, póngase en contacto con nosotros mediante la herramienta de comentarios para más ayuda. Como alternativa, puede elegir iniciar el Explorador de Storage desde la línea de comandos con la marca `--ignore-certificate-errors`. Cuando se inicia con esta marca, el Explorador de Storage pasará por alto los errores de certificado.

## <a name="sign-in-issues"></a>Problemas de inicio de sesión

### <a name="blank-sign-in-dialog"></a>Diálogo de inicio de sesión en blanco

Los diálogos de inicio de sesión en blanco se suele deberse a ADFS que pide el Explorador de Storage para realizar una redirección, que no es compatible con Electron. Para solucionar este problema, puede intentar usar el flujo de código de dispositivo para el inicio de sesión. Para ello, siga los pasos que se indican a continuación:

1. "Ir a la vista previa" -> "Usar inicio de sesión de código de dispositivo".
2. Abra el cuadro de diálogo de conexión (ya sea mediante el icono de interruptor de la barra vertical izquierda o con "Agregar cuenta" en el panel de la cuenta).
3. Elija qué entorno que desea iniciar sesión en.
4. Haga clic en el botón "Iniciar sesión".
5. Siga las instrucciones del siguiente panel.

Si se descubre tiene problemas para iniciar sesión en la cuenta que desea utilizar porque ya está registrado el explorador predeterminado en una cuenta diferente, puede:

1. Copie manualmente el vínculo y el código en una sesión privada del explorador.
2. Copie manualmente el vínculo y el código en otro navegador distinto.

### <a name="reauthentication-loop-or-upn-change"></a>Bucle de reautenticación o cambio UPN

Si se encuentra en un bucle de reautenticación o ha cambiado el UPN de una de sus cuentas, intente lo siguiente:

1. Quite todas las cuentas y cierre el Explorador de Storage.
2. Elimine la carpeta .IdentityService de la máquina. En Windows, la carpeta se encuentra en `C:\users\<username>\AppData\Local`. En Mac y Linux, puede encontrar la carpeta en la raíz de su directorio de usuario.
3. Si está en Mac o Linux, también deberá eliminar la entrada Microsoft.Developer.IdentityService del almacén de claves de su sistema operativo. En Mac, el almacén de claves es la aplicación de "Gnome Keychain". En Linux, la aplicación normalmente se denomina "Keyring" pero el nombre puede ser diferente en función de su distribución.

### <a name="conditional-access"></a>Acceso condicional

El acceso condicional no se admite cuando se usa el Explorador de Storage en Windows 10, Linux o macOS. Esto se debe a una limitación en la biblioteca de AAD usada por el Explorador de Storage.

## <a name="mac-keychain-errors"></a>Errores de cadena de claves de Mac

En ocasiones, la cadena de claves de macOS puede entrar en un estado que ocasiona problemas en la biblioteca de autenticación del Explorador de Storage. Para obtener la cadena de claves fuera de este estado, pruebe los pasos siguientes:

1. Cierre el Explorador de Storage.
2. Abra la cadena de claves (**cmd + espacio**, escriba la cadena de claves y presione Entrar).
3. Seleccione la cadena de claves "login".
4. Haga clic en el icono de candado para bloquear la cadena de claves (el candado se moverá a una posición de bloqueo; esta operación puede tardar unos segundos según qué aplicaciones tenga abiertas).

    ![imagen](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Inicie el Explorador de Storage
6. Debería aparecer un elemento emergente en el que se indica algo parecido a esto: "El centro de servicio desea acceder a la cadena de claves". Cuando lo haga, escriba la contraseña de la cuenta de administrador de Mac y haga clic en **Permitir siempre** (o **Permitir** si **Permitir siempre** no está disponible).
7. Intente iniciar sesión.

### <a name="general-sign-in-troubleshooting-steps"></a>Pasos generales para solucionar problemas en inicio de sesión

* Si encuentra en Mac OS y nunca aparece la ventana de inicio de sesión a través de la "en espera para la autenticación de..." cuadro de diálogo, a continuación, intente [estos pasos](#mac-keychain-errors)
* Reinicio de Explorador de Storage
* Si la ventana de autenticación está en blanco, espere al menos un minuto antes de cerrar el cuadro de diálogo de autenticación.
* Asegúrese de que el proxy y el certificado están configurados correctamente tanto para la máquina como para el Explorador de Storage.
* Si se encuentra en Windows y tener acceso a Visual Studio 2017 en el mismo equipo e iniciar sesión, intente iniciar sesión en Visual Studio 2017. Después de un correcto inicio de sesión en Visual Studio 2017, debe ser capaz de abrir el Explorador de Storage y ver su cuenta en el panel de la cuenta.

Si ninguno de estos métodos funciona, [abra una incidencia en GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Suscripciones que faltan e inquilinos rotos

Si no puede recuperar las suscripciones después de iniciar sesión correctamente, siga los métodos siguientes de solución de problemas:

* Compruebe que la cuenta tiene acceso a las que espera. Puede comprobar el acceso al iniciar sesión en el portal para el entorno de Azure que está intentando usar.
* Asegúrese de que ha iniciado sesión con el entorno correcto de Azure (Azure, Azure China 21Vianet, Azure Alemania, Azure US Government, o entorno personalizado).
* Si está detrás de un proxy, asegúrese de que ha configurado correctamente el proxy del Explorador de Storage.
* Pruebe a quitar la cuenta y volver a agregarla.
* Si hay un vínculo "Más información", vea qué mensajes de error se notifican para los inquilinos en los que se producen errores. Si no está seguro de qué hacer con los mensajes de error que ve, si lo desea, [abra una incidencia en GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>No se puede quitar la cuenta adjunta o el recurso de almacenamiento

Si no puede quitar una cuenta adjunta o un recurso de almacenamiento a través de la interfaz de usuario, puede eliminar manualmente todos los recursos adjuntos mediante la eliminación de las siguientes carpetas:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
> Cierre el Explorador de Storage antes de eliminar las carpetas anteriores.

> [!NOTE]
> Si alguna vez ha importado un certificado SSL, haga una copia de seguridad del contenido del directorio `certs`. Después, puede utilizar la copia de seguridad para volver a importar los certificados SSL.

## <a name="proxy-issues"></a>Problemas de proxy

En primer lugar, asegúrese de que la siguiente información que especificó sea correcta:

* La dirección URL del proxy y el número de puerto
* El nombre de usuario y la contraseña si los exige el proxy

> [!NOTE]
> Explorador de Storage no admite archivos de configuración automática de proxy para la configuración de proxy.

### <a name="common-solutions"></a>Soluciones comunes

Si todavía tiene problemas, pruebe los siguientes métodos de solución de problemas:

* Si puede conectarse a Internet sin usar el proxy, compruebe que el Explorador de Storage funciona sin la configuración del proxy habilitada. Si este es así, puede haber un problema con la configuración de proxy. Trabaje con el administrador del servidor proxy para identificar los problemas.
* Compruebe que otras aplicaciones que utilicen el servidor proxy funcionen según lo esperado.
* Compruebe que puede conectarse al portal del entorno de Azure que está intentando usar
* Compruebe que puede recibir las respuestas de los puntos de conexión de servicio. Especifique una de las direcciones URL de punto de conexión en el explorador. Si puede conectarse, debería recibir un InvalidQueryParameterValue o una respuesta XML similar.
* Si otra persona también usa el Explorador de Storage con el servidor proxy, compruebe que pueda conectarse. Si se puede conectar, quizá deba ponerse en contacto con el administrador del servidor proxy.

### <a name="tools-for-diagnosing-issues"></a>Herramientas para el diagnóstico de problemas

Si dispone de herramientas de red, como Fiddler para Windows, es posible que pueda diagnosticar los problemas como se indica a continuación:

* Si tiene que trabajar a través del proxy, es posible que tenga que configurar la herramienta de red para conectarse a través del proxy.
* Compruebe el número de puerto utilizado por la herramienta de red.
* Escriba la dirección URL del host local y el número de puerto de la herramienta de red como configuración de proxy en el Explorador de Storage. Cuando lo haya hecho correctamente, la herramienta de red inicia el registro de las solicitudes de red realizadas por el Explorador de Storage para puntos de conexión de servicio y administración. Por ejemplo, escriba https://cawablobgrs.blob.core.windows.net/ para el punto de conexión de blob en un explorador y recibirá una respuesta similar a la siguiente, lo que sugiere que el recurso existe, aunque no se puede obtener acceso a él.

![ejemplo de código](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Póngase en contacto con el administrador del servidor proxy

Si es correcta la configuración de proxy, tendrá que ponerse en contacto con el administrador del servidor proxy y, además, deberá hacer lo siguiente:

* Asegúrese de que el proxy no bloquee el tráfico a los puntos de conexión de administración de Azure o de los recursos.
* Compruebe el protocolo de autenticación utilizado por el servidor proxy. En estos momentos, el Explorador de Storage no admite los servidores proxy NTLM.

## <a name="unable-to-retrieve-children-error-message"></a>Mensaje de error "No se pueden recuperar los elementos secundarios"

Si está conectado a Azure a través de un servidor proxy, compruebe que la configuración de proxy sea correcta. Si se conceden acceso a un recurso desde el propietario de la cuenta o suscripción, compruebe que ha leído o lista de permisos para ese recurso.

## <a name="connection-string-does-not-have-complete-configuration-settings"></a>La cadena de conexión no tiene una configuración completa

Si recibe este mensaje de error, es posible que no tenga los permisos necesarios para obtener las claves para la cuenta de almacenamiento. Para confirmar si este es el caso, vaya al portal y busque la cuenta de almacenamiento. Puede hacerlo rápidamente con el botón secundario en el nodo de la cuenta de almacenamiento y haciendo clic en "Abrir en Portal". Cuando haya hecho esto, vaya a la hoja "Claves de acceso". Si no tiene permisos para ver las claves, verá una página con el mensaje "No tiene acceso". Para solucionar este problema, puede obtener la clave de cuenta de otra persona y asociar con el nombre y la clave, o puede pedirle a alguien de la SAS en la cuenta de almacenamiento y usarlo para asociar la cuenta de almacenamiento.

Si ve las claves de cuenta, registre un problema en GitHub, por lo que podemos ayudarle a resolver el problema.

## <a name="issues-with-sas-url"></a>Problemas con la URL de SAS

Si se conecta a un servicio mediante una dirección URL de SAS y se produce este error:

* Compruebe que la dirección URL proporciona los permisos necesarios para leer o enumerar los recursos.
* Compruebe que la dirección URL no haya expirado.
* Si la dirección URL de SAS se basa en una directiva de acceso, compruebe que la directiva de acceso no haya sido revocada.

Si asoció accidentalmente una dirección URL de SAS no válida y no se puede desasociar, siga estos pasos:

1. Cuando se ejecute el Explorador de Storage, presione F12 para abrir la ventana de herramientas de desarrollador.
2. Haga clic en la pestaña Aplicación y, a continuación, haga clic en Almacenamiento Local > file:// en el árbol de la izquierda.
3. Busque la clave asociada al tipo de servicio del URI de SAS problemática. Por ejemplo, si el identificador URI de SAS incorrecto es para un contenedor de blobs, busque la clave llamada `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4. El valor de la clave debe ser una matriz JSON. Busque el objeto asociado con el URI incorrecto y quítelo.
5. Presione Ctrl + R para volver a cargar el Explorador de Storage.

## <a name="linux-dependencies"></a>Dependencias de Linux

En general, los paquetes siguientes son necesarios para ejecutar el Explorador de Storage en Linux:

* [Tiempo de ejecución de .NET core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libgnome-keyring-common` y `libgnome-keyring-dev`
* `libgconf-2-4`

Dependiendo de su distribución, puede haber diferentes o más paquetes necesite instalar.

Explorador de Storage es compatible oficialmente con Ubuntu 18.04, 16.04 y 14.04. Pasos de instalación de una máquina limpia son los siguientes:

# <a name="ubuntu-1804tab1804"></a>[Ubuntu 18.04](#tab/1804)

1. Descarga del explorador de almacenamiento
2. Instalar .NET Core Runtime, versión más reciente de comprobado es: [2.0.8](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/runtime-2.0.8) (si ya ha instalado una versión más reciente, debe revisar el Explorador de Storage, consulte a continuación)
3. Ejecute `sudo apt-get install libgconf-2-4`
4. Ejecute `sudo apt install libgnome-keyring-common libgnome-keyring-dev`

# <a name="ubuntu-1604tab1604"></a>[Ubuntu 16.04](#tab/1604)

1. Descarga del explorador de almacenamiento
2. Instalar .NET Core Runtime, versión más reciente de comprobado es: [2.0.8](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu16-04/runtime-2.0.8) (si ya ha instalado una versión más reciente, debe revisar el Explorador de Storage, consulte a continuación)
3. Ejecute `sudo apt install libgnome-keyring-dev`

# <a name="ubuntu-1404tab1404"></a>[Ubuntu 14.04](#tab/1404)

1. Descarga del explorador de almacenamiento
2. Instalar .NET Core Runtime, versión más reciente de comprobado es: [2.0.8](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu14-04/runtime-2.0.8) (si ya ha instalado una versión más reciente, debe revisar el Explorador de Storage, consulte a continuación)
3. Ejecute `sudo apt install libgnome-keyring-dev`

---

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>Explorador de almacenamiento de aplicación de revisiones para las versiones más recientes de .NET Core 
Si tiene una versión de .NET Core mayor 2.0 instalado y ejecuta el Explorador de Storage versión 1.7.0 o una versión anterior, probablemente necesitará revisar el Explorador de almacenamiento mediante los pasos siguientes:
1. Descargar versión 1.5.43 de StreamJsonRpc [desde nuget](https://www.nuget.org/packages/StreamJsonRpc/1.5.43). Busque el vínculo "Descargar el paquete" en el lado derecho de la página.
2. Después de descargar el paquete, cambie su extensión de archivo de `.nupkg` a `.zip`
3. Descomprima el paquete
4. Vaya a `streamjsonrpc.1.5.43/lib/netstandard1.1/`.
5. Copia `StreamJsonRpc.dll` a las siguientes ubicaciones dentro de la carpeta del explorador de Storage:
    1. `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
    2. `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-azure-portal-doesnt-work"></a>Abrir en el Explorador de portal de Azure no funciona

Si el botón "Abrir en el explorador" en el portal de Azure no funciona, asegúrese de que está utilizando un explorador compatible. Se han probado los siguientes exploradores para la compatibilidad.
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Pasos siguientes

Si ninguna de las soluciones funciona, [abrir una incidencia en GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). También puede acceder rápidamente a GitHub con el botón "Notificar problema a GitHub" en la esquina inferior izquierda.

![Comentarios](./media/storage-explorer-troubleshooting/feedback-button.PNG)
