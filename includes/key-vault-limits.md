---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 9a39abf77a7396302f93e5a423271402b7c3edb3
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54084011"
---
Transacciones clave (N.º máximo de transacciones permitidas en 10 segundos, por almacén y región<sup>1</sup>):

|Tipo de clave|Clave HSM<br>Clave CREAR|Clave HSM<br>Las restantes transacciones|Clave de software<br>Clave CREAR|Clave de software<br>Las restantes transacciones|
|:---|---:|---:|---:|---:|
|2048 bits de RSA|5|1000|10|2000|
|3072 bits de RSA|5|250|10|500|
|4096 bits de RSA|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

> [!NOTE]
> Los umbrales anteriores se ponderan y el cumplimiento se basa en su suma. Puede hacer 125 operaciones RSA-HSM-4k y 0 RSA-HSM-2k, o 124 RSA-HSM-4k y 16 RSA-HSM-2k. Posteriormente, en el mismo intervalo de 10 segundos, cualquier otra operación, producirá una excepción de cliente de Azure Key Vault.

> [!NOTE]
> Si observa la tabla siguiente, verá que para las claves respaldadas por software se permiten 2000 transacciones cada 10 segundos, y para las claves respaldadas por HSM, 1000 transacciones cada 10 segundos. La proporción de transacciones respaldadas por software para 3072 claves a 2048 claves es de 500/2000 o 0,4. Esto significa que si un cliente realiza 500 transacciones de 3072 claves en 10 segundos, alcanza su límite máximo y no puede realizar otras operaciones de clave. 
   
|Tipo de clave  | Clave de software |Clave HSM  |
|---------|---------|---------|
|2048 bits de RSA     |    2000     |   1000    |
|3072 bits de RSA     |     500    |    250     |
|4096 bits de RSA     |    125     |    250     |
|ECC P-256     |    2000     |  1000     |
|ECC P-384     |    2000     |  1000     |
|ECC P-521     |    2000     |  1000     |
|ECC SECP256K1     |    2000     |  1000     |


Secretos, claves de cuentas de almacenamiento administradas y transacciones de almacén:
| Tipo de transacciones | N.º máximo de transacciones permitidas en 10 segundos, por almacén y región<sup>1</sup> |
| --- | --- |
| Todas las transacciones |2000 |
|

Vea la [Guía de las limitaciones de Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md) para obtener información sobre cómo controlar la limitación cuando se superan estos límites.

<sup>1</sup> El límite global para la suscripción para todos los tipos de transacciones es 5 veces el límite del almacén de claves. Por ejemplo, en "HSM - otras transacciones" el límite es de 5000 transacciones en 10 segundos por suscripción.
