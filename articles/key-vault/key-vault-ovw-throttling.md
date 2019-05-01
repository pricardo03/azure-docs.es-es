---
title: Guía de las limitaciones de Azure Key Vault
description: La limitación de Key Vault limita el número de llamadas simultáneas para evitar el uso excesivo de recursos.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: mbaldwin
ms.openlocfilehash: 06b96ac6ea0202b74638b9331d754c0e0cc28ad0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695244"
---
# <a name="azure-key-vault-throttling-guidance"></a>Guía de las limitaciones de Azure Key Vault

La limitación es un proceso que se inicia y que limita el número de llamadas simultáneas al servicio de Azure para evitar el uso excesivo de recursos. Azure Key Vault se ha diseñado para controlar un volumen muy alto de solicitudes. Si tiene lugar un número excesivo de solicitudes, la limitación de las solicitudes del cliente le ayuda a mantener un rendimiento óptimo y la confiabilidad del servicio Azure Key Vault.

Las limitaciones varían según el escenario. Por ejemplo, si está realizando un gran volumen de escrituras, la posibilidad de limitación es mayor que si solo realiza lecturas.

## <a name="how-does-key-vault-handle-its-limits"></a>¿Cómo maneja Key Vaults sus límites?

Los límites de servicio en Key Vault sirven para evitar el uso incorrecto de los recursos y garantizar la calidad de servicio para todos los clientes de Key Vault. Cuando se supera un umbral de servicio, Key Vault limita las solicitudes sucesivas de ese cliente durante un período de tiempo. Cuando esto sucede, Key Vault devuelve el código de estado HTTP 429 (demasiadas solicitudes) y se producirá un error en las solicitudes. Además, las solicitudes con error que devuelven un código 429 cuentan para la limitación cuyo seguimiento realiza Key Vault. 

Si tiene un caso empresarial válido para limitaciones superiores, póngase en contacto con nosotros.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Limitación de la aplicación en respuesta a los límites de servicio

Los siguientes son **procedimientos recomendados** deben implementar cuando está limitado a su servicio:
- Reduzca el número de operaciones por solicitud.
- Reduzca la frecuencia de las solicitudes.
- Evite los reintentos inmediatos. 
    - Todas las solicitudes se acumulan en los límites de utilización.

Al implementar el control de errores de la aplicación, utilice el código de error HTTP 429 para detectar la necesidad de limitación del cliente. Si la solicitud vuelve a enviar un código de error HTTP 429, todavía se está produciendo un límite de servicio Azure. Continúe usando el método de limitación del cliente recomendado y vuelva a intentar la solicitud hasta que lo consiga.

A continuación, se muestra el código que implementa el retroceso exponencial. 
```
    public sealed class RetryWithExponentialBackoff
    {
        private readonly int maxRetries, delayMilliseconds, maxDelayMilliseconds;

        public RetryWithExponentialBackoff(int maxRetries = 50,
            int delayMilliseconds = 200,
            int maxDelayMilliseconds = 2000)
        {
            this.maxRetries = maxRetries;
            this.delayMilliseconds = delayMilliseconds;
            this.maxDelayMilliseconds = maxDelayMilliseconds;
        }

        public async Task RunAsync(Func<Task> func)
        {
            ExponentialBackoff backoff = new ExponentialBackoff(this.maxRetries,
                this.delayMilliseconds,
                this.maxDelayMilliseconds);
            retry:
            try
            {
                await func();
            }
            catch (Exception ex) when (ex is TimeoutException ||
                ex is System.Net.Http.HttpRequestException)
            {
                Debug.WriteLine("Exception raised is: " +
                    ex.GetType().ToString() +
                    " –Message: " + ex.Message +
                    " -- Inner Message: " +
                    ex.InnerException.Message);
                await backoff.Delay();
                goto retry;
            }
        }
    }

    public struct ExponentialBackoff
    {
        private readonly int m_maxRetries, m_delayMilliseconds, m_maxDelayMilliseconds;
        private int m_retries, m_pow;

        public ExponentialBackoff(int maxRetries, int delayMilliseconds,
            int maxDelayMilliseconds)
        {
            m_maxRetries = maxRetries;
            m_delayMilliseconds = delayMilliseconds;
            m_maxDelayMilliseconds = maxDelayMilliseconds;
            m_retries = 0;
            m_pow = 1;
        }

        public Task Delay()
        {
            if (m_retries == m_maxRetries)
            {
                throw new TimeoutException("Max retry attempts exceeded.");
            }
            ++m_retries;
            if (m_retries < 31)
            {
                m_pow = m_pow << 1; // m_pow = Pow(2, m_retries - 1)
            }
            int delay = Math.Min(m_delayMilliseconds * (m_pow - 1) / 2,
                m_maxDelayMilliseconds);
            return Task.Delay(delay);
        }
    }
```


Utilice este código en un cliente C\# aplicación es sencilla. En el ejemplo siguiente se muestra cómo hacerlo, mediante la clase HttpClient.

```csharp
public async Task<Cart> GetCartItems(int page)
{
    _apiClient = new HttpClient();
    //
    // Using HttpClient with Retry and Exponential Backoff
    //
    var retry = new RetryWithExponentialBackoff();
    await retry.RunAsync(async () =>
    {
        // work with HttpClient call
        dataString = await _apiClient.GetStringAsync(catalogUrl);
    });
    return JsonConvert.DeserializeObject<Cart>(dataString);
}
```

Recuerde que este código es adecuado solo como una prueba de concepto. 

### <a name="recommended-client-side-throttling-method"></a>Método de limitación del cliente recomendado

En el código de error HTTP 429, comience la limitación del cliente mediante un enfoque de retroceso exponencial:

1. Espere 1 segundo, reintente la solicitud
2. Si todavía está limitado, espere 2 segundos, reintente la solicitud
3. Si todavía está limitado, espere 4 segundos, reintente la solicitud
4. Si todavía está limitado, espere 8 segundos, reintente la solicitud
5. Si todavía está limitado, espere 16 segundos, reintente la solicitud

En este momento, se deben no está recibiendo los códigos de respuesta HTTP 429.

## <a name="see-also"></a>Vea también

Para obtener una orientación más profunda de la limitación en Microsoft Cloud, consulte [Patrón de limitación](https://docs.microsoft.com/azure/architecture/patterns/throttling).

