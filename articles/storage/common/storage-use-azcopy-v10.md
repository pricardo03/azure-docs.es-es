---
title: Copia o transferencia de datos a Azure Storage con AzCopy v10 | Microsoft Docs
description: AzCopy es una utilidad de línea de comandos que puede usar para copiar datos entre cuentas de almacenamiento. En este artículo sirve de ayuda para descargar AzCopy, conectarse a la cuenta de almacenamiento y, a continuación, transferir archivos.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 56b8099d1b777eff935c2b7ae075f8d104e50439
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561511"
---
# <a name="get-started-with-azcopy"></a>Introducción a AzCopy

AzCopy es una utilidad de línea de comandos que puede usar para copiar blobs o archivos a una cuenta de almacenamiento o desde una cuenta de almacenamiento. En este artículo sirve de ayuda para descargar AzCopy, conectarse a la cuenta de almacenamiento y, a continuación, transferir archivos.

> [!NOTE]
> AzCopy **V10** es la versión admitida actualmente de AzCopy.
>
> Si necesita usar AzCopy **v8.1**, consulte la sección [Uso de la versión anterior de AzCopy](#previous-version) de este artículo.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>Descargar AzCopy

En primer lugar, descargue el archivo ejecutable de AzCopy V10 en cualquier directorio en el equipo. 

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Si desea copiar datos desde y hacia su servicio de [almacenamiento de Azure Table](https://docs.microsoft.com/azure/storage/tables/table-storage-overview), instale [AzCopy versión 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>Ejecución de AzCopy

Para mayor comodidad, considere la posibilidad de agregar la ubicación del directorio del ejecutable de AzCopy a la ruta de acceso del sistema para facilitar su uso. De este modo, puede escribir `azcopy` desde cualquier directorio del sistema.

Si decide no agregar el directorio de AzCopy a la ruta de acceso, tendrá que cambiar los directorios a la ubicación de su archivo ejecutable de AzCopy y escribir `azcopy` o `.\azcopy` en los símbolos del sistema de Windows PowerShell.

Para ver una lista de los comandos, escriba `azcopy -h` y, a continuación, presione la tecla ENTRAR.

Para obtener información acerca de un comando específico, basta con que incluya el nombre del comando (por ejemplo: `azcopy list -h`).

![Ayuda en línea](media/storage-use-azcopy-v10/azcopy-inline-help.png)

> [!NOTE] 
> Como propietario de la cuenta de Azure Storage, no se le asignan automáticamente permisos para tener acceso a datos. Antes de hacer nada significativo con AzCopy, debe decidir cómo proporcionará las credenciales de autorización al servicio de almacenamiento. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Elección del modo de proporcionar las credenciales de autorización

Puede proporcionar las credenciales de autorización mediante Azure Active Directory (AD) o mediante un token de firma de acceso compartido (SAS).

Use esta tabla como guía:

| Tipo de almacenamiento | Método de autorización admitido actualmente |
|--|--|
|**Blob Storage** | Azure AD y SAS |
|**Almacenamiento de blobs (espacio de nombres jerárquico)** | Azure AD y SAS |
|**Almacenamiento de archivos** | SAS solo |

### <a name="option-1-use-azure-ad"></a>Opción 1: Use Azure AD

Con Azure AD, puede proporcionar credenciales una vez en lugar de anexar un token de SAS a cada comando.  

El nivel de autorización que necesita se basa en si va a cargar los archivos o solo a descargarlos.

Si solo desea descargar los archivos, compruebe que el [lector de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) se ha asignado a su identidad de usuario o entidad de servicio. 

> [!NOTE]
> Las identidades de usuario y las entidades de servicio son un tipo de *entidad de seguridad*, por lo que vamos a usar el término *entidad de seguridad* durante el resto de este artículo.

Si desea cargar archivos, compruebe que uno de estos roles se ha asignado a la entidad de seguridad:

- [Colaborador de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Estos roles pueden asignarse a la identidad en cualquiera de estos ámbitos:

- Contenedor (sistema de archivos)
- Cuenta de almacenamiento
- Grupos de recursos
- Subscription

Para aprender a verificar y asignar roles, consulte [Conceder acceso a datos blob y cola de Azure con RBAC en Azure Portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

No es necesario tener asignado uno de estos roles a su entidad de seguridad si esta se agrega a la lista de control de acceso (ACL) del directorio o contenedor de destino. En la ACL, la entidad de seguridad necesita permiso de escritura en el directorio de destino y permiso de ejecución en el contenedor y cada directorio primario.

Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-a-user-identity"></a>Autenticación de una identidad de usuario

Después de comprobar que se ha dado el nivel de autorización necesario a su identidad de usuario, abra un símbolo del sistema, escriba el comando siguiente y, a continuación, presione la tecla ENTRAR.

```azcopy
azcopy login
```

Si pertenece a más de una organización, incluya el identificador de inquilino de la organización a la que pertenece la cuenta de almacenamiento.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Reemplace el marcador de posición `<tenant-id>` por el identificador de inquilino de la organización a la que pertenece la cuenta de almacenamiento. Para buscar el identificador de inquilino, seleccione **Azure Active Directory > Propiedades > Identificador de directorio** en Azure Portal.

Este comando devuelve un código de autenticación y la dirección URL de un sitio web. Abra el sitio web, proporcione el código y, después, elija el botón **Siguiente**.

![Crear un contenedor](media/storage-use-azcopy-v10/azcopy-login.png)

Aparece una ventana de inicio de sesión. En esa ventana, inicie sesión en la cuenta de Azure con sus credenciales de la cuenta de Azure. Cuando haya iniciado sesión correctamente, puede cerrar la ventana del explorador y comenzar a usar AzCopy.

<a id="service-principal" />

#### <a name="authenticate-a-service-principal"></a>Autenticación de una entidad de servicio

Esta es una buena opción si va a usar AzCopy dentro de un script que se ejecuta sin interacción del usuario. 

Antes de ejecutar ese script, tendrá que iniciar sesión interactivamente al menos una vez para que pueda proporcionar a AzCopy las credenciales de la entidad de servicio.  Esas credenciales se almacenan en un archivo seguro y cifrado para que el script no tenga que proporcionar esa información confidencial.

Para iniciar sesión en su cuenta, use un secreto de cliente o la contraseña de un certificado asociado al registro de la aplicación de la entidad de servicio. 

Para obtener más información sobre cómo crear una entidad de servicio, consulte [Cómo: portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Para obtener más información sobre las entidades de servicio en general, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

##### <a name="using-a-client-secret"></a>Uso de un secreto de cliente

Para comenzar, establezca la variable de entorno `AZCOPY_SPA_CLIENT_SECRET` en el secreto de cliente del registro de la aplicación de la entidad de servicio. 

> [!NOTE]
> Asegúrese de establecer este valor desde el símbolo del sistema y no en la configuración de la variable de entorno del sistema operativo. De este modo, el valor está disponible solo en la sesión actual.

En este ejemplo se muestra cómo podría hacer esto en PowerShell.

```azcopy
$env:AZCOPY_SPA_CLIENT_SECRET="$(Read-Host -prompt "Enter key")"
```

> [!NOTE]
> Considere la posibilidad de utilizar un símbolo del sistema como se muestra en este ejemplo. De este modo, el secreto de cliente no aparecerá en el historial de comandos de la consola. 

A continuación, escriba el comando siguiente y presione la tecla ENTRAR.

```azcopy
azcopy login --service-principal --application-id <application-id>
```

Reemplace el marcador de posición `<application-id>` por el identificador de aplicación del registro de la aplicación de la entidad de servicio.

##### <a name="using-a-certificate"></a>Uso de un certificado

Si prefiere usar sus propias credenciales para la autorización, puede cargar un certificado en el registro de la aplicación y utilizar ese certificado para iniciar sesión.

Además de cargar el certificado en el registro de la aplicación, también deberá tener una copia del certificado guardada en el equipo o la máquina virtual donde se ejecutará AzCopy. Esta copia del certificado debe tener el formato .PFX o .PEM e incluir la clave privada. La clave privada debe estar protegida por contraseña. Si está usando Windows y el certificado solo existe en un almacén de certificados, asegúrese de exportarlo a un archivo PFX (incluida la clave privada). Para obtener instrucciones, consulte [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps)

A continuación, establezca la variable de entorno `AZCOPY_SPA_CERT_PASSWORD` en la contraseña del certificado.

> [!NOTE]
> Asegúrese de establecer este valor desde el símbolo del sistema y no en la configuración de la variable de entorno del sistema operativo. De este modo, el valor está disponible solo en la sesión actual.

En este ejemplo se muestra cómo podría hacer esto en PowerShell.

```azcopy
$env:AZCOPY_SPA_CERT_PASSWORD="$(Read-Host -prompt "Enter key")"
```

A continuación, escriba el comando siguiente y presione la tecla ENTRAR.

```azcopy
azcopy login --service-principal --certificate-path <path-to-certificate-file>
```

Reemplace el marcador de posición `<path-to-certificate-file>` por la ruta de acceso completa o relativa al archivo del certificado. AzCopy guarda la ruta de acceso a este certificado, pero no guarda una copia del certificado, así que asegúrese de tener el certificado listo para usar.

> [!NOTE]
> Considere la posibilidad de utilizar un símbolo del sistema como se muestra en este ejemplo. De este modo, la contraseña no aparecerá en el historial de comandos de la consola. 

### <a name="option-2-use-a-sas-token"></a>Opción 2: Uso de un token de SAS

Puede anexar un token de SAS a cada dirección URL de origen o destino que use en los comandos de AzCopy.

El comando de este ejemplo copia recursivamente los datos desde un directorio local a un contenedor de blobs. Un token de SAS ficticio se anexa al final de la dirección URL del contenedor.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Para más información sobre los tokens de SAS y de cómo obtener uno, consulte [Uso de firmas de acceso compartido (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Transferencia de archivos

Después de autenticar la identidad o de obtener un token de SAS, puede comenzar la transferencia de archivos.

Para obtener ejemplos de comandos, consulte cualquiera de estos artículos.

- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)

- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)

- [Transferencia de datos con AzCopy y cubos de Amazon S3](storage-use-azcopy-s3.md)

- [Transferencia de datos con AzCopy y Azure Stack Storage](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#azcopy)

## <a name="use-azcopy-in-a-script"></a>Uso de AzCopy en un script

Antes de ejecutar ese script, tendrá que iniciar sesión interactivamente al menos una vez para que pueda proporcionar a AzCopy las credenciales de la entidad de servicio.  Esas credenciales se almacenan en un archivo seguro y cifrado para que el script no tenga que proporcionar esa información confidencial. Para obtener ejemplos, consulte la sección de este artículo [Autenticación de la entidad de servicio](#service-principal).

Con el tiempo, el [vínculo de descarga](#download-and-install-azcopy) de AzCopy apuntará a nuevas versiones de AzCopy. Si el script descarga AzCopy, puede que deje de funcionar si una versión más reciente de AzCopy modifica las características de las que depende. 

Para evitar estos problemas, obtenga un vínculo estático (sin cambios) a la versión actual de AzCopy. De este modo, el script descarga exactamente la misma versión de AzCopy cada vez que se ejecuta.

Para obtener el vínculo, ejecute este comando:

| Sistema operativo  | Get-Help |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Para Linux, `--strip-components=1` en el comando `tar` elimina la carpeta de nivel superior que contiene el nombre de la versión y, en su lugar, extrae el archivo binario directamente en la carpeta actual. Esto permite que el script se actualice con una nueva versión de `azcopy` al actualizar únicamente la dirección URL de `wget`.

La dirección URL aparece en la salida de este comando. A continuación, el script puede descargar AzCopy mediante esa dirección URL.

| Sistema operativo  | Get-Help |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

## <a name="use-azcopy-in-storage-explorer"></a>Uso de AzCopy en el Explorador de Storage

Si desea aprovechar las ventajas de rendimiento de AzCopy, pero prefiere usar el Explorador de Storage en lugar de la línea de comandos para interactuar con los archivos, habilite AzCopy en el Explorador de Storage. 

En el Explorador de Storage, elija **Versión preliminar**->**Use AzCopy for Improved Blob Upload and Download** (Usar AzCopy para mejorar la carga y la descarga de blobs).

![Habilitación de AzCopy como un motor de transferencia en el Explorador de Azure Storage](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> No debe habilitar esta opción si ha habilitado un espacio de nombres jerárquico en su cuenta de almacenamiento. Esto se debe a que el Explorador de Storage utiliza automáticamente AzCopy en las cuentas de almacenamiento que tienen un espacio de nombres jerárquico.  

El Explorador de Azure Storage utiliza la clave de la cuenta para realizar operaciones, por lo que después de iniciar sesión en el Explorador de Azure Storage, no tendrá que proporcionar credenciales de autorización adicionales.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Uso de la versión anterior de AzCopy

Si necesita usar la versión anterior de AzCopy (AzCopy v8.1), consulte alguno de los vínculos siguientes:

- [AzCopy en Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [AzCopy en Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configuración, optimización y solución de problemas de AzCopy

Consulte [Configuración, optimización y solución de problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="next-steps"></a>Pasos siguientes

Si tiene alguna pregunta, problemas o comentarios generales, envíelos en [la página GitHub](https://github.com/Azure/azure-storage-azcopy).
