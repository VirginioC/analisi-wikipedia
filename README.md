# Analisi e Classificazione dei Contenuti di Wikipedia

## Descrizione e obiettivi del progetto
Il progetto, sviluppato durante il master in Data Science di Profession AI, consiste nell'effettuare un'analisi esplorativa dei contenuti di Wikipedia, suddivisi per categoria tematica, e sviluppare un sistema di classificazione automatica per categorizzare i nuovi articoli basandosi sul loro contenuto. Il progetto viene svolto utilizzando **Apache Spark** sulla piattaforma cloud **Databricks**.

I risultati attesi sono quindi:
- Ottimizzare l'organizzazione dei contenuti.
- Automatizzare il processo di categorizzazione dei nuovi articoli.
- Ottimizzare l'allocazione delle risorse editoriali grazie agli insights ottenuti con l'EDA.

### Dataset
Il dataset a disposizione, **`wikipedia.csv`**, è salvato su S3 e viene importato su Databricks come dataframe di Pandas per poi essere trasformato in un table di Spark. 

Esso è costituito da 4 colonne di tipo stringa: 
   - **`title`**: titolo dell'articolo.
   - **`summary`**: introduzione breve dell'articolo.
   - **`documents`**: contenuto dell'articolo.
   - **`category`**: categoria tematica di appartenenza dell'articolo (***economics***, ***politics***, ***culture***, ***science***, ***sports***, ***energy***, ***finance***, ***humanities***, ***pets***, ***trade***, ***technology***, ***transport***, ***medicine***, ***engineering*** e ***research***). 

## Struttura del progetto
1. **Gestione dei valori nulli e dei duplicati**
   
2. **Analisi esplorativa dei dati (EDA)**:
   - **Conteggio degli articoli presenti per ogni categoria**: 
   ![articoli_per_categoria](https://github.com/user-attachments/assets/42bb5885-5cfe-4851-aa90-694635c16332)
   - **Numero medio di parole per articolo**: il numero medio di parole nei 74574 articoli presenti nel dataset è 794.10.
   - **Lunghezza dell'articolo più lungo e di quello più corto per ciascuna categoria**: l'articolo più lungo è all'interno di *finance* (33479 parole) mentre l'articolo più corto è in *technology* (2 parole).
   - **Creazione di nuvole di parole rappresentative per ogni categoria, per identificare i termini più frequenti e rilevanti**.
     
3. **Classificazione automatica**:
   - Rimozione dei samples con stesso contenuto testuale ma differente categoria: task di **classificazione multiclasse**, ogni articolo può appartenere ad una sola delle 15 classi tematiche.
   - Preprocessing del testo (*summary* e *documents*): conversione in lowercase, rimozione di punteggiatura e caratteri speciali, rimozione di numeri, tokenizzazione, rimozione stopwords e lemmatizzazione.
   - Label encoding: a ciascuna categoria tematica si assegna un'etichetta numerica (da 0 a 14).
   - Splitting del dataset in train set (80 %) e test set (20 %).
   - Calcolo dei **class weights** da applicare per bilanciare le classi: valori da 0.62 (*transport*) a 19.72 (*politics*).
   - Si sceglie di valutare:
      - **Logistic Regression**
      - **Complement Naive Bayes**
      - **Random Forest**
        
     per ciascune delle seguenti configurazioni:
      - **Modello1**: uso esclusivo del sommario come input.
      - **Modello2**: uso esclusivo dell'articolo completo come input.
        
   - Il **training** viene effettuato addestrando una pipeline contenente **HashingTF**, **IDF** e il generico modello di ML. I pesi delle classi possono essere applicati attraverso la funzione `setWeightCol`. 
   - L'**evaluation** dei modelli si basa sull'**accuracy** e sulla **confusion matrix**.   
   - Il miglior modello, per accuracy e riconoscimento delle classi minoritarie, è la **regressione logistica con l'articolo completo come input**. Si effettua allora l'ottimizzazione degli iperparametri di tale modello ottenendo infine le seguenti prestazioni:
        - **Train Set Accuracy: 97.68 %**
        - **Test Set Accuracy: 86.23 %**
        - **Test Set Confusion Matrix:**

          ![confusion_matrix](https://github.com/user-attachments/assets/a9530645-bbfc-469a-98e9-d3f86dba2161)

   - In conclusione le prestazioni del modello possono essere considerate abbastanza soddisfacenti con un'accuracy piuttosto alta, nonostante l'overfitting significativo.

## Tecnologie Utilizzate  
- **Linguaggio**: Python  
- **Ambiente di sviluppo**: Databricks (per l'esecuzione di Apache Spark)  
- **Librerie**:  
  - `PySpark` (incluso `pyspark.sql` per operazioni SQL e `pyspark.ml` per machine learning)
  - `pandas`
  - `seaborn`
  - `Matplotlib`
  - `WordCloud`  
  - `NLTK`

## Utilizzo  
1. Scarica o clona il repository.
2. Apri il file `analisi_wikipedia.dbc` su Databricks.
3. Esegui il codice passo-passo per ottenere i risultati.

## Autore
[Virginio Cocciaglia](https://github.com/VirginioC)

---
