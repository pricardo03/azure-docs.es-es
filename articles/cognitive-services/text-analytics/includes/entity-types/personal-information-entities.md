---
title: Entidades con nombre de información personal
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/21/2019
ms.author: aahi
ms.openlocfilehash: 3aa4da9a9cf3d1d4b664e81f1fd18f2b225d731d
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73800135"
---
## <a name="personal-information-entity-types"></a>Tipos de entidades de información personal:

### <a name="phone-number"></a>Número de teléfono

Números de teléfono. 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo           | DESCRIPCIÓN                                           |
|------------------------|-------------------------------------------------------|
| N/D                    | Números de teléfono, como `+1 123-123-123`.          |
| Número de teléfono de la UE        | Números de teléfono específicos de la Unión Europea.         |
| Número de teléfono móvil de la UE | Números de teléfono móviles específicos de la Unión Europea. |

### <a name="eu-gps-coordinates"></a>Coordenadas GPS de la UE

 Coordenadas GPS de ubicaciones de la Unión Europea. 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | DESCRIPCIÓN                               |
|--------------|-------------------------------------------|
| N/D          | Coordenadas GPS de la Unión Europea |

### <a name="azure-information"></a>Información de Azure

Información de Azure identificable, incluida la información de autenticación y las cadenas de conexión. 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo                          | DESCRIPCIÓN                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Clave de autorización de Azure DocumentDB             | Clave de autorización para un servidor de Azure DocumentDB.                           |
| Cadena de conexión de base de datos IaaS de Azure | Cadena de conexión de una base de datos de infraestructura como servicio (IaaS) de Azure. |
| Cadena de conexión de Azure SQL           | Cadena de conexión de una instancia de Azure SQL Database.                                |
| Cadena de conexión de Azure IoT           | Cadena de conexión de Azure IoT (Internet de las cosas).                        |
| Contraseña de configuración de publicación de Azure        | Contraseña para la configuración de publicación de Azure.                                        |
| Cadena de conexión de Azure Redis Cache   | Cadena de conexión de una instancia de Azure Cache for Redis.                             |
| SAS de Azure                             | Cadena de conexión de SAS (software como servicio) de Azure Service Bus.                     |
| Cadena de conexión de Azure Service Bus   | Cadena de conexión de una instancia de Azure Service Bus.                                |
| Clave de la cuenta de Azure Storage             | Clave de una cuenta de Azure Storage.                                   |
| Clave de la cuenta de Azure Storage (genérica)   | Clave de cuenta genérica para una cuenta de Azure Storage.                           |
| Cadena de conexión de SQL Server          | Cadena de conexión de una instancia de SQL Server.                                         |

### <a name="identification"></a>Identificación

Lenguajes:

* Versión preliminar pública: `English`

#### <a name="financial-account-identification"></a>Identificación de cuenta financiera

| Nombre del subtipo               | DESCRIPCIÓN                                                                |
|----------------------------|----------------------------------------------------------------------------|
| Números de enrutamiento ABA        | Números de enrutamiento de tránsito de American Banker Association (ABA).                  |
| Código SWIFT                 | Códigos SWIFT de información de instrucciones de pago.                           |
| Tarjeta de crédito                | Números de tarjeta de crédito.                                                       |
| Código IBAN                  | Códigos IBAN de información de instrucciones de pago.                            |

#### <a name="government-and-country-specific-identification"></a>Identificación específica del país y del gobierno

Las entidades siguientes se agrupan y enumeran por país:

Argentina
* Número de identidad nacional (DNI)

Australia
* Número de expediente fiscal 
* Id. del permiso de conducir
* Id. del pasaporte
* Número de cuenta del seguro médico
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Bélgica
* Número nacional

Brasil
* Número de entidad jurídica (CNPJ)
* Número CPF
* Tarjeta de identificación nacional (RG)

Canadá
* Id. del pasaporte
* Id. del permiso de conducir
* Número de seguro médico
* Número de identificación personal médico (PHIN)
* Número del seguro social
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Chile
* Número del carné de identidad 

China
* Número del carné de identidad
* Número de la tarjeta de identificación de residente (PRC)

Croacia
* Número del carné de identidad
* Número de identificación personal (OIB)

República Checa
* Número de carné de identificación nacional

Dinamarca
* Número de identificación personal

Unión Europea (UE)
* Número de identificación nacional
* Id. del pasaporte
* Id. del permiso de conducir
* Número del seguro social (SSN) o identificador equivalente
* Número de identificación fiscal de la UE (TIN)
* Número de tarjeta de débito de la UE

Finlandia
* Número de identificación nacional
* Id. del pasaporte

Francia
* Tarjeta de identificación nacional (CNI)
* Número del seguro social (INSEE)
* Id. del pasaporte
* Id. del permiso de conducir

Alemania
* Número del carné de identidad
* Id. del pasaporte
* Id. del permiso de conducir

Grecia 
* Número de carné de identificación nacional

RAE de Hong Kong
* Número del carné de identidad (HKID)

India
* Número de cuenta permanente (PAN)
* Número de identificador único (Aadhaar)

Indonesia
* Número del carné de identidad (KTP)

Irlanda
* Número de servicio público personal (PPS)

Israel
* Identificador nacional
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Italia
* Id. del permiso de conducir

Japón
* Número de registro de residente
* Número de la tarjeta de residencia
* Id. del permiso de conducir
* Número del seguro social (SIN)
* Id. del pasaporte
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Malasia
* Número del carné de identidad

Países Bajos
* Número de servicio ciudadano (BSN)

Nueva Zelanda
* Número del Ministerio de Sanidad

Noruega
* Número del carné de identidad

Filipinas
* Número de identificación multiuso unificado

Polonia
* Número del carné de identidad
* Identificador nacional (PESEL)
* Id. del pasaporte

Portugal 
* Número de la tarjeta de ciudadano

Arabia Saudí
* Identificador nacional

Singapur
* Número de la tarjeta de identificación de registro nacional (NRIC)

Sudáfrica
* Número de identificación
* Número de registro de residente

Corea del Sur
* Número de registro de residente

España 
* Número de seguridad social

Suecia
* Identificador nacional
* Id. del pasaporte

Taiwán 
* Identificador nacional
* Número de certificado de residente (ARC/TARC)
* Id. del pasaporte

Tailandia
* Código de identificación de población

Reino Unido
* Id. del pasaporte
* Id. del permiso de conducir
* Número del seguro social (NINO)
* Número del NHS (National Health Service)

Estados Unidos
* Número de seguridad social
* Id. del permiso de conducir
* Id. del pasaporte
* Número de lista electoral
* Número de identificación fiscal individual (ITIN)
* Número de la agencia antidroga de Estados Unidos (DEA)
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)
