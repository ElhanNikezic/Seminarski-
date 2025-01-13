# Seminarski-
Graf algoritmi predstavljaju osnovne komponente za rješavanje problema u mnogim realnim aplikacijama, od analize mreža do optimizacije transportnih sistema. Ovo istraživanje istražuje primjene graf algoritama u različitim industrijama, kao i njihovu primjenu u rješavanju konkretnih problema.
import networkx as nx
import matplotlib.pyplot as plt
import random
import community as community_louvain

# Kreiranje nasumičnog grafika sa 20 čvorova i verovatnoćom za povezivanje
def create_random_graph(num_nodes, probability):
    G = nx.erdos_renyi_graph(num_nodes, probability)
    return G

# Dijkstra algoritam - Najkraći put između dva čvora
def dijkstra(graph, start, end):
    try:
        path = nx.dijkstra_path(graph, start, end)
        length = nx.dijkstra_path_length(graph, start, end)
        print(f"Najkraći put između {start} i {end}: {path}, dužina puta: {length}")
        return path, length
    except nx.NetworkXNoPath:
        print(f"Nema puta između {start} i {end}")
        return None, None

# BFS algoritam - Pretraga u širinu
def bfs(graph, start):
    bfs_tree = nx.bfs_tree(graph, start)
    bfs_nodes = list(bfs_tree.nodes)
    print(f"BFS pretraga od {start}: {bfs_nodes}")
    return bfs_tree

# DFS algoritam - Pretraga u dubinu
def dfs(graph, start):
    dfs_tree = nx.dfs_tree(graph, start)
    dfs_nodes = list(dfs_tree.nodes)
    print(f"DFS pretraga od {start}: {dfs_nodes}")
    return dfs_tree
# Louvain algoritam - Prepoznavanje zajednica
def louvain(graph):
    partition = community_louvain.best_partition(graph)
    # Kreiranje zajednica na osnovu partitiona
    communities = {}
    for node, community in partition.items():
        if community not in communities:
            communities[community] = []
        communities[community].append(node)
print("Prepoznate zajednice:", communities)
    return communities

# Vizualizacija grafa sa oznakama i bojenjem čvorova prema zajednicama
def visualize_graph(graph, communities=None):
    plt.figure(figsize=(10, 8))
    
    # Ako su zajednice prepoznate, bojimo čvorove prema zajednicama
    if communities:
        color_map = []
        for node in graph.nodes():
            for community, nodes in communities.items():
                if node in nodes:
                    color_map.append(community)
                    break
        nx.draw(graph, with_labels=True, font_weight='bold', node_color=color_map, cmap=plt.cm.rainbow,
                node_size=3000, font_size=12, edge_color="gray")
    else:
        nx.draw(graph, with_labels=True, font_weight='bold', node_color='skyblue', node_size=3000, font_size=12, edge_color="gray")

    plt.title("Vizualizacija grafa")
    plt.show()

# Generisanje grafova sa različitim parametrima
def generate_graphs():
    G1 = create_random_graph(20, 0.3)
    G2 = create_random_graph(20, 0.5)
    return G1, G2

# Testiranje algoritama na generisanim grafovima
def test_graph_algorithms():
    G1, G2 = generate_graphs()
    
    print("\nTestiranje grafika G1 sa 20 čvorova i verovatnoćom 0.3:")
    # Testiranje Dijkstra algoritma
    dijkstra(G1, 0, 15)
    # Testiranje BFS algoritma
    bfs(G1, 0)
    # Testiranje DFS algoritma
    dfs(G1, 0)
