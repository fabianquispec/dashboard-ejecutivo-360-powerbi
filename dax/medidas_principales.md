# Medidas DAX — Dashboard Ejecutivo 360°

Documentación de las medidas DAX utilizadas en el modelo analítico. Organizadas por módulo funcional, con descripción del propósito de negocio y la lógica técnica aplicada.

> **Nota:** Las medidas relacionadas con Forecast están excluidas de esta documentación por pertenecer a un módulo independiente.

---

## 1. Ventas y Crecimiento

Medidas base del modelo comercial. Alimentan la mayoría de visualizaciones del dashboard.

```dax
Ventas Totales =
SUM ( Fact_Ventas[VentaTotal] )
```

```dax
Ventas Año Anterior =
CALCULATE (
    [Ventas Totales],
    SAMEPERIODLASTYEAR ( Dim_Fecha[Fecha] )
)
```

```dax
Crecimiento YoY % =
DIVIDE (
    [Ventas Totales] - [Ventas Año Anterior],
    [Ventas Año Anterior]
)
```

```dax
Ventas Mes Anterior =
CALCULATE (
    [Ventas Totales],
    DATEADD ( Dim_Fecha[Fecha], -1, MONTH )
)
```

```dax
Crecimiento MoM % =
DIVIDE (
    [Ventas Totales] - [Ventas Mes Anterior],
    [Ventas Mes Anterior]
)
```

```dax
Ventas YTD =
TOTALYTD (
    [Ventas Totales],
    Dim_Fecha[Fecha]
)
```

---

## 2. Meta y Cumplimiento

Medidas que controlan el desempeño vs objetivo comercial.

```dax
Meta Ventas =
SUM ( Fact_Metas_Ventas[MetaVentas] )
```

```dax
Cumplimiento Meta % =
DIVIDE ( [Ventas Totales], [Meta Ventas] )
```

```dax
Brecha Meta =
[Meta Ventas] - [Ventas Totales]
```

```dax
Cumplimiento Meta YTD % =
DIVIDE (
    TOTALYTD ( [Ventas Totales], Dim_Fecha[Fecha] ),
    TOTALYTD ( [Meta Ventas], Dim_Fecha[Fecha] )
)
```

```dax
Cumplimiento Meta 3M =
DIVIDE (
    CALCULATE (
        [Ventas Totales],
        DATESINPERIOD ( Dim_Fecha[Fecha], MAX ( Dim_Fecha[Fecha] ), -3, MONTH )
    ),
    CALCULATE (
        [Meta Ventas],
        DATESINPERIOD ( Dim_Fecha[Fecha], MAX ( Dim_Fecha[Fecha] ), -3, MONTH )
    )
)
```

```dax
-- Semáforo visual de cumplimiento de meta
Semáforo Cumplimiento =
SWITCH (
    TRUE (),
    [Cumplimiento Meta %] >= 1, "🟢 Cumplido",
    [Cumplimiento Meta %] >= 0.9, "🟡 Riesgo",
    "🔴 Incumplido"
)
```

---

## 3. Rentabilidad y Precio

```dax
Costo Total =
SUM ( Fact_Ventas[CostoTotal] )
```

```dax
Margen Bruto =
[Ventas Totales] - [Costo Total]
```

```dax
Margen Bruto % =
DIVIDE ( [Margen Bruto], [Ventas Totales] )
```

```dax
Margen Neto Estimado =
SUMX (
    Fact_Ventas,
    Fact_Ventas[VentaTotal]
        - Fact_Ventas[CostoTotal]
        - ( Fact_Ventas[PrecioLista] * Fact_Ventas[Descuento] )
)
```

```dax
Precio Promedio =
DIVIDE ( [Ventas Totales], [Cantidad Total] )
```

```dax
Descuento Promedio % =
AVERAGE ( Fact_Ventas[Descuento] )
```

```dax
-- Mide la sensibilidad de la demanda ante cambios de precio
Elasticidad Precio =
DIVIDE ( [Variación Cantidad %], [Variación Precio %] )
```

```dax
Tipo Elasticidad =
SWITCH (
    TRUE (),
    [Elasticidad Precio] <= -1, "Alta elasticidad (precio sensible)",
    [Elasticidad Precio] < 0, "Elasticidad moderada",
    [Elasticidad Precio] >= 0, "Demanda rígida o atípica",
    BLANK ()
)
```

