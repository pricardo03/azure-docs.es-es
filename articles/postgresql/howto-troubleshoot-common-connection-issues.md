---
title: Solución de problemas de conexión a Azure Database for PostgreSQL | Microsoft Docs
description: Obtenga información sobre la solución de problemas de conexión a Azure Database for PostgreSQL.
keywords: postgresql connection,connection string,connectivity issues,transient error,connection error
services: postgresql
author: janeng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/09/2018
ms.openlocfilehash: fea4c376f73afe2ef36948c59e95a2b679dbd7e8
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285487"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql"></a>Solución de problemas de conexión a Azure Database for PostgreSQL

Los problemas de conexión pueden deberse a una variedad de causas, como:

* Configuración de firewall
* Tiempo de espera de conexión agotado
* Información de inicio de sesión incorrecta
* Límite máximo alcanzado en algunos recursos de Azure Database for PostgreSQL
* Problemas con la infraestructura del servicio
* Mantenimiento en curso en el servicio
* Cambio de la asignación de proceso del servidor mediante el escalado del número de núcleos virtuales o el traslado a otro nivel de servicio

Por lo general, los problemas de conexión de Azure Database for PostgreSQL se pueden clasificar en los siguientes:

* Errores transitorios (corta duración o intermitentes)
* Errores persistentes o no transitorios (errores que se repiten con frecuencia)

## <a name="troubleshoot-transient-errors"></a>Solución de problemas de errores transitorios

Los errores transitorios se producen cuando se realiza el mantenimiento, el sistema encuentra un error con el hardware o software, o cambia el nivel de servicio o núcleos virtuales del servidor. El servicio Azure Database for PostgreSQL tiene alta disponibilidad integrada y está diseñado para mitigar estos tipos de problemas automáticamente. Sin embargo, la aplicación pierde su conexión con el servidor durante un breve período de tiempo que, normalmente, no supera los 60 segundos. Ocasionalmente, estos eventos pueden tardar más tiempo en mitigarse; por ejemplo, cuando una transacción grande requiere una recuperación de larga duración.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Pasos para resolver los problemas de conectividad transitorios

1. Compruebe el [panel de servicios de Microsoft Azure](https://azure.microsoft.com/status) para comprobar si se produjeron interrupciones durante el tiempo en el que la aplicación informó de los errores.
2. Para las aplicaciones que se conectan a un servicio en la nube, como Azure Database for PostgreSQL, se deben prever errores transitorios e implementar la lógica de reintento para controlar estos errores en lugar de mostrarlos como errores de la aplicación a los usuarios. Revise en [Control de errores de conectividad transitorios para Azure Database for PostgreSQL](concepts-connectivity.md) los procedimientos recomendados y las directrices de diseño para controlar los errores transitorios.
3. Conforme un servidor se acerca a sus límites de recursos, los errores pueden parecer un problema de conectividad transitorio. Vea [Limitaciones en Azure Database for PostgreSQL](concepts-limits.md).
4. Si los problemas de conectividad continúan, si el tiempo de detección del error por parte de la aplicación supera los 60 segundos o si el error se repite varias veces en un día determinado, realice una solicitud de soporte técnico a Azure; para ello, seleccione **Obtener soporte** en el sitio [Soporte técnico de Azure](https://azure.microsoft.com/support/options) .

## <a name="troubleshoot-persistent-errors"></a>Solución de problemas de los errores persistentes

Si la aplicación no se puede conectar a Azure Database for PostgreSQL de forma persistente, normalmente indica un problema con uno de los siguientes elementos:

* Configuración del firewall: el servidor o cliente de Azure Database for PostgreSQL bloquea las conexiones.
* Reconfiguración de la red en el lado cliente: se ha añadido un nuevo servidor de proxy o dirección IP.
* Error de usuario: por ejemplo, escribió incorrectamente los parámetros de conexión, como el nombre del servidor en la cadena de conexión o falta un sufijo *@servername* en el nombre de usuario.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Pasos para resolver los problemas de conectividad persistentes

1. Configure las [reglas de firewall](howto-manage-firewall-using-portal.md) para permitir la dirección IP del cliente. Con fines temporales de prueba solo, configure una regla de firewall empleando 0.0.0.0 como dirección IP inicial y 255.255.255.255 como dirección IP final. Se abrirá el servidor a todas las direcciones IP. Si se resuelve el problema de conectividad, quite esta regla y cree una regla de firewall para una dirección IP o intervalo de direcciones apropiadamente limitados.
2. En todos los firewalls entre el cliente e Internet, asegúrese de que el puerto 3306 está abierto para las conexiones salientes.
3. Compruebe la cadena de conexión y otras opciones de conexión.
4. Compruebe el estado del servicio en el panel. Si cree que hay una interrupción regional, consulte [Introducción a la continuidad empresarial con Azure Database for PostgreSQL](concepts-business-continuity.md) para obtener los pasos de recuperación en una región nueva.

## <a name="next-steps"></a>Pasos siguientes

* [Control de errores de conectividad transitorios para Azure Database for PostgreSQL](concepts-connectivity.md)
