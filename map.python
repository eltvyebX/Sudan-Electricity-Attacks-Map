
import folium
from folium.plugins import Fullscreen
import pandas as pd

# --------------------------------------------------
# 1. Dataset
# Coordinates are approximate and should be verified.
# --------------------------------------------------

data = [
    {
        "location": "Dongola Power Station",
        "latitude": 19.1755,
        "longitude": 30.4359,
        "attacks": 1
    },
    {
        "location": "Merowe Dam",
        "latitude": 18.6689,
        "longitude": 32.0503,
        "attacks": 3
    },
    {
        "location": "Berber Power Station",
        "latitude": 18.02,
        "longitude": 33.98,
        "attacks": 1
    },
    {
        "location": "Atbara Transformer Station",
        "latitude": 17.70,
        "longitude": 33.98,
        "attacks": 3
    },
    {
        "location": "Umm Dabakir Power Station",
        "latitude": 13.20676,
        "longitude": 32.82667,
        "attacks": 6
    },
    {
        "location": "Al-Shawak Power Station",
        "latitude": 14.30,
        "longitude": 35.75,
        "attacks": 1
    },
    {
        "location": "Al-Mirkhiat Distribution Station",
        "latitude": 15.65,
        "longitude": 32.43,
        "attacks": 4
    }
]

df = pd.DataFrame(data)

# --------------------------------------------------
# 2. Create map
# --------------------------------------------------

m = folium.Map(
    location=[16.5, 32.5],
    zoom_start=6,
    tiles="OpenStreetMap",
    control_scale=True
)

# --------------------------------------------------
# 3. Define marker colors
# --------------------------------------------------

def get_color(attacks):
    if attacks <= 1:
        return "#F4A261"
    elif attacks <= 3:
        return "#E76F51"
    else:
        return "#B2182B"


# --------------------------------------------------
# 4. Add facility markers
# --------------------------------------------------

for _, row in df.iterrows():

    color = get_color(row["attacks"])

    popup_html = f"""
    <div style="font-family: Arial; width: 220px;">
        <h4 style="margin-bottom: 8px;">
            {row['location']}
        </h4>
        <p>
            <b>Number of attacks:</b>
            {row['attacks']}
        </p>
        <p>
            <b>Coordinates:</b><br>
            {row['latitude']}, {row['longitude']}
        </p>
    </div>
    """

    folium.CircleMarker(
        location=[
            row["latitude"],
            row["longitude"]
        ],
        radius=5 + row["attacks"] * 2,
        color=color,
        fill=True,
        fill_color=color,
        fill_opacity=0.75,
        weight=2,
        popup=folium.Popup(
            popup_html,
            max_width=300
        ),
        tooltip=f"{row['location']} | {row['attacks']} attacks"
    ).add_to(m)


# --------------------------------------------------
# 5. Add legend
# --------------------------------------------------

legend_html = """
<div style="
    position: fixed;
    bottom: 35px;
    left: 35px;
    width: 190px;
    background-color: white;
    border: 1px solid #ccc;
    z-index: 9999;
    padding: 12px;
    font-family: Arial;
    font-size: 13px;
    border-radius: 5px;
">
    <b>Number of reported attacks</b>
    <br><br>

    <span style="color:#F4A261;">●</span>
    1 attack
    <br>

    <span style="color:#E76F51;">●</span>
    2–3 attacks
    <br>

    <span style="color:#B2182B;">●</span>
    4–6 attacks
    <br><br>

    <small>Circle size represents attack count.</small>
</div>
"""

m.get_root().html.add_child(
    folium.Element(legend_html)
)

# --------------------------------------------------
# 6. Add fullscreen control
# --------------------------------------------------

Fullscreen(
    position="topright"
).add_to(m)

# --------------------------------------------------
# 7. Save interactive map
# --------------------------------------------------

m.save("sudan_power_attacks_map.html")

print("Map successfully created!")
print("Total facilities:", len(df))
print("Total reported attacks:", df["attacks"].sum())

m
