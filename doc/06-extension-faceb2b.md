# Extensión de FACeB2B
Igual que es posible generar facturas listas para enviar a [FACe](https://face.gob.es/) añadiendo los objetos `FacturaeCentre` correspondientes dentro de la propiedad `centres` de un `FacturaeParty` (véase apartado de [entidades](02-entidades.md) para más información), existe su equivalente para el sector privado.

## Introducción a FACeB2B
FACeB2B es una plataforma casi idéntica a FACe para que empresas dadas de alta en [DIRe](https://dire.gob.es/) puedan enviarse facturas electrónicas unas a otras.

Para poder enviar facturas de forma automatizada en FACeB2B los documentos XML creados **deben contener el ID del destinatario(s) dentro de la plataforma**. Ese identificador se anexa al final de la factura para que FACeB2B sepa enrutarla al destinatario.

## Diferencias entre FACe y FACeB2B
Las facturas de FACe se pueden enviar a través del servicio web ofrecido por el Estado o desde la página web de FACe, mientras que las facturas de FACeB2B **solo pueden enviarse a través de su servicio web** (no hay interfaz gráfica).

Para poder subir una factura a FACe esta debe tener los centros (`FacturaeCentre`) declarados dentro del receptor (`FacturaeParty`). En cambio, FACeB2B declara las entidades de `FacturaeCentre` **en un anexo especial** al final de la factura.

> NOTA: una factura enviada por FACeB2B **sigue necesitando de emisor y receptor** (`FacturaeParty`) como cualquier otra, pese a declara los `FacturaeCentre` en un anexo.

## Usando FACeB2B con esta librería
Para convertir la factura generada en el [ejemplo de la introducción](01-introduccion.md) en un documento listo para enviar por FACeB2B, se añaden los centros a través de `$fac->getExtension('Fb2b')`:
```php
$b2b = $fac->getExtension('Fb2b');
$b2b->setReceiver(new FacturaeCentre([
  "code" => "51558103JES0001",
  "name" => "Centro administrativo receptor"
]));
```

Si existieran más partes implicadas en la transacción se añadirían con `addCentre`, debiendo indicar el rol de cada una:
```php
$b2b->addCentre(new FacturaeCentre([
  "code" => "51558103JES0002",
  "name" => "Unidad DIRe Compradora 0002",
  "role" => FacturaeCentre::ROLE_B2B_BUYER
]));
$b2b->addCentre(new FacturaeCentre([
  "code" => "51558103JES0003",
  "name" => "Unidad DIRe Fiscal 0003",
  "role" => FacturaeCentre::ROLE_B2B_FISCAL
]));
$b2b->addCentre(new FacturaeCentre([
  "code" => "51558103JES0004",
  "role" => FacturaeCentre::ROLE_B2B_COLLECTOR
]));
```

## Enviar facturas a FACeB2B
Esta extensión solo sirve para generar facturas electrónicas que puedan ser enviadas a FACeB2B. Para el proceso de envío programático consulta el apartado de [comunicación con FACeB2B](08-faceb2b.md).
