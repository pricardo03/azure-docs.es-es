---
title: Guía de seguridad de almacenamiento de Azure Data Lake Storage Gen2 | Microsoft Docs
description: Detalles de los distintos métodos de seguridad de Azure Storage, incluidos, entre otros, el control de acceso basado en rol y Storage Service Encryption
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 02/07/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 22b070e6d70208057c85ad6a2322cc440d12a0fa
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58008209"
---
# <a name="azure-data-lake-storage-gen2-security-guide"></a>Guía de seguridad de Azure Data Lake Storage Gen2

Azure Data Lake Storage Gen2 es un conjunto de funcionalidades basadas en las cuentas de Azure Storage. Por tanto, todas las referencias de este artículo están relacionadas con una cuenta de Azure Storage con un espacio de nombres jerárquico habilitado (funcionalidades de Data Lake Storage Gen2).

- Todos los datos escritos en Azure Storage se cifran automáticamente con el [cifrado del servicio Storage (SSE)](storage-service-encryption.md). Para obtener más información, consulte [Announcing Default Encryption for Azure Blobs, Files, Tables and Queue Storage](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/) (Presentación del cifrado predeterminado de Azure Blobs, Files, Tables y Queue Storage).
- Azure Active Directory (Azure AD) y el control de acceso basado en rol (RBAC) son compatibles con Azure Storage para las operaciones de administración de recursos y las operaciones de datos, como se indica a continuación:
    - Puede asignar roles de RBAC en el ámbito de la cuenta de almacenamiento para las entidades de seguridad y utilizar Azure AD para autorizar las operaciones de administración de recursos, como la administración de claves.
    - La integración con Azure AD se admite para las operaciones de datos en Azure Storage. Puede asignar roles de RBAC en el ámbito de una suscripción, un grupo de recursos, una cuenta de almacenamiento o un sistema de archivos individual a una entidad de seguridad o identidad administrada para los recursos de Azure. Para obtener más información, consulte [Autenticación del acceso a Azure Storage con Azure Active Directory](storage-auth-aad.md).
- Se puede conceder acceso delegado a los objetos de datos de Azure Storage mediante las [Firmas de acceso compartido](../storage-dotnet-shared-access-signature-part-1.md).

Este artículo ofrece una visión general de cada una de estas características de seguridad que se pueden usar con Azure Storage. Se incluyen vínculos a artículos en los que se detalla cada una de ellas, así que puede examinar un tema determinado con mayor detalle si así lo desea.

Estos son los temas que se tratarán en este artículo:

