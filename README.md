# ProgettoEsame
Il presente progetto riguarda la gestione di dati economici di diversi paesi europei negli anni dal 2007 al 2016. Esso è realizzato in linguaggio Java e prevede il download di un dataset di riferimento, la memorizzazione dei dati in formato JSON e la possibilità di richiedere, tramite server locale, i dati stessi, i metadati, le statistiche sui dati.

Le classi di cui si compone il progetto sono suddivise nei seguenti package secondo un criterio logico.

 - `com.esame.progetto`;
 - `dataset`;
 - `operazioni`;

## Funzionamento
All'avvio dell'applicazione viene eseguito il download del dataset di riferimento, rintracciato all'interno del JSON ottenibile all'indirizzo http://data.europa.eu/euodp/data/api/3/action/package_show?id=funds-absorption-rate.
Tale dataset prevede i seguenti campi: Member State, Year, Fund, Total Paid, Advance, Interim, Absorption (%), dove i primi tre sono gestiti dall'applicazione come variabili di tipo String, mentre i rimanenti come variabili di tipo double.
//Sulla base del dataset viene effettuato il parsing dei dati, il quale produce come risultato degli oggetti JSON che rappresentano i record del dataset stesso.
Successivamente viene avviato un server web locale sulla porta 8080 ricorrendo all'utilizzo del framework Spring. Tramite tale server sarà possibile effettuare le seguenti richieste GET.
`localhost:8080`:
 - `/data`: restituisce i dati, ovvero gli oggetti JSON corrispondenti ai record del dataset;
 - `/metadata`: restituisce i metadati, ovvero l'elenco dei nomi degli attributi di ogni record e dei loro tipo e nome con cui sono
   memorizzati nell'applicazione;
 - `/stats`: restituisce alcune statistiche sui dati.
## Implementazione delle statistiche
Meritevole di approfondimento è la modalità con cui vengono implementate le statistiche. Esse si differenziano in base al tipo dell'attributo cui si fa riferimento: su attributi di tipo String sarà possibile ottenere per ogni campo il numero di occorrenze nell'intero dataset mentre su attributi di tipo
numerico (nel presente caso sono tutti double) sarà possibile ottenere la media (avg), il minimo (min), il massimo (max), la deviazione standard (dev std),
la somma (sum) e il conteggio (count).
L'attributo va specificato nella richiesta delle statistiche aggiungendo un parametro la cui chiave è "field" e il cui valore può essere "memberState", "year", "fund", "totalPaid", "advance", "interim", "absorption", corrispondenti rispettivamente ai campi nominati in precedenza.
Pertanto un esempio di richiesta di statistiche per un attributo di tipo String è: localhost:8080/stats?field=memberState, che restituisce l'elenco di tutti i campi "Member State" del dataset, ognuno con relativo numero di occorrenze.
Un esempio di richiesta di statistiche per un attributo di tipo numerico, invece, è: localhost:8080/stats?field=absorption, che restituisce le statistiche numeriche prima descritte (media, min, ecc.) calcolate su tutti i valori corrispondenti al campo "absorption" relativi a ogni record del dataset.
### Gestione dei problemi in fase di richiesta
L'applicazione prevede la gestione di eventuali problemi in fase di richiesta: se il parametro specificato ha una chiave che differisce da "field" viene inserito nell'oggetto JSON il messaggio `"errore": "specificare un parametro di tipo 'field'"`; se, invece, il campo inserito non corrisponde ad alcuno di quelli elencati precedentemente il messaggio che verrà inserito è `"errore": "campo inesistente"`.
Si osservi che le statistiche su attributi di tipo numerico risultano essere, per tutti i campi eccetto "absorption", in notazione esponenziale, in quanto eccedono il valore 10^7, limite superiore oltre il quale Java ricorre a tale notazione.
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTkzNTE4ODM1LDk0MzU0NDYyMCwtMjExOT
E4Njc0MiwtMTA3Njk0NzEyMCwtOTY0MzgxOTMyXX0=
-->