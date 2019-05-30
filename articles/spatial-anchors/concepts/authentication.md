---
title: Autenticación y autorización a Azure espacial delimitadores | Microsoft Docs
description: Obtenga información sobre las distintas formas en que puede autenticar una aplicación o servicio a Azure espacial delimitadores y los niveles de control que tiene que obtener acceso a Azure espacial delimitadores.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 05/28/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: e641025d49dd42125aa692925c0697235489b1db
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307158"
---
# <a name="authentication-and-authorization-to-azure-spatial-anchors"></a>Autenticación y autorización a Azure espacial delimitadores

En esta sección, trataremos las diversas maneras de que poder autenticarse en Azure delimitadores espacial de la aplicación o servicio web y las formas en que puede usar el Control de acceso basado en roles de Azure Directory (Azure AD) para controlar el acceso a las cuentas de delimitadores espacial.  

## <a name="overview"></a>Información general

![Información general de autenticación para Azure espacial delimitadores](./media/spatial-anchors-authentication-overview.png)

Para obtener acceso a una determinada cuenta de Azure espacial delimitadores, los clientes deben obtener un token de acceso de Azure Mixed Reality Security Token Service (STS). Los tokens obtenidos de STS live durante 24 horas y contienen información de los servicios espaciales delimitadores tomar decisiones de autorización en la cuenta y asegúrese de que solo las entidades autorizadas pueden tener acceso a esa cuenta. 

Los tokens de acceso pueden obtenerse en exchange desde cualquier claves de cuenta, o desde los tokens emitidos por AD de Azure. 

Las claves de cuenta le permiten comenzar rápidamente sobre el uso del servicio Azure espacial delimitadores; Sin embargo, antes de implementar la aplicación en producción, se recomienda que actualice la aplicación para usar la autenticación basada en AD de Azure. 

Los tokens de autenticación de Azure AD pueden obtenerse de dos maneras:

- Si está creando una aplicación empresarial y su empresa usa Azure AD como su sistema de identidad, puede usar Azure basada en usuario autenticación AD en la aplicación y conceder acceso a las cuentas de delimitadores espaciales mediante los grupos de seguridad de Azure AD existentes, o directamente a los usuarios de su organización. 
- En caso contrario, se recomienda que obtener los tokens de Azure AD desde un servicio web de la aplicación. Uso de un servicio web de soporte es el método de autenticación recomendado para aplicaciones de producción, ya que evita la incrustación de las credenciales para tener acceso a Azure espacial anclajes en la aplicación cliente. 

## <a name="account-keys"></a>Claves de cuenta

Uso de claves de cuenta para tener acceso a su cuenta de Azure espacial delimitadores es la manera más sencilla de empezar a trabajar. Encontrará las claves de cuenta en el portal de Azure. Vaya a la cuenta y seleccione la pestaña "Claves".

![Información general de autenticación para Azure espacial delimitadores](../../../includes/media/spatial-anchors-get-started-create-resource/view-account-key.png)


Dos claves se ponen a disposición, ambas al mismo tiempo válidas para el acceso a la cuenta de delimitadores espacial. Se recomienda que actualice periódicamente la clave para acceder a la cuenta; tener dos separar habilitar claves válidas como actualizaciones sin tiempo de inactividad; solo debe actualizar también la clave principal y la clave secundaria. 

El SDK tiene compatibilidad integrada para la autenticación con claves de cuenta; basta con establecer la propiedad de AccountKey en el objeto cloudSession. 

```csharp
this.cloudSession.Configuration.AccountKey = @"MyAccountKey";
```

Una vez hecho esto, el SDK controlará el intercambio de la clave de cuenta para un token de acceso y el almacenamiento en caché necesaria de tokens para la aplicación. 

> [!WARNING] 
> Se recomienda el uso de claves de cuenta sobre la incorporación rápida, pero durante el desarrollo y la creación de prototipos solo. Se recomienda encarecidamente no para enviar su aplicación en producción mediante una clave de cuenta incrustado en él y usar en su lugar Azure basado en usuario o servicio de autenticación de AD aproxima a la siguiente lista.

## <a name="azure-ad-user-authentication"></a>Autenticación de usuario de Azure AD

Para las aplicaciones destinadas a los usuarios de Azure Active Directory, el enfoque recomendado es usar un token de Azure AD para el usuario, que puede obtenerse mediante la biblioteca ADAL como se describe en la siguiente documentación: https://docs.microsoft.com/azure/active-directory/develop/v1-overview; debe seguir los pasos enumerados en "Inicios rápidos", que incluyen:

