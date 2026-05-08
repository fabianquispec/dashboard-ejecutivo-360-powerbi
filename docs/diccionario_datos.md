# Diccionario de Datos — Dashboard Ejecutivo 360°

Descripción del modelo dimensional utilizado en el dashboard. Cada tabla incluye su tipo, granularidad, volumen aproximado y los campos principales con su descripción de negocio.

> **Fuente de datos:** Archivos CSV simulados con escenarios de negocio reales.  
> **Esquema:** Estrella (Star Schema)  
> **Herramienta de transformación:** Power Query (M)

---

## Tablas de Hechos

### Fact_Ventas
| Atributo | Detalle |
|----------|---------|
| **Granularidad** | Una fila por transacción de venta |
| **Volumen** | ~20,000 filas |
| **Descripción** | Tabla central del modelo. Registra cada venta con su precio, costo, cantidad y descuento aplicado. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveVenta` | Entero | Identificador único de la transacción |
| `ClaveCliente` | Entero | Llave foránea a `Dim_Cliente` |
| `ClaveVendedor` | Entero | Llave foránea a `Dim_Vendedor` |
| `ClaveProducto` | Entero | Llave foránea a `Dim_Producto` |
| `ClaveFecha` | Entero | Llave foránea a `Dim_Fecha` |
| `VentaTotal` | Decimal | Monto total de la venta (precio × cantidad) |
| `CostoTotal` | Decimal | Costo total asociado a la venta |
| `PrecioLista` | Decimal | Precio de lista antes de descuento |
| `PrecioNeto` | Decimal | Precio final aplicado al cliente |
| `Cantidad` | Entero | Unidades vendidas en la transacción |
| `Descuento` | Decimal | Porcentaje de descuento aplicado (0 a 1) |

---

### Fact_Metas_Ventas
| Atributo | Detalle |
|----------|---------|
| **Granularidad** | Una fila por vendedor por mes |
| **Volumen** | ~360 filas |
| **Descripción** | Contiene las metas comerciales asignadas por vendedor y periodo. Permite calcular cumplimiento, brecha y semáforos de desempeño. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveVendedor` | Entero | Llave foránea a `Dim_Vendedor` |
| `ClaveFecha` | Entero | Llave foránea a `Dim_Fecha` |
| `MetaVentas` | Decimal | Meta de ventas asignada para el periodo |

---

### Fact_Inventario
| Atributo | Detalle |
|----------|---------|
| **Granularidad** | Una fila por producto por periodo |
| **Volumen** | ~720 filas |
| **Descripción** | Registra el nivel de stock por SKU y su punto de reposición. Permite calcular rotación, días de inventario, quiebres de stock y ventas perdidas. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveProducto` | Entero | Llave foránea a `Dim_Producto` |
| `ClaveFecha` | Entero | Llave foránea a `Dim_Fecha` |
| `StockUnidades` | Entero | Unidades disponibles en inventario |
| `PuntoReposición` | Entero | Umbral mínimo de stock antes de reponer |
| `ValorStock` | Decimal | Valor monetario del stock disponible |

---

### Fact_Oportunidad_Etapa
| Atributo | Detalle |
|----------|---------|
| **Granularidad** | Una fila por oportunidad por etapa del funnel |
| **Volumen** | ~2,189 filas |
| **Descripción** | Registra el avance de cada oportunidad comercial a través de las etapas del pipeline. Permite calcular conversión, win rate, ciclo de venta y pipeline ponderado. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveOportunidad` | Entero | Identificador único de la oportunidad |
| `ClaveVendedor` | Entero | Llave foránea a `Dim_Vendedor` |
| `EtapaVenta` | Texto | Etapa actual del pipeline (Lead, Propuesta, Negociación, Ganada, Perdida) |
| `MontoEsperado` | Decimal | Valor estimado de la oportunidad |
| `ProbabilidadEtapa` | Decimal | Probabilidad de cierre según la etapa (0 a 1) |
| `FechaCreacion` | Fecha | Fecha en que se registró la oportunidad |
| `FechaCierreFinal` | Fecha | Fecha de cierre real (ganada o perdida) |
| `ResultadoFinal` | Texto | Resultado de la oportunidad (Ganada / Perdida / en blanco si abierta) |

---

## Tablas de Dimensiones

### Dim_Fecha
| Atributo | Detalle |
|----------|---------|
| **Descripción** | Tabla de calendario completa con jerarquías temporales. Eje de todas las medidas de inteligencia de tiempo (YoY, MoM, YTD, DATESINPERIOD). |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `Fecha` | Fecha | Fecha completa (clave principal) |
| `Año` | Entero | Año de la fecha |
| `#Mes` | Entero | Número de mes (1–12) |
| `Mes` | Texto | Nombre del mes |
| `Trimestre` | Texto | Trimestre (Q1, Q2, Q3, Q4) |
| `Año Mes` | Texto | Formato "YYYY-MM" para agrupación mensual |
| `Año Mes Short` | Entero | Versión numérica de Año Mes para ordenamiento |
| `Indice_Mes` | Entero | Índice secuencial de mes para cálculos de forecast |

---

