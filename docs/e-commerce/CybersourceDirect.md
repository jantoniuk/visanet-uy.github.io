---
layout: page
title: CybersourceDirect
parent: e-commerce
nav_order: 1
---

# Visanet Uruguay: e-commerce vía Cybersource
Internet tiene un peso cada vez más relevante en los hábitos de consumo.  La posibilidad de pagar on-line sin fricciones y de forma segura es fundamental para la expansión de las operativas actuales y el desarrollo de nuevas oportunidades de negocio en Visanet Uruguay.

Recientemente Visanet Uruguay implementó [VisaNetPagos](www.visanetpagos.com.uy) como un servicio para que tarjetahabientes VISA puedan realizar pagos a las Empresas e Instituciones (públicas y privadas) adheridas al mismo, accesible desde cualquier dispositivo (PC, notebook, tablet, smartphone, etc.) con conexión a Internet desde cualquier lugar del mundo, las 24 horas, los 365 días.

VisaNetPagos está basado en la plataforma de [Cybersource](https://developer.visa.com/products/cybersource). Mediante esta solución el flujo de autorización es procesado directamente por VISA internacional. Con esta arquitectura es posible aplicar varias funcionalidades que permiten ofrecer soluciones de pago vía Internet avanzadas para nuevos modelos de negocio y se utiliza un [potente motor de gestión de fraude transaccional](https://www.cybersource.com/products/fraud_management/decision_manager/) manteniendo la posibilidad de autentificación mediante [Verified By Visa](https://www.visa.com.uy/pague-con-visa/tecnologias-presentadas/vf.html).

El objetivo de este documento es presentar los aspectos a considerar para que comercios o integradores (de aquí en más Integrador) con  capacidad tecnológica que cumplan los requisitos operativos y de seguridad  de Visanet Uruguay y Visa Internacional así como los estándares que apliquen ([PCI](https://www.pcisecuritystandards.org/), etc) puedan realizar una integración con Cybersource para comercios adquiridos por Visanet Uruguay.

# Funcionalidad
## Conectividad directa vía Cybersource
En esta nueva modalidad para comercio electrónico Visanet Uruguay está aplicando el modelo de conectividad directa. De esta forma el comercio (o integrador) accede a la infraestructura de clase mundial de VISA Internacional para el proceso de autorización, mientras que la participación de Visanet Uruguay en su rol adquirente está concentrada en la gestión del negocio y proceso de liquidación (pasos 7 y 8).

![Visa Conectividad Directa](https://developer.visa.com/images2/products/cybersource_payments/cybs_getting_started.jpeg)


1. El cliente realiza un pedido y proporciona el número de la tarjeta de crédito, la fecha de caducidad de la tarjeta e información adicional sobre la tarjeta.

2. El comercio transfiere con seguridad la información del pedido a CyberSource (por ejemplo, utilizando la API de pago de CyberSource).

3. CyberSource formatea los detalles de la transacción de manera adecuada y enruta de forma segura la solicitud de autorización de transacción al procesador.

4. La transacción se envía al banco Emisor para solicitar la autorización de la transacción.

5. La transacción es autorizada o rechazada por el banco Emisor.

6. CyberSource devuelve el mensaje al comerciante.

7. El comercio captura la autorización. El banco Emisor aprueba la transferencia de dinero al adquirente.

8. El adquirente acredita en la cuenta del comercio.

## Productos de Cybersource considerados
### Decision Manager
Cybersource dispone del modulo [Decision Manager](https://www.cybersource.com/products/fraud_management/decision_manager/) para administrar en su conjunto, el riesgo involucrado en las transacciones realizadas en ambiente electrónico y que pasan por su radar. Este módulo constituye un potente motor antifraude, que maneja cientos de variables en tiempo real, no sólo sobre el comportamiento histórico de la tarjeta que está realizando la transacción sino sobre el dispositivo (laptop, pc, Smartphone) desde el cual se está realizando la misma, lo que se conoce como “Device Fingerprint”.

Asimismo, se pueden aplicar diversas reglas de negocio para cada caso en particular y así construir modelos de prevención de fraude que apliquen a rubros o comercios particulares.

### Secure Acceptance
CyberSource [Secure Acceptance Web / Mobile](https://www.cybersource.com/products/payment_security/secure_acceptance_web_mobile/) permite aceptar de forma segura pagos realizados en la Web o en navegadores móviles en todo el mundo. Debido a que los datos sensibles se transmiten desde el dispositivo del cliente directamente a CyberSource, el alcance de PCI DSS puede reducirse significativamente.

### Payment Gateway: Simple Order API
CyberSource ofrece varias alternativas de integración a su [API](https://www.cybersource.com/products/payment_processing/gateway_processing_connections/) para procesamiento de pagos.

### Tokenización
Cybersource también ofrece un servicio de [Tokenización](https://www.cybersource.com/products/payment_security/payment_tokenization/) que permite la implementación de pagos recurrentes y soluciones “one click” de forma segura, minimizando requisitos de compliance dado que los datos de tarjeta son almacenados únicamente en infraestructura de Cybersource.

## Requisitos locales de Visanet Uruguay
Así como cada mercado tiene condiciones comerciales y regulatorias específicas, Visanet Uruguay ofrece distintos [planes de cuotas](https://www.visanet.com.uy/comercios/planes/) y se deben cumplir requisitos normativos de devolución de impuestos de acuerdo al [Programa de Inclusión Financiera](http://inclusionfinanciera.uy).

Los comercios/integradores **deben considerar** las siguientes herramientas y condiciones operativas en la integración con Cybersource para comercios adquiridos por Visanet Uruguay.

### Validación de cuenta
En los casos que se almacene la tarjeta para pagos recurrentes o modalidad “one-click” ya sea mediante el servicio de tokenización de Cybersource o uno propio del comercio/integrador, **se debe realizar la validación de cuenta** incluyendo el CVV2.

Esta validación puede estar asociada a una compra real o se puede solicitar una transacción de verificación de cuenta que corresponde a una autorización por monto cero ($0).

### Autenticación Verified By Visa
En los casos de uso que sea técnicamente posible se deben priorizar las transacciones autenticadas VBV.

### Anti-fraude
El comercio/integrador **debe coordinar** con Visanet la implementación de políticas de seguridad.
Cuanto más información adicional a la extrictamente monetaria se envíe con cada transacción (en merchantDefinedData **MDD**) hay más posibilidades para implementar mejores reglas.
En todos los casos donde sea técnicamente posible se debe incluir el [Device Fingerprint](https://support.cybersource.com/cybskb/index?page=content&id=C740&actp=LIST)

### Política de re-intentos
En casos en que la solicitud de autorización fuera denegada hay comercios/integradores que tienen la estrategia de volver a intentar; en estos casos **se deben** limitar a 3 re-intentos con una frecuencia no menor a 1 hora.

### API LIF
Visanet Uruguay implemento una API ([API LIF](https://api-lif.vnet.uy/Help) ) para ser consumida por el comercio/integrador cuando corresponda con los siguientes servicios:

- Consulta de características de tarjeta (tipo, si se permiten cuotas, etc)
- Calculo de devolución de Impuestos

![flujo API LIF ](https://visanet-uy.github.io/images/API_LIF_flow.svg)

### Datos adicionales a la transacción
Referencia: <http://apps.cybersource.com/library/documentation/dev_guides/CC_Svcs_SO_API/Credit_Cards_SO_API.pdf>

La información local ya sea de cuotas o de devolución de impuestos se envía en la transacción mediante el campo *issuer_additionalData*. La responsabilidad de informar aplicación de Devolución de Impuestos es del comercio/integrador considerando la normativa vigente; las condiciones a aplicar dependen del comercio, comprador, monto de la operacion, tipo de tarjeta (crédito, débito, prepago), etc.

Formato del campo *issuer_additionalData*:

|Posición|Largo|Descripción|
|-|-|-|
|1-2|2|Plan|
|3|1|Meses Diferidos|
|4-5|2|Cantidad de cuotas|
|6|1|Indicador envío de pista = 0|
|7-15|9|Cédula de Identidad, padding 0 a la izquierda|
|16|1|INDI = Indicador de Devolución de Impuestos<br>0 - No aplica devolución<br>1 - Ley 17.934 (Restaurantes)<br>6 - Ley 19.210 (Inclusión Financiera)|
|17-28|12|Importe devolución de IVA (10 enteros 2 decimales)|
|29-35|7|Número de factura (7 dígitos)|

**NOTA:** Para tarjetas de débito las posiciónes del 1-6 tienen el valor fijo "003000"

A su vez, **se debe enviar** junto a cada transacción como mínimo la siguiente información como merchantDefinedData **MDD*

|MDD|Presencia|Descripción|
|-|-|-|
|MDD1|Opcional|Identificador de Vendedor|
|MDD2|Opcional|Tipo de identificación:<br>"01"= C.I.<br>"02"=RUT|
|MDD3|Mandatorio|Monto Total(10 enteros 2 decimales)|
|MDD4|Mandatorio|Monto Gravado(10 enteros 2 decimales)|
|MDD5|Mandatorio|texto enviado en *issuer_additionalData*|
|MDD6|Mandatorio|Número de factura completo|
|MDD7|Mandatorio|Identificador de comercio/integrador<br>segun informado por Visanet|
|MDD8|Mandatorio|BIN|
|MDD9|Opcional|Codigo de Promoción<br>segun informado por Visanet|

#### Ejemplo
Para una transacción realizada con tarjeta de Débito que aplique ley 19.210
Importe total: $ 1220
Importe Gravado: $ 1000 

|MDD|Valor|
|-|-|
|MDD3|000000122000|
|MDD4|000000100000|
|MDD5|003000&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;60000000040001234567|
|MDD6|0123456789|
|MDD7|ID|
|MDD8|411111|

---
actualizado: 30 Junio 2017
