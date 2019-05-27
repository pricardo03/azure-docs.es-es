---
title: Notificaciones de inserción a aplicaciones iOS Swift con Azure Notification Hubs | Microsoft Docs
description: Aprenda a enviar notificaciones a aplicaciones iOS Swift mediante Azure Notification Hubs.
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 05/21/2019
ms.author: miparker
ms.openlocfilehash: 64a05c6cbd412953ae10b31efc1d141c9cefd062
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65994772"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-using-the-notification-hubs-rest-api"></a>Tutorial: Notificaciones de inserción a aplicaciones iOS Swift con la API de REST de Notification Hubs

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

En este tutorial, use Azure Notification Hubs para enviar notificaciones push a una aplicación de iOS basado en Swift con la [API de REST](/rest/api/notificationhubs/). Cree una aplicación iOS vacía que reciba notificaciones push mediante [Apple Push Notification Service (APN)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
> * Generación del archivo de solicitud de firma de certificado
> * Solicite en su aplicación notificaciones push
> * Creación de un perfil de aprovisionamiento para la aplicación
> * Creación de un centro de notificaciones
> * Configurar el centro de notificaciones con información de APNS
> * Conexión de la aplicación iOS a Notification Hubs
> * Probar la solución

## <a name="prerequisites"></a>Requisitos previos
Para continuar, necesita:

- Vaya a través de [Introducción a Azure Notification Hubs](notification-hubs-push-notification-overview.md) si no está familiarizado con el servicio. 
- Obtenga información sobre los registros y la instalación: [Administración de registros](notification-hubs-push-notification-registration-management.md)
- Cuando se activa [cuenta de desarrollador de Apple](https://developer.apple.com) 
- Un equipo Mac con Xcode junto con un certificado de desarrollador válida instalado en la cadena de claves
- Un dispositivo iPhone físico se puede ejecutar y depurar con (no es posible probar las notificaciones de inserción con el simulador)
- El dispositivo iPhone físico registrado en el [Portal Apple](https://developer.apple.com) y asociado con el certificado
- Un [suscripción Azure](https://portal.azure.com) donde puede crear y administrar recursos

Debe ser posible seguir el tutorial para crear este ejemplo primero principios sin experiencia previa. Sin embargo, estar familiarizado con los conceptos siguientes será beneficioso:

- Compilar aplicaciones de iOS con Xcode y Swift
- Configurar un [Azure Notification Hub](notification-hubs-ios-apple-push-notification-apns-get-started.md) para iOS
- Familiaridad con la [Portal Apple Developer](https://developer.apple.com) y [portal de Azure](https://portal.azure.com)

> [!NOTE]
> El centro de notificaciones se configurarán para utilizar el *Sandbox* solo modo de autenticación. No se debe usar este modo de autenticación para las cargas de trabajo de producción.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-notification-hubs"></a>Conexión de la aplicación iOS a Notification Hubs
En esta sección, compilará la aplicación de iOS que se conectará al centro de notificaciones.  

### <a name="create-an-ios-project"></a>Crear un proyecto de iOS
1. En **Xcode**, cree un nuevo proyecto de iOS y seleccione el **aplicación de vista única** plantilla.
2. Al configurar las opciones para el nuevo proyecto, siga estos pasos:
    1. Usar el mismo **Product Name** (es decir, **PushDemo**) y **identificador de la organización** (es decir, `com.<organization>`) que usó durante la **agrupación Identificador** se estableció el **Portal Apple Developer**. 
    2. Elija la **equipo** que la **Id. de aplicación** se configuró.
    3. Establecer el **lenguaje** a **Swift**.
    4. Haga clic en **Siguiente**
3. Cree una carpeta nueva denominada **SupportingFiles**.
4. Cree un nuevo **plist** archivo denominado **devsettings.plist** bajo el **SupportingFiles** carpeta. No olvide agregar esta carpeta para su **gitignore** archivo por lo que no se confirman cuando se trabaja con un **repositorio de git**. En una aplicación de producción, se podrían probablemente se condicionalmente estableciendo estos secretos como parte de un proceso de compilación automatizado. Sin embargo, no se trata como parte de este tutorial.
5. Actualización **devsettings.plist** para incluir las siguientes entradas de configuración (con sus propios valores de la **centro de notificaciones** se aprovisionan):

   | Clave                            | Type                     | Valor                     |               
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | \<hubKey\>                |
   | notificationHubKeyName         | String                   | \<hubKeyName\>            |
   | notificationHubName            | String                   | \<hubName\>               |
   | notificationHubNamespace       | String                   | \<hubNamespace\>          |
    
   Puede encontrar los valores necesarios, vaya a la **centro de notificaciones** recursos en el **portal Azure**. Puede encontrar el **notificationHubName** y **notificationHubNamespace** valores en la esquina superior derecha de la **Essentials** resumen dentro de la  **Información general sobre** página.

   ![Resumen de Notification Hubs Essentials](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   Puede encontrar el **notificationHubKeyName** y **notificationHubKey** valores, vaya a **las directivas de acceso**y haga clic en los respectivos **acceso Directiva**. Por ejemplo, `DefaultFullSharedAccessSignature`. A continuación, copiar desde el **cadena de conexión principal** como prefijo el valor `SharedAccessKeyName=` para `notificationHubKeyName` y el valor de prefijo `SharedAccessKey=` para el `notificationHubKey`. La cadena de conexión debe tener el formato siguiente:
    
   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Para no complicarlo, deberá usar *DefaultFullSharedAccessSignature* por lo que puede usar el token para enviar notificaciones, así. Sin embargo, en la práctica el `DefaultListenSharedAccessSignature` sería una mejor opción para situaciones donde solo desea recibir notificaciones.       
6. En **Project Navigator**, haga clic en el **nombre del proyecto**, a continuación, haga clic en el **General** ficha
7. Buscar **identidad**, a continuación, establezca el **identificador de paquete** valor para que coincida con lo que se usa para la **Id. de aplicación** (del paso anterior), es decir, `'com.<organization>.PushDemo'`
8. Buscar **firma**, asegúrese de seleccionar la apropiada **equipo** para su **cuenta de desarrollador de Apple** (lo que había creado los certificados y perfiles anteriormente).  **Xcode** debe desplegar automáticamente adecuado **perfil de aprovisionamiento** según la **identificador de paquete**. Si no ve la nueva **perfil de aprovisionamiento**, intente actualizar los perfiles para el **identidad de firma** (*Xcode > Preferencias > cuenta > Ver detalles*). Al hacer clic en el **identidad de firma**, a continuación, haga clic en el **actualizar** botón en la esquina inferior derecha debe descargar los perfiles.
9. Seleccione el **capacidades** pestaña y asegúrese de que **notificaciones de inserción** están habilitadas.
10. Abra su **AppDelegate.swift** archivo para implementar la *UNUserNotificationCenterDelegate* de protocolo y agregue el código siguiente a la parte superior de la clase:
    
    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
        
        ...

        var configValues: NSDictionary?
        var notificationHubNamespace : String?
        var notificationHubName : String?
        var notificationHubKeyName : String?
        var notificationHubKey : String?
        let tags = ["12345"]
        let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
        
        ...
    }
    ```

    Estos miembros se usará más adelante. El *etiquetas* y *genericTemplate* se usará como parte del registro. Para obtener más información sobre las etiquetas, consulte [etiquetas para registros](notification-hubs-tags-segment-push-message.md) y [los registros de plantilla](notification-hubs-templates-cross-platform-push-messages.md).
 
11. En el mismo archivo, agregue el código siguiente en el *didFinishLaunchingWithOptions* función:

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
        if let configValues = NSDictionary(contentsOfFile: path) {
            self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
            self.notificationHubName = configValues["notificationHubName"] as? String
            self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
            self.notificationHubKey = configValues["notificationHubKey"] as? String
        }
    }
        
    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in
                
            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    Este código recupera los valores de configuración de **devsettings.plist**, Establece la **AppDelegate** de clase como el *UNUserNotificationCenter* delegar, la autorización de solicitudes para las notificaciones de inserción y, a continuación, llama a *registerForRemoteNotifications*.
    
    Para no complicarlo, es compatible con el código **iOS 10 y posteriores solo**. Puede agregar la compatibilidad con versiones anteriores del sistema operativo condicionalmente utilizando las API y los métodos respectivos como lo haría normalmente.

12. En el mismo archivo, agregue las siguientes funciones:

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        self.window?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    El código usa el *installationId* y *pushChannel* los valores para registrar con el **centro de notificaciones**. En este caso, usa *UIDevice.current.identifierForVendor* nos proporcione un valor único para identificar el dispositivo y, a continuación, dar formato el *deviceToken* nos proporcione el deseado*pushChannel* valor. El *showAlert* función es simplemente mostrar texto de mensaje para fines de demostración.

13. Todavía en **AppDelegate.swift**, agregue el *willPresent* y *didReceive* **UNUserNotificationCenterDelegate** funciones a mostrar una alerta cuando la aplicación se ejecuta en el primer y segundo plano respectivamente la llegada de la notificación
    
    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        willPresent notification: UNNotification, 
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }
    
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        didReceive response: UNNotificationResponse, 
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```    

14. Agregar instrucciones de impresión a la parte inferior de la *didRegisterForRemoteNotificationsWithDeviceToken* función para comprobar que *installationId* y *pushChannel* se están valores asignados
15. Crear carpetas (**modelos**, **servicios**, y **utilidades**) para los componentes fundamentales que se agregarán al proyecto más adelante
16. Compruebe que el proyecto se compila y se ejecuta en un dispositivo físico (no se pueden probar las notificaciones mediante el simulador de inserción)

### <a name="create-models"></a>Crear modelos
En este paso, creará un conjunto de modelos para representar el [API de REST de Notification Hubs](/rest/api/notificationhubs/) cargas y para almacenar la información necesaria **token de SAS** datos.


1.  Agregue un nuevo archivo de swift para el **modelos** llamado **PushTemplate.swift**. Este modelo proporciona una estructura que representa el **cuerpo** de una plantilla individual como parte de la **DeviceInstallation** carga:
    
    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String
    
        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. Agregue un nuevo archivo de swift para el **modelos** carpeta denominada **DeviceInstallation.swift**. Este archivo define una estructura que representa la carga para crear o actualizar un **instalación de dispositivo**. Agregue el siguiente código al archivo:
    
    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>
    
        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

3.  Agregue un nuevo archivo swift bajo **modelos** llamado **TokenData.swift**. Este modelo se usará para almacenar un **token de SAS** junto con la fecha de expiración

    ```swift
    import Foundation

    struct TokenData {
    
        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

### <a name="generate-a-sas-token"></a>Generar un token de SAS
**Notification Hubs** utilizan la misma infraestructura de seguridad como **Azure Service Bus**. Para llamar a la API de REST, deberá [generar mediante programación un token de SAS](/rest/api/eventhub/generate-sas-token) que puede utilizarse en el **autorización** encabezado de la solicitud.  

El token resultante tendrá el formato siguiente: 

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

El propio proceso implica los mismos seis pasos principales:  

1.  Informática de la expiración en [tiempo de época de UNIX](https://en.wikipedia.org/wiki/Unix_time) formato (segundos transcurridos desde el 1 de enero de 1970 de 00:00:00 UTC)
2.  Aplicar formato a la **ResourceUrl** (que representa el recurso que está intentando obtener acceso, es decir, `'https://\<namespace\>.servicebus.windows.net/\<hubName\>'`) por lo que es en minúscula y codificados por porcentaje
3.  Preparando la **StringToSign**, que se compone de `'\<**UrlEncodedResourceUrl**\>\n\<**ExpiryEpoch**\>'`
4.  Informática (y codificación de base 64) el **firma** utilizando el **HMAC-SHA256** de la **StringToSign** valor con el **clave** forma parte de la **Cadena de conexión** (para los respectivos **regla de autorización**)
5.  Aplicar formato a la codificado en base 64 **firma** por lo que es codificación de porcentaje
6.  Construir la **token** con el formato esperado mediante la **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**y **UrlEncodedResourceUrl** valores

Consulte la [documentación de Azure Service Bus](../service-bus-messaging/service-bus-sas.md) para obtener una introducción más completa de **firma de acceso compartido** y cómo se usa por **Azure Service Bus** y  **Notification Hubs**.

Para los fines de este ejemplo Swift, vas a usar abierto Apple **CommonCrypto** biblioteca para ayudar con el hash de la firma. Ya que es una biblioteca de C, no es accesible en Swift-de-fábrica. Sin embargo, puede hacerlo disponible con un encabezado puente. Para agregar y configurar el encabezado puente:

1. En **Xcode**, vaya a **archivo**, a continuación, **New**, a continuación, **archivo** y seleccione **archivo de encabezado** asígnele el nombre *'BridgingHeader.h'*
2. Edite el archivo para importar **CommonHMAC**

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

3. Actualizar el destino **configuración de compilación** para hacer referencia el encabezado puente. Abra el **configuración de generación de** pestaña y desplácese hacia abajo hasta la **compilador Swift** sección. Asegúrese de que el **instalar Objective-C compatibilidad encabezado** opción establecida en **Sí** y escriba la ruta del archivo en el encabezado de puente en la **de encabezado puente Objective-c.**   que es la opción `'\<ProjectName\>/BridgingHeader.h'`. Si no se puede encontrar estas opciones, asegúrese de tener la **todas** vista seleccionada (en lugar de **básica** o **personalizado**).
    
   Hay muchas bibliotecas de contenedor de código abierto de terceros disponibles, esto podría provocar que el uso de **CommonCrypto** un poco más fácil. Está fuera del ámbito de este artículo.

4. Agregue un nuevo archivo Swift bajo el **utilidades** carpeta denominada **TokenUtility.swift** y agregue el código siguiente:

   ```swift
   import Foundation

   struct TokenUtility {    
        typealias Context = UnsafeMutablePointer<CCHmacContext>
    
        static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
            let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
            let encodedUrl = urlEncodedString(withString: resourceUrl)
            let stringToSign = "\(encodedUrl)\n\(expiry)"
            let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
            let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
            let encodedSignature = urlEncodedString(withString: signature)
            let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
            let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)
        
            return tokenData
        }
    
        private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
            let context = Context.allocate(capacity: 1)
            CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
            CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
            var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
            CCHmacFinal(context, &hmac)
        
            let result = NSData(bytes: hmac, length: hmac.count)
            context.deallocate()
        
            return result as Data
        }
    
        private static func urlEncodedString(withString stringToConvert : String) -> String {
            var encodedString = ""
            let sourceUtf8 = (stringToConvert as NSString).utf8String
            let length = strlen(sourceUtf8)
        
            let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
            let asUInt8Array = String(charArray).utf8.map{ Int8($0) }
        
            for i in 0..<length {
                let currentChar = sourceUtf8![i]
            
                if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                    (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                    (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                    (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                    encodedString += String(format:"%c", currentChar)
                }
                else {
                    encodedString += String(format:"%%%02x", currentChar)
                }
            }
        
            return encodedString
        }
    }
   ```
    
    Esta utilidad encapsula la lógica responsable de generar el **token de SAS**. El *getSasToken* función organiza los pasos generales necesarios para preparar el token, como se describió anteriormente y será llamada por el servicio de instalación en los pasos más adelante en este tutorial. Las otras dos funciones se denominan el *getSasToken función*; *sha256HMac* para calcular la firma y *urlEncodedString* para codificar la cadena de dirección Url correspondiente. El *urlEncodedString* función era necesaria, ya que era imposible de lograr el resultado esperado que se usa el método integrado *addingPercentEncoding* función. El [SDK de iOS de Azure Storage](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) sirve como un excelente ejemplo de cómo enfocar estas operaciones aunque en Objective-C. Obtener más información en **tokens de SAS de Azure Service Bus** puede encontrarse en el [documentación de Azure Service Bus](../service-bus-messaging/service-bus-sas.md). 

### <a name="verify-the-sas-token"></a>Comprobar el token SAS
Antes de implementar el servicio de instalación en el cliente, puede comprobar que la aplicación está generando correctamente el **token de SAS** mediante la utilidad de http de elección. Para los fines de esta publicación, será nuestra herramienta que prefiera **Postman**.

Tome nota de la *installationId* y *token* valores que va a generar la aplicación mediante un correctamente colocado impresión instrucción o punto de interrupción. 

Siga estos pasos para llamar a la *instalaciones* API:

1. En **Postman**, abra una nueva pestaña
2. Establecer la solicitud **obtener** y la dirección siguiente:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

3. Configure los encabezados de solicitud como sigue:
    
   | Clave           | Valor            |
   | ------------- | ---------------- |
   | Content-Type  | application/json |
   | Autorización | \<sasToken\>     |
   | x-ms-version  | 2015-01          |

4. Haga clic en el **código** botón (parte superior derecha bajo la **guardar** botón). La solicitud debe ser similar al ejemplo siguiente:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

5. Haga clic en el **enviar** botón

Para el elemento especificado no existe ningún registro *installationId* en este momento, sin embargo debe tener como resultado un **404 no encontrado** respuesta lugar **401 no autorizado**. Este resultado debe confirmar que el **token de SAS** ha aceptado.

### <a name="implement-the-installation-service-class"></a>Implementar la clase de servicio de instalación
A continuación implementaremos nuestra contenedor básico en torno a la [API de REST de las instalaciones](/rest/api/notificationhubs/create-overwrite-installation).  

Agregue un nuevo archivo Swift bajo el **servicios** carpeta denominada **NotificationRegistrationService.swift**, a continuación, agregue el código siguiente a este archivo:

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.windows.net/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil
    
    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }
    
    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {
        
        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)
        
        if let tags = tags {
            deviceInstallation.tags = tags
        }
        
        if let templates = templates {
            deviceInstallation.templates = templates
        }
        
        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"
            
            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"
            
            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }
            
            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)
            
            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil && (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }
    
    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }
    
    private func getSasToken() -> String {
        if (tokenData == nil ||
            Date(timeIntervalSince1970: Double((tokenData?.expiration)!)) < Date(timeIntervalSinceNow: -(5 * 60))) {
            self.tokenData = TokenUtility.getSasToken(forResourceUrl: getBaseAddress(), withKeyName: self.keyName, andKey: self.key)
        }

        return (tokenData?.token)!
    }
    
    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```
 
Se proporcionan los detalles necesarios como parte de la inicialización. Etiquetas y plantillas, opcionalmente, se pasan a la *registrar* función formen parte de la **instalación de dispositivo** carga JSON.  

El *registrar* función llama a las demás funciones privadas para preparar la solicitud. Una vez que se recibe una respuesta, la finalización se denomina que indica si el registro ha sido correcto o no.  

El punto de conexión de la solicitud se construye mediante la *getBaseAddress* funcionan según el **centro de notificaciones** parámetros **espacio de nombres** y **nombre**proporcionada durante la inicialización.  

El *getSasToken* función comprobará si el token almacenado actualmente es válido, en caso contrario, llamará a la **TokenUtility** para generar uno nuevo y almacenarlo antes de devolver un valor. 

Por último el *encodeToJson* convertirá los objetos del modelo correspondiente en JSON para su uso como parte del cuerpo de la solicitud.

### <a name="invoke-the-notification-hubs-rest-api"></a>Invocar la API de REST de Notification Hubs
El último paso consiste en actualizar **AppDelegate** para usar el **NotifiationRegistrationService** para registrar con nuestro **NotificationHub**. 

1. Abra **AppDelegate.swift** y agregue una variable de nivel de clase para almacenar una referencia a la **NoficiationRegistrationService**:

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

2. En el mismo archivo, actualice el *didRegisterForRemoteNotificationsWithDeviceToken* esa función para inicializar el **NotificationRegistrationService** con los parámetros necesarios, a continuación, llame a la *registrar* función.

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

        // Initialize the Notification Registration Service
        self.registrationService = NotificationRegistrationService(
            withInstallationId: installationId,
            andPushChannel: pushChannel,
            andHubNamespace: notificationHubNamespace!,
            andHubName: notificationHubName!,
            andKeyName: notificationHubKeyName!,
            andKey: notificationHubKey!)
    
        // Call register passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Para no complicarlo, vas a usar un par de instrucciones print para actualizar la ventana de salida con el resultado de la *registrar* operación. 

3. Ahora, compile y ejecute la aplicación (en un dispositivo físico). Debería ver **"Registrado"** en la ventana de salida.

## <a name="test-the-solution"></a>Probar la solución
En esta fase, nuestra aplicación se registra con **NotificationHub** y que puede recibir notificaciones de inserción. En **Xcode**, detenga el depurador y cerrar la aplicación (si se está ejecutando actualmente). A continuación, comprobará que el **instalación de dispositivo** detalles son según lo previsto y que nuestra aplicación ahora realmente puede recibir notificaciones de inserción.  

### <a name="verify-the-device-installation"></a>Comprobar la instalación del dispositivo
Ahora se puede realizar la misma solicitud como hizo anteriormente con **Postman** para [comprobar el token de SAS](#verify-the-sas-token). Suponiendo que el **token de SAS** no ha expirado, la respuesta debe incluir ahora los detalles de instalación que proporcionó como las etiquetas y plantillas.  

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

### <a name="send-a-test-notification-azure-portal"></a>Enviar una notificación de prueba (Azure portal)
La forma más rápida que ahora puede recibir notificaciones de prueba es navegar a la **centro de notificaciones** en el **portal Azure**.

1. En el **portal de Azure**, vaya a la **Introducción** ficha su **centro de notificaciones**
2. Haga clic en **envío de prueba** (parte superior izquierda) por encima del **Essentials** resumen

    ![Botón de envío de prueba de resumen de Notification Hubs Essentials](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)
3. Elija **plantilla personalizada** en la lista de **plataformas**
4. Escriba **12345** para el **enviar a expresión de etiqueta** (había especificado en la instalación de esta etiqueta)
5. Opcionalmente, modifique el **mensaje** en la carga de JSON
    
    ![Envío de prueba de Notification Hubs](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)
6. Haga clic en **enviar** y el portal debería indicar si la notificación se ha enviado correctamente al dispositivo

    ![Prueba de Notification Hubs enviar resultados](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    También verá una notificación en el **centro de notificaciones** en el dispositivo (suponiendo que no se está ejecutando la aplicación en primer plano). Pulsar la notificación debe abrir la aplicación y mostrar la alerta.

    ![Recibe notificación de ejemplo](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-postman"></a>Enviar una notificación de prueba (Postman)
Puede enviar notificaciones a través de sus respectivos [API de REST](/rest/api/notificationhubs/) a través de **Postman** como bien y pueden ser una manera más cómoda para probar. 

1. En **Postman**, abra una nueva pestaña
2. Establecer la solicitud **POST** y escriba la dirección siguiente:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

3. Configure los encabezados de solicitud como sigue:
    
   | Clave                            | Valor                          |
   | ------------------------------ | ------------------------------ |
   | Content-Type                   | application/json;charset=utf-8 |
   | Autorización                  | \<sasToken\>                   |
   | ServiceBusNotification-Format  | plantilla                       |
   | `Tags`                           | "12345"                        |

4. Configure la solicitud **cuerpo** usar **RAW - JSON (application.json)** con la carga de JSON siguiente:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

5. Haga clic en el **código** botón (parte superior derecha bajo la **guardar** botón). La solicitud debe ser similar al ejemplo siguiente:

    ```html
    POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json;charset=utf-8.
    ServiceBusNotification-Format: template
    Tags: "12345"
    Authorization: <sasToken>
    Cache-Control: no-cache
    Postman-Token: <postmanToken>

    {
        "message" : "Hello from Postman!"
    }
    ```

5. Haga clic en el **enviar** botón

Debe obtener un código de estado correcto y recibir la notificación en el dispositivo cliente.

## <a name="next-steps"></a>Pasos siguientes
Ahora tiene una aplicación Swift básica de iOS conectada a un **centro de notificaciones** a través de la [API de REST](/rest/api/notificationhubs/) y puede enviar y recibir notificaciones. Para más información, consulte los siguientes artículos. 

- [Introducción a Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [API de REST de Notification Hubs](/rest/api/notificationhubs/)
- [SDK de Notification Hubs para las operaciones de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification Hubs SDK en GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registrar con el back-end de aplicación](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Administración de registros](notification-hubs-push-notification-registration-management.md)
- [Trabajar con etiquetas](notification-hubs-tags-segment-push-message.md) 
- [Trabajar con plantillas personalizadas](notification-hubs-templates-cross-platform-push-messages.md)
- [Control de acceso de Service Bus con firmas de acceso compartido](../service-bus-messaging/service-bus-sas.md)
- [Generar mediante programación los tokens de SAS](/rest/api/eventhub/generate-sas-token)
- [Seguridad de Apple: cifrado común](https://developer.apple.com/security/)
- [Tiempo de época de UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
