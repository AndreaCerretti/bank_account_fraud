# Bank Account Fraud

## Step di allenamento del modello di previsione delle frodi bancarie

- Lettura csv Base.csv
- OneHotEncoder sulle feature non numeriche
- Eliminazione feature meno significative per distinguere frodi e non frodi
- Eliminazione di una parte delle non frodi per rendere il rapporto frodi-non frodi 1 a 25
- Separazione dataset training e test con rapporto 75-25
- Scaling feature
- PCA di 16 componenti
- Allenamento modello RandomTreeClassifier
- Test modello con visulizzazione delle metriche principali


## Codice finale(senza import)
```python

def ohe(df, list_of_columns):
    # Inizializzare OneHotEncoder
    encoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')
    
    # Manteniamo una copia del DataFrame originale
    df_encoded = df.copy()

    # Iterare sulle colonne da codificare
    for col in list_of_columns:
        # Applicare OneHotEncoder alla colonna specifica
        encoded = encoder.fit_transform(df[[col]])

        # Creare un DataFrame con le nuove colonne codificate, usando gli stessi indici del DataFrame originale
        encoded_df = pd.DataFrame(encoded, columns=encoder.get_feature_names_out([col]), index=df.index)

        # Concatenare le nuove colonne codificate al DataFrame originale
        df_encoded = pd.concat([df_encoded, encoded_df], axis=1)
    df_encoded = df_encoded.drop(list_of_columns, axis=1)

    # Restituire il DataFrame finale, mantenendo le colonne originali
    return df_encoded


def train_and_test(model, X_train, y_train, X_test, y_test):
    model.fit(X_train, y_train)

    y_pred = model.predict(X_test)

    accuracy = accuracy_score(y_test, y_pred)
    print(f'Accuratezza: {accuracy}')

    # Calcolare la precisione
    precision = precision_score(y_test, y_pred)
    print(f'Precisione: {precision}')

    # Calcolare il recall (sensibilità)
    recall = recall_score(y_test, y_pred)
    print(f'Recall: {recall}')

    # Calcolare FPR (False Positive Ration)
    print(f'FPR: {1 - recall}')

    # Matrice di confusione
    cm = confusion_matrix(y_test, y_pred)
    print(f'Matrice di confusione:\n{cm}')

    return model


# Lettura Csv

df = pd.read_csv("Base.csv")

# OneHotEncoder sulle feature non numeriche
df = ohe(df,['employment_status','payment_type','housing_status', 'source', 'device_os'])

# Eliminazione feature numeriche meno significative
df = df.drop(['session_length_in_minutes', 'zip_count_4w', 'velocity_6h', 'velocity_24h'], axis=1)

# Eliminazione non frodi per rendere il rapporto frodi-non frodi 1 a 25
df_frodi = df[(df['fraud_bool'] == 1)]
n = df[(df['fraud_bool'] == 1)].shape[0]
df_not_frodi = df[(df['fraud_bool'] != 1)].head(n*25)

df_rapp = pd.concat([df_frodi, df_not_frodi])

# Creazione dataset feature e target
features = df_rapp.drop(['fraud_bool'], axis=1)
targets = df_rapp['fraud_bool']

# Split train e test 80-20
X_train, X_test, y_train, y_test = train_test_split(features, targets, random_state=0)

# Scaling dei dataset contenenti le feature di train e test
sc = RobustScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)

# Principal Component Analysis
pca = PCA(n_components=16)
X_train = pca.fit_transform(X_train)
X_test = pca.transform(X_test)

classifier = RandomForestClassifier()
model = train_and_test(classifier, X_train, y_train, X_test, y_test)
```

## Risultato finale


- Accuratezza: 0.9858974180139213
- Precisione: 0.9876750700280112
- Recall: 0.640625
- FPR: 0.359375
- Matrice di confusione:

$\begin{bmatrix}68915 & 22\\989 & 1763 \end{bmatrix}$





## Recap Test


### Test senza eliminazione troppe non frodi, solo con ohe

- Accuratezza: 0.989048
- Precisione: 0.5
- Recall: 0.002191380569758948
- FPR: 0.997808619430241
- Matrice di confusione:

 $\begin{bmatrix}247256 & 6\\2732 & 6 \end{bmatrix}$


### Senza standardizzazione e pca

- Accuratezza: 0.986050858569655
- Precisione: 1.0
- Recall: 0.6366279069767442
- FPR: 0.3633720930232558
- Matrice di confusione:

$\begin{bmatrix}68937 & 0\\1000 & 1752 \end{bmatrix}$


### Standard Scaler PCA 16

- Accuratezza: 0.9806106934118205
- Precisione: 0.9348659003831418
- Recall: 0.5319767441860465
- FPR: 0.46802325581395354
- Matrice di confusione:

$\begin{bmatrix}68835 & 102\\1288 & 1464 \end{bmatrix}$


### RobustScaler PCA 16

- Accuratezza: 0.9850604695281006
- Precisione: 0.9816618911174785
- Recall: 0.6224563953488372
- FPR: 0.37754360465116277
- Matrice di confusione:

$\begin{bmatrix}68905 & 32\\1039 & 1713 \end{bmatrix}$


### Cambio rapporto train test in 80-20 (prima era 75-25)

- Accuratezza: 0.9860159369496608
- Precisione: 0.9853963838664812
- Recall: 0.6446769790718835
- FPR: 0.3553230209281165
- Matrice di confusione:

$\begin{bmatrix}55132 & 21\\781 & 1417 \end{bmatrix}$

 
### PCA 10

- Accuratezza: 0.9848651815480757
- Precisione: 0.9690489589195272
- Recall: 0.6257267441860465
- FPR: 0.37427325581395354
- Matrice di confusione:

$\begin{bmatrix}68882 & 55\\1030 & 1722 \end{bmatrix}$


### PCA 5
- Accuratezza: 0.9844606564465957
- Precisione: 0.9632352941176471
- Recall: 0.6188226744186046
- FPR: 0.3811773255813954
- Matrice di confusione:

$\begin{bmatrix}68872 & 65\\1049 & 1703 \end{bmatrix}$


### Eliminazione feature meno significative

- Accuratezza: 0.9858555705896302
- Precisione: 0.986562150055991
- Recall: 0.6402616279069767
- FPR: 0.3597383720930233
- Matrice di confusione:

$\begin{bmatrix}68913 & 24\\990 & 1762 \end{bmatrix}$
