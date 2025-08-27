
# MassBalanceMachine — Chile (Regionalización Climática)

**Resumen (ES)**  
Este repositorio documenta la adaptación y extensión de los flujos de _MassBalanceMachine_ a **Chile**, incorporando **regionalización climática**, **código para medición de performance** y un **script para balance de masa (distribuido)**. La base proviene de implementaciones para **Perú** y **Suiza**.

**Overview (EN)**  
This repository adapts and extends _MassBalanceMachine_ workflows to **Chile**, adding **climate regionalization**, **performance evaluation utilities**, and a **(distributed) mass-balance script**. The starting codebase was derived from **Peru** and **Switzerland** implementations.

---

## Créditos / Credits

- **Programación base (Perú/Swiss → Chile):**  
  Duilio Fonseca-Gallardo (UdeC) (Notebooks 1–4)  
  Javier Norambuena (INACH–UMAG), apoyo en Notebook 1

- **Equipo Chile — UdeC:**  
  Ilaria Tabone · David Farías Barahona

- **Equipo Chile — UMAG–INACH:**  
  Ricardo Jaña · Inti González

---

## Regionalización climática / Climate Regionalization

- **ES:** Se emplea una regionalización explícita (p. ej., *Desert Andes*, *Central Andes*, *North Patagonian Andes*, *South Patagonian Andes*) para **filtrar**, **entrenar** y **evaluar** modelos, y para comparaciones entre regiones.  
- **EN:** An explicit regionalization (e.g., *Desert Andes*, *Central Andes*, *North Patagonian Andes*, *South Patagonian Andes*) is used to **filter**, **train**, and **evaluate** models, enabling inter-regional comparison.

---

## Estructura del repositorio / Repository Structure

- `1_data_processing.ipynb` — Ingesta/limpieza, mapeo a RGI, features topográficas y climáticas, mensualización.  
- `2a_model_training_nn_region.ipynb` — Entrenamiento por región con NN (skorch/torch + MBM).  
- `2b_model_training_xgboost_region.ipynb` — Entrenamiento por región con XGBoost (Grid/Randomized Search).  
- `4_Distributed_mass_balance.ipynb` — Cálculo/visualización de balance de masa distribuido.  
- `README.md` — Este documento.  

---

## Requisitos / Requirements

- **Python** ≥ 3.10  
- **Paquetes / Packages:** `pandas`, `numpy`, `xarray`, `geopandas`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `torch`, `skorch`, `massbalancemachine`  
- **Datos / Data:**
  - **RGI Chile shapefile** (contornos/glacier outlines)  
  - **ERA5-Land** y **geopotential** (NetCDF) para Chile  
  - **CSV de estacas** con columnas mínimas: `POINT_ID`, `FROM_DATE`, `TO_DATE`, `POINT_LON`, `POINT_LAT`, `POINT_ELEVATION`, `POINT_BALANCE`

**Instalación rápida / Quick install**
- Crear entorno, instalar dependencias y `massbalancemachine` (vía `pip` o local editable).
- Asegurar proyección/CRS y nombres de variables compatibles (`valid_time → time`, etc.).

---

## Datos / Data Notes

- **ERA5-Land (mínimo):** `t2m`, `tp`, `slhf`, `sshf`, `ssrd`, `fal`, `str`  
- **RGI Chile:** para asignar **RGIId** a cada estaca  
- **CSV estacas:** fechas `YYYY-MM-DD`, coordenadas WGS84

> **Tip:** Descarga con **CDS API** y normaliza variables/dimensiones antes de extraer features.

---

## Uso / How to Run

1) **Procesamiento de datos** (`1_data_processing.ipynb`)  
   - Limpieza de fechas (→ `YEAR`, `yyyymmdd`), mapeo a **RGIId**, extracción de **topografía** y **clima**, conversión a **mensual**.

2) **Entrenamiento por región**  
   - **NN** (`2a_model_training_nn_region.ipynb`): arquitectura compacta (torch/skorch), normalización, CV (k-fold), búsqueda de hiperparámetros.  
   - **XGBoost** (`2b_model_training_xgboost_region.ipynb`): Grid/Randomized Search con CV, mejores hiperparámetros y métricas.

3) **Balance de masa distribuido** (`4_Distributed_mass_balance.ipynb`)  
   - Proyección espacial del modelo seleccionado y visualización de resultados.

---

## Métricas y performance / Metrics & Performance

- **Nivel fila / Row-level:** MAE, RMSE, r de Pearson; residuales vs. predicción.  
- **Nivel ID (agregado) / ID-level (aggregated):** promedio por `ID`; comparaciones por glaciar y por región.  
- **Reportes / Reports:** tablas CSV de métricas globales/por región, figuras (`.png`) de dispersión 1:1 y residuales.

---
