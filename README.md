# Dashboard Ejecutivo para Dirección 360° | Power BI

![Dirección General](assets/direccion-general.jpeg)

> **Más que un reporte: una herramienta para decidir mejor.**

---

## 📌 Contexto del problema

En la mayoría de organizaciones, la información comercial está fragmentada. Ventas, clientes, forecast, rentabilidad, inventarios y mercado operan en silos: cada área analiza sus propios KPIs, pero Dirección termina recibiendo múltiples reportes aislados que muestran **qué pasó**, sin explicar dónde actuar, qué está en riesgo o qué decisiones priorizar.

Este proyecto nació de esa observación: las empresas tienen muchos reportes, pero pocas herramientas reales para decidir.

---

## 🎯 Objetivo

Transformar datos operativos en señales ejecutivas claras — centralizando estrategia, operación y acción comercial dentro de un solo modelo analítico orientado a Dirección.

---

## 🗂️ Estructura del dashboard

El modelo integra **10 páginas analíticas** conectadas bajo una lógica ejecutiva única:

| Página | Propósito |
|--------|-----------|
| **Dirección General** | KPIs rectores, semáforo ejecutivo e insight automático del negocio |
| **Performance Comercial** | Desempeño de ventas por canal, categoría, marca y región |
| **Forecast** | Proyección de cierre, brechas vs meta y ritmo de ejecución |
| **Funnel Comercial** | Conversión por etapa y productividad de la fuerza de ventas |
| **Clientes RFM** | Segmentación por Recencia, Frecuencia y Monto — riesgo de fuga |
| **Rentabilidad y Precio** | Margen por categoría, productividad comercial y análisis de pricing |
| **Operación e Inventarios** | Salud operativa, cobertura de stock y rotación de inventario |
| **Mercado y Competencia** | Posición competitiva, participación de mercado y benchmarking |
| **Alertas de Ejecución** | Semáforos de riesgo, desviaciones críticas y prioridades de acción |
| **Vista Ejecutiva Clientes RFM** | Resumen estratégico de la base de clientes para dirección |

---

## 📸 Vistas del dashboard

---

### Dirección General
![Dirección General](assets/direccion-general.jpeg)

> *El negocio vende $4,755,651 con un margen saludable (32.6%), pero no alcanza la meta (87.9%) y presenta crecimiento negativo (-1.0%). No hay clientes VIP en riesgo. La ejecución requiere atención.*

**Insight 1 — El negocio opera con margen sólido pero incumple la meta comercial**
El KPI Rector Ejecutivo se ubica en 92.5 sobre 100, con ventas totales de $5M y un margen bruto del 33%. Sin embargo, el cumplimiento de meta alcanza solo el 88%, lo que implica una brecha activa de $720K sin cerrar frente al objetivo de $6M. El semáforo ejecutivo clasifica el estado como *Controlado*, pero la brecha es material y requiere gestión activa antes del cierre del período.

**Insight 2 — El crecimiento YoY es negativo y la curva mensual muestra volatilidad sin tendencia clara**
El crecimiento año contra año se sitúa en -1%, con una evolución mensual que oscila entre $361K (febrero) y $415K (noviembre) sin una tendencia sostenida al alza. La comparación contra el año anterior (línea punteada) evidencia que el negocio está por debajo de su propio ritmo histórico en la mayoría de los meses, lo que descarta la hipótesis de estacionalidad favorable.

**Insight 3 — Ropa concentra el 42% del revenue total con un margen idéntico al resto de categorías**
La distribución por categoría muestra que Ropa genera el 42% de las ventas, seguida de Calzado (32%) y Accesorios (26%). Los tres segmentos operan con un margen bruto uniforme del 33%, lo que indica que la palanca de crecimiento es volumétrica, no de precio ni de mix de producto.

**Recomendación prioritaria**
Activar un plan de cierre enfocado en la categoría Ropa — mayor volumen, mismo margen — para reducir la brecha de $720K vs meta. La uniformidad de márgenes descarta cambios de pricing como palanca inmediata; el foco debe estar en conversión y activación comercial.

---

### Performance Comercial
![Performance Comercial](assets/performance-comercial-1.jpeg)