1. Configuración en Azure portal
    1.  Registre la aplicación en Azure AD como **aplicación nativa**. Como parte del registro, deberá determinar si la aplicación debe ser apto para multiempresa o no y proporcione la redirección de direcciones URL permitidas para su aplicación.  
    2.  Conceda a la aplicación o los usuarios acceso al recurso de: 
        1.  Vaya al recurso delimitadores espaciales en Azure portal
        2.  Cambie a la **control de acceso (IAM)** ficha
        3.  Presione **Agregar asignación de roles**
            1.  [Seleccione un rol](#role-based-access-control)
            2.  En el **seleccione** , escriba el nombre de los usuarios, grupos o aplicaciones a la que quieres asignar acceso. 
            3.  Presione **guardar**.
2. En el código:
    1.  Asegúrese de usar el **Id. de aplicación** y **Uri de redirección** de su propia aplicación de Azure AD como el **Id. de cliente** y **RedirectUri** parámetros de ADAL
    2.  Establecer la información del inquilino:
        1.  Si la aplicación admite **mi organización sólo**, reemplace este valor por su **Id. de inquilino** o **nombredeinquilino** (por ejemplo, contoso.microsoft.com)
        2.  Si la aplicación admite **cuentas en el directorio de cualquier organización**, reemplace este valor por **organizaciones**
        3.  Si la aplicación admite **usuarios de cuentas Microsoft todas**, reemplace este valor por **comunes**
    3.  En la solicitud de token, establezca el **recursos** a "https://sts.mixedreality.azure.com". Este "recurso" indicará a Azure AD que la aplicación solicita un token para el servicio Azure espacial delimitadores.  

Con esto, la aplicación debe poder obtener desde ADAL un token de Azure AD; puede establecer ese token de Azure AD como el **authenticationToken** en el objeto de configuración de sesión en la nube. 

```csharp
this.cloudSession.Configuration.AuthenticationToken = @"MyAuthenticationToken";
```

## <a name="azure-ad-service-authentication"></a>Autenticación de servicio de Azure AD

La opción recomendada para implementar aplicaciones aprovechando Azure espacial anclajes en producción es aprovechar un servicio back-end que se las solicitudes de autenticación de broker. El esquema general debe ser como se describe en este diagrama:

![Información general de autenticación para Azure espacial delimitadores](./media/spatial-anchors-aad-authentication.png)

En este caso, se supone que la aplicación usa su propio mecanismo de (por ejemplo: Cuenta de Microsoft, PlayFab, Facebook, Google Id., nombre de usuario/contraseña personalizado, etcetera.) para autenticarse en su servicio back-end. Una vez que los usuarios se autentican en el servicio back-end, que puede recuperar el servicio de un token de Azure AD, cambiarlo por un token de acceso para Azure espacial delimitadores y devolverla a la aplicación cliente.

El token de acceso de Azure AD se recupera mediante la biblioteca ADAL como se describe en la siguiente documentación: https://docs.microsoft.com/azure/active-directory/develop/v1-overview; debe seguir los pasos indicados en "Inicios rápidos", que incluyen:

1.  Configuración en Azure portal:
    1.  Registrar la aplicación en Azure AD:
        1.  En Azure portal, vaya a **Azure Active Directory**y seleccione **registros de aplicaciones**
        2.  Seleccione **nuevo registro de aplicaciones**
        3.  Escriba el nombre de la aplicación, seleccione **aplicación Web / API** como tipo de aplicación y escriba la dirección URL de autenticación para el servicio. A continuación, presione **crear**.
        4.  En esa aplicación, presione **configuración**, a continuación, seleccione el **claves** ficha. Escriba el nombre de la clave, seleccione una duración y posicionamiento **guardar**. Asegúrese de guardar el valor de clave que se muestra en ese momento, ya que lo necesitará incluirlo en el código del servicio web.
    2.  Conceder el acceso de aplicación o los usuarios a sus recursos:
        1.  Vaya al recurso delimitadores espaciales en Azure portal
        2.  Cambie a la **control de acceso (IAM)** ficha
        3.  Presione **Agregar asignación de roles**
        1.  [Seleccione un rol](#role-based-access-control)
        2.  En el **seleccione** campo, escriba el nombre de las aplicaciones que ha creado y al que desea asignar acceso. Si desea que los usuarios de la aplicación tengan distintos roles en la cuenta de delimitadores espaciales, debe registrar varias aplicaciones en Azure AD y asignar a cada uno un rol independiente. A continuación, implemente la lógica de autorización para usar el rol correcto para los usuarios.  
    3.  Presione **guardar**.
2.  En el código (Nota: puede usar el servicio de ejemplo incluido en GitHub):
    1.  Asegúrese de usar el identificador de la aplicación, el secreto de aplicación y redirigir la Uri de la propia aplicación de Azure AD como el identificador de cliente, secreto y los parámetros de RedirectUri en ADAL
    2.  Establece el identificador del inquilino en su propio identificador de inquilino AAAzure agregar en el parámetro de autoridad en ADAL
    3.  En la solicitud de token, establezca el **recursos** a "https://sts.mixedreality.azure.com" 

Con esto, el servicio de back-end puede recuperar un token de Azure AD. A continuación, puede cambiarlo por un token de MR que va a devolver al cliente. Mediante un token de Azure AD para recuperar un token MR se realiza a través de una llamada de REST. Este es un ejemplo de llamada:

```
GET https://mrc-auth-prod.trafficmanager.net/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
Host: mrc-auth-prod.trafficmanager.net
Connection: Keep-Alive

HTTP/1.1 200 OK
Date: Sun, 24 Feb 2019 08:00:00 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1153
Accept: application/json
MS-CV: 05JLqWeKFkWpbdY944yl7A.0
{"AccessToken":"eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2MzYyMTk5ZTI2NjQxOGU4ZjE3MThlM2IyMThjZTIxIiwidHlwIjoiSldUIn0.eyJqdGkiOiJmMGFiNWIyMy0wMmUxLTQ1MTQtOWEzNC0xNzkzMTA1NTc4NzAiLCJjYWkiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJ0aWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJhaWQiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJhYW8iOi0xLCJhcHIiOiJlYXN0dXMyIiwicmlkIjoiL3N1YnNjcmlwdGlvbnMvNzIzOTdlN2EtNzA4NC00ODJhLTg3MzktNjM5Y2RmNTMxNTI0L3Jlc291cmNlR3JvdXBzL3NhbXBsZV9yZXNvdXJjZV9ncm91cC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1peGVkUmVhbGl0eS9TcGF0aWFsQW5jaG9yc0FjY291bnRzL2RlbW9fYWNjb3VudCIsIm5iZiI6MTU0NDU0NzkwMywiZXhwIjoxNTQ0NjM0MzAzLCJpYXQiOjE1NDQ1NDc5MDMsImlzcyI6Imh0dHBzOi8vbXJjLWF1dGgtcHJvZC50cmFmZmljbWFuYWdlci5uZXQvIiwiYXVkIjoiaHR0cHM6Ly9tcmMtYW5jaG9yLXByb2QudHJhZmZpY21hbmFnZXIubmV0LyJ9.BFdyCX9UJj0i4W3OudmNUiuaGgVrlPasNM-5VqXdNAExD8acFJnHdvSf6uLiVvPiQwY1atYyPbOnLYhEbIcxNX-YAfZ-xyxCKYb3g_dbxU2w8nX3zDz_X3XqLL8Uha-rkapKbnNgxq4GjM-EBMCill2Svluf9crDmO-SmJbxqIaWzLmlUufQMWg_r8JG7RLseK6ntUDRyDgkF4ex515l2RWqQx7cw874raKgUO4qlx0cpBAB8cRtGHC-3fA7rZPM7UQQpm-BC3suXqRgROTzrKqfn_g-qTW4jAKBIXYG7iDefV2rGMRgem06YH_bDnpkgUa1UgJRRTckkBuLkO2FvA"}
```

En el encabezado de autorización tiene el formato siguiente: `Bearer <accoundId>:<accountKey>`

Y la respuesta contiene el token MR en texto sin formato.
 
Ese token MR, a continuación, se devuelve al cliente. La aplicación cliente, a continuación, puede establecer que su token de acceso en la configuración de sesión en la nube.

```csharp
this.cloudSession.Configuration.AccessToken = @"MyAccessToken";
```

## <a name="role-based-access-control"></a>Control de acceso basado en rol

Para ayudar a controlar el nivel de acceso concedidos a las aplicaciones, servicios o usuarios de Azure AD de su servicio, se crearon las siguientes funciones de asignación según sea necesario en las cuentas de Azure espacial delimitadores:

- **Propietario de la cuenta de delimitadores espacial**: aplicaciones o usuarios que tienen este rol pueden crear delimitadores espaciales, realizar consultas y eliminarlos. Cuando se autentica con una cuenta mediante claves de cuenta, el **propietario de la cuenta de delimitadores espacial** rol se asigna a la entidad de seguridad autenticada. 
- **Colaborador de la cuenta de delimitadores espacial**: las aplicaciones o los usuarios que tienen este rol pueden crear delimitadores espaciales, la consulta para ellos, pero no puede eliminarlas. 
- **Lector de cuenta delimitadores espacial**: aplicaciones o usuarios que tienen este rol solo son capaces de consultar para delimitadores espaciales, pero no se puede crear otros nuevos, eliminar los existentes o actualizar los metadatos de los anclajes espaciales. Esto se utiliza normalmente para las aplicaciones que algunos usuarios ajustar el entorno, mientras que otros sólo pueden recuperar delimitadores colocados anteriormente en ese entorno.

## <a name="next-steps"></a>Pasos siguientes

Cree su primera aplicación con Azure espacial delimitadores.

> [!div class="nextstepaction"]
> [Unity](../unity-overview.yml)

> [!div class="nextstepaction"]
> [iOS](../quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](../quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](../quickstarts/get-started-hololens.md)
