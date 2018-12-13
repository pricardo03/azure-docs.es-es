---
title: Filtrado geográfico en un dominio para Azure Front Door Service | Microsoft Docs
description: En este artículo, obtendrá información acerca de la directiva de filtrado geográfico para Azure Front Door Service
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/09/2018
ms.author: sharadag
ms.openlocfilehash: a2ba0fb34dd34129a134aa2639d06816f3523408
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865512"
---
# <a name="geo-filtering-geographic-based-access-control-to-azure-front-door-service-frontends"></a>Filtrado geográfico: control de acceso basado en geografía para los servidores front-end de Azure Front Door Service

De forma predeterminada, Azure Front Door Service responde a las solicitudes del usuario independientemente de la ubicación del usuario que realiza la solicitud. Sin embargo, puede que en algunos casos desee restringir el acceso a las aplicaciones web por país. La seguridad del nivel de aplicación de Azure Front Door permite definir una directiva que use reglas de protección personalizadas en una ruta de acceso concreta de su punto de conexión para permitir o bloquear el acceso desde determinados países. 

Habitualmente, las directivas de seguridad de la aplicaciones incluyen un conjunto de reglas personalizadas. Las reglas constan de condiciones de coincidencia, acciones y prioridades. En las condiciones de coincidencia, se definen una variable de coincidencia, un operador y un valor de coincidencia.  En el caso de la regla de filtrado geográfico, la variable de coincidencia es REMOTE_ADDR, el operador es GeoMatch y el valor es el código de país de dos letras que interese. Puede combinar una condición GeoMatch y una condición de coincidencia de la cadena REQUEST_URI para crear una regla de filtrado geográfico basada en ruta de acceso.

Las directivas de filtrado geográfico para Front Door se pueden configurar mediante [Azure PowerShell](front-door-tutorial-geo-filtering.md) o nuestra [plantilla de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering).

## <a name="country-code-reference"></a>Referencia de código de país

|Código de país | Nombre de país |
| ----- | ----- |
| AD | Andorra |
| AE | Emiratos Árabes Unidos|
| AF | Afganistán|
| AG | Antigua y Barbuda|
| AL | Albania|
| AM | Armenia|
| AO | Angola|
| AR | Argentina|
| AS | Samoa Americana|
| AT | Austria|
| AU | Australia|
| AZ | Azerbaiyán|
| BA | Bosnia y Herzegovina|
| BB | Barbados|
| BD | Bangladés|
| BE | Bélgica|
| BF | Burkina Faso|
| BG | Bulgaria|
| BH | Bahréin|
| BI | Burundi|
| BJ | Benín|
| BL | San Bartolomé|
| BN | Brunéi Darussalam|
| BO | Bolivia|
| BR | Brasil|
| BS | Bahamas|
| BT | Bután|
| BW | Botsuana|
| BY | Bielorrusia|
| BZ | Belice|
| CA | Canadá|
| CD | Congo, República Democrática del|
| CF | República Centroafricana|
| CH | Suiza|
| CI | Costa de Marfil|
| CL | Chile|
| CM | Camerún|
| CN | China|
| CO | Colombia|
| CR | Costa Rica|
| CU | Cuba|
| CV | Cabo Verde|
| CY | Chipre|
| CZ | República Checa|
| DE | Alemania|
| DK | Dinamarca|
| DO | República Dominicana|
| DZ | Argelia|
| EC | Ecuador|
| EE | Estonia|
| EG | Egipto|
| ES | España|
| ET | Etiopía|
| FI | Finlandia|
| FJ | Islas Fiji|
| FM | Micronesia, Estados Federados de|
| VF | Francia|
| GB | Reino Unido|
| GE | Georgia|
| GF | Guayana Francesa|
| GH | Ghana|
| GN | Guinea|
| GP | Guadalupe|
| GR | Grecia|
| GT | Guatemala|
| GY | Guayana|
| HK | Hong Kong|
| HN | Honduras|
| HR | Croacia|
| HT | Haití|
| HU | Hungría|
| ID | Indonesia|
| IE | Irlanda|
| IL | Israel|
| IN | India|
| IQ | Iraq|
| IR | Irán, República Islámica de|
| IS | Islandia|
| IT | Italia|
| JM | Jamaica|
| JO | Jordania|
| JP | Japón|
| KE | Kenia|
| KG | Kirguistán|
| KH | Camboya|
| KI | Kiribati|
| KN | San Cristóbal y Nieves|
| KP | Corea, República Popular Democrática de|
| KR | Corea, República de|
| KW | Kuwait|
| KY | Islas Caimán|
| KZ | Kazajistán|
| LA | República Democrática Popular de Laos|
| LB | Líbano|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesoto|
| LT | Lituania|
| LU | Luxemburgo|
| LV | Letonia|
| LY | Libia|
| MA | Marruecos|
| MD | Moldavia, República de|
| MG | Madagascar|
| MK | Macedonia|
| ML | Malí|
| MM | Myanmar|
| MN | Mongolia|
| MO | Macao|
| MQ | Martinica|
| MR | Mauritania|
| MT | Malta|
| MV | Maldivas|
| MW | Malaui|
| MX | México|
| MY | Malasia|
| MZ | Mozambique|
| N/D | Namibia|
| NE | Níger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Países Bajos|
| NO | Noruega|
| NP | Nepal|
| NR | Nauru|
| NZ | Nueva Zelanda|
| OM | Omán|
| PA | Panamá|
| PE | Perú|
| PH | Filipinas|
| PK | Pakistán|
| PL | Polonia|
| PR | Puerto Rico|
| PT | Portugal|
| PW | Palaos|
| PY | Paraguay|
| QA | Qatar|
| RE | Reunión|
| RO | Rumania|
| RS | Serbia|
| RU | Federación Rusa|
| RW | Ruanda|
| SA | Arabia Saudí|
| SD | Sudán|
| SE | Suecia|
| SG | Singapur|
| SI | Eslovenia|
| SK | Eslovaquia|
| SN | Senegal|
| SO | Somalia|
| SR | Surinam|
| SS | Sudán del Sur|
| SV | El Salvador|
| SY | República Árabe Siria|
| SZ | Suazilandia|
| TC | Islas Turcas y Caicos|
| TG | Togo|
| TH | Tailandia|
| TN | Túnez|
| TR | Turquía|
| TT | Trinidad y Tobago|
| TW | Taiwán|
| TZ | Tanzania, República Unida de|
| UA | Ucrania|
| UG | Uganda|
| US | Estados Unidos|
| UY | Uruguay|
| UZ | Uzbekistán|
| VC | San Vicente y las Granadinas|
| VE | Venezuela|
| VG | Islas Vírgenes Británicas|
| VI | Islas Vírgenes de EE. UU.|
| VN | Vietnam|
| ZA | Sudáfrica|
| ZM | Zambia|
| ZW | Zimbabue|

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de la [seguridad de la capa de aplicaciones con Front Door](front-door-application-security.md).
- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
