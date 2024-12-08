# STRUMENTI DI VERSIONAMENTO DEI DATI GEOGRAFICI

Gestire il flusso di lavoro e tenere traccia delle modifiche dei dati geospaziali con strumenti open source

[(C)2024 CC-BY-SA](https://creativecommons.org/licenses/by-sa/3.0/it/) Enrico Ferreguti enricofer@gmail.com

--

## Le tabelle non hanno memoria





--

## Perchè lavorare con le versioni

- per tenere traccia storica delle modifiche
- per spiegare le modifiche
- per fare delle prove sui dati esistenti
- per coordinare le modifiche su vari dataset
- per approvare un pacchetto di modifiche
- per gestire modifiche concorrenti
- per modificare in modo coordinato vari dataset

--

## Campi di applicazione

- Urbanistica
- Reti ed impianti
- Database topografici
- 

--

## Strategie di versionamento

- copia e rinomina
- frazionare i dati per temi e contesti
- taglia ed incolla

--

## Versionamento in altri ambiti operativi

- Database
- BIM/CAD
- Software
  - SVN
  - [GIT](https://git-scm.com/about/)



--

## Strumenti di versionamento opensource

- strumenti a livello di DB [POSTGRES](https://pgxn.org/search?q=version&in=extensions)

  

- o strumenti basati su [GIT](https://git-scm.com/about/)

  - https://geogig.org/

  - https://kartproject.org/

    


--

# Kart

https://kartproject.org/

Sviluppato da koordinates.com

Basato su git

Strumento a riga di comando per tutti le piattaforme

Imposta una workingcopy su geopackage o postgres in cui con opportuni trigger viene tenuta traccia delle modifiche effettuate



--

![](/home/enrico/dev/html/geospatial_versioning/doc/kart_features.png)

--

| INIZIALIZZAZIONE DEL REPOSITORY                            | MAIN               | RAMO A                      | RAMO B                      |
| ---------------------------------------------------------- | ------------------ | --------------------------- | --------------------------- |
| `kart init`<br />`kart create-workingcopy PATH.gpkg`<br /> |                    | ![](doc/mapplaceholder.svg) | ![](doc/mapplaceholder.svg) |
| `kart import <source> [<table>] [<table>]`<br />           | ![](doc/map01.svg) | ![](doc/mapplaceholder.svg) | ![](doc/mapplaceholder.svg) |



--

| CREAZIONE DI UN BRANCH               | MAIN               | RAMO A             | RAMO B                      |
| ------------------------------------ | ------------------ | ------------------ | --------------------------- |
| `kart checkout -b RAMO_A`<br />      | ![](doc/map01.svg) | ![](doc/map01.svg) | ![](doc/mapplaceholder.svg) |
| `kart commit -m "modifiche A"`<br /> | ![](doc/map01.svg) | ![](doc/map02.svg) | ![](doc/mapplaceholder.svg) |



--

| CREAZIONE DI UN BRANCH               | MAIN               | RAMO A             | RAMO B             |
| ------------------------------------ | ------------------ | ------------------ | ------------------ |
| `kart checkout -b RAMO_B`<br />      | ![](doc/map01.svg) | ![](doc/map02.svg) | ![](doc/map01.svg) |
| `kart commit -m "modifiche B"`<br /> | ![](doc/map01.svg) | ![](doc/map02.svg) | ![](doc/map03.svg) |



--

| COMANDI                   | MAIN               | RAMO A             | RAMO B             |
| ------------------------- | ------------------ | ------------------ | ------------------ |
| `kart merge RAMO_A`<br /> | ![](doc/map02.svg) | ![](doc/map02.svg) | ![](doc/map03.svg) |
| `kart merge RAMO_B`<br /> | ![](doc/map05.svg) | ![](doc/map02.svg) | ![](doc/map03.svg) |



--

| RISOLUZIONE DEI CONFLITTI                                    | MAIN                |                             | RAMO C              |
| ------------------------------------------------------------ | ------------------- | --------------------------- | ------------------- |
| `kart commit -m "modifiche B2"`<br />                        | ![](doc/map05.svg)  | ![](doc/mapplaceholder.svg) | ![](doc/map04.svg)  |
| `kart merge RAMO_C`<br />`kart conflicts`<br />`kart resolve`<br /> | ![](doc/map05c.svg) | ![](doc/mapplaceholder.svg) | ![](doc/map04c.svg) |



--

| Comandi principali       |      |      |
| ------------------------ | ---- | ---- |
| `kart init [<repository>]`<br />`kart init --workingcopy=PATH.gpkg`<br />`kart init --workingcopy=postgresql://USER:PASSWD@HOST/DBNAME/DBSCHEMA` | inizializzazione del repository con indicazione della workingcopy |      |
| `kart create-workingcopy PATH.gpkg` | creazione di una nuova workingcopy | |
| `kart import <source> [<table>] [<table>]` | importazione di dati geospaziali da sorgenti esterne |      |
| `kart commit -m "message"` | registrazione di un pacchetto di modifiche |      |
| `kart status` | per ottenere informazioni sul repository e sullo stato della workingcopy | |
| `kart log` | per ottenere la storia delle modifiche | |
| `kart diff` | per ottenere informazioni sulle differenze tra commit | |
| `kart restore` | per cancellare le modifiche nella workingcopy e riportarla allo stato registrato con l'ultimo commit | |
| `kart reset` | per annullare un pacchetto di modifiche | |

--

| comandi                            |                                                              |      |
| ---------------------------------- | ------------------------------------------------------------ | ---- |
| `kart checkout -b <branch>`        | Creare un ramo di modifica (branch) che raggruppa un insieme coordinato di commit |      |
| `kart checkout <branch>`           | Passare da un branch all'altro aggiornando la workingcopy con le relative modifiche |      |
| `kart merge <branch>` `            | Includere un branch "figlio" nel branch "padre"              |      |
| `kart conflicts`                   | Elencare i conflitti di merge                                |      |
| `kart resolve`                     | Risoluzione dei conflitti di merge                           |      |
| `kart status`                      | per ottenere informazioni sul repository e sullo stato della workingcopy |      |
| `kart log`                         | per ottenere la storia delle modifiche                       |      |
| `kart diff`                        | per ottenere informazioni sulle differenze tra commit        |      |
| `kart clone [url_repository]`</br> | Clonare un repository remoto per effettuare delle modifiche locali |      |
| `kart pull [remote] [branch]`      | Importare le modifiche da un repository remoto               |      |
| `kart push [remote] [branch]`      | Esportare le modifiche verso un repository remoto            |      |



--

# Kart QGIS plugin

Kart nasce come strumento a riga di comando. Per ovviare alla ridotta operatività e migliorare il controllo sui dati è stato sviluppato un plugin per QGIS https://github.com/koordinates/kart-qgis-plugin/blob/main/docs/index.md

![](doc/qgis_plugin.svg)

--

- il processo di modifica derivato da git consente di controllare ogni aspetto del versionamento (importazione dati/commit-restore/branching e merging)
- consente il versionamento distribuito
- 

--

## Il caso "Urbanistica"

- la pianificazione vigente viene servita per la visualizzazione (servizi cartografici) e per la stampa (produzione di pdf) e per analisi e monitoraggi e la catalogazione
- La modifica della pianificazione vigente è in capo al consiglio comunale che adotta un piano e approva le controdeduzioni alle osservazioni dei cittadini nell'ambito della partecipazione. E' quindi una procedura complessa discontinua ed intermittente in cui una variante è in lavorazione per lungo tempo parallelamente ad altre varianti
- La variante può coinvolgere uno o più dataset
- Per ogni variante deve essere possibile esaminare l'assetto complessivo e le variazioni puntuali effettuate
- Deve essere tenuta traccia delle variazioni effettuate
- Modifica, verifica, approvazione, monitoraggio devono poter essere svolti in modo semplice



# Djakart

#### A DJAngo wrapper to KART versioning tool   

La soluzione applicativa sviluppata per l'ufficio urbanistica del Comune di Padova consente di:

- Gestire versioni di dataset vettoriali usando repository kart, in modo completamente trasparente all'utente
- Organizzare i Dataset in modo Genitore/figlio a partire dal dataset originario
- Disporre delle informazioni in un ambiente multiutente con diritti di modifica/approvazione differenziati
- Proteggere il dataset originario non consentendo la modificabilità diretta se non attrattraverso la creazione di una versione.
- Centralizzare le workingcopy dei vari repository in schemi di un database Postgres
- Centralizzare la gestione delle versioni per facilitare il backup ed il controllo
- Creare un'interfaccia semplice ed amichevole per le principali operazioni di versionamento 
- Mette a disposizione la storia del dataset in modo semplice
- Verificare le modifiche ai dataset in modo visuali
- Risolvere i conflitti in modo visuale
- Usare QGIS per editare le modifiche, generando un progetto con le sorgenti dati correttamente pre-configurate con adeguata simbologia ottenuta da un template

--

## Deployment

Da un punto di vista strettamente tecnologico l'applicazione consiste in:

- una webapp sviluppata in django che funge da interfaccia a kart e che consente l'accesso multiutente con diritti di modifica/approvazione differenziati
- Un database postgresql che contiene le workingcopies dei repository amministrati da djakart
- un'istanza di QGIS usato in configurazione server, usato per la visualizzazione dei dataset e la generazione "al volo" dei progetti di QGIS preconfigurati e printi all'uso 
  - La funzionalità per la creazione "on the fly" del progetto di QGIS è implementata per mezzo di un plugin implementato specificatamente allo scopo, ma che potrebbe essere utile in altre casi d'uso: https://github.com/enricofer/djakart/tree/master/webapp/djakart/otf_qgisserver_plugin
- Orchestrazione delle varie componenti in container docker: https://github.com/enricofer/djakart/blob/master/docker-compose.yml

--

## Elenco Versioni

![](doc/elenco_versioni.png)

--

## Dettagli Versione

![](doc/versione_dettagli.png)

--

## Contronto tra versioni

![](doc/versione_confronto2.png)

--

## DIFF

![image-20241208133208673](doc/djakart_diff.png)

--

## Risoluzione dei conflitti

![](https://github.com/enricofer/djakart/raw/master/doc/screenshot09.png)

--

# DJAKART

#### https://github.com/enricofer/djakart





# .... Grazie per l'attenzione!