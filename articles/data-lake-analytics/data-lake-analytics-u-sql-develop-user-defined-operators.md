---
title: Desarrollo de operadores U-SQL definidos por el usuario (UDO) en Azure Data Lake Analytics
description: Obtenga información sobre cómo desarrollar operadores definidos por el usuario para usarse y volverse a usar en trabajos de Azure Data Lake Analytics.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 9751801449eebc5d07ca4bbb9b408d9679fef24e
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43040997"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>Desarrollo de operadores U-SQL definidos por el usuario (UDO)
En este artículo se describe cómo desarrollar operadores definidos por el usuario para procesar datos en un trabajo de U-SQL.

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Definición y uso de un operador definido por el usuario en U-SQL
**Para crear y enviar un trabajo de U-SQL**

1. En el menú de Visual Studio, seleccione **Archivo > Nuevo > Proyecto > U-SQL Project** (Proyecto de U-SQL).
2. Haga clic en **OK**. Visual Studio crea una solución con un archivo Script.usql.
3. En el **Explorador de soluciones**, expanda Script.usql y haga doble clic en **Script.usql.cs**.
4. Pegue el código siguiente en el archivo:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Abra **Script.usql** y pegue el siguiente script de U-SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Especifique la cuenta de Análisis de Data Lake, la base de datos y el esquema.
8. En el **Explorador de soluciones**, haga clic con el botón derecho en **Script.usql** y después haga clic en **Build Script** (Compilar script).
9. En el **Explorador de soluciones**, haga clic con el botón derecho en **Script.usql** y después haga clic en **Submit Script** (Enviar script).
10. Si no se ha conectado a su suscripción de Azure, se le pedirá que especifique sus credenciales de la cuenta de Azure.
11. Haga clic en **Enviar**. Los resultados del envío y el vínculo del trabajo están disponibles en la ventana de resultados cuando se completa el envío.
12. Haga clic en el botón **Actualizar** para ver el estado del trabajo más reciente y actualizar la pantalla.

**Para ver la salida**:

1. En el **Explorador de servidores**, expanda **Azure**, **Data Lake Analytics**, su cuenta de Data Lake Analytics y **Cuentas de almacenamiento**, haga clic con el botón derecho en el almacén predeterminado y, finalmente, haga clic en **Explorador**.
2. Expanda Ejemplos, expanda Salidas y, finalmente, haga doble clic en **Drivers.csv**.

## <a name="see-also"></a>Otras referencias
* [Extensión de las expresiones U-SQL con código de usuario](https://msdn.microsoft.com/library/azure/mt621316.aspx)
* [Uso de Data Lake Tools for Visual Studio para desarrollar aplicaciones de U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