> *La ejecución comercial alcanza solo el 87.9% de la meta, mostrando una brecha relevante del 12.1% que requiere acciones correctivas prioritarias.*

**Insight 1 — La ejecución comercial acumula una brecha del 12.1% frente a meta con doble señal negativa**
Las ventas totales de $5M conviven con un crecimiento MoM de -0.2% y un YoY de -1%, configurando una situación de doble desaceleración: el negocio no solo está por debajo de la meta anual, sino que también pierde ritmo respecto al mes anterior. El ticket promedio de $160 se mantiene estable, descartando presión de precios como causa de la caída.

**Insight 2 — La caída MoM está concentrada en Ropa, la categoría de mayor peso**
El panel de ventas por categoría activa la alerta "Atención: caída MoM" sobre Ropa, que a pesar de liderar en volumen absoluto ($2M) registra la mayor desaceleración mensual. Calzado y Accesorios también se ubican en niveles similares (~$2M y $1M respectivamente), con las tres marcas (A, B, C) distribuyendo el volumen de forma relativamente uniforme dentro de cada categoría.

**Insight 3 — La dispersión margen-volumen por categoría no muestra ventaja competitiva diferenciada**
El scatter de Margen Bruto % vs Ventas Totales posiciona a Ropa ($2M, 32.55%), Calzado ($1.5M, 32.55%) y Accesorios ($1.2M, 32.50%) en un rango de margen casi idéntico. Esta convergencia indica que no existe una categoría de alto margen que pueda compensar caídas de volumen en las demás — el negocio depende estructuralmente de mantener el volumen en todas las líneas simultáneamente.

**Recomendación prioritaria**
Investigar las causas de la caída MoM en Ropa a nivel de marca y región antes de escalar acciones comerciales. Dado que Ropa representa el 42% del revenue y su margen es equivalente al resto, cualquier recuperación en esta categoría tiene impacto directo y desproporcionado sobre el cumplimiento de meta.

---

### Análisis RFM de Clientes
![Clientes RFM](assets/analisis-rfm-clientes.jpeg)

> *Existen 35 clientes potenciales con oportunidad de crecimiento.*

**Insight 1 — La base activa de 200 clientes concentra un CLV promedio de $780K con 27% de ventas generadas por el segmento VIP**
Los KPIs de cabecera revelan una base de alto valor unitario: $780K de Customer Lifetime Value promedio con 0 clientes VIP actualmente en riesgo. Sin embargo, el 27% de las ventas totales dependiendo del segmento VIP implica una concentración relevante: perder uno o dos clientes de ese segmento tendría impacto material sobre el revenue.

**Insight 2 — Los Clientes en Riesgo y Clientes Perdidos acumulan más de $2M en ventas históricas combinadas**
El gráfico de ventas por segmento RFM muestra que Clientes VIP lideran con $1.28M, pero Clientes en Riesgo ($859K) y Clientes Perdidos ($809K) suman $1.67M en valor que el negocio ya generó y está en proceso de perder o ha perdido. La tabla de prioridad comercial confirma que clientes como el 185 (Perdido, $35.7K en ventas) siguen siendo recuperables por su volumen histórico.

**Insight 3 — Los Clientes Potenciales representan la mayor oportunidad de crecimiento orgánico no activada**
Con 35 clientes potenciales identificados y un CLV promedio cercano al general ($1.04M–$1.24M según la tabla), este segmento es la palanca de crecimiento más inmediata: ya tienen historial de compra relevante, no están en riesgo de churn y su recencia promedio (369 días) sugiere margen para reactivación con acciones dirigidas.

**Recomendación prioritaria**
Diseñar dos tracks de acción diferenciados: reactivación urgente para Clientes en Riesgo con recencia superior a 370 días y CLV alto (ejecutivos senior asignados), y plan de desarrollo para los 35 Clientes Potenciales mediante ofertas de up-sell por categoría — Accesorios muestra la mayor participación en ese segmento.

---

### Funnel de Ventas
![Funnel de Ventas](assets/funnel-de-ventas.jpeg)

> *Embudo saludable: la conversión global alcanza 45%, con niveles controlados de pérdida de oportunidades.*

