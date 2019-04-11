---
title: Base de datos SQL de Azure administra la zona de horaria de la instancia | Microsoft Docs"
description: Obtenga información sobre aspectos específicos de la zona horaria de la instancia administrada de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 04/09/2019
ms.openlocfilehash: c9be129624c87ca0bff1e85a5d1c71425ef06b89
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471913"
---
# <a name="time-zone-in-azure-sql-database-managed-instance"></a>Zona horaria en instancia administrada de Azure SQL Database

Si bien mediante la hora Universal coordinada (UTC) es una práctica recomendada para la capa de datos de soluciones en la nube, la instancia administrada de Azure SQL Database ofrece la posibilidad de zona horaria para satisfacer las necesidades de las aplicaciones existentes que almacenan valores de fecha y hora y fecha de llamada y funciones de tiempo con un contexto implícito de una zona horaria concreta.

Las funciones de Transact-SQL como [GETDATE()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) o código CLR observar la zona horaria establecidos en la instancia de nivel. Trabajos del Agente SQL también siguen la programación según la zona horaria de la instancia.

  >[!NOTE]
  > Instancia administrada es la opción de implementación única de Azure SQL Database que admite la configuración de zona horaria. Otras opciones de implementación siguen siempre UTC.
Use [AT TIME ZONE](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) en únicos agrupados SQL bases de datos y si tiene que interpretar la información de fecha y hora en la zona horaria no es UTC.

## <a name="supported-time-zones"></a>Zonas horarias admitidas

Se hereda un conjunto de zonas horarias compatibles del sistema operativo subyacente de la instancia administrada y se está actualizando con regularidad para obtener nuevas definiciones de zona horaria y reflejar los cambios en las existentes.

Una lista con los nombres de las zonas horarias compatibles se expone a través de la [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) vista del sistema.

## <a name="setting-time-zone"></a>Configuración de zona horaria

Durante la creación de instancia solo se puede establecer una zona horaria de la instancia administrada. La zona horaria predeterminada es la hora Universal coordinada (UTC).

  >[!NOTE]
  > No se puede cambiar la zona horaria de una instancia administrada existente.

### <a name="setting-the-time-zone-through-azure-portal"></a>Establecer la zona horaria a través del portal de Azure

Al especificar parámetros para la nueva instancia, seleccione una zona horaria en la lista de zonas horarias compatibles:
  