---

## 4. Clientes y Segmentación RFM

Medidas del módulo de clientes. La segmentación RFM se calcula a nivel de tabla en Power Query; estas medidas complementan el análisis ejecutivo.

```dax
Clientes Activos =
DISTINCTCOUNT ( Fact_Ventas[ClaveCliente] )
```

```dax
Ticket Promedio Cliente =
DIVIDE ( [Ventas Totales], DISTINCTCOUNT ( Fact_Ventas[ClaveCliente] ) )
```

```dax
-- Detecta clientes VIP que deterioraron su comportamiento en los últimos 6 meses
Clientes_VIP_en_Riesgo =
CALCULATE (
    DISTINCTCOUNT ( Dim_Cliente[ClaveCliente] ),
    Dim_Cliente[Segmento_RFM_12M] = "Clientes VIP",
    Dim_Cliente[Segmento_RFM_6M] IN {
        "Clientes en Riesgo",
        "Clientes Perdidos"
    }
)
```

```dax
% Ventas Clientes VIP =
DIVIDE (
    CALCULATE (
        [Ventas Totales],
        Dim_Cliente[Segmento RFM] = "Clientes VIP"
    ),
    [Ventas Totales]
)
```

```dax
-- Valor de vida del cliente estimado
CLV =
[Ticket Promedio Cliente] * [Frecuencia Mensual] * [Vida Cliente (Meses)] * [Margen Promedio %]
```

```dax
Vida Cliente (Meses) =
VAR PrimeraCompra = CALCULATE (
    MIN ( Dim_Fecha[Fecha] ),
    ALLEXCEPT ( Dim_Cliente, Dim_Cliente[ClaveCliente] )
)
RETURN DATEDIFF ( PrimeraCompra, [Fecha Última Compra], MONTH )
```

```dax
-- Alerta dinámica basada en migración de segmento RFM
Alerta RFM =
VAR VIPRiesgo =
    CALCULATE (
        COUNTROWS ( Dim_Cliente ),
        Dim_Cliente[Segmento_RFM_12M] = "Clientes VIP",
        Dim_Cliente[Recencia_6M_Dias] > 90
    )
VAR Dormidos =
    CALCULATE (
        COUNTROWS ( Dim_Cliente ),
        Dim_Cliente[Segmento_RFM_6M] = "Dormidos"
    )
RETURN SWITCH (
    TRUE (),
    VIPRiesgo > 0, "Alerta: Clientes VIP en riesgo",
    Dormidos > 0, "Alerta: Clientes dormidos detectados",
    "Sin alertas RFM relevantes"
)
```

---

## 5. Fuerza de Ventas

```dax
Cumplimiento Meta Vendedor % =
AVERAGEX (
    VALUES ( Dim_Vendedor[ClaveVendedor] ),
    [Cumplimiento Meta %]
)
```

```dax
Ranking Vendedores =
RANKX (
    ALL ( Dim_Vendedor[Vendedor] ),
    [Ventas Totales],
    ,
    DESC
)
```

```dax
-- Índice compuesto que pondera cumplimiento, calidad de cartera y CLV
Índice Calidad Comercial =
0.5 * [Cumplimiento Meta %]
+ 0.3 * [% Ventas Clientes VIP]
+ 0.2 * DIVIDE ( [CLV por Vendedor], CALCULATE ( [CLV por Vendedor], ALL ( Dim_Vendedor ) ) )
```

```dax
Ranking Estratégico =
RANKX (
    ALL ( Dim_Vendedor[Vendedor] ),
    [Índice Calidad Comercial],
    ,
    DESC
)
```

---

## 6. Inventario

```dax
Stock Total =
SUM ( Fact_Inventario[StockUnidades] )
```

```dax
Rotación Inventarios =
DIVIDE ( [Ventas Totales], [Stock Total] )
```

```dax
Días Inventario =
DIVIDE ( 365, [Rotación Inventarios] )
```

```dax
Quiebre Stock =
IF (
    MIN ( Fact_Inventario[StockUnidades] ) < MIN ( Fact_Inventario[PuntoReposición] ),
    1,
    0
)
```