### Dim_Cliente
| Atributo | Detalle |
|----------|---------|
| **Volumen** | ~200 filas |
| **Descripción** | Base de clientes enriquecida con segmentación RFM calculada en Power Query para dos ventanas temporales (12M y 6M). Permite análisis de retención, migración de segmento y riesgo de abandono. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveCliente` | Entero | Identificador único del cliente |
| `NombreCliente` | Texto | Nombre o razón social |
| `Segmento RFM` | Texto | Segmento RFM general (VIP, Leal, Potencial, en Riesgo, Perdido) |
| `Segmento_RFM_12M` | Texto | Segmento RFM calculado sobre los últimos 12 meses |
| `Segmento_RFM_6M` | Texto | Segmento RFM calculado sobre los últimos 6 meses |
| `Recencia_6M_Dias` | Entero | Días transcurridos desde la última compra dentro de la ventana de 6 meses. Permite detectar clientes VIP con inactividad reciente. |
| `Frecuencia_Compras` | Entero | Número total de transacciones del cliente |

---

### Dim_Vendedor
| Atributo | Detalle |
|----------|---------|
| **Volumen** | ~15 filas |
| **Descripción** | Fuerza de ventas. Permite analizar desempeño individual, ranking estratégico y distribución de cartera de clientes. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveVendedor` | Entero | Identificador único del vendedor |
| `Vendedor` | Texto | Nombre del vendedor |
| `Region` | Texto | Región comercial asignada |
| `Zona` | Texto | Zona de cobertura dentro de la región |

---

### Dim_Producto
| Atributo | Detalle |
|----------|---------|
| **Volumen** | ~30 filas |
| **Descripción** | Catálogo de productos. Permite análisis por SKU, categoría y marca. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveProducto` | Entero | Identificador único del producto |
| `NombreProducto` | Texto | Nombre del producto |
| `ClaveCategoria` | Entero | Llave foránea a `Dim_Categoria` |
| `Marca` | Texto | Marca del producto |

---

### Dim_Categoria
| Atributo | Detalle |
|----------|---------|
| **Volumen** | 3 filas |
| **Descripción** | Categorías comerciales del portafolio. Permite agrupar ventas, márgenes e inventario por línea de negocio. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveCategoria` | Entero | Identificador único de la categoría |
| `Categoria` | Texto | Nombre de la categoría (Ropa, Calzado, Accesorios) |

---

### Dim_Etapa_Venta
| Atributo | Detalle |
|----------|---------|
| **Volumen** | 6 filas |
| **Descripción** | Define las etapas del pipeline comercial con su orden y probabilidad asociada. Permite construir el funnel de conversión. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `EtapaVenta` | Texto | Nombre de la etapa (Lead, Propuesta, Negociación, Ganada, Perdida) |
| `OrdenEtapa` | Entero | Orden secuencial para visualización del funnel |
| `ProbabilidadEtapa` | Decimal | Probabilidad de cierre estándar por etapa (0 a 1) |

---

### Dim_Mercado_Estimado_Valor
| Atributo | Detalle |
|----------|---------|
| **Volumen** | ~72 filas |
| **Descripción** | Estimación del tamaño de mercado por categoría y periodo. Permite calcular participación de mercado, brecha competitiva e índice de competitividad. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClaveCategoria` | Entero | Llave foránea a `Dim_Categoria` |
| `ClaveFecha` | Entero | Llave foránea a `Dim_Fecha` |
| `Valor Mercado Realista` | Decimal | Estimación del tamaño total del mercado para la categoría |
| `Crecimiento Mercado % YoY` | Decimal | Tasa de crecimiento anual estimada del mercado |

---

### Dim_Acciones_RFM
| Atributo | Detalle |
|----------|---------|
| **Volumen** | 5 filas |
| **Descripción** | Playbook de acciones comerciales recomendadas por segmento RFM. Permite traducir el análisis de clientes en acciones concretas dentro del dashboard. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `Segmento RFM` | Texto | Segmento al que aplica la acción |
| `Acción Comercial` | Texto | Acción recomendada para el segmento |
| `Prioridad` | Texto | Nivel de prioridad (Alta / Media / Baja) |
| `Responsable` | Texto | Área o rol responsable de ejecutar la acción |

---

### Dim_Promocion
| Atributo | Detalle |
|----------|---------|
| **Volumen** | 5 filas |
| **Descripción** | Catálogo de promociones activas. Permite segmentar el análisis de ventas y márgenes según el tipo de promoción aplicada. |

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ClavePromocion` | Entero | Identificador único de la promoción |
| `NombrePromocion` | Texto | Nombre descriptivo de la promoción |
| `TipoPromocion` | Texto | Clasificación del tipo de promoción |

---

## Diagrama del modelo

```
Dim_Fecha ──────────────────────────────────────────────────┐
Dim_Cliente ──────────────────────────────────────┐         │
Dim_Vendedor ──────────────────────────┐          │         │
Dim_Producto ──────────────┐           │          │         │
Dim_Categoria ─── Dim_Producto         │          │         │
                           │           │          │         │
                    Fact_Ventas ────────┴──────────┴─────────┘
                    Fact_Metas_Ventas ──────────────┬─────────┘
                    Fact_Inventario ────────────────┘
                    Fact_Oportunidad_Etapa ──────────────────┐
                                                             │
Dim_Etapa_Venta ─────────────────────────────────────────────┘
Dim_Mercado_Estimado_Valor (relacionada a Dim_Categoria + Dim_Fecha)
Dim_Acciones_RFM (relacionada a Dim_Cliente por Segmento RFM)
```

---

*Documentación generada para el repositorio [dashboard-ejecutivo-360-powerbi](https://github.com/fabianquispec/dashboard-ejecutivo-360-powerbi)*