**Insight 1 — El funnel muestra una tasa de conversión global del 45% con pérdida estructural concentrada en la etapa final**
De 248 leads iniciales, 112 oportunidades se ganan (62.22%) frente a 68 perdidas (37.78%), resultando en un Win Rate del 62% y una conversión de negociación a ganada del 55%. La caída más significativa del embudo ocurre entre Propuesta (217) y Negociación (202) — 15 oportunidades — y entre Negociación y Ganada (90 oportunidades), siendo esta última la etapa de mayor fricción comercial.

**Insight 2 — El ciclo de venta promedio de 45 días presenta picos recurrentes que sugieren estacionalidad en el proceso comercial**
La evolución mensual del ciclo de venta oscila entre 41 días (mínimo en febrero y julio) y 49 días (máximo en enero), con una variación de 8 días que se repite de forma cíclica a lo largo del año. Esta regularidad indica que los picos no son aleatorios sino estructurales — probablemente relacionados con cierres de período, vacaciones o ciclos de aprobación del cliente.

**Insight 3 — El pipeline ponderado evidencia sobre-valoración en etapas tempranas**
El análisis de pipeline por etapa muestra que Lead tiene un monto pipeline del 83.33% pero un pipeline ponderado de solo 28.57% — una brecha de 55 puntos porcentuales. Calificado (71.43% vs 28.57%) y Propuesta (62.50% vs 37.50%) replican el patrón. Esto indica que el equipo comercial puede estar registrando probabilidades de cierre infladas en etapas tempranas, lo que distorsiona el forecast.

**Recomendación prioritaria**
Intervenir en la etapa Negociación → Ganada con revisión de las 90 oportunidades perdidas: identificar los patrones comunes de pérdida (precio, competencia, timing) y ajustar el proceso de calificación temprana para reducir el ingreso de oportunidades con baja probabilidad real de cierre.

---

### Mercado y Competencia
![Mercado y Competencia](assets/mercado-y-competencia.jpeg)

> *Alerta competitiva: el crecimiento está por debajo del mercado (-5.0 pp). Participación actual: 31.9%. Enfocar acciones en Ropa.*

**Insight 1 — El negocio opera con alerta crítica de mercado: crece 5 puntos porcentuales por debajo del sector**
La brecha de crecimiento vs mercado de -5pp activa el semáforo en rojo (Crítica), con un share de mercado del 32% sobre un valor de mercado total de $15M. La brecha en términos absolutos es de $10.17M — lo que el negocio no captura del mercado disponible. Esta señal es la más grave del dashboard: no se trata de una caída interna, sino de pérdida de posición competitiva relativa.

**Insight 2 — Ropa lidera en ventas absolutas pero acumula la mayor brecha de mercado**
La Matriz Ejecutiva revela que Ropa genera $2M en ventas (32% share) pero enfrenta una brecha de mercado de $4.29M y una desviación de -5pp vs crecimiento del mercado. Calzado sigue con $1.52M y brecha de $3.72M (-8pp). Accesorios es la categoría con mejor posición relativa (36% share, -1pp), siendo el único segmento con señal menos crítica en el BCG de mercado.

**Insight 3 — El BCG de mercado posiciona a Calzado como la categoría con mayor deterioro competitivo**
El scatter Oportunidad vs Desempeño (BCG de mercado) ubica a Calzado en la zona de mayor riesgo: alto share de mercado (10%) combinado con la mayor brecha de crecimiento negativa (-8pp). Ropa aparece en el cuadrante superior derecho con alto share (13%) pero también con brecha negativa (-5pp). Solo Accesorios muestra una posición más defensible con menor deterioro relativo.

**Recomendación prioritaria**
Priorizar acciones comerciales y de pricing en Accesorios — única categoría con brecha competitiva contenida — para consolidar posición antes de abordar la recuperación en Ropa y Calzado. En paralelo, investigar qué está haciendo el mercado en Calzado para crecer mientras el negocio pierde terreno a -8pp.

---

### Rentabilidad y Precio
![Rentabilidad y Precio](assets/rentabilidad-y-precio.jpeg)

> *La rentabilidad es sólida. El margen bruto se encuentra 2.6% por encima del nivel objetivo.*