![Establecer zona horaria durante la creación de instancias](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure

Especificar propiedad timezoneId en su [plantilla de Resource Manager](https://aka.ms/sql-mi-create-arm-posh) para establecer la zona horaria durante la creación de instancias.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Lista de valores admitidos para la propiedad timezoneId puede encontrarse al final de este artículo.

Si no se especifica, se establecerá la zona horaria a UTC.

## <a name="checking-the-time-zone-of-instance"></a>Comprobación de la zona horaria de instancia

[CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timestamp-transact-sql) función devuelve un nombre para mostrar de la zona horaria de la instancia.

## <a name="cross-feature-considerations"></a>Consideraciones de entre características

### <a name="restore-and-import"></a>Importación y la restauración

Puede restaurar el archivo de copia de seguridad o importar datos a instancia administrada de una instancia o un servidor con la configuración de zona horaria diferente. Sin embargo, asegúrese de hacerlo con precaución y para analizar el comportamiento de la aplicación y los resultados de las consultas e informes, al igual que al transferir datos entre dos instancias de SQL Server con la configuración de zona horaria diferente.

### <a name="point-in-time-restore"></a>Restauración a un momento dado

Al realizar una restauración en el momento, el tiempo para restaurar a se interpreta como hora UTC para evitar cualquier ambigüedad debido al horario de verano y sus posibles cambios.

### <a name="auto-failover-groups"></a>Grupos de conmutación por error automática

Con la misma zona horaria a través de la instancia principal y secundaria en la conmutación por error no se aplica el grupo, pero se recomienda encarecidamente.
  >[!IMPORTANT]
  > Aunque hay escenarios válidos para tener la zona horaria diferente instancia secundaria con replicación geográfica se usa para el escalado de lectura solo, tenga en cuenta que en el caso de conmutación por error de manual o automática a la instancia secundaria mantendrá su zona horaria original.

## <a name="limitations"></a>Limitaciones

- No se puede cambiar la zona horaria de la instancia administrada existente.
- Los procesos externos que se inician desde los trabajos del Agente SQL no tienen en cuenta la zona horaria de la instancia.
- Nativa de la instancia administrada [New AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance) cmdlet de PowerShell no compatibilidad con la hora de pasar de la zona parámetro todavía. Contenedor de PowerShell de uso con [plantilla de Resource Manager](https://aka.ms/sql-mi-create-arm-posh) en su lugar.
- Comando de CLI [crear instancia administrada de sql az](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-create) aún no admite el parámetro de zona horaria.

## <a name="list-of-supported-time-zones"></a>Lista de zonas horarias compatibles

| **Id. de zona horaria** | **Nombre para mostrar de la zona horaria** |
| --- | --- |
| Hora estándar de línea de fecha | (UTC-12:00) Línea internacional de cambio de fecha oeste |
| UTC-11 | (UTC-11:00) Hora universal coordinada-11 |
| Hora estándar de las Islas Aleutianas | (UTC-10:00) Islas Aleutianas |
| Hora estándar de Hawai | (UTC-10:00) Hawái |
| Hora estándar de Marquesas | (UTC-09:30) Islas Marquesas |
| Hora estándar de Alaska | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Hora universal coordinada-09 |
| Hora estándar del Pacífico (México) | (UTC-08:00) Baja California |
| UTC-08 | (UTC-08:00) Hora universal coordinada-08 |
| Hora estándar del Pacífico | (UTC-08:00) Hora del Pacífico (EE.UU. y Canadá) |
| Hora estándar de las Montañas, EE. UU. | (UTC-07:00) Arizona |
| Hora estándar de las Montañas (México) | (UTC-07:00) Chihuahua, La Paz, Mazatlán |
| Hora estándar de las Montañas | (UTC-07:00) Hora de las Montañas Rocosas (EE. UU. y Canadá) |
| Hora estándar de América Central | (UTC-06:00) América Central |
| Hora estándar del Centro | (UTC-06:00) Hora central (EE.UU. y Canadá) |
| Hora estándar de Isla de Pascua | (UTC-06:00) Isla de Pascua |
| Hora estándar central (México) | (UTC-06:00) Guadalajara, Ciudad de México, Monterrey |
| Hora estándar de Centro de Canadá | (UTC-06:00) Saskatchewan |
| Hora estándar de Sudamérica Pacífico | (UTC-05:00) Bogotá, Lima, Quito, Río Branco |
| Hora estándar del Este (México) | (UTC-05:00) Chetumal |
| Hora estándar del Este | (UTC-05:00) Hora del este (EE.UU. y Canadá) |
| Hora estándar de Haití | (UTC-05:00) Haití |
| Hora estándar de Cuba | (UTC-05:00) La Habana |
| Hora estándar del Este de EE. UU. | (UTC-05:00) Indiana (este) |
| Islas Turcas y Caicos Standard Time | (UTC-05:00) Islas Turcas y Caicos |
| Hora estándar de Paraguay | (UTC-04:00) Asunción |
| Hora estándar del Atlántico | (UTC-04:00) Hora del Atlántico (Canadá) |
| Hora estándar de Venezuela | (UTC-04:00) Caracas |
| Hora estándar de Brasil central | (UTC-04:00) Cuiabá |
| Hora estándar de Sudamérica Oeste | (UTC-04:00) Georgetown, La Paz, Manaos, San Juan |
| Hora estándar de Sudamérica Pacífico | (UTC-04:00) Santiago |
| Hora estándar de Terranova | (UTC-03:30) Terranova |
| Hora estándar de Tocantins | (UTC-03:00) Araguaína |
| E. Hora estándar Sudamérica este | (UTC-03:00) Brasilia |
| Hora estándar de Sudamérica Este | (UTC-03:00) Cayena, Fortaleza |
| Hora estándar de Argentina | (UTC-03:00) Ciudad Autónoma de Buenos Aires |
| Hora estándar de Groenlandia | (UTC-03:00) Groenlandia |
| Hora estándar de Montevideo | (UTC-03:00) Montevideo |
| Hora estándar de Magallanes | (UTC-03:00) Punta Arenas |
| Hora estándar de Saint Pierre | (UTC-03:00) San Pedro y Miquelón |
| Hora estándar de Bahía | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Hora universal coordinada-02 |
| Hora est. Atlántico Central | (UTC-02:00) Atlántico central - Antiguo |
| Hora estándar de las Azores | (UTC-01:00) Azores |
| Hora estándar de cabo Verde | (UTC-01:00) Isla de Cabo Verde |
| UTC | (UTC) Hora universal coordinada |
| Hora estándar GMT | (UTC+00:00) Dublín, Edinburgo, Lisboa, Londres |
| Hora estándar de Greenwich | (UTC+00:00) Monrovia, Reykjavik |
| W. Hora estándar Europa | (UTC+01:00) Ámsterdam, Berlín, Berna, Roma, Estocolmo, Viena |
| Hora estándar Europa Central | (UTC+01:00) Belgrado, Bratislava, Budapest, Liubliana, Praga |
| Hora estándar romance | (UTC+01:00) Bruselas, Copenhague, Madrid, París |
| Hora estándar de Marruecos | (UTC+01:00) Casablanca |
| Hora estándar de Santo Tomé | (UTC+01:00) Santo Tomé |
| Hora estándar centroeuropea | (UTC+01:00) Sarajevo, Skopie, Varsovia, Zagreb |
| W. Hora est. África Central | (UTC+01:00) África Central Occidental |
| Hora estándar de Jordania | (UTC+02:00) Ammán |
| Hora estándar GTB | (UTC+02:00) Atenas, Bucarest |
| Hora estándar de Oriente Medio | (UTC+02:00) Beirut |
| Hora estándar de Egipto | (UTC+02:00) El Cairo |
| E. Hora estándar Europa | (UTC+02:00) Chisinau |
| Hora estándar de Siria | (UTC+02:00) Damasco |
| Hora estándar de Cisjordania | (UTC+02:00) Gaza, Hebrón |
| Hora estándar de Sudáfrica | (UTC+02:00) Harare, Pretoria |
| Hora estándar FLE | (UTC+02:00) Helsinki, Kiev, Riga, Sofia, Tallin, Vilna |
| Hora estándar de Israel | (UTC+02:00) Jerusalén |
| Hora estándar de Kaliningrado | (UTC+02:00) Kaliningrado |
| Hora estándar de Sudán | (UTC + 02:00) Jartum |
| Hora estándar de Libia | (UTC+02:00) Trípoli |
| Hora estándar de Namibia | (UTC + 02:00) Windhoek |
| Hora estándar arábica | (UTC+03:00) Bagdad |
| Hora estándar de Turquía | (UTC+03:00) Estambul |
| Hora estándar árabe | (UTC+03:00) Kuwait, Riad |
| Hora estándar de Belarús | (UTC+03:00) Minsk |
| Hora estándar de Rusia | (UTC+03:00) Moscú, San Petersburgo |
| E. Hora est. África | (UTC+03:00) Nairobi |
| Hora estándar de Irán | (UTC+03:30) Teherán |
| Hora estándar árabe | (UTC+04:00) Abu Dhabi, Mascate |
| Hora estándar de Astrakhan | (UTC+04:00) Astrakhan, Ulyanovsk |
| Hora estándar de Azerbaiyán | (UTC+04:00) Bakú |
| Zona horaria de Rusia 3 | (UTC+04:00) Izhevsk, Samara |
| Hora estándar de Mauricio | (UTC+04:00) Port Louis |
| Hora estándar de Sarátov | (UTC+04:00) Sarátov |
| Hora estándar de Georgia | (UTC+04:00) Tiflis |
| Hora estándar de Volgogrado | (UTC+04:00) Volgogrado |
| Hora estándar del Cáucaso | (UTC+04:00) Ereván |
| Hora estándar de Afganistán | (UTC+04:30) Kabul |
| Hora estándar Asia Occidental | (UTC+05:00) Asjabad, Tashkent |
| Hora estándar de Ekaterimburgo | (UTC+05:00) Ekaterinburgo |
| Hora estándar de Pakistán | (UTC+05:00) Islamabad, Karachi |
| Hora estándar de India | (UTC+05:30) Chennai, Kolkata, Mumbai, Nueva Delhi |
| Hora estándar de Sri Lanka | (UTC+05:30) Sri Jayawardenepura |
| Hora estándar de Nepal | (UTC+05:45) Katmandú |
| Hora estándar de Asia Central | (UTC+06:00) Astaná |
| Hora estándar de Bangladés | (UTC+06:00) Dacca |
| Hora estándar de Omsk | (UTC+06:00) Omsk |
| Hora estándar de Myanmar | (UTC+06:30) Yangón (Rangún) |
| Hora estándar de Asia Sureste | (UTC+07:00) Bangkok, Hanói, Yakarta |
| Hora estándar de Altai | (UTC+07:00) Barnaul, Gorno-Altaysk |
| W. Hora estándar de Mongolia | (UTC+07:00) Hovd |
| Hora estándar Asia Norte | (UTC+07:00) Krasnoyarsk |
| N. Hora estándar de Asia Central | (UTC+07:00) Novosibirsk |
| Hora estándar de Tomsk | (UTC+07:00) Tomsk |
| Hora estándar de China | (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Hora estándar este, Asia Norte | (UTC+08:00) Irkutsk |
| Hora estándar de Singapur | (UTC+08:00) Kuala Lumpur, Singapur |
| W. Hora estándar de Australia | (UTC+08:00) Perth |
| Hora estándar de Taipéi | (UTC+08:00) Taipéi |
| Hora estándar de Ulán Bator | (UTC+08:00) Ulán Bator |
| Hora estándar de Windows del centro de Australia | (UTC+08:45) Eucla |
| Hora estándar de Transbaikal | (UTC+09:00) Chita |
| Hora estándar de Tokio | (UTC+09:00) Osaka, Sapporo, Tokio |
| Hora estándar de Corea del Norte | (UTC + 09:00) Pyongyang |
| Hora estándar de Corea | (UTC+09:00) Seúl |
| Hora estándar de Yakutsk | (UTC+09:00) Yakutsk |
| CEN. Hora estándar de Australia | (UTC+09:30) Adelaida |
| Hora estándar central australiana | (UTC+09:30) Darwin |
| E. Hora estándar de Australia | (UTC+10:00) Brisbane |
| Hora estándar oriental australiana | (UTC+10:00) Canberra, Melbourne, Sídney |
| Hora est. Pacífico Occidental | (UTC+10:00) Guam, Port Moresby |
| Hora estándar de Tasmania | (UTC+10:00) Hobart |
| Hora estándar de Vladivostok | (UTC+10:00) Vladivostok |
| Hora estándar de Lord Howe | (UTC+10:30) Isla Lord Howe |
| Hora estándar de Bougainville | (UTC+11:00) Isla Bougainville |
| Zona horaria de Rusia 10 | (UTC+11:00) Chokurdakh |
| Hora estándar de Magadán | (UTC+11:00) Magadán |
| Hora estándar de Norfolk | (UTC+11:00) Isla Norfolk |
| Hora estándar de Sakhalin | (UTC+11:00) Sajalín |
| Hora estándar Pacífico Central | (UTC+11:00) Islas Solomon, Nueva Caledonia |
| Zona horaria de Rusia 11 | (UTC+12:00) Anádyr, Petropávlovsk-Kamchatski |
| Hora estándar de Nueva Zelanda | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Hora universal coordinada+12 |
| Hora estándar de Fiji | (UTC+12:00) Fiyi |
| Hora estándar de Kamchatka | (UTC+12:00) Petropavlovsk-Kamchatsky - Antiguo |
| Hora estándar de las Islas Chatham | (UTC+12:45) Islas Chatham |
| UTC+13 | (UTC+13:00) Hora universal coordinada+13 |
| Hora estándar de Tonga | (UTC+13:00) Nukualofa |
| Hora estándar de Samoa | (UTC+13:00) Samoa |
| Hora estándar de Islas de la Línea | (UTC+14:00) Isla de Kiritimati |

## <a name="see-also"></a>Vea también

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [EN la zona HORARIA (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)