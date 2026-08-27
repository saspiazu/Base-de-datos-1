# Base-de-datos-1
PROYECTO 1 — AVANCE DE PROYECTO
Gestión financiera y de inventario "Farmacia Lauren"
1. Narración del cliente — Farmacia "Lauren"
Título del caso

Control de ventas, fiados, inventario, compras a proveedores y gastos — Farmacia "Lauren"

Narración del cliente

"Soy la dueña de la Farmacia Lauren, la manejo hace nueve años y hasta ahora todo lo llevo a mano, en cuadernos y notas del celular. Necesito un sistema que me ayude a controlar bien las finanzas, porque ya no sé con exactitud qué entra y qué sale del negocio.

En la farmacia trabajamos tres personas: yo, mi hija y un empleado de mostrador, y los tres cobramos ventas. Manejamos más de trescientos productos distintos entre medicamentos genéricos, de marca, vitaminas, higiene y artículos de bebé, organizados por categorías. Cada producto tiene un precio, pero también me interesa saber en qué lote llegó y cuándo vence, porque se me han vencido medicamentos sin darme cuenta y eso es una pérdida que ni siquiera queda registrada.

Cuando vendo, a veces registro el producto, la cantidad y el precio en el cuaderno, pero en horas de mucho movimiento simplemente cobro y no anoto nada. La mayoría de los clientes paga en efectivo, algunos con tarjeta, y a los clientes de confianza del barrio les fío: anoto en otro cuaderno el nombre y el monto que deben, pero se me pierden datos o se me olvida cobrar. Un mismo cliente puede tener varias compras fiadas a la vez, y después puede ir pagando esa deuda en una o varias partes, no siempre de una sola vez.

Del lado de las compras, trabajo con cinco proveedores distintos que me traen mercadería. A algunos les pago de contado y a otros a crédito, con plazos de quince o treinta días, y necesito saber cuánto le debo a cada proveedor y para cuándo, porque ya me pasó que un proveedor me cobró dos veces la misma factura porque no tenía cómo comprobar que ya le había pagado. Cada compra puede traer varios productos distintos, en distintas cantidades, y cada entrega de producto forma un lote con su propia fecha de vencimiento.

Además de la mercadería, tengo otros gastos del negocio: alquiler, luz, agua, el sueldo del empleado y reparaciones del refrigerador donde guardo medicamentos con cadena de frío. Todo ese dinero sale de la misma caja donde entran las ventas, y como a veces yo misma saco plata de la caja para gastos de la casa sin anotarlo, al final del día no sé cuánto es ganancia real del negocio.

Me gustaría que el sistema me permita saber en cualquier momento cuánto tengo de inventario, qué productos están por vencer, cuánto me deben los clientes, cuánto debo yo a los proveedores, y cuál es la ganancia real del mes después de descontar todos los gastos."

Suposiciones
Se identifica al cliente por nombre y teléfono; el registro de cliente es opcional para una venta al contado, pero obligatorio cuando la venta es fiada (a crédito).
Una venta pertenece a un único cliente (o ninguno, si es al contado sin registrar datos) y es atendida por un único empleado, pero puede recibir varios pagos si el cliente cancela su fiado en distintas fechas.
El precio de cada producto dentro de una venta o de una compra se "congela" en el detalle correspondiente, para no verse afectado por cambios posteriores del precio de lista.
Cada entrada de mercadería (compra) genera uno o varios lotes por producto, cada uno con su propia fecha de vencimiento, ya que un mismo producto puede tener lotes con vencimientos distintos.
El control de caja (ingresos por venta, pagos de fiado, retiros y pagos de gastos) se modela como una bitácora de movimientos, no solo como un saldo final.
La validación de que no se venda un producto sin existencias suficientes, o que un lote vencido no se pueda vender, se resuelve a nivel de aplicación.
Entidades iniciales detectadas (sustantivos relevantes)

Cliente, Empleado, Proveedor, Categoría, Producto, Lote, Venta, Crédito/Fiado, Compra, Gasto, Movimiento de Caja.

Reglas y restricciones extra
El código de cada producto es único.
La cantidad en un lote debe ser mayor o igual a 0.
El estado de una venta es uno de: pagada, fiada, fiada parcial.
El estado de un fiado (crédito de cliente) es uno de: pendiente, pagado parcial, pagado total.
El estado de una compra es uno de: pendiente, pagada, pagada parcial.
El método de pago es uno de: efectivo, tarjeta.
El tipo de movimiento de caja es uno de: ingreso por venta, pago de cliente (fiado), pago a proveedor, pago de gasto, retiro personal.
2. Modelo conceptual — Entidades, atributos y relaciones
2.1 Entidades y atributos
CLIENTE
id_cliente
nombre
apellido
telefono
direccion
EMPLEADO
id_empleado
nombre
apellido
cargo
telefono
PROVEEDOR
id_proveedor
nombre_empresa
contacto
telefono
plazo_credito_dias
CATEGORIA
id_categoria
nombre
PRODUCTO
id_producto
nombre
descripcion
precio_venta
id_categoria
requiere_receta
LOTE
id_lote
id_producto
id_compra
cantidad
fecha_vencimiento
fecha_ingreso
VENTA
id_venta
id_cliente
id_empleado
fecha_hora
estado
total
VENTA_DETALLE (entidad asociativa)
id_venta
id_lote
cantidad
precio_unitario
COMPRA
id_compra
id_proveedor
fecha_compra
fecha_vencimiento_pago
estado
total
COMPRA_DETALLE (entidad asociativa)
id_compra
id_producto
cantidad
precio_unitario_compra
GASTO
id_gasto
concepto
monto
fecha_gasto
tipo (alquiler, servicios, sueldo, reparación, otro)
MOVIMIENTO_CAJA
id_movimiento
tipo
monto
fecha_hora
id_venta (opcional)
id_compra (opcional)
id_gasto (opcional)
descripcion
2.2 Relaciones y cardinalidades (justificación)