**Insight 1 — El margen bruto del 33% es uniforme entre categorías y se sitúa 2.6pp sobre el objetivo, pero el margen neto estimado se reduce a $1M**
El negocio genera $2M de margen bruto sobre $5M de ventas (33%), distribuido de forma idéntica entre Ropa, Calzado y Accesorios (33% cada uno). Sin embargo, el margen neto estimado cae a $1M — una compresión del 50% que evidencia una estructura de costos operativos relevante no visible en el margen bruto. El descuento promedio del 7% es un factor que contribuye a esta compresión.

**Insight 2 — La elasticidad precio de -2 en Ropa indica sensibilidad moderada que limita el uso de pricing como palanca de crecimiento**
El análisis ejecutivo por producto muestra que Ropa tiene una elasticidad de -2: un incremento de precio del 1% reduce la demanda en 2%. Calzado comparte el mismo coeficiente (-2) mientras que Accesorios presenta elasticidad positiva (+3), un resultado atípico que sugiere que en ese segmento los incrementos de precio no deprimen el volumen — o que existe un error de medición que merece revisión.

**Insight 3 — La variación precio es prácticamente nula en todas las categorías, descartando el pricing como causa del crecimiento negativo**
El scatter de Variación Precio vs Variación Cantidad muestra variaciones de precio cercanas a 0.00–0.01 en todas las categorías, con variaciones de cantidad negativas en Ropa y Calzado (-0.01). Esto confirma que la caída de ventas no está siendo impulsada por subidas de precio, sino por reducción de volumen — lo que desplaza el problema hacia la demanda, la distribución o la posición competitiva.

**Recomendación prioritaria**
Revisar la estructura de costos que comprime el margen bruto del 33% a un margen neto del ~21%. La palanca de pricing está limitada por elasticidades desfavorables en las categorías de mayor peso; el camino más viable para mejorar rentabilidad neta es reducción de costos operativos y optimización del descuento promedio (actualmente en 7%).

---

### Operación e Inventario
![Operación e Inventario](assets/operacion-e-inventario.jpeg)

> *Los quiebres de stock están generando pérdidas estimadas de $9,919. Se recomienda priorizar reposición.*

**Insight 1 — El inventario opera con salud del 96% pero los quiebres de stock activos generan pérdidas concretas de $9,919**
El Índice de Salud de Inventario del 96% es positivo, con una rotación de 48 veces y solo 8 días de inventario en mano. Sin embargo, la tabla de impacto por quiebre revela pérdidas estimadas de $9,919 concentradas en Producto 10 ($3,248), Producto 13 ($2,090) y Producto 12 ($947). Estos tres productos representan el grueso del impacto y todos tienen DOI de 5 días — al límite del umbral operativo.

**Insight 2 — La rotación de inventario es consistente entre categorías pero Ropa lidera con 53 vueltas anuales**
El gráfico de rotación por categoría muestra a Ropa en 53, Calzado en 46 y Accesorios en 45. La mayor rotación de Ropa es coherente con su liderazgo en ventas (42% del revenue), pero también significa que el margen de error ante quiebres es menor: con 53 vueltas anuales, un retraso de reposición de días se traduce en pérdida de venta inmediata.

**Insight 3 — El stock total se mantiene estable entre 7.4K y 8.9K unidades mensuales pero las unidades vendidas muestran variabilidad que no está siendo absorbida por el inventario**
La evolución mensual muestra que el stock total oscila sin una lógica clara de anticipación a la demanda: en julio (mínimo de stock: 7.4K) coincide con un nivel de ventas de 2.5K unidades, sin señal de que el inventario haya sido pre-posicionado para absorber picos. Esta desconexión entre stock disponible y patrones de demanda es el origen estructural de los quiebres.

**Recomendación prioritaria**
Implementar reposición inmediata para Producto 10, 13 y 12 — responsables del 63% de las pérdidas por quiebre. En paralelo, revisar el modelo de reabastecimiento para Ropa: con 53 rotaciones anuales y DOI de 8 días, el punto de reorden actual no está generando alertas con suficiente anticipación.

---

### Alertas de Ejecución
![Alertas de Ejecución](assets/alertas-de-ejecucion.jpeg)

> *No se detectan alertas críticas en el negocio.*

