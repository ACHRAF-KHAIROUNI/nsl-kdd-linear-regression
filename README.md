# Régression Linéaire sur le Trafic Réseau (Dataset NSL-KDD)

Ce projet implémente un modèle de **régression linéaire** appliqué à l'analyse de données de trafic réseau et de cybersécurité issues du benchmark **NSL-KDD**.

L'objectif principal est d'identifier automatiquement les variables numériques présentant la plus forte dépendance statistique, d'ajuster une droite de régression pour modéliser cette relation, et d'analyser les résidus pour détecter des comportements anormaux.

---

## 📊 Jeu de Données (NSL-KDD)

Le projet exploite deux fichiers de données :
- **`Train_data.csv`** : 25 192 observations, 42 caractéristiques.
- **`Test_data.csv`** : 22 544 observations, 41 caractéristiques.

### Prétraitement et sélection des variables
1. **Extraction des colonnes numériques communes** entre l'ensemble d'entraînement et l'ensemble de test.
2. **Filtrage des attributs à variance quasi-nulle** ($\sigma \le 10^{-5}$).
3. **Sélection automatique par matrice de corrélation** : recherche de la paire $(X, y)$ maximisant le coefficient de corrélation linéaire absolu $|r|$.

### Paire optimale retenue
- **Variable explicative ($X$)** : `num_compromised` (nombre de conditions compromises détectées)
- **Variable cible ($y$)** : `num_root` (nombre d'accès administrateur / *root* obtenus)
- **Coefficient de corrélation** : $|r| \approx 0.9990$

---

## 🧠 Modélisation et Résultats

Le modèle est entraîné avec l'algorithme des moindres carrés ordinaires (`scikit-learn.linear_model.LinearRegression`).

### Équation du Modèle
$$\hat{y} = \beta_0 + \beta_1 \cdot x$$

- **Ordonnée à l'origine ($\beta_0$)** : `-0.0014`
- **Pente ($\beta_1$)** : `1.1029`

$$\hat{y} = -0.0014 + 1.1029 \times \text{num\_compromised}$$

### Métriques d'Évaluation sur l'ensemble de test

| Métrique | Valeur | Interprétation |
| :--- | :---: | :--- |
| **MAE** (Mean Absolute Error) | **0.0344** | Erreur moyenne absolue très faible |
| **MSE** (Mean Squared Error) | **0.5217** | Écart quadratique moyen bien maîtrisé |
| **$R^2$** (Coefficient de détermination) | **0.9919** | **99.19%** de la variance de `num_root` est expliquée par `num_compromised` |

---

## 📁 Structure du Dépôt

```plaintext
.
├── TP2_Regression_Lineaire_Trafic.ipynb  # Notebook Jupyter complet avec étapes et graphiques
├── Train_data.csv                        # Données d'entraînement (NSL-KDD)
├── Test_data.csv                         # Données de test (NSL-KDD)
├── .gitignore                            # Fichiers et dossiers à ignorer par Git
└── README.md                             # Documentation du projet
```

---

## 🛠️ Installation et Exécution

### 1. Prérequis
Assurez-vous d'avoir Python 3.8+ installé ainsi que `pip`.

### 2. Cloner le dépôt
```bash
git clone https://github.com/ACHRAF-KHAIROUNI/nsl-kdd-linear-regression.git
cd nsl-kdd-linear-regression
```

### 3. Installer les dépendances
```bash
pip install numpy pandas matplotlib scikit-learn jupyter
```

### 4. Lancer le Notebook
```bash
jupyter notebook TP2_Regression_Lineaire_Trafic.ipynb
```

---

## 🛡️ Application en Cybersécurité

Dans un environnement de détection d'intrusion (IDS) :
- La droite de régression définit un profil de référence entre les machines compromises et les élévations de privilèges *root*.
- **L'analyse des résidus ($e = y - \hat{y}$)** permet de repérer les attaques furtives ou atypiques où des accès root sont obtenus sans corrélation proportionnelle usuelle avec `num_compromised`.

---

## 👤 Auteur

- **Achraf Khairouni** - [ACHRAF-KHAIROUNI](https://github.com/ACHRAF-KHAIROUNI)
