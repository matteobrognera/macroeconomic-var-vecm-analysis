# Macroeconomic Time Series Analysis: Oil Price, PPI, and CPI (1970–2025)

Progetto di analisi econometrica e modellazione delle serie storiche multivariate focalizzato sulle dinamiche e sulle interdipendenze tra il prezzo del petrolio greggio (WTI), l'Indice dei Prezzi alla Produzione (PPI) e l'Indice dei Prezzi al Consumo (CPI) negli Stati Uniti.

## 📊 Dataset Overview

Il dataset utilizzato (`data/serie_economici_fred.xlsx`) contiene **672 osservazioni mensili** dal **gennaio 1970 al dicembre 2025**, estratte tramite API dal database FRED (Federal Reserve Economic Data).

| Variabile | Colonna Dataset | Descrizione | Codice FRED |
| :--- | :--- | :--- | :--- |
| **CPI** | `prezzi_consumo_cpi` | Consumer Price Index for All Urban Consumers | `CPIAUCSL` |
| **Petrolio WTI** | `prezzo_petrolio` | Crude Oil Prices: West Texas Intermediate | `WTISPLC` |
| **PPI** | `prezzi_produzione_ppi` | Producer Price Index by Commodity: Total Finished Goods | `PPIACO` |

---

## 🛠️ Metodologia Econometrica

L'analisi segue un flusso rigoroso per identificare le relazioni di breve e lungo periodo:

1. **Analisi Esplorativa & Stazionarietà**:
   * Trasformazione logaritmica e differenziazione prima per stabilizzare media e varianza ($I(1)$).
   * Analisi di autocorrelazione (ACF) e correlazione incrociata (CCF).
2. **Modelli Transfer Function (TF)**:
   * Procedura di pre-whitening della variabile di input (petrolio) tramite modelli AR(p).
   * Identificazione della risposta simultanea tra le componenti di costo e inflazione.
3. **Vector Autoregression (VAR)**:
   * Selezione del ritardo ottimale tramite criteri di informazione (AIC, HQ, SC) $\rightarrow$ modello $VAR(12)$.
   * Diagnostica residuale: test di Ljung-Box, Jarque-Bera, Portmanteau e ARCH multivariato.
   * Analisi di Causalità di Granger e Causalità Istantanea.
   * Funzioni di Risposta all'Impulso (IRF) e Decodifica della Varianza dell'Errore di Previsione (FEVD).
   * Validation out-of-sample (rolling window a 24 mesi) e confronto predittivo con modelli ARIMA univariati via Diebold-Mariano test.
4. **Cointegrazione & Modelli VECM**:
   * Test di radice unitaria ADF sui livelli e sulle differenze prime.
   * Test di Cointegrazione di Johansen per l'individuazione di relazioni di equilibrio di lungo periodo ($r=2$).
   * Stima del Vector Error Correction Model (VECM) e analisi dei coefficienti di aggiustamento $\alpha$ e $\beta$.

---

## 📂 Struttura del Progetto

```text
├── README.md                  # Descrizione generale e guida al progetto
├── data/
│   └── serie_economici_fred.xlsx # Dataset originale (FRED)
├── R/
│   ├── 01_data_download.R     # Script per download e preprocessing dati
│   ├── 02_exploratory_ccf.R   # Analisi esplorativa e trasformazioni log-diff
│   ├── 03_transfer_function.R # Modelli Transfer Function e pre-whitening
│   ├── 04_var_analysis.R      # Stima VAR, test diagnostici, IRF, FEVD e forecasting
│   └── 05_cointegration_vecm.R# Test di Johansen e stima modello VECM
└── reports/
    └── report_finale.pdf      # Report tecnico completo con grafici e tabelle