```dax
Ventas Perdidas por Quiebre =
CALCULATE (
    [Ventas Totales],
    FILTER (
        Fact_Inventario,
        Fact_Inventario[StockUnidades] < Fact_Inventario[PuntoReposición]
    )
)
```

```dax
-- Proporción de productos sin quiebre de stock (1 = inventario perfecto)
Índice Salud Inventario =
1 - DIVIDE (
    CALCULATE (
        COUNTROWS ( Fact_Inventario ),
        Fact_Inventario[StockUnidades] < Fact_Inventario[PuntoReposición]
    ),
    COUNTROWS ( Fact_Inventario )
)
```

---

## 7. Mercado y Competencia

```dax
Valor Mercado ($) =
SUM ( Dim_Mercado_Estimado_Valor[Valor Mercado Realista] )
```

```dax
Share Mercado % =
DIVIDE ( [Ventas Totales], [Valor Mercado ($)] )
```

```dax
Brecha Crecimiento vs Mercado (pp) =
VAR CrecEmpresa = [Crecimiento YoY %]
VAR CrecMercado = [Crecimiento Mercado YoY %]
RETURN IF (
    NOT ISBLANK ( CrecEmpresa ) && NOT ISBLANK ( CrecMercado ),
    CrecEmpresa - CrecMercado,
    BLANK ()
)
```

```dax
-- Ratio de competitividad: >1 significa que la empresa crece más rápido que el mercado
Indice Competitividad =
DIVIDE ( 1 + [Crecimiento YoY %], 1 + [Crecimiento Mercado YoY %] )
```

---

## 8. KPI Rector Ejecutivo

El KPI más importante del modelo. Integra 5 dimensiones del negocio en un índice único de 0 a 100.

```dax
KPI Rector Ejecutivo =
VAR Cumpl = COALESCE ( [Cumplimiento Meta %], 0 )
VAR Margen = COALESCE ( [Margen Bruto %], 0 )
VAR SaludInv = COALESCE ( [Índice Salud Inventario], 0 )
VAR VIPRiesgo = COALESCE ( [Clientes_VIP_en_Riesgo], 0 )
VAR DesvForecast =
    COALESCE (
        ABS ( [Desviación % vs Forecast] ),
        ABS ( 1 - COALESCE ( [Cumplimiento Forecast %], 1 ) )
    )

-- Score Cumplimiento (0..1): 100% meta = score 1
VAR ScoreCumpl = MIN ( 1, MAX ( 0, DIVIDE ( Cumpl, 1 ) ) )

-- Score Margen (0..1): 20% = 0, 35% = 1
VAR ScoreMargen =
    MIN ( 1, MAX ( 0, DIVIDE ( Margen - 0.20, 0.35 - 0.20 ) ) )

-- Score Salud Inventario (0..1): 75% = 0, 95% = 1
VAR ScoreInv =
    MIN ( 1, MAX ( 0, DIVIDE ( SaludInv - 0.75, 0.95 - 0.75 ) ) )

-- Score Riesgo VIP: penaliza fuertemente si hay clientes VIP en riesgo
VAR ScoreVIP =
    SWITCH (
        TRUE (),
        VIPRiesgo = 0, 1,
        VIPRiesgo <= 2, 0.70,
        VIPRiesgo <= 5, 0.40,
        0.10
    )

-- Score Forecast: basado en desviación absoluta vs proyección
VAR ScoreForecast =
    SWITCH (
        TRUE (),
        DesvForecast <= 0.05, 1.00,
        DesvForecast <= 0.10, 0.80,
        DesvForecast <= 0.20, 0.50,
        DesvForecast <= 0.30, 0.30,
        0.10
    )

-- Pesos ponderados (suma = 1.00)
VAR wCumpl    = 0.35
VAR wMargen   = 0.20
VAR wInv      = 0.15
VAR wVIP      = 0.15
VAR wForecast = 0.15

VAR ScoreFinal =
    ( ScoreCumpl * wCumpl )
    + ( ScoreMargen * wMargen )
    + ( ScoreInv * wInv )
    + ( ScoreVIP * wVIP )
    + ( ScoreForecast * wForecast )

RETURN ROUND ( 100 * ScoreFinal, 1 )
```

