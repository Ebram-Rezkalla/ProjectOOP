# ProjectOOP
#Progetto-Programmazione-Ad-Oggetti
## Eventi in Messico
La nostra applicazione permette di avere informazioni riguradanti eventi che avranno luogo in Messico,anche da la possibila' di avere statistiche per ogni stato e statistiche globali.
L'applicazione permette anche di filtrare le statistiche.
---
### INDICE
Diagrammi UML

   1.Diagramma dei casi d'uso

   2.Diagramma delle classi

   3.Diagramma delle sequenze (rotta "/ricerca)

   4.Diagramma delle sequenze (rotta "/ricerca)

  L'applicazione
        
   1.Avvio
    
  2.Rotte disponibili
* State Events
* State Statistic
* Mexico Statistics
* Filters
    
3.Software utilizzati

---

#Diagrammi UML

#####Diagramma dei casi d'uso
---

#L'applicazione

Tramite L'API Ticketmaster il programma riceve, salva e processa gli eventi riguardanti gli stati ; per far questo utilizza più precisamente l'API "events", la quale descrizione è disponibile al seguente [link](https://developer.ticketmaster.com/products-and-docs/apis/discovery-api/v2/#supported-markets)

 Più in particolare con gli eventi si intendono il nome dell'evento, la data , il prezzo, la classificazione.
 
---
#Avvio
 
All'avvio dell'applicazione verrà caricato dal file data.JSON, presente all'interno della cartella del programma. Se durante il caricamento viene lanciata una qualunque eccezione, viene inizializzato un nuovo data vuoto.

Quando l'utente effetua una chiamata vengono impostati i seguenti parametri:

* "API_key" che contiene la chiave per le API di Ticketmaster da inserire nell'URL delle richieste


* "periodo_aggiornamento"  il periodo di tempo in millisecondi che deve passare tra una richiesta di dati e l'altra, di default è impostato su 10800000 ms, ossia 3 ore.

---

#Rotte disponibili

Dal programma vengono rese disponibili le seguenti rotte sulla porta 8080 del localhost:
     
         Rotta     |     Metodo    | Funzione  
     ------------- |:-------------:| -----:
     /StateEvents  |     POST      | restituisce uno stato con gli eventi 
    /StateStatistic|     POST      | restituisce le statistiche di uno  
    /MexicoStatistics|   Get       | restituisce le statistiche globali
    /filters       |     Post		 | filtra le statistiche

 
 
---
#State Events


Per poter avere gli eventi di uno stato viene resa disponibile la rotta "/StateEvents", che deve essere utilizzata con il metodo POST. Il parametro della ricerca deve essere passato tramite il body della richiesta che deve contenere un file JSON nel quale deve essere presente la seguente "chiave":"valore":

         Chiave    |     Valore    |  
     ------------- |:-------------:|
          State    | nome dello Stato  |
             
 Un esempio può essere:

    {
    "State": "Monterrey",
    }            
              
La rotta restituirà un file JSON contenente i seguenti campi:


* name: che contiene il nome dello stato che e' stato richiesto.


* id: che contiene id dello stato.


* events: vector che contiene tutti gli eventi con i propri dati:

>* name: nome dell'evento.

>* ID: id dello dell'evento.

>* url: il sito dell'evento.

>* Date: data dell'evento.

>* Time: orario dell'evento.

>* price: prezzo del biglietto dell'evento.

>* Classification: la classificazione dellevento in base al genre. 

Nel caso dell'esempio, la prima parte del ritorno sarebbe simile a questa:

    {
    "name": ""Monterrey"",
    "id":402
    "events": [
        {
            "name": "Abierto Mexicano de Tenis. Miércoles",
            "id":"17AZvbG62X1NsqR",
            "url":"https://www.ticketmaster.com.mx/abierto-mexicano-de-tenis-miercoles-acapulco-guerrero-03-17-2021/event/140058FF8A8F176F",
            "Date":"2021-03-17",
            "Time":"18:00:00",
               "price":[
                {
                    "min": 990,0,
                    "max": 1760.0,
                }
            ],
            "classification":[ 
            {
            
            "segment": "Sports",
            "genre":"Tennis",
            }
            ]


Durante l'utilizzo della rotta possono essere lanciate, in caso di errori, le seguenti eccezioni:

* ParseException : se si verifica un errore durante il parsing dei dati forniti da OpenWeather


* BadRequestException : se la richiesta dei dati all'API di ticketmaster non è andata a buon fine


* MalformedURLException : se l'URL della richiesta ad ticketmaster non è formato correttamente


* IOException : se ci sono errori durante la lettura dei dati forniti da ticketmaster


* StateNameNotValid : se l'utente ha inserito un nome di stato non valido
 
---

#State Statistics


Per poter ottenere delle statistiche riguardo gli eventi di uno stato  viene resa disponibile la rotta "/StateStatistics", che deve essere utilizzata con il metodo POST. Le statistiche generate sono riguardo massimo,minimo, medio numero di eventi e numero di eventi divisi per genre. I parametri da utilizzare per generare le statistiche devono essere passati tramite il body della richiesta che deve contenere un file JSON contenente la seguente "chiave":"valore":


    Chiave    |     Valore    |  
     ------------- |:-------------:|
          State    | nome dello Stato  |
          
 Un esempio può essere:

    {
    "State": "Guadalajara",
    }   

La rotta restituirà un file JSON contenente i seguenti campi:  

* Numero totale di eventi: numero totale di eventi


* Numero eventi Sport


* Numero eventi Arte


* Numero eventi Musica


* Numero massimo di eventi al mese


* Numero minimo di eventi al mese


* Numero medio di eventi al mese

Nel caso dell'esempio, il ritorno sarebbe simile a questa:

      { 
    
    "Numero totale di eventi":25,
     
     "Numero eventi Sport":7,
     
     "Numero eventi Arte":9,
     
     "Numero eventi Musica":5,
     
     "Numero massimo di eventi al mese":10,
      
     "Numero minimo di eventi al mese":3,
       
     "Numero medio di eventi al mese":"5,
     
     }
	           
---

#Filters


Per poter filtrare delle statistiche riguardo gli eventi di uno stato viene resa disponibile la rotta "/filters", che deve essere utilizzata con il metodo POST. i filtri generate sono riguardo lo stato con il massimo,minimo, medio numero di eventi e numero di eventi divisi per genre. I parametri da utilizzare per generare le statistiche devono essere passati tramite il body della richiesta che deve contenere un file JSON contenente le seguente coppie "chiave":"valore":


     Chiave    |     Valore    |  
     ------------- |:-------------:|
          name1    | nome del primo Stato  |
			name2		|	nome del secondo Stato|
		   genre		|	il genre dell'evento	|
			from   	|	la data iniziale		| 
			to			| la data finale         |

Un esempio può essere:


    {
          "Stati": [
             {
               "name1": "Mexico City and Metropolitan Area"
             },
             {
               "name2": "Monterrey"
             }
             ],
          "genre": "Sport",
          "from":"2021-03-17",
          "to":"2021-08-17"
          }

Nel caso dell'esempio, il ritorno sarebbe simile a questa:


    {
    "Monterrey ha un numero maggiore di eventi sportivi ripetto a Guadalajara nel perido tra 2021-03-17 e 2021-08-17",
      }
      
Durante l'utilizzo della rotta possono essere lanciate, in caso di errori, le seguenti eccezioni:

* ParamValueNoteValid: se l'utente ha inserito un genre non valido


* DateNotValid: se l'utente ha inserito un data non valida


---

#Mexico Statistics

Per poter ottenere delle statistiche riguardo gli eventi globali(tutto Messico) viene resa 

disponibile la rotta di tipo get  "/MexicoStatistics",le statistiche generate sono riguardo lo 

stato con il massimo,minimo numero di eventi e numero di eventi divisi per genre.

---

#Software utilizzati

* Eclipse - Ambiente di sviluppo


* UML Designer - software per la realizzazione dei diagrammi UML


* Maven - software di gestione di progetti e librerie


* Spring Boot - framework per sviluppo di applicazioni in Java


* Postman - ambiente di sviluppo API per effettuare richieste
