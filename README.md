# Analisi dei dati COVID-19

Analisi esplorativa in Python del dataset **COVID-19 di Our World in Data (OWID)**: distribuzione dei casi per continente, andamento dei contagi in Italia nel 2022 e confronto tra paesi europei su terapie intensive e ricoveri ospedalieri.

Il lavoro è contenuto in un unico notebook Jupyter: `ANALISI__COVID19_complete.ipynb`.

## Obiettivi

- Esplorare struttura e qualità del dataset (dimensioni, tipi di dato, valori nulli).
- Confrontare i casi di COVID-19 tra i continenti.
- Analizzare l'evoluzione dei contagi in Italia durante il 2022.
- Confrontare Italia, Francia e Germania sui pazienti in terapia intensiva.
- Confrontare Italia, Francia, Germania e Spagna sui pazienti ospedalizzati nel 2021, gestendo i dati mancanti in modo corretto.

## Dataset

- **Fonte:** [Our World in Data – COVID-19 dataset](https://github.com/owid/covid-19-data)
- **File richiesto:** `owid-covid-data.csv`
- Il CSV **non è incluso** nel repository: va scaricato e posizionato nella **stessa cartella del notebook**. Se il file non viene trovato, il notebook solleva un `FileNotFoundError` con un messaggio esplicativo.

Colonne principali utilizzate: `location`, `continent`, `date`, `total_cases`, `new_cases`, `icu_patients`, `hosp_patients`.

## Contenuto dell'analisi

### 1. Caricamento e panoramica dei dati
Lettura del CSV con controllo del percorso, poi `.info()` e `.shape` per individuare dimensioni del dataset e colonne con valori mancanti.

### 2. Casi per continente
Raggruppamento per `continent` sulla colonna `total_cases` e calcolo della ripartizione percentuale tra i continenti.

### 3. Italia – evoluzione dei contagi nel 2022
- Filtro sul solo anno 2022 e conversione della colonna `date` in formato datetime.
- In Italia i nuovi casi sono registrati su base settimanale: la maggior parte dei giorni ha quindi `new_cases == 0` senza una misurazione reale. Questi giorni vengono esclusi, mantenendo solo quelli con un dato effettivo.
- Grafici dell'evoluzione dei **casi totali** e dei **nuovi casi** nel tempo.

### 4. Terapie intensive: Germania, Francia, Italia
- Periodo analizzato: **maggio 2022 – aprile 2023**.
- Box plot di `icu_patients` per paese (quartili, mediana, valori anomali).
- Dal confronto delle mediane emerge che Germania e Francia hanno valori più alti dell'Italia, con la Germania di poco sopra la Francia.

### 5. Ospedalizzazioni: Italia, Germania, Francia, Spagna (2021)
- Aggregazione con `sum(min_count=1)` per preservare i `NaN` invece di trasformarli in zeri.
- Dal controllo dei nulli emerge che la **Germania non ha alcun dato** di `hosp_patients` per tutto il 2021.
- **Gestione dei dati mancanti:** i valori non vengono sostituiti (né con 0, né con media o interpolazione), perché non esiste alcun dato reale su cui basare una stima e una sostituzione falserebbe il confronto. I `NaN` restano tali e vengono semplicemente non tracciati nei grafici.
- Grafico a linee dei pazienti ospedalizzati per paese.

## Requisiti

- Python 3.9+
- Librerie:
  - `pandas`
  - `matplotlib`
  - `seaborn`
  - `jupyter` (o JupyterLab)

## Installazione ed esecuzione

```bash
# 1. Clona il repository
git clone https://github.com/tonyanto99/<NOME-REPOSITORY>.git
cd <NOME-CARTELLA>

# 2. (Opzionale) crea un ambiente virtuale
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Installa le dipendenze
pip install pandas matplotlib seaborn jupyter

# 4. Scarica owid-covid-data.csv e mettilo accanto al notebook

# 5. Avvia il notebook
jupyter notebook ANALISI__COVID19_complete.ipynb
```

## Struttura del progetto

```
.
├── ANALISI__COVID19_complete.ipynb   # notebook con l'intera analisi
├── owid-covid-data.csv               # dataset (da scaricare, non incluso)
└── README.md
```

## Note e limiti

- La disponibilità dei dati su ricoveri e terapie intensive varia molto da paese a paese, perché OWID raccoglie dati da fonti nazionali diverse. Alcuni paesi riportano i dati su base settimanale, altri hanno interruzioni o cambi di fonte, che generano "buchi" nelle serie storiche.
- I confronti tra paesi vanno quindi letti tenendo conto di queste differenze di reporting.

## Autore

**Antonio Antonelli** – GitHub: [@tonyanto99](https://github.com/tonyanto99)

## Licenza

<Indica qui la licenza del progetto, ad esempio MIT>. I dati appartengono a Our World in Data e sono soggetti alla loro licenza.
