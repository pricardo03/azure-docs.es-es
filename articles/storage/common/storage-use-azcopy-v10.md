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
ms.openlocfilehash: bfa3e5a943ee59b1ed335f45e113a60f62572675
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735023"
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

Antes de hacer nada significativo con AzCopy, debe decidir cómo proporcionará las credenciales de autorización al servicio de almacenamiento.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Elección del modo de proporcionar las credenciales de autorización

Puede proporcionar las credenciales de autorización mediante Azure Active Directory (AD) o mediante un token de firma de acceso compartido (SAS).

Use esta tabla como guía:

| Tipo de almacenamiento | Método de autorización admitido actualmente |
|--|--|
|**Blob Storage** | Azure AD y SAS |
|**Almacenamiento de blobs (espacio de nombres jerárquico)** | Solo Azure AD |
|**Almacenamiento de archivos** | SAS solo |

### <a name="option-1-use-azure-ad"></a>Opción 1: Use Azure AD

El nivel de autorización que necesita se basa en si va a cargar los archivos o solo a descargarlos.

#### <a name="authorization-to-upload-files"></a>Autorización para cargar archivos

Compruebe que uno de estos roles se ha asignado a su identidad:

- [Colaborador de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Estos roles pueden asignarse a la identidad en cualquiera de estos ámbitos:

- Contenedor (sistema de archivos)
- Cuenta de almacenamiento
- Grupos de recursos
- Subscription

Para aprender a verificar y asignar roles, consulte [Conceder acceso a datos blob y cola de Azure con RBAC en Azure Portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

No es necesario tener asignado uno de estos roles a su identidad, si la identidad se agrega a la lista de control de acceso (ACL) del directorio o contenedor de destino. En la ACL, su identidad necesita permiso de escritura en el directorio de destino y permiso de ejecución en el contenedor y cada directorio primario.

Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authorization-to-download-files"></a>Autorización para descargar archivos

Compruebe que uno de estos roles se ha asignado a su identidad:

- [Lector de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader)
- [Colaborador de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Estos roles pueden asignarse a la identidad en cualquiera de estos ámbitos:

- Contenedor (sistema de archivos)
- Cuenta de almacenamiento
- Grupos de recursos
- Subscription

Para aprender a verificar y asignar roles, consulte [Conceder acceso a datos blob y cola de Azure con RBAC en Azure Portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

No es necesario tener asignado uno de estos roles a su identidad, si la identidad se agrega a la lista de control de acceso (ACL) del directorio o contenedor de destino. En la ACL, su identidad necesita permiso de lectura en el directorio de destino y permiso de ejecución en el contenedor y cada directorio primario.

Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-your-identity"></a>Autenticación de la identidad

Después de comprobar que se le ha dado el nivel de autorización necesario a su identidad, abra un símbolo del sistema, escriba el siguiente comando y, a continuación, presione la tecla ENTRAR.

```azcopy
azcopy login
```

Este comando devuelve un código de autenticación y la dirección URL de un sitio web. Abra el sitio web, proporcione el código y, después, elija el botón **Siguiente**.

![Crear un contenedor](media/storage-use-azcopy-v10/azcopy-login.png)

Aparece una ventana de inicio de sesión. En esa ventana, inicie sesión en la cuenta de Azure con sus credenciales de la cuenta de Azure. Cuando haya iniciado sesión correctamente, puede cerrar la ventana del explorador y comenzar a usar AzCopy.

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

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configuración, optimización y solución de problemas de AzCopy

Consulte [Configuración, optimización y solución de problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="use-azcopy-in-storage-explorer"></a>Uso de AzCopy en el Explorador de Storage

Si desea aprovechar las ventajas de rendimiento de AzCopy, pero prefiere usar el Explorador de Storage en lugar de la línea de comandos para interactuar con los archivos, habilite AzCopy en el Explorador de Storage.

En el Explorador de Storage, elija **Versión preliminar**->**Use AzCopy for Improved Blob Upload and Download** (Usar AzCopy para mejorar la carga y la descarga de blobs).

![Habilitación de AzCopy como un motor de transferencia en el Explorador de Azure Storage](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> No debe habilitar esta opción si ha habilitado un espacio de nombres jerárquico en su cuenta de almacenamiento. Esto se debe a que el Explorador de Storage utiliza automáticamente AzCopy en las cuentas de almacenamiento que tienen un espacio de nombres jerárquico.  

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Uso de la versión anterior de AzCopy

Si necesita usar la versión anterior de AzCopy (AzCopy v8.1), consulte alguno de los vínculos siguientes:

- [AzCopy en Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)
- [AzCopy en Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="next-steps"></a>Pasos siguientes

Si tiene alguna pregunta, problemas o comentarios generales, envíelos en [la página GitHub](https://github.com/Azure/azure-storage-azcopy).