* [Seguridad en el plano de la administración](#management-plane-security): protección de la cuenta de almacenamiento

  El plano de la administración está compuesto por los recursos que se usan para administrar la cuenta de almacenamiento. En esta sección se habla sobre el modelo de implementación de Azure Resource Manager y cómo usar el control de acceso basado en roles (RBAC) para controlar el acceso a las cuentas de almacenamiento. También se explica cómo administrar las claves de cuenta de almacenamiento y cómo volver a generarlas.
* [Seguridad en el plano de los datos](#data-plane-security) : protección del acceso a los datos

  En esta sección, analizaremos el modo de permitir el acceso a los objetos de datos reales de la cuenta de almacenamiento, como archivos y directorios, y el uso de Firmas de acceso compartido y Directivas de acceso almacenadas. Nos fijaremos en las Firmas de acceso compartido tanto a nivel de servicio como a nivel de cuenta. También veremos cómo limitar el acceso a una dirección IP específica (o un intervalo de direcciones IP), cómo limitar el protocolo utilizado para HTTPS y cómo revocar una Firma de acceso compartido sin esperar a que expire.
* [Cifrado en tránsito](#encryption-in-transit)

En esta sección se describe cómo proteger los datos cuando se transfieren a o desde una cuenta de almacenamiento con Data Lake Storage Gen2 habilitado. Hablaremos sobre el uso recomendado de HTTPS.

## <a name="management-plane-security"></a>Seguridad en el plano de la administración

El plano de la administración consta de las operaciones que afectan a la propia cuenta de almacenamiento. Por ejemplo, puede crear o eliminar una cuenta de almacenamiento, obtener una lista de cuentas de almacenamiento en una suscripción, recuperar las claves de cuenta de almacenamiento o volver a generar las claves de cuenta de almacenamiento.

Esta guía se centra en el modelo de implementación de Resource Manager, que es el medio para crear cuentas de almacenamiento con las funcionalidades de Data Lake Storage Gen2. Con las cuentas de almacenamiento de Resource Manager, en lugar de proporcionar acceso a toda la suscripción, puede controlar el acceso en un nivel más detallado en el plano de administración mediante el control de acceso basado en rol (RBAC).

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Protección de su cuenta de almacenamiento con el control de acceso basado en rol (RBAC)

Vamos a ver qué es el control de acceso basado en roles y cómo se utiliza. Cada una de las suscripciones de Azure está asociada a una instancia de Azure Active Directory. Es posible conceder acceso a los usuarios, los grupos y las aplicaciones desde ese directorio a la administración de recursos de la suscripción de Azure que usan el modelo de implementación de Resource Manager. A este tipo de seguridad se le conoce como control de acceso basado en rol (RBAC). Para administrar este acceso, use el **control de acceso (IAM)** en Azure Portal.

Con el modelo de Resource Manager, se coloca la cuenta de almacenamiento en un grupo de recursos y se controla el acceso al plano de la administración de dicha cuenta de almacenamiento específica mediante Azure Active Directory. Por ejemplo, puede proporcionar a usuarios específicos la posibilidad de tener acceso a las claves de cuenta de almacenamiento, mientras que otros usuarios pueden ver información sobre la cuenta de almacenamiento pero no pueden tener acceso a sus claves.

#### <a name="granting-access"></a>Concesión de acceso

El acceso se concede mediante la asignación de rol RBAC adecuado a los usuarios, grupos y aplicaciones en el ámbito correcto. Para conceder acceso a toda la suscripción, asigne un rol a nivel de suscripción. Puede conceder acceso a todos los recursos de un grupo de recursos mediante la concesión de permisos al propio grupo de recursos. También puede asignar roles específicos a recursos específicos, como cuentas de almacenamiento.

Estos son los puntos principales que necesita saber acerca del uso del control de acceso basado en rol para tener acceso a las operaciones de administración de una cuenta de Azure Storage:

* Para que otro usuario pueda tener acceso a los objetos de datos de la cuenta de almacenamiento, puede concederle permiso para leer las claves de cuenta de almacenamiento. De este modo, ese usuario podrá usar esas claves para tener acceso a los archivos y directorios.
* Los roles pueden asignarse específicamente a una cuenta de usuario, un grupo de usuarios o una aplicación.
* Cada rol tiene una lista de acciones y no acciones. Por ejemplo, el rol de colaborador de la máquina virtual tiene una acción "listKeys" que permite leer las claves de cuenta de almacenamiento. El colaborador tiene "no acciones", como actualizar el acceso de los usuarios en Active Directory.
* Entre los roles para el almacenamiento se incluyen, entre otros, los siguientes:

  * Propietario: puede administrar todo, incluido el acceso.
  * Colaborador: puede hacer lo mismo que el propietario, salvo asignar acceso. Un usuario con este rol puede ver y volver a generar las claves de cuenta de almacenamiento. Con las claves de cuenta de almacenamiento, puede tener acceso a los objetos de datos.
  * Lector: puede ver información sobre la cuenta de almacenamiento, excepto los secretos. Por ejemplo, si asigna un rol con permisos de lectura sobre la cuenta de almacenamiento a otro usuario, este puede ver las propiedades de la cuenta de almacenamiento, pero no puede realizar cambios en las propiedades ni ver las claves de cuenta de almacenamiento.
  * Colaborador de la cuenta de almacenamiento: puede administrar la cuenta de almacenamiento, leer los grupos de recursos y los recursos de la suscripción, y crear y administrar implementaciones de los grupos de recursos de la suscripción. También puede acceder a las claves de cuenta de almacenamiento, lo que a su vez implica que puede acceder al plano de los datos.
  * Administrador de acceso de usuarios: puede administrar el acceso de los usuarios a la cuenta de almacenamiento. Por ejemplo, puede conceder acceso de lectura a un usuario específico.
  * Colaborador de la máquina virtual: puede administrar máquinas virtuales, pero no la cuenta de almacenamiento o la red virtual a la que están conectadas. Este rol puede listar las claves de cuenta de almacenamiento, lo que significa que el usuario a quien asigne este rol puede actualizar el plano de los datos.

    Para que un usuario pueda crear una máquina virtual, tiene que poder crear el archivo VHD correspondiente en una cuenta de almacenamiento. Para ello, es necesario que pueda recuperar la clave de la cuenta de almacenamiento y pasarla a la API que está creando la máquina virtual. Por lo tanto, es necesario que disponga de este permiso para listar las claves de cuenta de almacenamiento.
* La posibilidad de definir roles personalizados es una característica que le permite crear un conjunto de acciones desde una lista de acciones disponibles que se puede llevar a cabo sobre recursos de Azure.
* Es necesario que el usuario se haya configurado en Azure Active Directory para poder asignarle un rol.
* Puede crear un informe de quién concedió/revocó qué tipo de acceso a quién y en qué ámbito con PowerShell o la CLI de Azure.

#### <a name="resources"></a>Recursos

* [Control de acceso basado en roles de Azure Active Directory](../../role-based-access-control/role-assignments-portal.md)

  En este artículo se explica el control de acceso basado en rol de Azure Active Directory y su funcionamiento.
* [RBAC: Roles integrados](../../role-based-access-control/built-in-roles.md)

  En este artículo se detalla todas las funciones integradas disponibles en el control de acceso basado en rol.
* [Descripción de la implementación de Resource Manager y la implementación clásica](../../azure-resource-manager/resource-manager-deployment-model.md)

### <a name="managing-your-storage-account-keys"></a>Administración de las claves de cuenta de almacenamiento

Las claves de la cuenta de almacenamiento son cadenas de 512 bits creadas por Azure que, junto con el nombre de la cuenta de almacenamiento, pueden usarse para acceder a los objetos de datos almacenados en la cuenta de almacenamiento; por ejemplo, blobs, entidades dentro de una tabla, mensajes de una cola y archivos en un recurso compartido de archivos de Azure. El control del acceso a las claves de cuenta de almacenamiento controla el acceso al plano de los datos de esa cuenta de almacenamiento.

Cada cuenta de almacenamiento tiene dos claves que se conocen como "Key 1" y "Key 2" en [Azure Portal](https://portal.azure.com/) y en los cmdlets de PowerShell. Es posible volver a generarlas manualmente con diferentes métodos, como el uso de [Azure Portal](https://portal.azure.com/), PowerShell o la CLI de Azure, o mediante programación con la biblioteca de clientes de Storage de .NET o la API de REST de los servicios de Azure Storage.

Hay varias razones que justifican volver a generar las claves de cuenta de almacenamiento.

* Puede cambiarlas de nuevo de manera periódica por motivos de seguridad.
* Podría volver a generar las claves de cuenta de almacenamiento si alguien consiguiera atacar una aplicación y recuperar la clave que se codificó de forma rígida o se guardó en un archivo de configuración, con lo que tendría acceso completo a la cuenta de almacenamiento.
* También podría volver a generar la clave si el equipo utilizara una aplicación de explorador de Storage que conserve la clave de la cuenta de almacenamiento y uno de los miembros abandonara el equipo. La aplicación seguiría funcionando y permitiría a ese antiguo miembro acceder a la cuenta de almacenamiento cuando ya no forme parte del equipo. Este es realmente el motivo principal por el que se crean Firmas de acceso compartido de nivel de cuenta, ya que al utilizarlas no es necesario almacenar las claves de acceso en un archivo de configuración.

#### <a name="key-regeneration-plan"></a>Plan de regeneración de claves

No es buena idea limitarse a volver a generar la clave que está utilizando sin planificación. Si no lo hace así, podría cortar todo el acceso a esa cuenta de almacenamiento, lo que podría provocar una interrupción importante. Este es el motivo por el cual hay dos claves. Puede volver a generar una de ellas y luego la otra en vez de hacerlo de manera simultánea.

Antes de volver a generar las claves, asegúrese de que dispone de una lista de todas las aplicaciones que dependen de la cuenta de almacenamiento, así como cualquier otro servicio que se use en Azure. Por ejemplo, si usa instancias de Azure Media Services que dependen de su cuenta de almacenamiento, debe volver a sincronizar las claves de acceso con los servicios multimedia después de volver a generar las claves. Si está utilizando otras aplicaciones, como un explorador de Storage, debe proporcionarles también las nuevas claves. Si tiene máquinas virtuales cuyos archivos VHD se almacenan en la cuenta de almacenamiento, estos no se verán afectados por la regeneración de las claves de cuenta de almacenamiento.

Puede volver a generar las claves en Azure Portal. Una vez que se vuelven a generar, las claves pueden tardar hasta diez minutos en sincronizarse en todos los servicios de Storage.

Cuando esté listo, este es el proceso general que detalla cómo se debe cambiar la clave. En este caso, suponemos que actualmente usa la Clave 1 y que va a cambiar todo para usar la Clave 2 en su lugar.

1. Vuelva a generar la Clave 2 para asegurarse de que sea segura. Puede hacerlo en Azure Portal.
2. En todas las aplicaciones donde se almacena la clave de almacenamiento, cámbiela para usar el nuevo valor de Key 2. Pruebe y publique la aplicación.
3. Una vez que todas las aplicaciones y los servicios estén activos y se estén ejecutando correctamente, vuelva a generar la Clave 1. De este modo se asegura de que nadie a quien no haya proporcionado expresamente la nueva clave seguirá teniendo acceso a la cuenta de almacenamiento.

Si actualmente utiliza la Clave 2, siga este mismo proceso pero invierta los nombres de las claves.

Puede migrar en un par de días, cambiando cada aplicación para que use la nueva clave y publicándola. Cuando lo haya hecho con todas, debe volver y generar de nuevo la clave antigua para que deje de funcionar.

Otra opción consiste en poner la clave de la cuenta de almacenamiento en un [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) como un secreto y hacer que las aplicaciones recuperen la clave desde esa ubicación. Así, cuando vuelva a generar la clave y actualice Azure Key Vault, no será necesario volver a implementar las aplicaciones porque tomarán la nueva clave desde dicho almacén automáticamente. Puede hacer que la aplicación lea la clave cada vez que la necesite, o bien puede almacenarla en la memoria caché y, si se produce un error al usarla, se recuperaría de nuevo desde Azure Key Vault.

El uso de Azure Key Vault también agrega otro nivel de seguridad a las claves de almacenamiento. Si utiliza este método, nunca tendrá la clave de almacenamiento codificada de manera rígida en un archivo de configuración, lo que impide que alguien obtenga acceso a las claves sin permiso específico.

Otra ventaja de usar Azure Key Vault es que también puede controlar el acceso a las claves con Azure Active Directory. Esto significa que puede conceder acceso a la serie de aplicaciones que necesitan recuperar las claves de Azure Key Vault, y sabe que otras aplicaciones no podrán tener acceso a las claves sin concederles el permiso específicamente.

> [!NOTE]
> Microsoft recomienda usar solo una de las claves en todas las aplicaciones al mismo tiempo. Si utiliza la Clave 1 en algunos lugares y la Clave 2 en otros, no podrá rotar las claves sin que alguna aplicación pierda el acceso.

#### <a name="resources"></a>Recursos

* [Administración de la configuración de cuentas de almacenamiento en Azure Portal](storage-account-manage.md)
* [Referencia de API de REST de proveedor de recursos de Azure Storage](https://msdn.microsoft.com/library/mt163683.aspx)

## <a name="data-plane-security"></a>Seguridad en el plano de los datos
La seguridad en el plano de los datos hace referencia a los métodos utilizados para proteger los objetos de datos almacenados en Azure Storage. Hemos visto métodos para cifrar los datos y la seguridad durante el tránsito de los datos, pero ¿qué hay sobre controlar el acceso a los objetos?

Tiene tres opciones para autorizar el acceso a los objetos de datos en Azure Storage, que incluyen:

- Uso de Azure AD para autorizar el acceso a los sistemas de archivos y las colas. Azure AD proporciona ventajas sobre otros enfoques para la autorización, incluida la eliminación de la necesidad de almacenar secretos en el código. Para obtener más información, consulte [Autenticación del acceso a Azure Storage con Azure Active Directory](storage-auth-aad.md). 
- Uso de las claves de la cuenta de almacenamiento para autorizar el acceso mediante una clave compartida. La autorización mediante una clave compartida requiere almacenar las claves de la cuenta de almacenamiento en la aplicación, por lo que Microsoft recomienda usar Azure AD en su lugar, siempre que sea posible. Para las aplicaciones de producción o para autorizar el acceso a los archivos y las tablas de Azure, puede seguir usando una clave compartida en tanto que la integración con Azure AD está en versión preliminar.
- Uso de firmas de acceso compartido para conceder permisos controlados para objetos de datos específicos por un período de tiempo determinado.

Además de limitar el acceso mediante la autorización, puede usar [firewalls y redes virtuales](storage-network-security.md) para limitar el acceso a la cuenta de almacenamiento basado en reglas de red.  Este método permite denegar el acceso al tráfico de Internet público y conceder acceso a determinadas redes virtuales de Azure o determinados intervalos de direcciones IP de Internet públicas.

### <a name="storage-account-keys"></a>Claves de cuenta de almacenamiento

Las claves de cuenta de almacenamiento son cadenas de 512 bits creadas por Azure que, junto con el nombre de la cuenta de almacenamiento, pueden utilizarse para tener acceso a los objetos de datos almacenados en la cuenta de almacenamiento.

Por ejemplo, puede leer archivos. Muchas de estas acciones pueden realizarse a través de Azure Portal, o usando una de las muchas aplicaciones del Explorador de Storage. También puede escribir código para usar la API de REST para realizar estas operaciones.

Como se ha tratado en la sección sobre la [seguridad del plano de administración](#management-plane-security), el acceso a las claves de almacenamiento para una cuenta de almacenamiento mediante el modelo de Azure Resource Manager puede controlarse mediante el control de acceso basado en rol (RBAC).

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Delegación del acceso a objetos en la cuenta mediante Firmas de acceso compartido y Directivas de acceso almacenadas

Una Firma de acceso compartido es una cadena que contiene un token de seguridad que puede asociarse a un URI que le permite delegar el acceso a objetos de almacenamiento y especificar restricciones, como permisos o un intervalo de fecha y hora para el acceso.

Puede conceder acceso a archivos o directorios.

Podría dar a un cliente un token de Firma de acceso compartido que pueda utilizar para cargar imágenes a un sistema de archivos en Blob Storage y conceder permiso a una aplicación web para que lea esas imágenes. En ambos casos, se trata de procesos independientes: es posible dar a cada aplicación solo el acceso que requiere para realizar sus tareas. Esto es posible mediante el uso de Firmas de acceso compartido.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Razones para usar las Firmas de acceso compartido

¿Por qué desearía utilizar una Firma de acceso compartido en lugar de simplemente proporcionar la clave de la cuenta de almacenamiento, que es mucho más fácil? Si comparte la clave de su cuenta de almacenamiento es como si prestara a alguien las llaves de su reino de almacenamiento. Con ella, es posible acceder a todo el contenido. Cualquiera podría usar sus claves y cargar toda su biblioteca de música en la cuenta de almacenamiento. También podría reemplazar sus archivos por versiones infectadas o robar sus datos. Conceder acceso ilimitado a su cuenta de almacenamiento es algo que no debe hacerse a la ligera.

Con las Firmas de acceso compartido, es posible conceder a un cliente únicamente los permisos que necesite durante un período de tiempo limitado. Por ejemplo, si alguien está cargando un archivo a su cuenta, puede concederle acceso de escritura durante un tiempo suficiente que le permita cargar el archivo (dependerá del tamaño del archivo, por supuesto). Y si cambia de opinión, puede revocar el acceso.

Además, puede especificar que las solicitudes realizadas mediante una Firma de acceso compartido se limiten a una cierta dirección IP o un intervalo de direcciones IP externas a Azure. También puede requerir que las solicitudes se realicen mediante un protocolo específico (HTTPS o HTTP/HTTPS). Esto significa que si solo desea permitir el tráfico HTTPS, puede establecer que el protocolo requerido sea solo HTTPS y se bloqueará el tráfico HTTP.

#### <a name="definition-of-a-shared-access-signature"></a>Definición de una Firma de acceso compartido

Una Firma de acceso compartido es un conjunto de parámetros de consulta anexo a la dirección URL que señala al recurso

que proporciona información sobre el acceso permitido y el intervalo de tiempo durante el que se permite el acceso. Aquí presentamos un ejemplo: este URI proporciona acceso de lectura a un blob durante cinco minutos. Los parámetros de consulta de la Firma de acceso compartido deben presentar codificación URL; por ejemplo %3A para los dos puntos (:) o %20 para un espacio.

```
http://mystorage.dfs.core.windows.net/myfilesystem/myfile.txt (URL to the file)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)
```

#### <a name="how-the-shared-access-signature-is-authorized-by-the-azure-storage-service"></a>Autorización de la Firma de acceso compartido por el servicio Azure Storage

Cuando el servicio de almacenamiento recibe la solicitud, toma los parámetros de consulta de entrada y crea una firma utilizando el mismo método que el programa que realiza la llamada. A continuación, compara las dos firmas. Si coinciden, el servicio de Storage puede comprobar la versión del servicio para asegurarse de que es válida, comprobar que la fecha y la hora actuales están dentro del período especificado, asegurarse de que el acceso solicitado corresponde a la solicitud realizada, etc.

Por ejemplo, con la dirección URL anterior, si la dirección URL apuntase a un archivo en lugar de a un blob, esta solicitud produciría un error porque especifica que la Firma de acceso compartido es para un blob. Si el comando REST al que se llama fuera para actualizar un blob, se produciría un error porque la Firma de acceso compartido especifica que se permite acceso de solo lectura.

#### <a name="types-of-shared-access-signatures"></a>Tipos de Firmas de acceso compartido

* Es posible utilizar una Firma de acceso compartido a nivel de servicio para tener acceso a recursos específicos de una cuenta de almacenamiento. Algunos ejemplos de esto son recuperar una lista de archivos en un sistema de archivos o descargar un archivo.
* Una Firma de acceso compartido a nivel de cuenta se puede utilizar para tener acceso a lo mismo para lo que se puede usar una Firma de acceso compartido a nivel de servicio. Además, puede dar opciones a los recursos que no se permiten con una Firma de acceso compartido a nivel de servicio, como la posibilidad de crear sistemas de archivos.

#### <a name="creating-a-sas-uri"></a>Creación de un URI de Firma de acceso compartido

1. Puede crear un URI a petición, definiendo todos los parámetros de consulta cada vez.

   Este enfoque es realmente flexible, pero si tiene un conjunto lógico de parámetros que se parecen cada vez, es mejor utilizar una Directiva de acceso almacenada.
2. Puede crear una Directiva de acceso almacenada para un sistema de archivos completo, el recurso compartido de archivos, la tabla o la cola. A continuación, puede utilizar esto como base para los URI de Firma de acceso compartido que cree. Los permisos basados en Directivas de acceso almacenadas se pueden revocar fácilmente. Puede tener hasta cinco directivas definidas en cada sistema de archivos, cola, tabla o recurso compartido de archivos.

   Por ejemplo, si en su caso muchas personas fueran a leer los blobs de un sistema de archivos determinado, podría crear una Directiva de acceso almacenada para "conceder acceso de lectura" y cualquier otro valor que se repita. A continuación, puede crear un URI de Firma de acceso compartido mediante la configuración de la Directiva de acceso almacenada y la especificación de la fecha y hora de expiración. Esto presenta la ventaja de que no es necesario especificar siempre todos los parámetros de consulta.

#### <a name="revocation"></a>Revocación

Supongamos que se ha puesto en riesgo su Firma de acceso compartido o que desea cambiarla porque así se lo exigen los requisitos de seguridad de la empresa o el cumplimiento normativo. ¿Cómo se podría revocar el acceso a un recurso que use esa Firma de acceso compartido? Depende de cómo haya creado el URI de la Firma de acceso compartido.

Si utiliza URI ad hoc, tiene tres opciones. Puede emitir tokens de Firmas de acceso compartido con directivas de expiración breves y esperar a que las firmas expiren. Puede cambiar el nombre del recurso o eliminarlo (suponiendo que el token estuviera limitado a un único objeto). Puede cambiar las claves de cuenta de almacenamiento. Esta última opción puede tener importantes repercusiones, en función de cuántos servicios estén utilizando esa cuenta de almacenamiento, y no suele ser una buena idea hacerlo sin planificarlo previamente.

Si está utilizando una Firma de acceso compartido derivada de una directiva de acceso almacenada, puede quitar el acceso revocando la Directiva de acceso almacenada (puede cambiar solo lo que ya ha expirado o bien quitarlo todo en su conjunto). Esto surte efecto inmediatamente e invalida todas las Firmas de acceso compartido creadas utilizando esa Directiva de acceso almacenada. El hecho de actualizar o quitar la Directiva de acceso almacenada puede repercutir en los usuarios que accedan específicamente a ese sistema de archivos, recurso compartido de archivos, tabla o cola a través de la Firma de acceso compartido, pero si se han escrito los clientes de manera que soliciten una nueva Firma de acceso compartido cuando la anterior deje de ser válida, esto funcionará correctamente.

Dado que el uso de una Firma de acceso compartido derivada de una Directiva de acceso almacenada ofrece la posibilidad de revocar esa firma de acceso compartido de inmediato, se recomienda usar siempre las Directivas de acceso almacenadas cuando sea posible.

#### <a name="resources"></a>Recursos

Para más información sobre el uso de Firmas de acceso compartido y Directivas de acceso almacenadas, junto con ejemplos, consulte los artículos siguientes:

* Estos son los artículos de referencia.

  * [Ejemplos SAS del servicio.](https://msdn.microsoft.com/library/dn140256.aspx)

    Este artículo proporciona ejemplos de cómo utilizar una SAS de nivel de servicio con blobs, mensajes de cola, intervalos de tabla y archivos.
  * [Creación de una SAS de servicio](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Creación de una SAS de cuenta](https://msdn.microsoft.com/library/mt584140.aspx)
* Se trata de tutoriales para usar la biblioteca de cliente .NET a fin de crear Firmas de acceso compartido y Directivas de acceso almacenadas.

  * [Uso de Firmas de acceso compartido (SAS)](../storage-dotnet-shared-access-signature-part-1.md)
  * [Firmas de acceso compartido, Parte 2: Creación y uso de una Firma de acceso compartido con Blob service](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Este artículo incluye una explicación del modelo de SAS, ejemplos de SAS y recomendaciones para el mejor uso práctico de SAS. También trata la revocación de los permisos concedidos.

* Authentication

  * [Autenticación para los servicios de Azure Storage](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Tutorial de introducción a las Firmas de acceso compartido

  * [Getting Started with Shared Access Signatures (SAS) (Introducción a las Firmas de acceso compartido)](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Cifrado en tránsito

### <a name="transport-level-encryption--using-https"></a>Cifrado de nivel de transporte – Uso de HTTPS

Otro paso que debe seguir para garantizar la seguridad de los datos de Azure Storage es cifrar los datos entre el cliente y Azure Storage. La primera recomendación es utilizar siempre el protocolo [HTTPS](https://en.wikipedia.org/wiki/HTTPS), que garantiza una comunicación segura a través de la red pública de Internet.

Para tener un canal de comunicación seguro, siempre debe usar HTTPS al llamar a las API de REST u obtener acceso a objetos de almacenamiento. Además, las **Firmas de acceso compartido**, que pueden utilizarse para delegar el acceso a objetos de Azure Storage, incluyen una opción para especificar que se utilice solo el protocolo HTTPS con las Firmas de acceso compartido, lo que garantiza que cualquiera que envíe vínculos con tokens de SAS utilice el protocolo adecuado.

Puede exigir el uso de HTTPS al llamar a las API de REST para acceder a objetos de cuentas de almacenamiento habilitando la opción [Se requiere transferencia segura](../storage-require-secure-transfer.md) para la cuenta de almacenamiento. Una vez que esta opción esté habilitada, se rechazarán las conexiones que usan HTTP.

## <a name="encryption-at-rest"></a>Cifrado en reposo

### <a name="storage-service-encryption-sse"></a>cifrado del servicio de almacenamiento (SSE)

SSE está habilitado para todas las cuentas de almacenamiento y no puede deshabilitarse. SSE cifra automáticamente los datos al escribirlos en Azure Storage. Al leer datos de Azure Storage, Azure Storage los descifra antes de devolverlos. SSE permite proteger los datos sin tener que modificar el código o agregar código a las aplicaciones.

Puede usar claves administrada por Microsoft o sus propias claves personalizadas. Microsoft genera las claves administradas y se encarga de guardarlas en un lugar seguro y de cambiarlas de forma periódica de acuerdo con la directiva interna de Microsoft. Para más información sobre el uso de claves personalizadas, consulte [Uso de Storage Service Encryption con claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md).
