---
title: Seguridad del borde del cuadro de datos | Microsoft Docs
description: Describe las características de seguridad y privacidad que protegen el dispositivo de borde del cuadro de datos, servicios y datos locales y en la nube.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/02/2019
ms.author: alkohli
ms.openlocfilehash: de737f20147e8208dd18388eedcac11583c8cb97
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891810"
---
# <a name="data-box-edge-security-and-data-protection"></a>Seguridad del borde del cuadro de datos y protección de datos

Seguridad es una preocupación importante adoptar una nueva tecnología, especialmente si se usa la tecnología con datos confidenciales o propios. Solución de borde del cuadro de datos de Microsoft Azure ayuda a garantizar que solo las entidades autorizadas pueden ver, modificar o eliminar los datos.

En este artículo se describe las características de seguridad de borde del cuadro de datos que ayudan a proteger cada uno de los componentes de la solución y los datos almacenados en ellas.

La solución de borde del cuadro de datos de Azure consta de cuatro componentes principales que interactúan entre sí:

- **Borde del cuadro de datos / servicio de puerta de enlace de datos cuadro hospedado en Azure** : el recurso de administración que usar para crear el orden de los dispositivos, configurar el dispositivo y, a continuación, controlar el orden hasta su finalización.
- **Dispositivo de borde del cuadro de datos** : el dispositivo de transferencia que se envía al importar los datos en el entorno local a Azure.
- **Clientes/hosts conectados al dispositivo** : los clientes de la infraestructura que se conectan al dispositivo de borde del cuadro de datos y contienen datos que deben protegerse.
- **Almacenamiento en la nube** : la ubicación en la nube de Azure donde se almacenan los datos. Esta ubicación es normalmente la cuenta de almacenamiento vinculada al recurso de borde del cuadro de datos que ha creado.


## <a name="data-box-edgedata-box-gateway-service-protection"></a>Protección de servicio de puerta de enlace de cuadro de borde/datos de cuadro de datos

El servicio de puerta de enlace de datos cuadro Edge/datos de cuadro es un servicio de administración hospedado en Microsoft Azure. El servicio se usa para configurar y administrar el dispositivo.

