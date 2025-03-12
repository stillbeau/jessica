import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

def mm_to_inches(mm_value):
    """Convert millimeters to inches (1 inch = 25.4 mm)."""
    return mm_value / 25.4

@st.cache_data
def load_data():
    # Replace with your CSV export URL. Make sure your sheet is shared publicly.
    url = "https://docs.google.com/spreadsheets/d/15NJIEEk-CX--sNW9vyDl20755RgpuW8AmcSqBac7I4s/export?format=csv"
    data = pd.read_csv(url)
    return data

# --- Load the data ---
data = load_data()

st.title("Counter Drawing Viewer")
st.markdown("Select a Location from the dropdown below to view the counter drawing and details.")

# --- Create the dropdown ---
locations = data['Location'].unique()
selected_loc = st.selectbox("Select Location:", locations)

# --- Retrieve the selected row ---
row = data[data['Location'] == selected_loc].iloc[0]

# Extract values (using exact column names from your sheet)
location = row['Location']
ahu = row['AHU']
priority = row['Priority']
width_mm = row['Width']
depth_mm = row['Depth']
finish = row.get('Finish', 'N/A') or 'N/A'
rolled_width = row.get('Rolled width', 'N/A')
backsplash = row.get('Backsplash', 'N/A')
cap_lh = row.get('Cap LH', 'N/A')
cap_rh = row.get('Cap RH', 'N/A')
comments = row.get('Comments', '')

# --- Convert dimensions from mm to inches ---
width_in = mm_to_inches(width_mm)
depth_in = mm_to_inches(depth_mm)

# --- Create the drawing with Matplotlib ---
fig, ax = plt.subplots(figsize=(10, 6))

# Draw the rectangle (the counter piece)
rect = plt.Rectangle((0, 0), width_in, depth_in, fill=False, edgecolor='blue', linewidth=2)
ax.add_patch(rect)

# Annotate dimensions outside the rectangle:
# Bottom side (Width)
ax.annotate("", xy=(0, -0.5), xytext=(width_in, -0.5),
            arrowprops=dict(arrowstyle="<->", lw=1.5))
ax.text(width_in/2, -0.7, f"{width_in:.2f} in", ha="center", va="top", fontsize=10)

# Top side (Width)
ax.annotate("", xy=(0, depth_in+0.5), xytext=(width_in, depth_in+0.5),
            arrowprops=dict(arrowstyle="<->", lw=1.5))
ax.text(width_in/2, depth_in+0.7, f"{width_in:.2f} in", ha="center", va="bottom", fontsize=10)

# Left side (Depth)
ax.annotate("", xy=(-0.5, 0), xytext=(-0.5, depth_in),
            arrowprops=dict(arrowstyle="<->", lw=1.5))
ax.text(-0.7, depth_in/2, f"{depth_in:.2f} in", ha="right", va="center", fontsize=10, rotation=90)

# Right side (Depth)
ax.annotate("", xy=(width_in+0.5, 0), xytext=(width_in+0.5, depth_in),
            arrowprops=dict(arrowstyle="<->", lw=1.5))
ax.text(width_in+0.7, depth_in/2, f"{depth_in:.2f} in", ha="left", va="center", fontsize=10, rotation=90)

# --- Additional Information ---
# Top info inside the piece (Location and AHU)
top_info_text = f"Location: {location}\nAHU: {ahu}"
ax.text(width_in/2, depth_in * 0.95, top_info_text, fontsize=10, ha="center", va="top",
        bbox=dict(facecolor='white', alpha=0.8, edgecolor='none'))

# Bottom info (below the piece)
bottom_info_text = (
    f"Priority: {priority}\n"
    f"Backsplash: {backsplash}\n"
    f"Cap LH: {cap_lh}\n"
    f"Cap RH: {cap_rh}\n"
    f"Rolled width: {rolled_width}\n"
    f"Finish: {finish}\n"
    f"Comments: {comments}"
)
ax.text(width_in/2, -3.5, bottom_info_text, fontsize=10, ha="center", va="top",
        bbox=dict(facecolor='white', alpha=0.8, edgecolor='gray'))

# Set plot limits to ensure everything is visible
ax.set_xlim(-2, width_in + 2)
ax.set_ylim(-4, depth_in + 2)
ax.set_aspect('equal', adjustable='box')
plt.axis('off')

# --- Display the drawing in Streamlit ---
st.pyplot(fig)

# --- Print Instructions ---
st.info("To print this drawing, please use your browser's print function (e.g., press Ctrl+P or Cmd+P).")