```dax
-- Semáforo ejecutivo basado en el KPI Rector
Semaforo KPI Rector =
VAR S = [KPI Rector Ejecutivo]
RETURN SWITCH (
    TRUE (),
    S < 60, "🔴 Crítico",
    S < 80, "🟠 Atención",
    "🟢 Controlado"
)
```

---

## 9. Insights Automáticos

Medidas de narrativa analítica en lenguaje natural. Generan el texto ejecutivo de cada página automáticamente según el contexto del filtro activo.

```dax
-- Insight maestro de la página Dirección General
Insight_Maestro_Direccion_2 =
VAR Ventas = [Ventas Totales]
VAR Margen = [Margen Bruto %]
VAR Cumpl = [Cumplimiento Meta %]
VAR Crec = [Crecimiento YoY %]
VAR VIPRiesgo = [Clientes_VIP_en_Riesgo]

VAR TextoVIP =
    IF (
        VIPRiesgo > 0,
        "Se identifican " & FORMAT ( VIPRiesgo, "0" ) & " clientes VIP en riesgo, lo que requiere retención prioritaria.",
        "No hay clientes VIP en riesgo."
    )

VAR TextoAccion =
    IF (
        Cumpl < 0.95 || Crec < 0,
        "La ejecución requiere atención para corregir la brecha y reactivar el crecimiento.",
        "La ejecución se mantiene alineada y bajo control."
    )

RETURN
    "El negocio vende "
    & FORMAT ( Ventas, "#,0.00" )
    & " con un margen "
    & IF ( Margen >= 0.30, "saludable", "moderado" )
    & " (" & FORMAT ( Margen, "0.0%" ) & "), "
    & IF ( Cumpl >= 1, "supera", "no alcanza" )
    & " la meta (" & FORMAT ( Cumpl, "0.0%" ) & ") y "
    & IF ( Crec < 0, "presenta crecimiento negativo", "mantiene crecimiento positivo" )
    & " (" & FORMAT ( Crec, "0.0%" ) & "). "
    & TextoVIP & " " & TextoAccion
```

```dax
-- Insight de migración de clientes entre segmentos RFM
Insight_Maestro_Migracion =
VAR MigracionVIP =
    CALCULATE (
        COUNTROWS ( Dim_Cliente ),
        Dim_Cliente[Segmento_RFM_12M] = "Clientes VIP",
        Dim_Cliente[Segmento_RFM_6M] IN { "Clientes en Riesgo", "Clientes Perdidos" }
    )
VAR TotalVIP =
    CALCULATE (
        COUNTROWS ( Dim_Cliente ),
        Dim_Cliente[Segmento_RFM_12M] = "Clientes VIP"
    )
VAR PorcentajeMigracion = DIVIDE ( MigracionVIP, TotalVIP )

RETURN SWITCH (
    TRUE (),
    PorcentajeMigracion >= 0.25,
        "Riesgo crítico de migración: " & FORMAT ( PorcentajeMigracion, "0%" )
        & " de los clientes VIP han deteriorado su comportamiento. Se requiere acción inmediata.",
    PorcentajeMigracion >= 0.10,
        "Alerta de migración: " & FORMAT ( PorcentajeMigracion, "0%" )
        & " de los clientes VIP muestran señales de deterioro.",
    MigracionVIP > 0,
        "Migración bajo monitoreo: " & FORMAT ( PorcentajeMigracion, "0%" )
        & " de los clientes VIP han reducido su nivel de engagement.",
    "Cartera estable: no se detectan migraciones negativas relevantes."
)
```

---

## 10. Alertas Ejecutivas

Sistema de alertas automáticas que detecta condiciones críticas del negocio y las comunica en lenguaje directo.

```dax
Nivel Alerta =
SWITCH (
    TRUE (),
    [Cumplimiento Meta %] < 0.85
        || [Clientes_VIP_en_Riesgo] > 0
        || [Índice Salud Inventario] < 0.75, "Crítica",
    [Cumplimiento Meta %] < 0.95
        || [Crecimiento MoM %] < 0, "Atención",
    "Controlado"
)
```

```dax
Alerta Incumplimiento Meta =
IF ( [Cumplimiento Meta %] < 0.85, "Incumplimiento crítico de meta comercial", BLANK () )
```