CLIENTE 1 — N VENTA (opcional) Un cliente registrado puede generar muchas ventas a lo largo del tiempo, sobre todo cuando compra fiado; una venta puede no tener cliente asociado si es al contado y no se registran datos.

EMPLEADO 1 — N VENTA Un empleado atiende muchas ventas durante su turno; cada venta es atendida por un único empleado, lo que permite revisar quién registró cada movimiento.

CATEGORIA 1 — N PRODUCTO Una categoría agrupa muchos productos (varios medicamentos, varias vitaminas, etc.); cada producto pertenece a una única categoría.

PROVEEDOR 1 — N COMPRA Un proveedor entrega mercadería en muchas compras distintas; cada compra corresponde a un único proveedor.

COMPRA 1 — N LOTE Una compra puede generar varios lotes, uno por cada producto que trae; cada lote proviene de una única compra, lo que permite rastrear de qué entrega y proveedor vino.

PRODUCTO 1 — N LOTE Un producto puede tener muchos lotes con distintas fechas de vencimiento a lo largo del tiempo; cada lote pertenece a un único producto.

VENTA N — M LOTE (resuelta con VENTA_DETALLE) Una venta puede incluir varios productos (lotes) distintos, y un mismo lote puede venderse en varias ventas hasta agotarse. La entidad asociativa guarda la cantidad vendida y el precio congelado al momento de la venta, además de permitir descontar del lote correcto (control de vencimientos, tipo PEPS: primero en vencer, primero en salir).

COMPRA N — M PRODUCTO (resuelta con COMPRA_DETALLE) Una compra puede incluir varios productos distintos, y un mismo producto puede comprarse en muchas compras diferentes. La entidad asociativa guarda la cantidad y el precio de compra de cada producto en esa orden específica.

VENTA 1 — N MOVIMIENTO_CAJA Una venta fiada puede saldarse con varios pagos parciales, cada uno registrado como un movimiento de caja; una venta al contado genera un único movimiento de ingreso.

COMPRA 1 — N MOVIMIENTO_CAJA Una compra a crédito puede pagarse en varias partes; cada pago a proveedor queda registrado como un movimiento de caja asociado a esa compra.

GASTO 1 — N MOVIMIENTO_CAJA Cada gasto genera al menos un movimiento de caja que representa su pago; permite diferenciar salidas de dinero por gasto operativo de las salidas por pago a proveedores.

2.3 Restricciones de negocio importantes
No se debe permitir registrar una venta de un lote cuya cantidad disponible sea menor a la cantidad solicitada (se valida en la capa de aplicación).
No se debe permitir vender un lote cuya fecha_vencimiento ya haya pasado.
El precio_unitario en VENTA_DETALLE y COMPRA_DETALLE se copia del precio vigente al momento de la operación, para no verse afectado por cambios posteriores de precios.
Una venta no debería marcarse como pagada si la suma de los movimientos de caja asociados no cubre el total de la venta (validación de aplicación).
Un retiro personal se registra como MOVIMIENTO_CAJA de tipo "retiro personal", sin asociarse a ninguna venta, compra ni gasto, para poder separarlo de la ganancia real del negocio.
El sistema debe permitir identificar productos próximos a vencer comparando la fecha_vencimiento de cada lote contra la fecha actual.
2.4 Representación para el DER
Entidades (rectángulo/tabla): CLIENTE, EMPLEADO, PROVEEDOR, CATEGORIA, PRODUCTO, LOTE, VENTA, COMPRA, GASTO, MOVIMIENTO_CAJA.
Entidades asociativas (N:M con atributos propios): VENTA_DETALLE, COMPRA_DETALLE.
Cardinalidades: CLIENTE (1)—(N) VENTA; EMPLEADO (1)—(N) VENTA; CATEGORIA (1)—(N) PRODUCTO; PROVEEDOR (1)—(N) COMPRA; COMPRA (1)—(N) LOTE; PRODUCTO (1)—(N) LOTE; VENTA (N)—(M) LOTE; COMPRA (N)—(M) PRODUCTO; VENTA (1)—(N) MOVIMIENTO_CAJA; COMPRA (1)—(N) MOVIMIENTO_CAJA; GASTO (1)—(N) MOVIMIENTO_CAJA.
