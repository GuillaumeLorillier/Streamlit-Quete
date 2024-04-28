import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import streamlit as st

st.title('Dataset Voitures')

df = pd.read_csv(r'https://raw.githubusercontent.com/murpi/wilddata/master/quests/cars.csv')

df['continent'] = df['continent'].str.strip().str.replace('.', '')

df

st.write('Heatmap de corrélation entre les valeurs numériques')

# Sélectionnez uniquement les colonnes numériques
numeric_df = df.select_dtypes(include=[float, int])

# Calculez la matrice de corrélation
corr_matrix = numeric_df.corr()
# Créer un masque pour la partie haute du heatmap
mask = np.triu(np.ones_like(corr_matrix, dtype=bool))  

fig = plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', center=0, mask=mask)
st.pyplot(fig)

#Affichage de la distribution de tous les plot en utilisant la méthode de sturges pour les bins en histogramme avec seaborn
#subplot avec 4 lignes et 2 colonnes

st.write('Distribution des variables')


fig, axes = plt.subplots(4, 2, figsize=(12, 14))
#fig.suptitle('Distribution des variables', fontsize=20)
fig.tight_layout(pad=3.0)

for i, col in enumerate(df.columns[0:]):
    sns.histplot(df[col], bins='sturges', kde=True, ax=axes[i//2, i%2])
    axes[i//2, i%2].set_title(col)
    axes[i//2, i%2].set_xlabel('')
    axes[i//2, i%2].set_ylabel('')

st.pyplot(fig)


st.write('Graph avec selection par pays')

# Utilisez `selectbox` pour permettre à l'utilisateur de choisir la région
region = st.selectbox("Filtrer par région", options=["US", "Europe", "Japan"], index=0)

# Filtrez le DataFrame en fonction de la région sélectionnée
filtered_df = df[df['continent'] == region]

# Créez une seule colonne qui occupe toute la largeur de la page
main_col = st.columns(1)[0]

# Créez deux colonnes côte à côte
col1, col2 = main_col.columns(2)

# Colonne 1 : Afficher le scatterplot
with col1:
    # Créez une figure pour afficher le scatterplot
    fig_scatter, ax_scatter = plt.subplots(figsize=(12, 8))

    # Tracez le scatterplot avec sns.scatterplot()
    sns.scatterplot(data=filtered_df, x='weightlbs', y='hp', ax=ax_scatter, color='b')

    # Ajoutez un titre et des étiquettes d'axes
    ax_scatter.set_title(f"Scatterplot de weightlbs vs hp pour la région {region}")
    ax_scatter.set_xlabel('Weight lbs')
    ax_scatter.set_ylabel('HP')

    # Affichez la figure dans la colonne 1
    st.pyplot(fig_scatter)

# Colonne 2 : Afficher le bar chart
with col2:
    # Calculez la moyenne de `time-to-60` par année pour les données filtrées
    average_time_to_60_by_year = filtered_df.groupby('year')['time-to-60'].mean().reset_index()

    # Créez une figure pour afficher le bar chart
    fig_bar, ax_bar = plt.subplots(figsize=(12, 8))

    # Tracez le bar chart avec sns.barplot()
    sns.barplot(x='year', y='time-to-60', data=average_time_to_60_by_year, ax=ax_bar, hue='year')

    # Ajoutez un titre et des étiquettes d'axes
    ax_bar.set_title(f'Moyenne de time-to-60 par année pour la région {region}')
    ax_bar.set_xlabel('Année')
    ax_bar.set_ylabel('Moyenne de time-to-60')

    # Affichez la figure dans la colonne 2
    st.pyplot(fig_bar)

    