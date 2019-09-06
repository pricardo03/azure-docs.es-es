---
title: 'Ejecución segura del servicio de administración de certificados OPC Vault: Azure | Microsoft Docs'
description: En este artículo se describe cómo ejecutar de manera segura el servicio de administración de certificados OPC Vault en Azure y otras directrices de seguridad que se deben tener en cuenta.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: f35836f60fae11c0955c128e96a4cea188681942
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997659"
---
# <a name="how-to-run-the-opc-vault-certificate-management-service-securely"></a>Ejecución segura del servicio de administración de certificados OPC Vault

En este artículo se explica cómo ejecutar de manera segura el servicio de administración de certificados OPC Vault en Azure y otras directrices de seguridad que se deben tener en cuenta.

## <a name="roles"></a>Roles

### <a name="trusted-and-authorized-roles"></a>Roles de confianza y autorizados

El microservicio OPC Vault está configurado para permitir que distintos roles accedan a varias partes del servicio.

> [!IMPORTANT]
> Durante la implementación, el script solo agrega el usuario que ejecuta el script de implementación como usuario para todos los roles.
> Esta asignación de roles se debe revisar para una implementación de producción y se debe volver a configurar de manera adecuada según las instrucciones siguientes.
> Esta tarea requiere la asignación manual de los roles y los servicios en el portal de aplicaciones empresariales de Azure AD.

### <a name="certificate-management-service-roles"></a>Roles de servicios de administración de certificados

El microservicio OPC Vault define los roles siguientes:

- **Lector**: de manera predeterminada, todo usuario autenticado en el inquilino tiene acceso de lectura. 
  - Acceso de lectura a aplicaciones y solicitudes de certificado. Puede enumerar y consultar aplicaciones y solicitudes de certificados. Con el acceso de lectura, también se puede acceder a la información de detección de dispositivos y a los certificados públicos.
- **Escritor**: el rol Escritor se asigna a un usuario para agregar permisos de escritura para determinadas tareas. 
  - Acceso de lectura/escritura a aplicaciones y solicitudes de certificado. Puede registrar, actualizar y anular el registro de aplicaciones. Puede crear solicitudes de certificado y obtener certificados y claves privadas aprobadas. También puede eliminar claves privadas.
- **Aprobador**: el rol Aprobador se asigna a un usuario para aprobar o rechazar solicitudes de certificado. El rol no incluye ningún otro rol.
  - Además del rol Aprobador para tener acceso a la API del microservicio OPC Vault, el usuario debe tener también el permiso de firma de claves en Key Vault para poder firmar los certificados.
  - Los roles Escritor y Aprobador se deben asignar a distintos usuarios.
  - El rol principal del Aprobador es aprobar la generación y el rechazo de las solicitudes de certificado.
- **Administrator**: El rol Administrador se asigna a un usuario para administrar los grupos de certificados. El rol no admite el rol Aprobador, pero sí incluye el rol Escritor.
  - El administrador puede administrar los grupos de certificados, cambiar la configuración y revocar los certificados de aplicación mediante la emisión de una CRL nueva.
  - Idealmente, los roles Escritor, Aprobador y Administrador se asignan a distintos usuarios. Para mayor seguridad, un usuario con el rol Aprobador o Administrador también necesita el permiso de firma de claves en KeyVault para emitir certificados o para renovar un certificado de la entidad de certificación del emisor.
  - Además del rol de administración del microservicio, el rol incluye, entre otros elementos, lo siguiente:
    - La responsabilidad de administrar la implementación de los procedimientos de seguridad de la entidad de certificación.
    - La administración de la generación, revocación y suspensión de los certificados. 
    - La administración del ciclo de vida de las claves criptográficas (por ejemplo, la renovación de las claves de la entidad de certificación del emisor).
    - La instalación, la configuración y el mantenimiento de los servicios que operan la entidad de certificación.
    - La operación diaria de los servicios. 
    - La copia de seguridad y recuperación de bases de datos y entidades de certificación.

### <a name="other-role-assignments"></a>Otras asignaciones de roles

Los roles siguientes también se deben tener en cuenta y asignar al ejecutar el servicio:

