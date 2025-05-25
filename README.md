# unidad-2-tarea-3-Interfaces-graficas-de-usuario
import tkinter as tk
from tkinter import ttk, messagebox
from math import factorial

# Crear ventana principal
window = tk.Tk()
window.title("Calculadora: Factorial y Conjuntos")
window.geometry("550x500")

### Función para calcular el factorial ###
def calcular_factorial():
    try:
        numero = int(combo_factorial.get())
        if numero < 0:
            raise ValueError("Número negativo")
        resultado = factorial(numero)
        entrada_resultado_factorial.config(state="normal")
        entrada_resultado_factorial.delete(0, tk.END)
        entrada_resultado_factorial.insert(0, resultado)
        entrada_resultado_factorial.config(state="readonly")
    except ValueError:
        messagebox.showerror("Error", "Por favor selecciona un número válido del 1 al 10.")
        entrada_resultado_factorial.config(state="normal")
        entrada_resultado_factorial.delete(0, tk.END)
        entrada_resultado_factorial.insert(0, "Error")
        entrada_resultado_factorial.config(state="readonly")

### Funciones para conjuntos ###
def agregar_a_conjunto(lista, entrada):
    elemento = entrada.get().strip()
    if elemento:
        lista.insert(tk.END, elemento)
        entrada.delete(0, tk.END)
    else:
        messagebox.showwarning("Advertencia", "No se puede agregar un valor vacío.")

def eliminar_de_conjunto(lista):
    seleccionado = lista.curselection()
    if seleccionado:
        lista.delete(seleccionado)
    else:
        messagebox.showinfo("Información", "Seleccione un elemento para eliminar.")

def calcular_conjuntos():
    conjunto_a = set(lista_a.get(0, tk.END))
    conjunto_b = set(lista_b.get(0, tk.END))
    if not conjunto_a or not conjunto_b:
        messagebox.showwarning("Advertencia", "Ambos conjuntos deben tener elementos.")
        return

    resultado = conjunto_a | conjunto_b if opcion_union.get() else conjunto_a & conjunto_b

    lista_resultado.delete(0, tk.END)
    for item in sorted(resultado):
        lista_resultado.insert(tk.END, item)

### Interfaz de Factorial ###
frame_factorial = ttk.LabelFrame(window, text="Cálculo de Factorial")
frame_factorial.pack(padx=10, pady=10, fill="x")

ttk.Label(frame_factorial, text="Selecciona un número:").pack(anchor="w", padx=10, pady=5)
combo_factorial = ttk.Combobox(frame_factorial, values=list(range(0, 11)), state="readonly", width=10)
combo_factorial.pack(padx=10, pady=5)

ttk.Button(frame_factorial, text="Calcular Factorial", command=calcular_factorial).pack(pady=5)

entrada_resultado_factorial = ttk.Entry(frame_factorial, state="readonly", width=20)
entrada_resultado_factorial.pack(padx=10, pady=5)

### Interfaz de Conjuntos ###
frame_conjuntos = ttk.LabelFrame(window, text="Operaciones con Conjuntos")
frame_conjuntos.pack(padx=10, pady=10, fill="both", expand=True)

# Conjunto A
frame_a = ttk.LabelFrame(frame_conjuntos, text="Conjunto A")
frame_a.grid(row=0, column=0, padx=5, pady=5, sticky="n")

entrada_a = ttk.Entry(frame_a, width=15)
entrada_a.pack(pady=2)
ttk.Button(frame_a, text="Agregar", command=lambda: agregar_a_conjunto(lista_a, entrada_a)).pack(pady=2)
ttk.Button(frame_a, text="Eliminar", command=lambda: eliminar_de_conjunto(lista_a)).pack(pady=2)
lista_a = tk.Listbox(frame_a, height=6)
lista_a.pack(padx=5, pady=5)

# Conjunto B
frame_b = ttk.LabelFrame(frame_conjuntos, text="Conjunto B")
frame_b.grid(row=0, column=1, padx=5, pady=5, sticky="n")

entrada_b = ttk.Entry(frame_b, width=15)
entrada_b.pack(pady=2)
ttk.Button(frame_b, text="Agregar", command=lambda: agregar_a_conjunto(lista_b, entrada_b)).pack(pady=2)
ttk.Button(frame_b, text="Eliminar", command=lambda: eliminar_de_conjunto(lista_b)).pack(pady=2)
lista_b = tk.Listbox(frame_b, height=6)
lista_b.pack(padx=5, pady=5)

# Operaciones de conjuntos
frame_opciones = ttk.LabelFrame(frame_conjuntos, text="Operación")
frame_opciones.grid(row=1, column=0, columnspan=2, pady=10)

opcion_union = tk.BooleanVar(value=True)
ttk.Radiobutton(frame_opciones, text="Unión", variable=opcion_union, value=True).pack(side="left", padx=10)
ttk.Radiobutton(frame_opciones, text="Intersección", variable=opcion_union, value=False).pack(side="left", padx=10)

ttk.Button(frame_conjuntos, text="Calcular", command=calcular_conjuntos).grid(row=2, column=0, columnspan=2, pady=10)

# Resultado
lista_resultado = tk.Listbox(frame_conjuntos, height=6)
lista_resultado.grid(row=3, column=0, columnspan=2, pady=5)

# Iniciar aplicación
window.mainloop()