- Acceso al servicio de puerta de enlace de datos cuadro Edge/Data cuadro requiere su organización tenga un contrato Enterprise (EA) o una suscripción de proveedor de soluciones en la nube (CSP). Para obtener más información, vaya a [registrarse para obtener una suscripción de Azure](https://azure.microsoft.com/resources/videos/sign-up-for-microsoft-azure/)!
- Dado que se hospeda el servicio de administración de Azure, se está protegido por las características de seguridad de Azure. Para obtener más información acerca de las características de seguridad que proporciona Microsoft Azure, visite el [Centro de confianza de Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).
- Para las operaciones de administración de SDK, está disponible para el borde del cuadro de datos clave de cifrado o recurso de puerta de enlace de datos cuadro en **las propiedades del dispositivo**. Puede ver la clave de cifrado solo si tiene permisos para el recurso de Graph API.

## <a name="data-box-edge-device-protection"></a>Protección de dispositivos de borde del cuadro de datos

El dispositivo de borde del cuadro de datos es un dispositivo local que ayuda a transformar los datos mediante procesarlo localmente y, a continuación, enviarlos a Azure. El dispositivo:

- Necesita una clave de activación para tener acceso al servicio de puerta de enlace de datos cuadro Edge/datos de cuadro.
- Se protege en todo momento mediante una contraseña de dispositivo.
- Es un dispositivo bloqueado. El dispositivo de BMC y BIOS están protegidas con contraseña con acceso de usuario limitado para que el BIOS.
- Ha habilitado el arranque seguro.
- Se ejecuta Device Guard de Windows Defender. Device Guard le permite ejecutar solo las aplicaciones de confianza que se definen en las directivas de integridad de código.
- Tiene una clave dentro de la portada que puede utilizarse para bloquear el dispositivo. Se recomienda que después de configurar el dispositivo, abra la cubierta. Busque la clave y, a continuación, bloquear la cubierta para evitar cualquier acceso no autorizado a los discos de datos situados delante del dispositivo.

### <a name="protect-the-device-via-activation-key"></a>Proteger el dispositivo a través de la clave de activación

Solo un dispositivo de borde del cuadro de datos autorizado tiene permiso para unir el servicio de puerta de enlace de datos cuadro Edge/datos de cuadro que creó en su suscripción de Azure. Para autorizar un dispositivo, debe usar una clave de activación para activar el dispositivo con el servicio de puerta de enlace de datos cuadro Edge/datos de cuadro. Para obtener más información, vaya a [obtener una clave de activación](data-box-edge-deploy-prep.md#get-the-activation-key).

La clave de activación que usar:

- Es una clave de autenticación basada en Azure Active Directory (AAD).
- Expira después de tres días.
- No se usa tras la activación del dispositivo.
 
Después de activa un dispositivo, utiliza los tokens para comunicarse con Microsoft Azure.

### <a name="protect-the-device-via-password"></a>Proteger el dispositivo a través de la contraseña

Las contraseñas de asegurarse de que los datos son accesibles sólo a usuarios autorizados. Borde del cuadro de datos y la puerta de enlace de datos de cuadro de dispositivos se inician en un estado bloqueado.

Puede:

- Conéctese a la IU del dispositivo mediante un explorador web local y, a continuación, proporcione una contraseña para iniciar sesión en el dispositivo.
- Conectarse de forma remota a la interfaz de PowerShell del dispositivo a través de HTTP. Administración remota está activada de forma predeterminada. A continuación, puede proporcionar la contraseña del dispositivo para iniciar sesión en el dispositivo. Para obtener más información, vaya a [conexión remota al dispositivo de borde del cuadro de datos](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

Tenga presente los siguientes procedimientos recomendados:

- El servicio de borde del cuadro de datos no puede recuperar contraseñas existentes: solo puede restablecer mediante el portal de Azure. Se recomienda almacenar todas las contraseñas en un lugar seguro para que no deba restablecer una contraseña si se le olvida. Si restablece una contraseña, asegúrese de notificar a todos los usuarios antes de hacerlo.
- Use local a la interfaz de usuario de web [cambiar la contraseña](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Si cambia la contraseña, asegúrese de notificar a todos los usuarios de acceso remoto para que no experimenten un error de inicio de sesión.
- La interfaz de Windows PowerShell del dispositivo puede tener acceso de forma remota a través de HTTP. Como práctica recomendada de seguridad, debe utilizar HTTP solo en redes de confianza.
- Asegúrese de que las contraseñas de dispositivos segura y bien protegida. Siga el [prácticas recomendadas de contraseñas](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices#enable-password-management).

## <a name="protect-the-data"></a>Proteger los datos

En esta sección se describe las características de seguridad de borde del cuadro de datos que protegen los datos en tránsito y los datos almacenados.

### <a name="protect-data-at-rest"></a>Protección de los datos en reposo

Para lo datos en reposo:

- Para lo datos en reposo, borde del cuadro de datos usa el cifrado de BitLocker XTS AES-256 para proteger los datos locales.
- Para los datos que residen en recursos compartidos, se restringe el acceso a los recursos compartidos.

    - Para los clientes SMB que tienen acceso a los datos del recurso compartido, necesitan credenciales de usuario asociadas al recurso compartido. Estas credenciales se definen en el momento de creación del recurso compartido.
    - Para los clientes NFS que tienen acceso a los recursos compartidos, las direcciones IP de los clientes deben agregarse en el momento de creación del recurso compartido.


### <a name="protect-data-in-flight"></a>Proteger datos en tránsito

Los datos de vuelos:

- Para los datos que pasan entre el dispositivo y Azure, se usa el estándar de TLS 1.2. No hay ninguna reserva para TLS 1.1 y versiones anteriores. Si no se admite TLS 1.2, se bloqueará la comunicación del agente. También es necesario para las operaciones de administración de SDK y portal de la TLS 1.2.
- Cuando los clientes acceder al dispositivo a través de la interfaz de usuario web local en un explorador, estándar de TLS 1.2 se utiliza como el protocolo seguro de forma predeterminada.

    - El procedimiento recomendado consiste en configurar el explorador para usar TLS 1.2.
    - Si el explorador no es compatible con TLS 1.2, puede usar TLS 1.1 o TLS 1.0.
- Para proteger los datos al copiar desde los servidores de datos, se recomienda el uso de SMB 3.0 con cifrado.

### <a name="protect-data-via-storage-accounts"></a>Protección de datos a través de cuentas de almacenamiento

El dispositivo está asociado con una cuenta de almacenamiento que se utiliza como un destino para los datos en Azure. Acceso a la cuenta de almacenamiento se controla mediante la suscripción y el almacenamiento de 512 bits dos asociadas con esa cuenta de almacenamiento de claves de acceso.

Una de las claves se usa para la autenticación cuando el dispositivo de borde del cuadro de datos tiene acceso a la cuenta de almacenamiento. La otra clave se mantiene en reserva, lo que le permite rotar periódicamente las claves.

Por motivos de seguridad, muchos centros de datos requieren la rotación de claves. Se recomienda seguir estos procedimientos recomendados para la rotación de claves:

- La clave de la cuenta de almacenamiento es similar a la contraseña raíz de la cuenta de almacenamiento. Proteja cuidadosamente la clave de cuenta. No distribuya la contraseña a otros usuarios, disco duro codificarlo o guardarlo en cualquier lugar en texto sin formato que sea accesible a otros usuarios.
- [Volver a generar la clave de cuenta](../storage/common/storage-account-manage.md#regenerate-access-keys) mediante Azure portal si cree puede verse comprometida.
- Girar y, a continuación, [sincronizar las claves de cuenta de almacenamiento](data-box-gateway-manage-shares.md#sync-storage-keys) con regularidad para ayudar a garantizar que la cuenta de almacenamiento no tiene acceso a los usuarios no autorizados.
- De forma regular, el administrador de Azure debe cambiar o volver a generar la clave principal o secundaria mediante la sección Storage de Azure Portal para acceder directamente a la cuenta de almacenamiento.


## <a name="manage-personal-information"></a>Administrar la información personal

El borde del cuadro de datos / servicio de puerta de enlace de datos cuadro recopila información personal en las instancias de la claves siguientes:

- **Detalles de pedidos**: una vez creado el pedido, la dirección de envío, el correo electrónico y la información de contacto de los usuarios se almacenan en Azure Portal. Entre la información guardada se incluyen los siguientes datos:
  - Nombre de contacto
  - Número de teléfono
  - Email
  - Dirección
  - City
  - Código postal
  - Estado
  - País/región/provincia
  - Número de seguimiento del envío

    Los detalles del pedido se cifran y almacenan en el servicio. El servicio conserva la información hasta que se elimine el recurso o el orden explícitamente. Además, se bloquea la eliminación de recursos y el orden correspondiente desde el momento en que el dispositivo se envía hasta que el dispositivo vuelve a Microsoft.

- **Dirección de envío** – después de que se realiza el pedido, el servicio de Data Box proporciona la dirección de envío a las compañías aéreas de terceros como SAI (UPS).

- **Compartir usuarios** -los usuarios en el dispositivo también pueden tener acceso a los datos que residen en los recursos compartidos. Se muestra y se puede ver una lista de los usuarios que pueden acceder a los datos del recurso compartido. Esta lista también se elimina cuando se eliminan los recursos compartidos. Para ver la lista de usuarios que pueden acceder o eliminar un recurso compartido, siga los pasos descritos en [administrar recursos compartidos en el borde del cuadro de datos](data-box-gateway-manage-shares.md).

Para obtener más información, revise la directiva de privacidad de Microsoft en el [Centro de confianza](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Pasos siguientes

[Implementar el dispositivo de borde del cuadro datos](data-box-edge-deploy-prep.md).

