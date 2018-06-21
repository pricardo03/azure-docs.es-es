---
title: Uso de Managed Identity en una máquina virtual Windows para acceder a Azure Storage
description: Tutorial que lo guía por el proceso de usar Managed Identity en una máquina virtual Windows para acceder a Azure Storage.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: 9ccc94727a18fbcd77f00000531934be57b8e132
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801371"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Tutorial: Uso de Managed Identity en una máquina virtual Windows para acceder a Azure Storage

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

En este tutorial se muestra cómo habilitar Managed Identity para una máquina virtual Windows y usar esa identidad para acceder a Azure Storage.  Aprenderá a:

> [!div class="checklist"]
> * Creación de una máquina virtual Windows en un nuevo grupo de recursos 
> * Habilitar Managed Identity en una máquina virtual (VM) Windows
> * Crear un contenedor de blobs en una cuenta de almacenamiento
> * Conceder a Managed Identity de la máquina virtual Windows acceso a una cuenta de almacenamiento 
> * Obtener un acceso y usarlo para llamar a Azure Storage 

> [!NOTE]
> La autenticación de Azure Active Directory para Azure Storage está en versión preliminar pública.

## <a name="prerequisites"></a>requisitos previos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Creación de una máquina virtual Windows en un nuevo grupo de recursos

En esta sección se creará una máquina virtual Windows a la que más tarde se concederá una identidad Managed Identity.