- El propietario empresarial del contrato de adquisición de certificados con la entidad de certificación raíz externa (en el caso en que el propietario adquiera los certificados de una entidad de certificación externa u opere una entidad de certificación subordinada a una CA externa).
- El desarrollo y la validación de la entidad de certificación.
- La revisión de los registros de auditoría.
- El personal que ayuda a respaldar la entidad de certificación o a administrar las instalaciones físicas y en la nube, pero que no son de confianza directamente para realizar operaciones de CA, se han definido para que tengan el rol autorizado. También se debe documentar el conjunto de tareas que pueden realizar quienes cuentan con el rol autorizado.

### <a name="memberships-of-trusted-and-authorized-roles-must-be-reviewed-annually"></a>Las pertenencias de roles de confianza y autorizados se deben revisar anualmente

La pertenencia de los roles de confianza y autorizados se debe revisar al menos trimestralmente para asegurarse de que el conjunto de personas (para procesos manuales) o identidades de servicio (para procesos automatizados) de cada rol se mantiene al mínimo.

### <a name="certificate-issuance-process-must-enforce-role-separation-between-certificate-requester-and-approver"></a>El proceso de emisión de certificados debe aplicar la separación de roles entre el solicitante del certificado y el aprobador

El proceso de emisión de certificados debe aplicar la separación de roles entre el solicitante de certificados y los roles de aprobador de certificados (personas o sistemas automatizados). La emisión de certificados debe estar autorizada por un rol de aprobador de certificados que compruebe que el solicitante de certificados está autorizado para obtener certificados. Las personas que tienen el rol de aprobador de certificados deben ser personas formalmente autorizadas.

### <a name="privileged-role-management-must-restrict-access-and-be-reviewed-quarterly"></a>La administración de roles con privilegios debe restringir el acceso y revisarse trimestralmente

La asignación de roles con privilegios, como la autorización de la pertenencia del grupo de administradores y aprobadores, se debe restringir a un conjunto limitado de personal autorizado. A cualquier cambio de un rol con privilegios se le debe revocar el acceso en 24 horas. Por último, las asignaciones de roles con privilegios deben revisarse trimestralmente y se debe quitar toda asignación innecesaria o expirada.

### <a name="privileged-roles-should-use-two-factor-authentication"></a>Los roles con privilegios deben usar la autenticación en dos fases

La autenticación multifactor (autenticación en dos fases, MFA o TFA) se debe usar para los inicios de sesión interactivos de los aprobadores y administradores en el servicio.

## <a name="certificate-service-operation-guidelines"></a>Directrices para la operación del servicio de certificados

### <a name="operational-contacts"></a>Contactos operativos

El servicio de certificado debe tener un plan de respuesta de seguridad actualizado en el archivo, el que contiene los contactos detallados de respuesta a los incidentes operativos.

### <a name="security-updates"></a>Actualizaciones de seguridad

Todos los sistemas se deben supervisar y actualizar continuamente con el cumplimiento de revisiones o las actualizaciones de seguridad más recientes.

> [!IMPORTANT]
> El repositorio de GitHub del servicio OPC Vault se actualiza continuamente con las revisiones de seguridad. Las actualizaciones en GitHub se deben supervisar y las actualizaciones se aplicarán al servicio a intervalos regulares.

### <a name="security-monitoring"></a>Supervisión de la seguridad

Suscríbase o implemente la supervisión de seguridad adecuada, por ejemplo, mediante la suscripción a una solución de supervisión central (como Azure Security Center, la solución de supervisión de O365) y configúrela correctamente para asegurarse de que los eventos de seguridad se transmiten a la solución de supervisión.

