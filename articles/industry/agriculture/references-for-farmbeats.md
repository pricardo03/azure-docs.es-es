---
title: Referencias de FarmBeats
description: ''
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 8e8e4524034f0a296045691309b065f8547bdaa0
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797237"
---
# <a name="references"></a>Referencias

A continuación se muestra una colección de notas e instrucciones que describen las API de FarmBeats de Azure.

## <a name="rest-api"></a>API DE REST

Las API de FarmBeats de Azure proporcionan a un negocio agrícola una interfaz RESTful estandarizada con respuestas basadas en JSON y esto ayudará a aprovechar las funciones de FarmBeats de Azure:

- API para obtener datos de sensor, de cámara, de dron, del tiempo, satélite y terrestres mantenidos.
- Normalización y contextualización de datos en proveedores de datos comunes.
- Acceso esquematizado y capacidades de consulta en todos los datos ingeridos.
- Generación automática de metadatos que se pueden consultar, basados en las características agronómicas.  
- Agregados de secuencia de tiempo generados automáticamente para la compilación rápida de modelos.
- Motor de Azure Data Factory (ADF) integrado para compilar fácilmente canalizaciones personalizadas de procesamiento de datos.

## <a name="application-development"></a>Desarrollo de aplicaciones

Las API contienen documentación técnica de Swagger. Para obtener información sobre las API y sus solicitudes y respuestas correspondientes, vea [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

Este es un resumen de todos los objetos y recursos del centro de datos de FarmBeats:

Granja | La granja corresponde a una ubicación física de interés dentro del sistema FarmBeats. Cada granja tiene un nombre de granja y un id. de granja único.
--- | ---|
Dispositivo  | El dispositivo corresponde a un dispositivo físico presente en la granja. Cada dispositivo tiene un id. de dispositivo único. El dispositivo normalmente se aprovisiona a una granja con un id. de granja.
DeviceModel  | DeviceModel corresponde a los metadatos del dispositivo, por ejemplo, el fabricante, el tipo de dispositivo (puerta de enlace o nodo).
Sensor  | El sensor corresponde a un sensor físico que registra valores. Un sensor normalmente se conecta a un dispositivo con un id. de dispositivo.
SensorModel  | SensorModel corresponde a los metadatos del sensor, por ejemplo, el fabricante, el tipo de sensor (analógico o digital), la medida de sensor (como temperatura ambiente, presión, etc.).
Telemetría  | La telemetría proporciona la capacidad de leer los mensajes de telemetría para un determinado intervalo de tiempo y sensor.
Trabajo  | El trabajo corresponde a cualquier flujo de trabajo de actividades, que se ejecutan en el sistema FarmBeats para obtener el resultado pretendido. Cada trabajo está asociado a un id. de trabajo y a un tipo de trabajo.
JobType  | JobType corresponde a distintos tipos de trabajo que el sistema admite. Incluye los tipos de trabajo definidos por el sistema y los definidos por el usuario.
ExtendedType  | ExtendedType corresponde a la lista de tipos definidos por el sistema y por el usuario en el sistema. Esto ayuda a configurar un nuevo tipo Sensor, Scene o Scenefile en el sistema FarmBeats.
Asociado  | El asociado corresponde al asociado de integración de sensor o imágenes para FarmBeats
Escena  | La escena corresponde a cualquier salida generada en el contexto de una granja. Cada escena tiene un id. de escena, un origen de la escena, un tipo de escena y un id. de granja asociados. Cada id. de escena puede tener varios archivos de escenas asociados.
SceneFile |SceneFile corresponde a todos los archivos que se generan para una sola escena. Un único id. de escena puede tener varios id. de SceneFile asociados.
Regla  |La regla corresponde a una condición de datos relacionados con la granja para desencadenar una alerta. Cada regla estará en el contexto de los datos de la granja.
Alerta  | La alerta corresponde a una notificación que se genera cuando se cumple una condición de regla. Cada alerta estará en el contexto de una regla.
RoleDefinition  | RoleDefinition define las acciones permitidas y no permitidas para un rol.
RoleAssignment  |RoleAssignment se corresponde a la asignación de un rol a un usuario o una entidad de servicio.

**Formato de datos**

JSON (notación de objetos JavaScript) es un formato de datos común independiente del lenguaje que proporciona una representación de texto simple de estructuras de datos arbitrarias. Para obtener más información, visite json.org.

## <a name="authentication-and-authorization"></a>Autenticación y autorización

Las solicitudes HTTP a la API REST están protegidas con Azure Active Directory (Azure AD).
Para realizar una solicitud autenticada a las API REST, el código de cliente requiere autenticación con credenciales válidas para poder llamar a la API. La autenticación la coordina Azure AD entre los distintos actores y proporciona al cliente un token de acceso como prueba de la autenticación. Después, el token se envía en el encabezado de autorización HTTP de las solicitudes de la API REST. Para obtener más información sobre la autenticación de Azure AD, vea [Azure Active Directory](https://portal.azure.com) para desarrolladores.

El token de acceso se debe enviar en las solicitudes de API posteriores en la sección de encabezado como:

```
headers = {"Authorization": "Bearer " + **access_token**}
```

**Encabezados de solicitud HTTP**

Estos son los encabezados de solicitud más comunes que deben especificarse al realizar una llamada API al centro de datos de FarmBeats de Azure:


**Encabezado** | **Descripción y ejemplo**
--- | ---
Content-Type  | El formato de la solicitud (Content-Type: application/<format>) para el formato de las API del centro de datos de FarmBeats de Azure es JSON. Content-Type: application/json
Authorization  | Especifica el token de acceso necesario para realizar una llamada API. **Autorización: portador <token_de_acceso>**
Accept | El formato de respuesta. En el caso de las API del centro de datos de FarmBeats de Azure, el formato es JSON: **Accept: Application/json**

**Solicitudes de API**

Para realizar una solicitud de API REST, se combina el método HTTP (GET, POST, PUT o DELETE), la dirección URL del servicio de API, el URI a un recurso para consultar, enviar datos, actualizar o eliminar, y uno o más encabezados de solicitud HTTP.

La dirección URL del servicio de API es la dirección URL del centro de datos https://\<nombre_del_sitio_web_del_centro_de_datos>.azurewebsites.net. Opcionalmente, puede incluir parámetros de consulta en las llamadas GET para filtrar, limitar el tamaño y ordenar los datos en las respuestas.

La siguiente solicitud de ejemplo se usa para obtener la lista de dispositivos:

```
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>”
```

La mayoría de llamadas GET, POST y PUT requieren un cuerpo de la solicitud JSON.

La siguiente solicitud de ejemplo se usa para crear un dispositivo (esta contiene un código JSON de entrada con el cuerpo de la solicitud).

```json
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

**Parámetros de consulta**

En el caso de las llamadas **GET** de REST, se pueden filtrar, limitar el tamaño y ordenar los datos en una respuesta de API al incluir uno o varios parámetros de consulta en el URI de solicitud. En el caso de los parámetros de consulta, vea la documentación de la API y las llamadas GET individuales.
Por ejemplo, al consultar la lista de dispositivos (llamada GET en /dispositivo), se pueden especificar los siguientes parámetros de consulta:  

![Proyecto de FarmBeats](./media/for-references/query-parameters-device-1.png)

**Control de errores**

Las API del centro de datos de FarmBeats de Azure devuelven los errores HTTP estándar. Los códigos de error más comunes son los siguientes:


 |Código de error             | DESCRIPCIÓN |
 |---                    | --- |
 |200                    | Correcto |
 |201                    | Creación (publicación) correcta. |
 |400                    | Solicitud incorrecta. Hay un error en la consulta. |
 |401                    | No autorizado. El autor de la llamada de la API no está autorizado para acceder al recurso. |
 |404                    | Recurso no encontrado. |
 |5XX                    | Error interno del servidor. Los códigos de error que empiezan por 5XX significan que hay algún error en el servidor. Consulte los registros de servidor y la sección siguiente para obtener más detalles. |


Además de los errores HTTP estándar, las API del centro de datos de FarmBeats de Azure también devuelven errores internos en el siguiente formato:

```
{
  "message": "<More information on the error>",
  "status": "<error code>”,
  "code": "<InternalErrorCode>",
  "moreInfo": "<Details of the error>"
}
```

Ejemplo: Al crear una granja, no se ha especificado un campo obligatorio "Name" en la carga de entrada. El mensaje de error resultante sería el siguiente:

```
{
  "message": "Model validation failed",
  "status": 400,
  "code": "ModelValidationFailed",
  "moreInfo": "[\"The Name field is required.\"]"
}
```

## <a name="adding-users-or-app-registrations-to-azure-active-directory"></a>Incorporación de usuarios o registros de aplicaciones a Azure Active Directory

 Se puede acceder a las API de FarmBeats de Azure mediante un usuario o un registro de aplicación en Azure Active Directory. Para crear un registro de aplicación en la instancia de Azure Active Directory, siga estos pasos:  

1. Vaya a [Azure Portal](https://portal.azure.com) **Azure Active Directory, Registros de aplicaciones**, > **Nuevo registro**. También puede usar una cuenta existente.
2. Para una cuenta nueva, asegúrese de completar lo siguiente:

    - Escribir un nombre
    - Seleccione **Cuentas de este directorio organizativo solo (inquilino único)**
    - Los valores predeterminados en el resto de campos.
    - Seleccione **Registrar**.

3. En el registro de aplicación nuevo o existente, **Información general**, complete lo siguiente.

    - Capture el **Id. de cliente** y el **Id. de inquilino**.
    - Vaya a **Certificados y secretos** para generar un secreto de cliente nuevo y capturar el **Secreto de cliente**.
    - Vuelva a Información general y haga clic en el vínculo situado junto a **Administrar la aplicación en el directorio local**.
    - Vaya a **Propiedades** para capturar el **Id. de objeto**.

4. Vaya al [Swagger del centro de datos](https://<yourdatahub>.azurewebsites.net/swagger/index.html) y siga estos pasos:
    - Vaya a la **API RoleAssignment**.
    - Realice un método **POST** con el fin de crear un elemento RoleAssignment para el **Id. de objeto** que se acaba de crear. – El JSON de entrada es el siguiente:

  > [!NOTE]
  > Para obtener más información sobre cómo agregar usuarios y registro de AD, vea [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Después de completar los pasos anteriores, el registro de aplicación (cliente) puede llamar a las API de FarmBeats de Azure mediante un token de acceso a través de la autenticación de portador.  

Use el token de acceso para enviarlo en las solicitudes de API posteriores en la sección de encabezado de la siguiente manera:

```
headers = {"Authorization": "Bearer " + **access_token**, "Content-Type" : "application/json" }
```
