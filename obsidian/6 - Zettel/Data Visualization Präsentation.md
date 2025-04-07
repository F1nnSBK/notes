#Note
2025-04-05
Tags: [[Grundlagen DSKI]] [[Data Visualization]]

# Codeabgabe für Data Visualization

> **<b><i>von Finn Hertsch</i></b>**<br><br> > <b>Kurs:</b> WWIDS124<br> > <b>Datum:</b> 12.03.2025<br> > <b>Datensatz:</b> Marketing

1. [Data Cleaning](#data_cleaning)
2. [Data Exploration](#data_exploration)
3. [Predictive Modelling](#predictive_modelling)

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

from helper import Helper

from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix, ConfusionMatrixDisplay, auc, roc_curve
from sklearn.model_selection import train_test_split
```

### 1. Data Cleaning <a name="data_cleaning">

#### Einlesen der Daten und grundlegende Informationen

```Python
file_path = "data/marketing.csv"
df = pd.read_csv(file_path, sep="|")

RAW_ROWS = df.shape[0]
RAW_COLUMNS = df.shape[1]

df.shape
# (45215, 18)

df.describe(include="all")
```

```python
helper = Helper()

helper.get_columns_with_dtype(df)
```

### Checkliste:

- [x] Doppelte Werte
- [x] Leere Felder
- [x] Numerische Ausreißer
- [x] Invalide Datenwerte
- [x] Standardisierung
- [x] Fehlerbehebung
- [x] Entfernung irrelevanter Spalten
- [x] Einheitliches Datenformat

##### Doppelte Werte

Es wurden Duplikate gefunden und mit den Originalen verglichen, um festzustellen, ob es tatsächlich richtige Duplikate sind (Jedes Feature den gleichen Wert hat).
Anschließend wurden die Duplikate entfernt.

Laut der Pandas Dokumentation von .duplicated() werden, wenn keine Parameter mitgegeben werden, alle Spalten auf doppelte Werte überprüft und nur wenn eine Zeile in jeder Zeile ein Duplikat hat, wird diese als solches markiert.

```python
df_duplicates = []

duplicated_ids = df.loc[df.duplicated(), "id"].tolist()
# 3 Duplikate gefunden id: 559, 1386, 1880
for duplicate_id in duplicated_ids:
    df_duplicates.append(df.loc[df["id"] == duplicate_id])

df = df.drop_duplicates()
print(f"Es wurden {RAW_ROWS - df.shape[0]} Zeilen entfernt.")
df_duplicates
```

#### Leere Felder

```python

fig , ax = plt.subplots(figsize=(20, 5))
ax.bar(df.columns,df.isna().sum())
ax.set_title("Anzahl der NaN Werte pro Feature")
ax.set_xlabel("Features")
ax.set_ylabel("Anzahl der NaN Werte")
plt.show()

print(df.isna().sum())
median_age = df['age'].median()

df.loc[df['age'].isna(), "age"] = median_age
print(f"\nBereinigt: {df.isna().sum()}")
```

#### Numerische Ausreißer

```python
numeric_columns = []
for column in df.columns:
    if df[column].dtype in [float, int]:
        numeric_columns.append(column)

print(f"Numerische Spalten: {numeric_columns}")
```

Numerische Spalten: ['age', 'balance', 'campaign', 'pdays', 'previous', 'id']

```python
for feature in numeric_columns:
    print(df[feature].describe(), "\n")
```

```python
numeric_columns.remove("pdays")

numeric_features = len(numeric_columns)
fig, ax = plt.subplots(numeric_features, figsize=(10, 10))

fig.suptitle("Boxplots der numerischen Features (raw)")

for i, feature in enumerate(numeric_columns):
    ax[i].boxplot(df[feature], widths=2, orientation='horizontal')
    ax[i].set_xlabel(feature)
    ax[i].set_ylabel(feature)
    ax[i].grid(axis="x")

plt.tight_layout(rect=[0, 0, 1, 0.98])
plt.show()
```

#### Invalide Datenwerte

_age_

- negatives Alter muss raus
- Kunden unter 18 dürfen keine Werbung erhalten
    - Werbung für Finanzprodukte per Direktmarketing nur, wenn Kunde völljährig
- 110 Jährige Person wird nicht beachtet

_balance, campaign, pdays_

- Werte sehen in Ordnung aus

_previous_

- Einzelener Ausreißer bei über 250, ist realistisch aber kann auch ausgebessert werden

_id_

- id wird nicht beachtet, aber es gäbe einen Fehler bei 500.000

_duration_

- Müsste duration nicht auch einen numerischen Wert haben? --> überprüfen

```python
# entfernen des previous outliers
df.loc[df["previous"] >= df["previous"].max(), "previous"] = df["previous"].median()

df["age"].describe()
# Ausreißer -5 als Minimum durch Median ersetzen
# Ausreißer 110 als Maximum, aber realistisch (evtl. überdenken)
df.loc[(df["age"] < 18) | (df["age"] >= 110), "age"] = df["age"].median()
```

```python
fig, ax = plt.subplots(1, 2,figsize=(8, 6))

ax[0].boxplot(df["age"], tick_labels=["age"])
ax[0].set_title("Feature Alter (bereinigt)")
ax[0].set_ylabel("Alter")
ax[0].grid(axis="y")

ax[1].boxplot(df["previous"], tick_labels=["previous"])
ax[1].set_title("Feature Previous (bereinigt)")
ax[1].set_ylabel("Vorherige Kontakte")
ax[1].grid(axis="y")

plt.show()
```

Untersuchung von age und balance, um zu überprüfen, ob es unrealistische Kontostände gibt.

```python
fig, ax = plt.subplots(figsize=(5,4))

age_df = df["age"]
balance_df = df["balance"]

ax.scatter(age_df, balance_df, s=20, edgecolor='black', lw=0.5)
ax.set_title("Alter und durchschnittliches Jahresguthaben")
ax.set_xlabel("Alter (Jahre)")
ax.set_ylabel("Durchschnittliches Jahresguthaben (Euro)")

plt.show()
```

Keine unrealistischen Werte feststellbar

#### Standardisierung und Fehlerbehebung

```python
helper.get_discrete_unique_values(df)
```

_job_

- 'admin.' soll zu 'admin' werden

_marital_

- 'single' und 'alone' zu 'single' zusammenführen

_education, default, housing & loan_

- keine

_contact_

- Annahme: 'x' bedeutet kein Kontakt
- 'xxxxx' und 'x' zu 'x' zusammenführen

_month_

- Zahlenwerte z.B. '06' zu 'jun'
- Einigung auf Schreibweise mit 3 Buchstaben (Kürzel)

_duration_

- zu numerischen Wert casten

_poutcome_

- was bedeutet 'other'? --> mit Business absprechen

_accepted_

- keine

```python
# job
df.loc[df["job"] == 'admin.', "job"] = 'admin'
df["job"].unique()

# mratial
df.loc[df["marital"] == 'alone', "marital"] = 'single'
df["marital"].unique()

# contact
df.loc[df["contact"] == "xxxxx", "contact"] = 'x'
df["contact"].unique()

# month
print("Vorher: ", df["month"].shape[0])
df.loc[:, "month"] = [helper.format_months(m) for m in df["month"]]
print("Nachher: ", df["month"].shape[0])
df["month"].unique()
```

Überprüfung der Standardisierung

```python
helper.get_discrete_unique_values(df)
```

#### Entfernung irrelevanter Spalten

_pet_

- entfernen, da es keine Varianz in den Werten gibt

_id_

- entfernen, da es keine nützliche Aussage über die Daten zulässt

```python
df = df.drop(["pet", "id"], axis=1)
```

#### Einheitliches Datenformat

_duration_

- "duration" sollte eine numerische Spalte sein.
    Dafür wandel ich "-" in "0" um und caste dann die Objects (Strings) zu numerischen Datentypen, in diesem Fall float, um keine Rundungsfehler zu verursachen, falls es welche geben sollte.

```python
# duration
df.loc[df["duration"] == "-", "duration"] = "0"
df["duration"] = df["duration"].astype(float)

helper.get_columns_with_dtype(df)
```

```python
# Verteilung von duration
fig, ax = plt.subplots(figsize=(10, 2))

ax.boxplot(df["duration"], widths=2, orientation='horizontal')
ax.set_xlabel("duration")
ax.set_ylabel("duration")
ax.grid(axis="x")
ax.set_title("Boxplot von duration")

plt.show()

df["duration"].describe()
```

Negative Werte bei der _duration_ ergeben keinen Sinn.

```python
df.loc[df["duration"] < 0, "duration"] = df["duration"].median()

fig, ax = plt.subplots(figsize=(10, 2))

ax.boxplot(df["duration"], widths=2, orientation='horizontal')
ax.set_xlabel("duration")
ax.set_ylabel("duration")
ax.grid(axis="x")
ax.set_title("Boxplot von duration (bereinigt)")

plt.show()

df["duration"].describe()
```

---

> ### 2. Data Exploration <a name="data_exploration">

Häufigkeit der Zielvariable accepted

```python
accepted = df["accepted"].value_counts()

fig, ax = plt.subplots(figsize=(6,4))

ax.bar(accepted.index, accepted.values, tick_label=["Nein", "Ja"], color=["red", "blue"])
ax.set_title("Verteilung von accepted")
ax.set_xlabel("Akzeptiert")
ax.set_ylabel("Häufigkeit")
ax.grid(axis="y")

plt.show()

print(accepted)
```

```python
df["accepted"].value_counts(normalize=True)
```

```python
fig, ax = plt.subplots(2, 1, figsize=(12, 8))

bin_count = int(1 + np.log2(df["age"].shape[0]) * 2)
print(f"Bin count: {bin_count}")

ax[0].set_title("Verteilung des Alters")
ax[0].hist(df["age"], bins=bin_count, edgecolor="black")
ax[0].axvline(df["age"].median(), color="orange", label="Median")
ax[0].set_ylabel("Häufigkeit")
ax[0].set_xlabel("Alter")

ax[1].boxplot(df["age"], orientation="horizontal", widths=0.5)
ax[1].set_yticklabels(["Age"])
ax[1].set_xlabel("Alter")
```

```python

fig, ax = plt.subplots(1, 2, figsize=(12, 4), dpi=300)

ax[0].set_title("Verteilung des jährlichen Guthabens")
ax[0].hist(df["balance"], bins='auto', edgecolor="black")
ax[0].set_xscale('log')
ax[0].axvline(df["balance"].median(), color="orange", label="Median")
ax[0].set_ylabel("Häufigkeit")
ax[0].set_xlabel("Balance")


ax[1].set_title("Verteilung der Dauer der Anrufe")
ax[1].hist(df["duration"], bins=212, edgecolor="black")
ax[1].axvline(df["duration"].median(), color="orange", label="Median")
ax[1].set_ylabel("Häufigkeit")
ax[1].set_xlabel("Dauer des Anrufs")
ax[1].set_xlim(0, 1000)
```

#### Wie beeinflussen die anderen Features accepted?

Untersuchung der Annahme eines Angebots (accepted) in Abhängigkeit des Alters (age) und der Dauer der letzten Kontaktaufnahme (duration).

```python
accepted_df = df.loc[df["accepted"] == "yes"]
not_accepted_df = df.loc[df["accepted"] == "no"]

df_length = accepted_df.shape[0] + not_accepted_df.shape[0]
# Kontrolle: 45212 --> keine nan Werte dabei (wie erwartet)

fig, ax = plt.subplots(figsize=(9,4))

ax.scatter(accepted_df["age"], accepted_df["duration"], c="b", s=5, label="Accepted")
ax.scatter(not_accepted_df["age"], not_accepted_df["duration"], c="r", s=5, label="Not Accepted")
ax.set_title("Accepted by Age and Duration")
ax.set_xlabel("Age")
ax.set_ylabel("Duration")
ax.legend()
#ax.grid("y")

plt.show()
```

```python
temp = np.divide(df["duration"].max(), 60**2)
max_duration_outlier = (((temp - 1) * 60)/100) + 1
round( max_duration_outlier, 2)

```

#### Korrelationsmatrix

- Faktorisierung von ["no", "yes"] Werten
    --> _default, housing, loan & accepted_ haben ["no", "yes"] Werte

```python
to_factorize = ["default", "housing", "loan", "accepted"]
df_temp = pd.get_dummies(df, columns=to_factorize)
helper.get_columns_with_dtype(df)

for column in to_factorize:
    df_temp[column] = df_temp[f"{column}_yes"]

    df_temp = df_temp.drop(columns=[f"{column}_yes", f"{column}_no"])

df_temp.head()
```

pd.get_dummies() nutzt One-Hot-Encoding um Werte wie "yes" und "no" in boole'sche Werte umzuwandeln.

```python
relevant_columns = (
    (df_temp.dtypes != object)
    & (df_temp.columns != "job")
    & (df_temp.columns != "marital")
    & (df_temp.columns != "education")
    & (df_temp.columns != "contact")
    & (df_temp.columns != "month")
    & (df_temp.columns != "poutcome")
)
df_corr = df_temp.loc[:, relevant_columns].corr()
df_corr
```

```python
fig, ax = plt.subplots(figsize=(7, 5))
img = ax.imshow(df_corr, cmap="Spectral")

fig.colorbar(img, shrink=0.8)

for i in range(df_corr.shape[0]):
    for j in range(df_corr.shape[1]):
        ax.text(j, i, f'{round(df_corr.iloc[i, j],2)}', ha='center', va='center', color='black', fontsize=7)

ax.set_xticks(range(df_corr.columns.shape[0]), df_corr.columns, rotation=90)
ax.set_yticks(range(df_corr.columns.shape[0]), df_corr.columns)
ax.set_title("Correlation Heatmap")

plt.show()

```

Durch verschiedene cmaps (colormaps) werden die Werte besser oder schlechter sichtbar.
Mir fallen 4 Werte besonders auf:

- Eine längere Dauer des Telefonats führt tendenziell dazu, dass das Angeobt wahrscheinlicher angenommen wird.
- Die Anzahl der Tage seit der letzten Kontaktaufnahme und die Anzahl der Kontaktaufnahmen vor der aktuellen Kampagne korrelieren, da sie auch direkt voneinander abhängig sind.
- Ältere Leute haben tendenziell kein Baudarlehen.
- Außerdem ist es auch so, dass Leute, die das Angebot annehmen, tendenziell kein Baudarlehen und kein Privatdarlehen haben.

Dauer/Annahme: Längere Dauer erhöht Annahmewahrscheinlichkeit.
pdays/previous: Tage korrelieren vorherige Kontakte.
Alter/Baudarlehen: Ältere Kunden selten Baudarlehen.
Annahme/Darlehen: Annahme ohne Baudarlehen, Privatdarlehen.

##### Genaueres Betrachen der Korrelationen

```python
df["pdays"].value_counts(normalize=True)
```

Über 80% der Werte von pdays sind -1 und bedeuten damit, dass er kein vorherigen Kontakt gab.

```python
fig, ax = plt.subplots(2,2, figsize=(15, 15))
# duration mit accepted
duration_accepted = df_temp.loc[df_temp["accepted"] == 1, "duration"]
duration_not_accepted = df_temp.loc[df_temp["accepted"] == 0, "duration"]

ax[0,0].hist(duration_not_accepted, bins=30, alpha=0.5, label="Not Accepted", color="red", edgecolor="black")
ax[0,0].hist(duration_accepted, bins=30, alpha=0.5, label="Accepted", color="blue", edgecolor="black")
ax[0,0].set_xlabel("duration")
ax[0,0].set_ylabel("Anzahl")
ax[0,0].axvline(duration_accepted.median(), color="blue")
ax[0,0].axvline(duration_not_accepted.median(), color="red")
ax[0,0].set_xlim(0, 2000)
ax[0,0].set_title("Duration Verteilung mit Acceptance")
ax[0,0].legend()
ax[0,0].grid(alpha=0.3)

# pdays mit previous
ax[0,1].scatter(df_temp["pdays"], df_temp["previous"], edgecolor="black", lw=0.5)
ax[0,1].set_xlabel("pdays")
ax[0,1].set_ylabel("previous")
ax[0,1].set_title("pdays in Abhängigkeit von previous")

b, m = np.polyfit(df_temp["pdays"], df_temp["previous"], deg=1)
ax[0,1].axline(xy1=(0, b), slope=m, label=f'y={round(m, 2)}x + {round(b, 2)}', lw=2, color="orange")
ax[0,1].legend()

# age mit housing

age_housing = df_temp.loc[df_temp["housing"] == 1, "age"]
age_no_housing = df_temp.loc[df_temp["housing"] == 0, "age"]

ax[1,0].hist(age_no_housing, bins=30, alpha=0.5, label="Not Housing", color="red", edgecolor="black")
ax[1,0].hist(age_housing, bins=30, alpha=0.5, label="Housing", color="green", edgecolor="black")
ax[1,0].set_xlabel("age")
ax[1,0].set_ylabel("Anzahl")
ax[1,0].axvline(age_housing.median(), color="lime")
ax[1,0].axvline(age_no_housing.median(), color="red")
ax[1,0].set_xlim(age_no_housing.min() - 5, age_no_housing.max() + 5)
ax[1,0].set_title("Age Verteilung mit Baudarlehen")
ax[1,0].legend()
ax[1,0].grid(alpha=0.3)

# accepted mit housing und loan
pivot_table = df_temp.pivot_table(index='loan', columns='housing', values='accepted', aggfunc='mean')
print(pivot_table)
img = ax[1,1].imshow(pivot_table, cmap="Greens")
fig.colorbar(img, shrink=0.8)

for i in range(pivot_table.shape[0]):
    for j in range(pivot_table.shape[1]):
        ax[1,1].text(j, i, f'{round(pivot_table.iloc[i, j], 2)}', ha='center', va='center', color='black', fontsize=7)

ax[1,1].set_title("Acceptance in Abhängigkeit von housing & loan")
ax[1,1].set_xticks(range(pivot_table.columns.shape[0]), pivot_table.columns, rotation=90)
ax[1,1].set_xlabel("housing (False/True)")
ax[1,1].set_yticks(range(pivot_table.columns.shape[0]), pivot_table.columns)
ax[1,1].set_ylabel("loan (True/False)")
```

```python
df['default'] = df_temp['default']
df['housing'] = df_temp['housing']
df['loan'] = df_temp['loan']

# Übernehmen der "default", "housing" und "loan" Spalten
print(df.head())
```

#### Untersuchung der nominalen Werte, die nicht in der Korrelationsmatrix sind

Das sind: _job, marital, education, contact, month, poutcome_

```python
jobs_accepted_df = df.pivot_table(index="accepted", columns="job", aggfunc="size")
jobs_accepted_df = jobs_accepted_df.drop(columns="unknown")
jobs_accepted_df
```

_unknown_ entferne ich als Attribut, da es mich keine nützliche Aussage machen lässt.

```python
ax = jobs_accepted_df.plot(kind='bar', figsize=(8,7), colormap="tab20", edgecolor="black")
ax.set_title("Accepted nach Jobs")
ax.set_ylabel("Häufigkeit accepted")
ax.grid(axis="y")

```

Wir erkennen, dass die Jobs, die am häufigsten Angebote ablehnen, genau auch die Jobs sind, die am häufigsten Angebote annehmen.

_blue_collar_ Jobs sind Berufe, die manuelle/praktische Arbeit bedeuten. (Handwerker, usw.)

```python
marital_accepted_df = df.pivot_table(index="accepted", columns="marital", aggfunc="size")
marital_accepted_df

ax = marital_accepted_df.plot(kind='bar', figsize=(8,7), colormap="tab20", edgecolor="black")
ax.set_title("Accepted nach Familienstand")
ax.set_ylabel("Häufigkeit accepted")
ax.grid(axis="y")

```

```python
education_accepted_df = df.pivot_table(index="accepted", columns="education", aggfunc="size")
education_accepted_df

ax = education_accepted_df.plot(kind='bar', figsize=(8,7), colormap="tab20", edgecolor="black")
ax.set_title("Accepted nach Bildungsgrad")
ax.set_ylabel("Häufigkeit accepted")
ax.grid(axis="y")
```

```python
contact_accepted_df = df.pivot_table(index="accepted", columns="contact", aggfunc="size")
print(contact_accepted_df)
contact_accepted_df = contact_accepted_df.drop(columns=["x"])
ax = contact_accepted_df.plot(kind='bar', figsize=(8,7), colormap="tab20", edgecolor="black")
ax.set_title("Accepted nach Art der letzten Kommunikation")
ax.set_ylabel("Häufigkeit accepted")
ax.grid(axis="y")
```

##### Accepted für jeden Monat

```python
sorted_months = helper.sort_months(df["month"].unique())
print(sorted_months)

pivot_table = df.pivot_table(index="accepted", columns="month", aggfunc="size")
pivot_table = pivot_table[sorted_months]

fig, ax = plt.subplots(figsize=(10, 6))

ax.plot(pivot_table.T, marker="o")
ax.set_title("Accepted nach Monat")
ax.set_xlabel("Monat")
ax.set_ylabel("Häufigkeit accepted")
ax.legend(["No", "Yes"])
ax.grid(axis="y")
```

---

> ### 3. Predictive Modelling <a name="predictive_modelling">

```python
# Code Platzhalter
feature_columns = [
    "age",
    "balance",
    "duration",
    "campaign",
    "pdays",
    "previous",
    "default",
    "housing",
    "loan",
]
target = "accepted"

X = df[feature_columns]
y = df[target].map({"yes": 1, "no": 0})

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
# Modell Training
clf = RandomForestClassifier()
clf.fit(X_train, y_train)
y_pred = clf.predict_proba(X_test)
y_pred = np.where(y_pred[:,1] >= 0.35, 1, 0)
print(classification_report(y_test, y_pred))

# Platzhalter für Confusion Matrix Visualisierung
cm = confusion_matrix(y_test, y_pred, labels=clf.classes_)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=['Not Accepted', 'Accepted'])
disp.plot(cmap='Blues')
plt.title('Marketing Acceptance Classification')
plt.show()
```

```python
y_pred_proba = clf.predict_proba(X_test)[:, 1]
fpr, tpr, thresholds = roc_curve(y_test, y_pred_proba)

roc_auc = auc(fpr, tpr)

plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color='darkorange', lw=2, label=f'ROC curve (area = {round(roc_auc, 2)})')
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver Operating Characteristic (ROC)')
plt.legend(loc="lower right")
plt.show()

print(f"AUC: {roc_auc}")
```

### helper Funktionen

```python
import datetime
import pandas as pd

# soll Dont Repeat Yourself unterstützen


class Helper:
    def __init__(self):
        pass

    def get_columns_with_dtype(self, df: pd.DataFrame) -> None:
        """
        Gibt die Namen der Spalten und deren Datentyp in die Konsole aus.

        Args:
            df (pd.DataFrame): Ein Pandas DataFrame.
        """
        for column in df.columns:
            print(f"{column}: {df[column].dtype}")

    def get_discrete_unique_values(self, df: pd.DataFrame) -> None:
        """
        Gibt den Spaltennamen und die einzigartigen Werte der Spalte aus.

        Args:
            df (pd.DataFrame): Ein Pandas DataFrame.
        """
        df_discrete = df.select_dtypes(include=["object"])
        discrete_columns = df_discrete.columns

        for column in discrete_columns:
            print(f"Spalte {column}: {df[column].unique()}")

    def format_months(self, month: str) -> str:
        """
        Wandelt Monatsnamen in eines Strings in 3-Buchstaben-Kürzel um.

        Args:
            month (str): Der Monat aus der Pandas Series als String.

        Returns:
            month: Als Kürzel oder, wenn keine Umwandlung stattfinden konnte, den Monat unbehandelt.
        """
        try:
            return datetime.datetime.strptime(month, "%B").strftime("%b").lower()
        except ValueError:
            try:
                return datetime.datetime.strptime(month, "%m").strftime("%b").lower()
            except ValueError:
                return month

    def sort_months(self, months: list) -> list:
        """
        Sortiert eine Liste von Monaten.

        Args:
            months (list): Eine Liste von Monaten.

        Returns:
            list: Die sortierte Liste von Monaten.
        """
        sorted_months = sorted(
            months, key=lambda x: datetime.datetime.strptime(x, "%b")
        )
        return sorted_months

```

---

## Info
