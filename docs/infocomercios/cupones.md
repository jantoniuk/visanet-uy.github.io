---
layout: page
title: Cupones
parent: InfoComercios
nav_order: 2
---


# Formato cupones Visanet - MEF/SELLOS
Basado en discusiones entre sellos...

## forma de acceso

## campos

|campo|tipo|descripcion|
|-----|----|----------|
|transaccion.sello|Alfanumérico|Identificador del sello (Amex, Anda, Cabal, Creditel, MasterCard, Oca, Visa, etc).
|transaccion.terminal|Alfanumérico|Indentificador único de la terminal en la cual se realizó la transacción.
|transaccion.fecha_cupon|Fecha Unix TimeStamp|Fecha en la cual se realizó la transacción.
|transaccion.fecha_proceso|Fecha Unix TimeStamp|Fecha en la cual se procesó la transacción por el adquirente para su posterior

```json
liquidación.
        "autorizacion":"", // Alfanumérico, Código de aprobación de la transaccion recibido por el comercio.
        "numero_factura":, // Numérico Entero, Numero de comprobante fiscal enviado por el comercio asociado a la transacción.
        "moneda":, // Numérico Entero, Código numérico ISO de la moneda de la transacción (ejemplo $- 858 - U$S 840)
        "importe":, // Numérico punto flotante, importe total de la transacción (incluye monto por ej propina, cashback)
        "propina":, // Numérico punto flotante, Monto propina total ingresada por el comercio.
        "lote":, // Numérico Entero, Número de lote de la terminal al momento de hacer la transacción en caso que corresponda.
        "origen":"", // Alfanumérico, Identificador del origen del medio de pago utilizado en la transacción (Ej Local/ Extranjera)
        "devolucion_impuesto":,// Numérico Entero, Identificador del beneficio fiscal otorgado a la transacción (xx: INDI / 00: NO Aplica Devolución / 99: No aplica campo)
        "importe_devolución_impuesto":,// Numérico punto flotante, Monto del befefico fiscal otorgado.
        "cuotas":,// Numérico Entero, Cantidad de cuotas ingresada por el comercio.
        "producto":"",// Alfanumérico , Identificador del tipo del medio de pago (ej: Visa Debito, Cabal BPS Prestaciones, Lider, OCA BPS Prestaciones, Creditel Prepaga, Anda Alimentación
        "tipo_transaccion":"",// Alfanumérico, Identificador del tipo de transacción (ej: Compra, Devolución, etc).
        "BIN":,// Numérico Entero, Primeros 6 dígitos del medio de pago utilizado en la transacción.
        "ultimos_4_Digitos":,// Numérico Entero, Ultimos 4 dígitos del medio de pago utilizado en la transacción.
        "canal":"",// Alfanumérico , Medio por el cual se inició la transacción (ej: POS, Manual, e-commerce, Débito automático, etc).
        "ticket":, // Numérico Entero, Número de ticket dentro del lote(si corresponde).
        "modo":"",// Alfanumérico, Modo de ingreso del medio de pago en la transacción (ej: manual, banda, chip, contactless,etc)
        "plan":"",// Alfanumérico, Identificador del plan comercial ingresado por el comercio al momento de la transacción (ej: Común, Nafta, Agro, Especial,etc)
    },
    "comercio":{ // Grupo de campos que siempre deben ser incluidos aunque la carga del valor sea opcional
        "nombre_fantasIa":"",// Alfanumérico, Nombre fantasía que identifica el comercio
        "razon_social":"",// Alfanumérico, Razón social que identifica el comercio.
        "tipo_documento":,// Alfanumérico, Identificador del tipo de documento del contribuyente (ej: RUT, Cedula, Pasaporte).
        "numero_de_documento":,// Numérico Entero, Número del documento del contribuyente.
        "codigo_comercio":,// Numérico Entero, Identificador del comercio ante el adquierente.
    },
    "liquidacion":{ // Grupo de campos que siempre deben ser incluidos aunque la carga del valor sea opcional
        "adquirente":"",// Alfanumérico, Identificador del adquirente de la transacción.
        "id":,// Alfanumérico , Identificador del proceso donde se liquida la transacción.
        "fecha_liquidacion":,// Fecha Unix TimeStamp, Fecha en la cual se liquidó la transacción para su posterior pago.
        "plan_venta":"",// Alfanumérico, Identificador del grupo de condiciones comerciales para la liquidación.
        "propina_excedente":,// Numérico punto flotante, Monto de la parte de la propina sobre la cual se calcula arancel (si corresponde).
        "fecha_pago":"" // Fecha Unix TimeStamp, Fecha en la cual se efectiviza el pago de la transacción al comercio.
        "banco_acreditacion":"",// Alfanumérico, Identificador de la institución financiera en la cual se acreditó el pago.
        "retencion_leyes":,// Numérico punto flotante, Monto del beneficio fiscal en la moneda de la transacción (si corresponde).
        "retencion_leyes_equivalente_$":,// Numérico punto flotante, Monto del beneficio fiscal expresado en pesos uruguayos.
        "porcentaje_beneficio_leyes":,// Numérico punto flotante, Puntos porcentuales del beneficio fiscal.
        "arancel",// Numérico punto flotante, Monto del arancel aplicado a la transacción por el adquirente (cobrado/devuelto).
        "iva_arancel",// Numérico punto flotante, Monto del iva del arancel aplicado a la transacción por el adquirente.
        "forma_de_pago",// Alfanumérico, Medio por el cual se efectiviza el pago al comercio (ej: Efectivo, Transferencia, Cheque, Retenido, Correo,etc)
    }
    “datos_especiales”:{ // alguno de estos datos serán soportados solo por algunos sellos.
        "IDWT" // Alfanumérico , // Código Aerolineas
        "cashback":,// Numérico punto flotante, // importe del dinero entregado al th por parte del comercio
        "codigo_minorista", // Aplica para terminales que manejan Mayoristas
        "código_mayorista", // Aplica para terminales que manejan Mayoristas
        "intereses_financiacion",// Numérico punto flotante, //
        "Iva_intereses_financiacion",// Numérico punto flotante,
        "fecha_entrega_cheque", // Fecha Unix TimeStamp,// Cabal
        "nro_pedido":,
    }
}
```
