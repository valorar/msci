# MSCI World vs Emerging Markets — Estudio de Evolución y Rebalanceo de Carteras

Este repositorio contiene datos históricos diarios de los índices **MSCI World** y **MSCI Emerging Markets** para analizar:

- 📈 La evolución comparada de mercados desarrollados vs emergentes
- ⚖️ Estrategias de rebalanceo de carteras entre ambos índices
- 📊 Visualizaciones y backtesting de distintas asignaciones de activos

## 📁 Datos

- **Archivo:** [`msci_world_em_historical.csv`](msci_world_em_historical.csv)
- **Rango:** 2009-01-02 → 2026-07-02
- **Filas:** 4,319 días de cotización
- **Columnas:** `Date`, `MSCI_World`, `MSCI_EM`
- **Frecuencia:** Diaria (precio de cierre ajustado)

## 🔍 Cómo se obtuvieron los datos

Los índices MSCI puros (Net Total Return) no están disponibles gratuitamente en Yahoo Finance. Se utilizaron ETFs de réplica física con tracking error mínimo como proxies:

| Índice | ETF utilizado | Ticker Yahoo Finance | Tipo | Tracking Error |
|--------|--------------|---------------------|------|----------------|
| MSCI World NR USD | iShares MSCI World UCITS ETF | `IWRD.L` | Réplica física, USD (Londres) | < 0.50% |
| MSCI Emerging Markets NR USD | iShares MSCI Emerging Markets ETF | `EEM` | Réplica física, USD (NYSE) | ~ 0.70% |

### Proceso de descarga

1. Se usó [`yfinance`](https://github.com/ranaroussi/yfinance) con `auto_adjust=True` (ajusta splits y dividendos)
2. Se descargó el histórico máximo disponible de cada ETF
3. Se alinearon las fechas tomando la intersección de ambos rangos (inner join)
4. Las filas sin datos en alguno de los dos índices se eliminaron

### ¿Por qué desde 2009?

- **EEM** (Emerging Markets): disponible desde **2003**
- **IWRD.L** (MSCI World): disponible desde **enero 2009**

El rango está limitado por el ETF de MSCI World más antiguo disponible en Yahoo Finance. Se descartaron alternativas como:
- `^990100` / `^891800` (índices MSCI puros): ❌ no disponibles en Yahoo Finance
- `URTH` (iShares MSCI World USA): solo desde 2012
- `MWOIX` (MFS Global Growth): fondo de gestión activa, no sigue el índice

### ⚠️ Nota sobre las escalas

Los valores absolutos de `MSCI_World` (~7,700) y `MSCI_EM` (~65) **no son comparables directamente**:
- `IWRD.L` refleja puntos/valor liquidativo del índice
- `EEM` es precio por acción del ETF

Para análisis y gráficos se recomienda **normalizar ambos a base 100** en la fecha inicial o trabajar con **variaciones porcentuales**.

## 🚀 Próximos pasos

- [ ] Análisis exploratorio: rentabilidades, volatilidad, correlación
- [ ] Backtesting de carteras con distintas asignaciones (60/40, 70/30, etc.)
- [ ] Simulación de rebalanceo periódico (anual, semestral, trimestral)
- [ ] Comparación con cartera 100% MSCI World

## 🛠 Herramientas

Descarga automatizada con [Hermes Agent](https://github.com/nousresearch/hermes-agent) + `yfinance`. Los datos se actualizarán periódicamente.
