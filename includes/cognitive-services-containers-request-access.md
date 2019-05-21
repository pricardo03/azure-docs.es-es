---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: 4cdcec850f32d7e94f33eb28e5bf7839e511f347
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65891240"
---
Rellene y envíe el [formulario de solicitud de contenedores de Cognitive Services visión](https://aka.ms/VisionContainersPreview) para solicitar acceso al contenedor. El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar el contenedor. Después de enviar el formulario, revisa el equipo de Azure Cognitive Services para asegurarse de que cumplen los criterios de acceso al registro de contenedor privado.

> [!IMPORTANT]
> Debe usar una dirección de correo electrónico asociada con una cuenta de Microsoft (MSA) o una cuenta de Azure Active Directory (Azure AD) en el formulario.

Si se aprueba su solicitud, recibirá un correo electrónico con instrucciones que describen cómo obtener las credenciales y tener acceso al registro de contenedor privado.

## <a name="log-in-to-the-private-container-registry"></a>Inicio de sesión en el registro de contenedor privado

Hay varias maneras para autenticarse con el registro de contenedor privado de contenedores de Cognitive Services. Se recomienda que utilice el método de línea de comandos mediante el uso de la [CLI de Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Use la [inicio de sesión de docker](https://docs.docker.com/engine/reference/commandline/login/) de comandos, como se muestra en el ejemplo siguiente, para iniciar sesión en `containerpreview.azurecr.io`, que es el registro de contenedor privado de contenedores de Cognitive Services. Reemplace *\<username\>* por el nombre de usuario y *\<password\>* por la contraseña proporcionada en las credenciales que recibió del equipo de Azure Cognitive Services.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Si protege las credenciales en un archivo de texto, puede concatenar el contenido de ese archivo de texto en el `docker login` comando. Use el `cat` de comandos, tal como se muestra en el ejemplo siguiente. Reemplace *\<Archivodecontraseña\>* con la ruta de acceso y el nombre del archivo de texto que contiene la contraseña. Reemplace *\<username\>* con el nombre de usuario proporcionado en sus credenciales.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