```dax
Alerta Clientes VIP =
VAR VIPRiesgo = [Clientes_VIP_en_Riesgo]
RETURN IF (
    VIPRiesgo > 0,
    VIPRiesgo & " clientes VIP en riesgo de abandono",
    BLANK ()
)
```

```dax
Alerta Quiebre Stock =
VAR Perdidas = [Ventas Perdidas por Quiebre]
RETURN IF (
    Perdidas > 0,
    "Quiebres de stock generan pérdidas estimadas de " & FORMAT ( Perdidas, "#,##0" ),
    BLANK ()
)
```

```dax
Alerta Margen =
IF ( [Margen Bruto %] < 0.25, "Margen bruto por debajo del nivel esperado", BLANK () )
```

---

## 11. Funnel Comercial

```dax
Oportunidades Totales =
DISTINCTCOUNT ( Fact_Oportunidad_Etapa[ClaveOportunidad] )
```

```dax
Tasa de Cierre % =
DIVIDE ( [Oportunidades Ganadas], [Oportunidades Cerradas] )
```

```dax
Win Rate =
DIVIDE (
    [Oportunidades Ganadas],
    [Oportunidades Ganadas] + [Oportunidades Perdidas]
)
```

```dax
% Conversión Embudo =
DIVIDE ( [Oportunidades Ganadas], [Leads Históricos] )
```

```dax
-- Ciclo de venta promedio en días para oportunidades cerradas
Ciclo de Venta Promedio (Días) =
VAR OppsCerradas =
    FILTER (
        VALUES ( Fact_Oportunidad_Etapa[ClaveOportunidad] ),
        NOT ISBLANK (
            CALCULATE (
                MAX ( Fact_Oportunidad_Etapa[ResultadoFinal] ),
                REMOVEFILTERS ( Fact_Oportunidad_Etapa[EtapaVenta] )
            )
        )
    )
RETURN AVERAGEX (
    OppsCerradas,
    VAR FechaCreacion = CALCULATE ( MIN ( Fact_Oportunidad_Etapa[FechaCreacion] ), REMOVEFILTERS ( Fact_Oportunidad_Etapa[EtapaVenta] ) )
    VAR FechaCierre   = CALCULATE ( MAX ( Fact_Oportunidad_Etapa[FechaCierreFinal] ), REMOVEFILTERS ( Fact_Oportunidad_Etapa[EtapaVenta] ) )
    RETURN IF (
        NOT ISBLANK ( FechaCreacion ) && NOT ISBLANK ( FechaCierre ),
        DATEDIFF ( FechaCreacion, FechaCierre, DAY ),
        BLANK ()
    )
)
```

```dax
Salud Funnel Comercial =
SWITCH (
    TRUE (),
    [Tasa de Cierre %] < 0.2, "Crítico",
    [Tasa de Cierre %] < 0.35, "En observación",
    "Saludable"
)
```

---

## Modelo dimensional

El modelo sigue un esquema estrella con las siguientes tablas principales:

| Tabla | Tipo | Descripción |
|-------|------|-------------|
| `Fact_Ventas` | Hecho | Transacciones de venta con precio, costo, cantidad y descuento |
| `Fact_Metas_Ventas` | Hecho | Metas comerciales por vendedor y periodo |
| `Fact_Inventario` | Hecho | Stock por SKU con punto de reposición |
| `Fact_Oportunidad_Etapa` | Hecho | Pipeline comercial por etapa del funnel |
| `Dim_Fecha` | Dimensión | Calendario completo con jerarquías temporales |
| `Dim_Cliente` | Dimensión | Base de clientes con segmentación RFM pre-calculada |
| `Dim_Vendedor` | Dimensión | Fuerza de ventas |
| `Dim_Producto` | Dimensión | Catálogo de productos y categorías |
| `Dim_Categoria` | Dimensión | Categorías comerciales |
| `Dim_Mercado_Estimado_Valor` | Dimensión | Estimación de tamaño de mercado por categoría |
| `Dim_Acciones_RFM` | Dimensión | Playbook de acciones comerciales por segmento RFM |

---

*Documentación generada para el repositorio [dashboard-ejecutivo-360-powerbi](https://github.com/fabianquispec/dashboard-ejecutivo-360-powerbi)*
