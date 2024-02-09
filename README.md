# mapcyberpunk2
import matplotlib.pyplot as plt
import networkx as nx
import folium

# --- NetworkX Graph Setup ---
coordinates = {
    "The Sprawl": (35.6804, 139.7690),  
    "Tech Zone": (40.7128, -74.0060), 
    "Old Town": (51.5074, -0.1278),     
    "The Decks": (47.6062, -122.3321), 
}

G = nx.Graph()
G.add_nodes_from(coordinates.keys())
G.add_edge("The Sprawl", "Tech Zone", weight=3)
G.add_edge("Tech Zone", "Old Town", weight=1)
# ... add all your edges with weights

# --- NetworkX Graph Plotting ---
plt.figure(figsize=(8, 6))
edge_widths = [G[u][v]['weight'] for u, v in G.edges()]
nx.draw(G, coordinates, with_labels=True, node_color='skyblue', edge_color='gray', width=edge_widths)
plt.show()

# --- Folium Map Setup ---
map_location = [45.5231, -122.6765]  # Your central map coordinates
map_zoom = 6

# Choose a tile provider (make sure to respect their attribution terms)
map_tiles = 'OpenStreetMap'  # Start with the standard provider
# map_tiles = 'Stamen Toner'  # Uncomment to use Stamen (include attribution) 

map = folium.Map(location=map_location, zoom_start=map_zoom, tiles=map_tiles)

# Add Markers with Popups
for district, coords in coordinates.items():
    popup_html = f"""
                  <h1>{district}</h1>
                  <p>A short description or lore piece goes here...</p> 
                  """
    folium.Marker(coords, popup=popup_html).add_to(map)

# Save the map
map.save("cyberpunk_map.html")