1.  Haga clic en el botón **+/Crear nuevo servicio** de la esquina superior izquierda de Azure Portal.
2.  Seleccione **Compute** y, después, seleccione **Windows Server 2016 Datacenter**. 
3.  Escriba la información de la máquina virtual. El **nombre de usuario** y la **contraseña** creados aquí son las credenciales que se usan para iniciar sesión en la máquina virtual.
4.  Elija la **suscripción** adecuada de la máquina virtual en la lista desplegable.
5.  Para seleccionar un nuevo **grupo de recursos** en el que quiere crear la máquina virtual, elija **Crear nuevo**. Cuando haya terminado, haga clic en **Aceptar**.
6.  Seleccione el tamaño de la máquina virtual. Para ver más tamaños, seleccione **Ver todo** o cambie el filtro **Supported disk type** (Tipo de disco admitido). En la hoja de configuración, conserve los valores predeterminados y haga clic en **Aceptar**.

    ![Texto alternativo de imagen](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-identity-on-your-vm"></a>Habilitación de Managed Identity en la máquina virtual

Una identidad Managed Identity de máquina virtual le permite obtener tokens de acceso desde Azure AD sin la necesidad de incluir credenciales en el código. En un segundo plano, habilitar Managed Identity en una máquina virtual mediante Azure Portal permite hacer dos cosas: registrar la máquina virtual con Azure AD para crear una identidad administrada y configurar la identidad en la máquina virtual. 

1. Desplácese hasta el grupo de recursos de la nueva máquina virtual y seleccione la máquina virtual que creó en el paso anterior.
2. En la categoría **Configuración**, haga clic en **Configuración**.
3. Para habilitar Managed Identity, seleccione **Sí**.
4. Haga clic en **Guardar** para aplicar la configuración. 

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento 

En esta sección se creará una cuenta de almacenamiento. 

1. Haga clic en el botón **+Crear un recurso** de la esquina superior izquierda de Azure Portal.
2. Haga clic en **Storage** (Almacenamiento) y, luego, en **Storage account - blob, file, table, queue** (Cuenta de almacenamiento: blob, archivo, tabla, cola).
3. En **Name** (Nombre), escriba un nombre para la cuenta de almacenamiento.  
4. **Deployment model** (Modelo de implementación) y **Account kind** (Clase de cuenta) se deben establecer en **Resource Manager** y **Storage (general purpose v1)** (Almacenamiento [de uso general v1]). 
5. Asegúrese de que **Suscripción** y **Grupo de recursos** coinciden con los que especificó cuando creó la máquina virtual en el paso anterior.
6. Haga clic en **Create**(Crear).

    ![Creación de una nueva cuenta de almacenamiento](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Creación de un contenedor de blobs y carga de un archivo a la cuenta de almacenamiento

Los archivos requieren almacenamiento de blobs, por lo que es necesario crear un contenedor de blobs donde se almacenará el archivo. Luego cargará un archivo en el contenedor de blobs de la cuenta de almacenamiento nueva.

1. Vuelva a la cuenta de almacenamiento recién creada.
2. En **Blob service**, haga clic en **Contenedores**.
3. Haga clic en **+ Contenedor** en la parte superior de la página.
4. En **Nuevo contenedor**, escriba un nombre para el contenedor, y mantenga el valor predeterminado en **Nivel de acceso público**.

    ![Creación de contenedores de almacenamiento](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Elija un editor de su preferencia y cree un archivo denominado *hello world.txt* en la máquina local.  Abra el archivo y agregue el texto (sin comillas) "Hola mundo :)" y guárdelo. 
6. Para cargar el archivo en el contenedor recién creado, haga clic en el nombre del contenedor y en **Cargar**.
7. En el panel **Carga de blob**, en **Archivos**, haga clic en el icono de carpeta y vaya al archivo **hello_world.txt** en la máquina virtual, selecciónelo y haga clic en **Cargar**.
    ![Carga de un archivo de texto](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Concesión de acceso a un contenedor de Azure Storage para la máquina virtual 

Puede usar la identidad Managed Identity de la máquina virtual para recuperar los datos en Azure Storage Blob.   

1. Vuelva a la cuenta de almacenamiento recién creada.  
2. Haga clic en el vínculo **Control de acceso (IAM)** en el panel izquierdo.  
3. Haga clic en **+ Agregar** en la parte superior de la página para agregar una asignación de roles nueva para la máquina virtual.
4. En la lista desplegable de **Rol**, seleccione **Lector de datos de blobs de almacenamiento (versión preliminar)**. 
5. En la lista desplegable siguiente, en **Asignar acceso a**, elija **Máquina virtual**.  
6. A continuación, asegúrese de que la suscripción adecuada aparece en el menú desplegable **Suscripción** y establezca **Grupo de recursos** en **Todos los grupos de recursos**.  
7. En **Seleccionar**, elija la máquina virtual y haga clic en **Guardar**. 

    ![Asignación de permisos](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Obtención de un token de acceso y su uso para llamar a Azure Storage 

Azure Storage admite de manera nativa la autenticación de Azure AD, por lo que puede aceptar directamente los tokens de acceso obtenidos mediante una identidad Managed Identity. Forma parte de la integración de Azure Storage con Azure AD y es diferente de proporcionar las credenciales en la cadena de conexión.

Este es un ejemplo de código .NET para abrir una conexión a Azure Storage con un token de acceso y luego leer el contenido del archivo que creó anteriormente. Este código se debe ejecutar en la máquina virtual para poder acceder al punto de conexión de la identidad Managed Identity de la máquina virtual. Se requiere .NET framework 4.6 o posterior para usar el método de token de acceso. Reemplace el valor de `<URI to blob file>` según corresponda. Para obtener este valor, vaya al archivo que creó y cargó en el almacenamiento de blobs y copie la **dirección URL** bajo **Propiedades** en la página **Información general**.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;

namespace StorageOAuthToken
{
    class Program
    {
        static void Main(string[] args)
        {
            //get token
            string accessToken = GetMSIToken("https://storage.azure.com/");
           
            //create token credential
            TokenCredential tokenCredential = new TokenCredential(accessToken);

            //create storage credentials
            StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

            Uri blobAddress = new Uri("<URI to blob file>");

            //create block blob using storage credentials
            CloudBlockBlob blob = new CloudBlockBlob(blobAddress, storageCredentials);
        
            //retrieve blob contents
            Console.WriteLine(blob.DownloadText());
            Console.ReadLine();
        }

        static string GetMSIToken(string resourceID)
        {
            string accessToken = string.Empty;
            // Build request to acquire MSI token
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=" + resourceID);
            request.Headers["Metadata"] = "true";
            request.Method = "GET";

            try
            {
                // Call /token endpoint
                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Pipe response Stream to a StreamReader, and extract access token
                StreamReader streamResponse = new StreamReader(response.GetResponseStream());
                string stringResponse = streamResponse.ReadToEnd();
                JavaScriptSerializer j = new JavaScriptSerializer();
                Dictionary<string, string> list = (Dictionary<string, string>)j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
                accessToken = list["access_token"];
                return accessToken;
            }
            catch (Exception e)
            {
                string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                return accessToken;
            }
        }            
    }
}
```

La respuesta incluye el contenido del archivo:

`Hello world! :)`

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a habilitar una identidad Managed Identity en una máquina virtual Windows para obtener acceso a Azure Storage.  Para más información sobre Azure Storage, consulte:

> [!div class="nextstepaction"]
> [Azure Storage](/azure/storage/common/storage-introduction)



