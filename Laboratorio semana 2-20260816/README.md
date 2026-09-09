# Restaurant Analytics — Semana 2 (Lab #1)

Business Data Analytics Applied Lab #1 — asignatura Data Analytics (43390860).

## Objetivo

Consolidar cuatro fuentes independientes (catálogo de productos, base de clientes y dos semanas de ventas), validar las uniones y construir indicadores de **ingresos**, **frecuencia de compra** y **recurrencia** que apoyen decisiones sobre menú y fidelización.

> **Alcance.** Los archivos contienen precios de venta pero **no costos, cantidades ni descuentos**. El análisis no habla de rentabilidad, utilidad ni margen.

## Datos

| Archivo | Contenido | Clave |
|---|---|---|
| `Restaurant-Foods.csv` | Catálogo de productos y precios | `Food ID` (PK) |
| `Restaurant-Customers.csv` | Base de clientes (no se publica) | `ID` (PK) |
| `Restaurant-Week1-Sales.csv` | 250 registros Semana 1 | `Customer ID`, `Food ID` (FK) |
| `Restaurant-Week2-Sales.csv` | 250 registros Semana 2 | `Customer ID`, `Food ID` (FK) |

Modelo relacional: `CUSTOMERS 1─N SALES N─1 FOODS`.

## Estructura del repositorio

```text
Laboratorio semana 2/
├── data/                              # (opcional) mover aquí los CSV
├── Restaurant-Foods.csv
├── Restaurant-Customers.csv           # NO publicar (contiene nombres)
├── Restaurant-Week1-Sales.csv
├── Restaurant-Week2-Sales.csv
├── lab_Sem2_DA_20261.ipynb            # notebook ejecutado (evidencia)
├── figuras/                           # PNGs usados por el informe
│   ├── fig_menu.png
│   ├── fig_semanal.png
│   └── fig_recurrencia_ocupacion.png
├── Informe_Ejecutivo_Restaurante.tex  # informe ejecutivo (LaTeX)
└── README.md
```

## Instrucciones de ejecución

1. **Requisitos:** Python 3.10+ con `pandas`, `matplotlib`, `sqlite3` (stdlib), `jupyter`.
   ```bash
   pip install pandas matplotlib jupyter
   ```
2. **Notebook.** Abrir `lab_Sem2_DA_20261.ipynb` y ejecutar todas las celdas de arriba hacia abajo. El notebook está diseñado para ser reproducible sin intervención manual (los CSV deben estar en la misma carpeta que el `.ipynb`).
3. **Informe LaTeX.** Compilar `Informe_Ejecutivo_Restaurante.tex` con `pdflatex`:
   ```bash
   pdflatex Informe_Ejecutivo_Restaurante.tex
   ```
   Las figuras están embebidas desde `figuras/`.

## Pipeline (resumen)

1. **Carga y etiquetado** de la semana en cada archivo de ventas.
2. **Validación** con `assert`: unicidad de PKs (`Food ID`, `ID`) e integridad referencial de FKs (`Food ID`, `Customer ID`).
3. **Consolidación** con dos `merge(how='left', validate='many_to_one')`. Se verifica que el número de filas no cambie y que no queden nulos en columnas esenciales (`Price`, `Occupation`).
4. **KPIs**: ingresos totales, resumen semanal (registros, ingresos, ticket promedio), top de productos por frecuencia e ingresos, clientes recurrentes, gasto agregado por ocupación.
5. **Contraste con SQL** (SQLite en memoria) para validar el resumen semanal.
6. **Visualizaciones**: menú (frecuencia vs ingresos), ingresos por semana y cohortes de clientes / ocupaciones.

## Hallazgos principales

- **Ingresos totales:** \$3.886,56 USD en 500 registros (250 por semana).
- **Variación semanal:** −\$38,80 (−1,98%) entre Semana 1 y Semana 2; volumen idéntico.
- **Producto ancla:** *Steak* concentra ~32% de los ingresos (\$1.249,50) con 50 registros.
- **Producto de mayor frecuencia:** *Drink* (59 registros, \$103,25). Frecuencia e ingresos **no coinciden**.
- **Recurrencia:** 46 de 221 clientes de la Semana 1 volvieron en la Semana 2 → **20,8%**.
- **Top ocupaciones (ingresos agregados):** Compensation Analyst, Sales Representative, Marketing Manager, Cost Accountant, Assistant Media Planner.

## Privacidad

- No se publican nombres de clientes.
- Si el repositorio se hace público, mover `Restaurant-Customers.csv` fuera del repo o añadirlo a `.gitignore`.

## Limitaciones

- No hay costos, cantidades ni descuentos → no se puede hablar de margen ni rentabilidad.
- Ventana de solo 2 semanas → no se pueden inferir tendencias ni causalidad.
- No hay marca temporal (día/hora) ni canal (dine-in / delivery / take-away).
- No hay identificador de ticket → cada fila es un ítem, no una cuenta.
