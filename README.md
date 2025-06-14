# app.py
teste pour concentratio du co2
# fichier : app.py

import streamlit as st
import matplotlib.pyplot as plt

st.title("🧪 Simulation de la concentration de CO₂ selon le temps et l’occupation")

st.markdown("""
Cette application modélise l’accumulation de **CO₂** dans un espace fermé en fonction du nombre de visiteurs et de la durée de la visite.
""")

# === Entrées utilisateur ===
volume = st.number_input("📐 Volume de l'espace (m³)", value=3100)
nb_personnes = st.number_input("👥 Nombre de visiteurs", min_value=1, value=20)
duree = st.slider("⏱️ Durée de la visite (minutes)", 1, 240, 60)
co2_initial = st.number_input("🌿 CO₂ initial (ppm)", value=420)
production_par_personne = st.number_input("💨 Production de CO₂ par personne (ppm/min)", value=0.005)
perte_naturelle = st.number_input("🌀 Déperdition naturelle (ppm/min)", value=0.001)

# === Simulation ===
co2 = [co2_initial]
for t in range(duree):
    generation = nb_personnes * production_par_personne
    perte = perte_naturelle * (co2[-1] - 400)  # 400 = fond naturel
    variation = generation - perte
    co2.append(co2[-1] + variation)

# === Affichage du graphique ===
fig, ax = plt.subplots()
ax.plot(range(duree+1), co2, label="Concentration CO₂ (ppm)", color='green')
ax.axhline(1000, color='red', linestyle='--', label="Seuil recommandé (1000 ppm)")
ax.set_xlabel("Temps (minutes)")
ax.set_ylabel("CO₂ (ppm)")
ax.set_title("Évolution de la concentration de CO₂")
ax.legend()
st.pyplot(fig)

# === Résultat final ===
st.success(f"🔍 CO₂ final après {duree} minutes : **{round(co2[-1])} ppm**")