**Insight 1 — El nivel de alerta ejecutiva es "Atención" con 15 quiebres de stock como señal operativa más urgente**
El panel de alertas ejecutivas clasifica el estado general como *Atención* — un escalón por debajo de Crítico. La única alerta activa con valor cuantificado es la de quiebre de stock: 15 quiebres con pérdidas estimadas de $9,919. Las alertas de incumplimiento de meta, clientes VIP en riesgo, vendedores bajo meta y alerta de margen no muestran valores activos en el período filtrado (2025), lo que indica que las variables comerciales y de clientes están dentro de parámetros aceptables.

**Insight 2 — La totalidad de clientes con alerta de churn pertenece al segmento "Clientes en Riesgo" con scores RFM deteriorados en los últimos 6 meses**
La tabla de Clientes con Alerta de Churn muestra que todos los registros presentan el patrón `RFM 6M < RFM 12M | Alerta de churn`, lo que significa que su comportamiento de compra reciente es inferior a su comportamiento histórico anual. El Cuadro de Acción Comercial Estratégico asigna a todos acción de "Oferta de reactivación + contacto proactivo" con prioridad Crítica y responsable Ejecutivo Senior — indicando que este segmento ya fue identificado y escalado.

**Insight 3 — El CLV de los clientes en alerta de churn oscila entre $491 y $1,013, representando un riesgo de revenue material si no se activa la retención**
Los valores de CLV visibles en la tabla del Cuadro de Acción oscilan ampliamente: desde $491 (cliente 19) hasta $1,013 (cliente 106). La suma parcial visible asciende a $773 de CLV promedio en el segmento. Dado que estos clientes ya fueron compras reales con tickets históricos altos, su pérdida no es solo de revenue futuro sino de base instalada consolidada.

**Recomendación prioritaria**
Ejecutar el plan de contacto proactivo asignado a los Ejecutivos Senior en un plazo máximo de 5 días hábiles, priorizando los clientes con mayor CLV (por encima de $900). La alerta ya está generada y la acción está definida — el riesgo en este punto es la demora en la ejecución, no la falta de información.

---

## 🔍 Diferencial analítico

El dashboard no se limita a describir resultados — explica contexto, riesgo y prioridad de acción:

- **Brecha de meta:** cuánto falta para cerrar el objetivo y cuántos clientes u oportunidades se necesitan para cubrirlo
- **Pérdida de ritmo:** detección de desaceleración frente al mercado antes de que impacte el resultado final
- **Riesgo de fuga:** identificación de clientes VIP en riesgo mediante segmentación RFM automatizada
- **Priorización comercial:** categorías ordenadas por impacto económico y desempeño competitivo
- **Narrativa automática:** cada página genera un insight ejecutivo en lenguaje natural orientado a decisión

---

## ⚙️ Stack tecnológico

| Herramienta | Uso |
|-------------|-----|
| **Power BI Desktop** | Desarrollo del modelo y visualizaciones |
| **DAX** | Medidas avanzadas, métricas compuestas y lógica de semaforización |
| **Power Query (M)** | ETL: limpieza, transformación e integración de fuentes |
| **Modelado dimensional** | Esquema estrella para rendimiento analítico óptimo |
| **CSV** | Fuente de datos simulada con escenarios de negocio reales |

---

## 💡 Valor para el negocio

| Beneficio | Impacto |
|-----------|---------|
| Centralización de información crítica | Una sola vista para toda la dirección comercial |
| Aceleración de decisiones | De horas de análisis manual a segundos de lectura ejecutiva |
| Visibilidad de riesgos | Alertas tempranas antes de que los problemas escalen |
| Priorización comercial | Foco en lo que más impacta el resultado del negocio |
| Reducción de reportes dispersos | Un modelo reemplaza múltiples informes aislados |

---

## 👤 Autor

**Fabian Quispe Castillo**  
Analista de Business Intelligence | Lima, Perú

[![LinkedIn](https://img.shields.io/badge/LinkedIn-fabianquispec-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/fabianquispec)

---

> *Este proyecto representa una propuesta de Business Intelligence orientada a gestión y dirección comercial. Su propósito no es mostrar más métricas — sino ayudar a decidir mejor.*
