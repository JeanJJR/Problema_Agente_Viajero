import tkinter as tk
from tkinter import messagebox
import random
import networkx as nx
import matplotlib.pyplot as plt

def generar_matriz(n):
    matriz = [[0] * n for _ in range(n)]
    for i in range(n):
        for j in range(i + 1, n):
            matriz[i][j] = random.randint(1, 100)
            matriz[j][i] = matriz[i][j]
    return matriz

def calcular_distancia_total(camino, distancias):
    distancia_total = 0
    for i in range(len(camino) - 1):
        distancia_total += distancias[camino[i]][camino[i + 1]]
    return distancia_total

def resolver_tsp():
    n = int(entry_n.get())
    if n < 5 or n > 15:
        messagebox.showerror("Error", "El valor de 'n' debe estar entre 5 y 15.")
        return
    
    distancias = []
    for i in range(n):
        fila = []
        for j in range(n):
            valor = entry_matrix[i][j].get()
            fila.append(int(valor))
        distancias.append(fila)
    
    result_label.config(text="Calculando...")
    root.update_idletasks()
    
    ciudad_inicial = 0  # Puedes cambiar la ciudad de inicio si lo deseas
    visitadas = [False] * n
    camino = [ciudad_inicial]
    visitadas[ciudad_inicial] = True
    
    for _ in range(n - 1):
        ciudad_actual = camino[-1]
        ciudad_mas_cercana = None
        distancia_minima = float("inf")
        
        for ciudad in range(n):
            if not visitadas[ciudad] and distancias[ciudad_actual][ciudad] < distancia_minima:
                ciudad_mas_cercana = ciudad
                distancia_minima = distancias[ciudad_actual][ciudad]
        
        camino.append(ciudad_mas_cercana)
        visitadas[ciudad_mas_cercana] = True
    
    camino.append(ciudad_inicial)
    distancia_total = calcular_distancia_total(camino, distancias)
    
    result_label.config(text=f"Ciclo hamiltoniano: {camino}\nDistancia total: {distancia_total}")
    
    G = nx.Graph()
    for i in range(n):
        for j in range(i + 1, n):
            G.add_edge(i, j, weight=distancias[i][j])
    pos = nx.spring_layout(G)  # Configura la posición de los nodos
    nx.draw(G, pos, with_labels=True, node_size=500, font_size=10, font_color="black")
    etiquetas_aristas = nx.get_edge_attributes(G, "weight")
    nx.draw_networkx_edge_labels(G, pos, edge_labels=etiquetas_aristas)
    plt.show()

def generar_matriz_y_mostrar():
    n = int(entry_n.get())
    distancias = generar_matriz(n)
    
    for i in range(n):
        for j in range(n):
            entry_matrix[i][j].delete(0, tk.END)
            entry_matrix[i][j].insert(0, str(distancias[i][j]))

def limpiar_matriz():
    for i in range(len(entry_matrix)):
        for j in range(len(entry_matrix[i])):
            entry_matrix[i][j].delete(0, tk.END)

def cerrar_programa():
    root.destroy()

root = tk.Tk()
root.title("Solucionador del PAV")

label_n = tk.Label(root, text="Ingrese la cantidad de ciudades (entre 5 y 15):")
label_n.pack()

entry_n = tk.Entry(root)
entry_n.pack()

matrix_frame = tk.Frame(root)
matrix_frame.pack()

entry_matrix = []
for i in range(15):  # Limitado a 15 filas para simplificar la interfaz
    fila_entradas = []
    for j in range(15):  # Limitado a 15 columnas para simplificar la interfaz
        entrada = tk.Entry(matrix_frame, width=5)
        entrada.grid(row=i, column=j)
        fila_entradas.append(entrada)
    entry_matrix.append(fila_entradas)

generar_button = tk.Button(root, text="Generar Matriz", command=generar_matriz_y_mostrar)
generar_button.pack()

limpiar_button = tk.Button(root, text="Limpiar Matriz", command=limpiar_matriz)
limpiar_button.pack()

resolver_button = tk.Button(root, text="Resolver PAV", command=resolver_tsp)
resolver_button.pack()

result_label = tk.Label(root, text="", wraplength=300)
result_label.pack()

cerrar_button = tk.Button(root, text="Cerrar Programa", command=cerrar_programa, bg="red", fg="white")  # Cambia el color de fondo (bg) a rojo y el color de texto (fg) a blanco
cerrar_button.pack()

root.mainloop()
