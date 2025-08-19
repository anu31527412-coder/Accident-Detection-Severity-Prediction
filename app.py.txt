import streamlit as st
import pandas as pd
import pickle

# Load trained model
model = pickle.load(open("models/accident_model.pkl", "rb"))

st.title("🚦 Accident Detection and Severity Prediction")

# Input fields
weather = st.selectbox("Weather Condition", ["Clear", "Rainy", "Snowy"])
road = st.selectbox("Road Type", ["Highway", "Urban", "Rural"])
vehicle_speed = st.slider("Vehicle Speed (km/h)", 0, 200, 60)

# Example encoding
weather_map = {"Clear": 0, "Rainy": 1, "Snowy": 2}
road_map = {"Highway": 0, "Urban": 1, "Rural": 2}

# Convert inputs to DataFrame
input_data = pd.DataFrame({
    "weather": [weather_map[weather]],
    "road": [road_map[road]],
    "speed": [vehicle_speed]
})

if st.button("Predict Severity"):
    prediction = model.predict(input_data)[0]
    st.success(f"Predicted Severity: {prediction}")
