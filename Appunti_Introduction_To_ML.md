
# **APPUNTI**
Google Colab:
https://colab.research.google.com/drive/1cLnA_cL9mNg-fRLrUILqlKc99bz8j1wo


Slide Online:
https://gft365-my.sharepoint.com/personal/cnci_gft_com/_layouts/15/onedrive.aspx?csf=1&web=1&e=AOf0WF&ovuser=55a2bc67%2Daec1%2D4ad2%2D9a9c%2D5b2457b91dcd%2Caaer
%40gft%2Ecom&OR=Teams%2DHL&CT=1728209763389&clickparams=eyJBcHBOYW1lIjoiVGVhbXMtRGVza3RvcCIsIkFwcFZlcnNpb24iOiI0OS8yNDA5MDEwMTQyMSIsIkhhc0ZlZGVyYXRlZFVzZXIiOmZhbHNlfQ%3D%3D&CID=113857a1%2Da095%2Da000%2D0887%2D209b57c38650&cidOR=SPO&id=%2Fpersonal%2Fcnci%5Fgft%5Fcom%2FDocuments%2F1%2EAPPRENDISTATO%2FML%20CORSO%2FMachine%20learning%20models%2D20240704&FolderCTID=0x012000E0963AEAEC7A8D4AA9ECB9D31FE52FDD&view=0


# PROGETTO


https://www.kaggle.com/datasets/sgpjesus/bank-account-fraud-dataset-neurips-2022/data?select=Base.csv

Record:1000000
Frodi:11029
1,102 %




# POTENZIALI DOMANDE

- Se per esempio facciamo una OneHotEncoder sul dataset di training e test,
per un eventuale test finale(per esempio quello di Keggle) possiamo rielaborare allo stesso modo anche il dataset che utilizzano loro?




# SVILUPPO

Supervised Learning
Classificazione




Steps:

- Caricare base.csv

- (Rapporto non frodi frodi 5 a 1)

- OneHotEncoder

- RandomForestClassifier come modello




# MODIFICHE

- Rimuovere le colonne che hanno sempre lo stesso valore
device_fraud_count

- Rimuovere le colonne che non hanno cambiamenti significativi per fare identificare le frodi
month




# IPOTESI PER ALTRI TEST


- Rimuovere le colonne non ritenute importanti ai fini dell'individuazione delle frodi (diminuire l'overfitting)
- Aumentare la dimensione dell'input(Base +  Varianti 1 e 2), cambiandogli sempre il rapporto frodi non frodi
- Ritoccare l'input in altro modo

- Provare ad aggiungere altri campi al OneHotEncoder

- Suddividere i dati in training e testing con rapporto diverso da 75-25

- Aggiungere un set di validation

- Testare con altri modelli (non RandomForestClassifier)










# TEST


- TEST 1
Allenamento con RandomForestClassifier e test su Base senza modifiche con rapporto 75-25

Accuratezza : 0.989072
Precisione 0.75
Matrice di confusione:[[247259    3] [2729       9]]


- TEST 2
Allenamento con RandomForestClassifier e test su Base con rapporto 75-25, con un rapporto frodi non frodi 1 a 5 (rimozione di un po' di non frodi)

Accuratezza: 0.9786025145067698
Precisione: 0.9995803608896349
Matrice di confusione:[[13808     1] [  353  2382]]


- TEST 3
Allenamento con RandomForestClassifier e test del modello su Base con rapporto 75-25, con un rapporto frodi non frodi 1 a 5 (rimozione di un po' di non frodi)
Test finale su tutte le Varianti e la Base non modificate in alcun modo

Non è andato bene, overfitting su troppi pochi casi negativi




- TEST 4
Allenamento con RandomForestClassifier e test del modello su Base con rapporto 75-25, con un rapporto frodi non frodi 1 a 10 (rimozione di un po' di non frodi)
Test finale su tutte le Varianti e la Base non modificate in alcun modo

Non è andato bene, overfitting su troppi pochi casi negativi



- TEST 5
Allenamento con RandomForestClassifier e test del modello su Base con rapporto 75-25, con un rapporto frodi non frodi 1 a 50 (rimozione di un po' di non frodi)
Test finale su tutte le Varianti e la Base non modificate in alcun modo

Andato meglio rispetto a 3 e 4 nella matrice di confusione(meno falsi positivi), ma comunque overfitting (percentuale precisione troppo bassa)





- TEST 6
Allenamento con RandomForestClassifier e test del modello su Base con rapporto 75-25, con un rapporto frodi non frodi 1 a 100 (rimozione di un po' di non frodi)
Test finale su tutte le Varianti e la Base non modificate in alcun modo

Ci sono pochi falsi positivi, però ci sono troppi falsi negativi

