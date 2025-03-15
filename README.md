from collections import deque
import heapq

class Problem():
    def __init__(self, knowledge, start_node, end_node):
        self.knowledge = knowledge
        self.start_node = start_node
        self.end_node = end_node

    # Retrieving graph
    def get_map(self):
        return self.knowledge

    # Retrieving start_node
    def get_startNode(self):
        return self.start_node

    # Retrieving end_node
    def get_endNode(self):
        return self.end_node

    # Dijkstra's Algorithm
    def dijkstra(self):
        graph = self.knowledge
        start = self.start_node
        end = self.end_node
        
        # Priority queue untuk menyimpan (cost, node)
        pq = [(0, start)]
        distances = {node: float('inf') for node in graph}
        distances[start] = 0
        previous_nodes = {node: None for node in graph}

        while pq:
            current_distance, current_node = heapq.heappop(pq)
            
            if current_node == end:
                break
            
            for neighbor, weight in graph[current_node].items():
                distance = current_distance + weight
                
                if distance < distances[neighbor]:
                    distances[neighbor] = distance
                    previous_nodes[neighbor] = current_node
                    heapq.heappush(pq, (distance, neighbor))
        
        # Rekonstruksi jalur terpendek
        path = []
        node = end
        while node is not None:
            path.append(node)
            node = previous_nodes[node]
        
        path.reverse()
        return path, distances[end]

# Contoh penggunaan
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}

problem = Problem(graph, 'A', 'D')
shortest_path, cost = problem.dijkstra()
print("Jalur Terpendek:", shortest_path)
print("Biaya Minimum:", cost)