> [!IMPORTANT]
> De manera predeterminada, el servicio OPC Vault se implementa con [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/devops) como solución de supervisión. Es muy recomendable agregar una solución de seguridad como [Azure Security Center](https://azure.microsoft.com/services/security-center/).

### <a name="assess-security-of-open-source-software-components"></a>Evaluación de la seguridad de los componentes de software de código abierto

Todos los componentes de código abierto que se usan dentro de un producto o servicio deben estar libres de vulnerabilidades de seguridad moderadas o mayores.

> [!IMPORTANT]
> El repositorio de GitHub del servicio OPC Vault examina todos los componentes durante las compilaciones de integración continua para detectar vulnerabilidades. Las actualizaciones en GitHub se deben supervisar y las actualizaciones se aplicarán al servicio a intervalos regulares.

### <a name="maintain-an-inventory"></a>Mantenimiento de un inventario

Se debe mantener un inventario de recursos para todos los hosts de producción (incluidas las máquinas virtuales persistentes), los dispositivos, todos los intervalos de direcciones IP internas, las VIP y los nombres de dominio DNS públicos. Para actualizar este inventario, agregue o quite un sistema, una dirección IP de dispositivo, una VIP o un dominio DNS público en un plazo de 30 días.

#### <a name="inventory-of-the-default-azure-opc-vault-microservice-production-deployment"></a>Inventario de la implementación de producción predeterminada del microservicio Azure OPC Vault: 

En **Azure**:
- **App Service Plan** (Plan de App Service): plan de App Service para los hosts de servicios. El valor predeterminado es S1.
- **App Service** para el microservicio: el host del servicio OPC Vault.
- **App Service** para la aplicación de ejemplo: el host de la aplicación de ejemplo de OPC Vault.
- **KeyVault Standard**: para almacenar secretos y claves de Cosmos DB para los servicios web.
- **KeyVault Premium**: para hospedar las claves de la entidad de certificación del emisor, para el servicio de firma, para la configuración del almacén y el almacenamiento de claves privadas de aplicación.
- **Cosmos DB**: base de datos para solicitudes de certificado y aplicación. 
- **Application Insights**: (opcional) solución de supervisión para aplicación y servicio web.
- **Registro de aplicación de AzureAD**: un registro de la aplicación de ejemplo, el servicio y el módulo Edge.

Para los servicios en la nube, se deben documentar todos los nombres de host, grupos de recursos, nombres de recursos, id. de suscripción y TenantId que se usaron para implementar el servicio. 

En **IoT Edge** o un **servidor de IoT Edge**:
- **Módulo IoT Edge de OPC Vault**: para admitir una instancia del servidor de detección global de OPC UA de la red de la fábrica. 

En el caso de los dispositivos IoT Edge, se deben documentar los nombres de host y las direcciones IP. 

### <a name="document-the-certification-authorities-cas"></a>Documentación de las entidades de certificación (CA)

La documentación de la jerarquía de CA debe contener todas las entidades de certificación operativas, incluidas todas las entidades de certificación subordinadas, las entidades de certificación principales y las entidades de certificación raíz, incluso cuando no estén administradas por el servicio. Se puede proporcionar un conjunto minucioso de todos los certificados de entidad de certificación no expirados en lugar de la documentación formal.

> [!NOTE]
> La aplicación de ejemplo de OPC Vault admite la descarga de todos los certificados usados y producidos en el servicio para la documentación.

### <a name="document-the-issued-certificates-by-all-certification-authorities-cas"></a>Documentación de los certificados emitidos por todas las entidades de certificación (CA)

Se debe proporcionar un conjunto minucioso de todos los certificados emitidos en los últimos 12 meses para documentación.

> [!NOTE]
> La aplicación de ejemplo de OPC Vault admite la descarga de todos los certificados usados y producidos en el servicio para la documentación.

### <a name="document-the-sop-for-securely-deleting-cryptographic-keys"></a>Documentación de SOP para eliminar de manera segura las claves criptográficas

La eliminación de claves solo puede producirse durante la vigencia de una entidad de certificación, es decir, es por esto que ningún usuario tiene asignado el permiso Eliminación de certificado de KeyVault y por qué no hay ninguna API expuesta para eliminar un certificado de entidad de certificación del emisor. El procedimiento operativo estándar manual para eliminar de manera segura las claves criptográficas de la entidad de certificación solo está disponible si se accede directamente a KeyVault en Azure Portal y se elimina el grupo de certificados en KeyVault. Para garantizar la eliminación inmediata, se debe deshabilitar la [eliminación temporal de KeyVault](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="certificates"></a>Certificados

### <a name="certificates-must-comply-with-minimum-certificate-profile"></a>Los certificados deben cumplir el perfil de certificado mínimo

El servicio OPC Vault es una entidad de certificación (CA) en línea que emite certificados de entidad final a los suscriptores.
El microservicio OPC Vault sigue estas directrices en la implementación predeterminada.

- Todos los certificados deben incluir los siguientes campos X. 509, tal como se especifica a continuación:
  - El contenido del campo version debe ser v3. 
  - El contenido del campo serialNumber debe incluir al menos 8 bytes de entropía obtenidos de un generador de números aleatorios aprobado por FIPS (Estándar federal de procesamiento de información) 140.<br>
    > [!IMPORTANT]
    > De manera predeterminada, el número de serie de OPC Vault es 20 bytes y se obtiene del generador de números aleatorios criptográficos del sistema operativo. El generador de números aleatorios es aprobado por FIPS 140 aprobado en dispositivos Windows, pero no en tipos de Linux. Este hecho se debe tener en cuenta al elegir una implementación de servicio, que usa máquinas virtuales Linux o contenedores Docker de Linux, en los que la tecnología subyacente OpenSSL no es aprobada por FIPS 140.
  - Los campos issuerUniqueID y subjectUniqueID no deben aparecer.
  - Los certificados de entidad final se deben identificar con la extensión Restricciones básicas de acuerdo con IETF RFC 5280.
  - El campo pathLenConstraint se debe establecer en 0 para el certificado de entidad de certificación del emisor. 
  - La extensión Uso mejorado de clave debe estar presente y debe contener el conjunto mínimo de identificadores de objetos (OID) de Uso mejorado de clave. No se debe especificar el OID anyExtendedKeyUsage (2.5.29.37.0). 
  - La extensión Punto de distribución de lista de revocación de certificados (CDP) debe estar presente en el certificado de entidad de certificación del emisor.<br>
    > [!IMPORTANT]
    > La extensión Punto de distribución de lista de revocación de certificados (CDP) está presente en los certificados de entidad de certificación de OPC Vault, pero los dispositivos de OPC UA usan métodos personalizados para distribuir listas de revocación de certificados.
  - La extensión Acceso a información de entidad emisora debe estar presente en los certificados del suscriptor.<br>
    > [!IMPORTANT]
    > La extensión Acceso a información de entidad emisora está presente en los certificados de suscriptor de OPC Vault, pero los dispositivos de OPC UA usan métodos personalizados para distribuir la información de entidad de certificación del emisor.
- Se deben usar algoritmos asimétricos, longitudes de clave, funciones hash y modos de relleno aprobados.
  - **RSA** y **SHA-2** son los únicos algoritmos admitidos (*).
  - RSA se puede utilizar para el cifrado, el intercambio de claves y la firma.
  - El cifrado RSA solo debe usar los modos de relleno OAEP, RSA-KEM o RSA-PSS.
  - Se requieren longitudes de clave de 2048 bits o más.
  - Use la familia SHA-2 de algoritmos hash (SHA256, SHA384 y SHA512).
  - Las claves de CA raíz de RSA con una duración típica de 20 años o más deben ser de 4096 bits o más.
  - Las claves de la entidad de certificación del emisor de RSA deben tener al menos 2048 bits. Si la fecha de expiración del certificado de CA es posterior a 2030, la clave de CA debe ser de 4096 bits o más.
- Duración del certificado
  - Certificados de entidad de certificación raíz: El período máximo de validez del certificado para las CA raíz no debe superar los 25 años.
  - Certificados de entidad de certificación del emisor en línea o subentidad de certificación: el período máximo de validez del certificado para las CA que están en línea y emiten solo certificados de suscriptor no debe superar los seis años. Para estas CA, la clave de firma privada relacionada no se debe usar más de tres años para emitir certificados nuevos.<br>
    > [!IMPORTANT]
    > El certificado del emisor tal como se genera en el microservicio OPC Vault predeterminado sin una entidad de certificación raíz externa se trata como una subentidad de certificación en línea con los requisitos y la duración respectivos. La duración predeterminada se establece en cinco años con una longitud de clave de 2048 o más.
  - Todas las claves asimétricas deben tener una duración máxima de cinco años, con una vigencia recomendada de un año.<br>
    > [!IMPORTANT]
    > De manera predeterminada, las duraciones de los certificados de aplicación emitidos con OPC Vault tienen una duración de dos años y se deben reemplazar todos los años. 
  - Cada vez que se renueva un certificado, se renueva con una clave nueva.
- Extensiones específicas de OPC UA en certificados de instancia de aplicación
  - La extensión subjectAltName incluye los nombres de host y el URI de la aplicación, que también pueden incluir direcciones FQDN, IPv4 e IPv6.
  - keyUsage incluye digitalSignature, nonRepudiation, keyEncipherment y dataEncipherment.
  - extendedKeyUsage incluye serverAuth o clientAuth.
  - authorityKeyIdentifier se especifica en los certificados firmados.

### <a name="certificate-authority-ca-keys-and-certificates-must-meet-minimum-requirements"></a>Los certificados y las claves de la entidad de certificación (CA) deben cumplir los requisitos mínimos

- **Claves privadas**: las claves de entidad de certificación del emisor de **RSA** deben tener al menos 2048 bits. Si la fecha de expiración del certificado de CA es posterior a 2030, la clave de CA debe ser de 4096 bits o más.
- **Duración**: el período máximo de validez del certificado para las CA que están en línea y emiten solo certificados de suscriptor no debe superar los seis años. Para estas CA, la clave de firma privada relacionada no se debe usar más de tres años para emitir certificados nuevos.

### <a name="ca-keys-are-protected-using-hardware-security-modules-hsm"></a>Las claves de CA están protegidas mediante módulos de seguridad de hardware (HSM)

- OpcVault usa Azure KeyVault Premium y las claves están protegidas por módulos de seguridad de hardware (HSM) de nivel 2 de FIPS 140-2. 

Los módulos criptográficos que utiliza Key Vault, sean HSM o software, tienen validación FIPS (Estándar federal de procesamiento de información).<br>
Las claves creadas o importadas protegidas con HSM se procesan dentro de un HSM y se validan con FIPS 140-2 de nivel 2.<br>
Las claves creadas o importadas con protección de software se procesan dentro de módulos criptográficos y se validan con FIPS 140-2 de nivel 1.

## <a name="operational-practices"></a>Prácticas operativas

### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-enrollment"></a>Documentación y mantenimiento de las prácticas operativas de infraestructura de clave pública estándar para la inscripción de certificado

Documente y mantenga procedimientos operativos estándar (PNT) para el modo en que las entidades de certificación emiten certificados, lo que incluye: 
- Cómo se identifica y se autentica el suscriptor. 
- Cómo se procesa y valida la solicitud de certificado (si es aplicable, incluya también cómo se procesan las solicitudes de renovación de certificados y regeneración de claves). 
- Cómo se distribuyen los certificados emitidos a los suscriptores. 

El SOP del microservicio OPC Vault se describe en los documentos de [información general](overview-opc-vault-architecture.md) y [administración](howto-opc-vault-manage.md). Las prácticas siguen la especificación de arquitectura unificada de OPC, parte 12: Detección y servicios globales.


### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-revocation"></a>Documentación y mantenimiento de las prácticas operativas de infraestructura de clave pública estándar para la revocación de certificado

El proceso de revocación de certificado se describe en los documentos de [información general](overview-opc-vault-architecture.md) y [administración](howto-opc-vault-manage.md).
    
### <a name="document-certification-authority-ca-key-generation-ceremony"></a>Documentación del evento de generación de claves de la entidad de certificación (CA) 

La generación de claves de entidad de certificación del emisor en el microservicio OPC Vault se simplifica debido al almacenamiento seguro en Azure KeyVault y se describe en la documentación de [administración](howto-opc-vault-manage.md).

Sin embargo, cuando se usa una entidad de certificación raíz externa, un evento de generación de claves de entidad de certificación (CA) debe cumplir los siguientes requisitos:

El evento de generación de claves de CA se debe realizar en un script documentado que incluya al menos los elementos siguientes: 
1. Definición de roles y responsabilidades de los participantes.
2. Aprobación de la realización del evento de generación de claves de CA.
3. Materiales de activación y hardware criptográficos que se requieren para el evento.
4. Preparación de hardware (incluida la aprobación y la actualización de la información de recursos/configuración).
5. Instalación del sistema operativo.
6. Pasos específicos realizados durante el evento de generación de claves de entidad de certificación, por ejemplo: 
7. Instalación y configuración de la aplicación de entidad de certificación.
8. Generación de claves de entidad de certificación.
9. Copia de seguridad de clave de entidad de certificación.
10. Firma de certificado de entidad de certificación.
9. Importación de claves firmadas en el HSM protegido del servicio.
11. Cierre del sistema de entidad de certificación.
12. Preparación de los materiales para el almacenamiento.


## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a administrar OPC Vault de manera segura, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Protección de dispositivos de OPC UA con OPC Vault](howto-opc-vault-secure.md)