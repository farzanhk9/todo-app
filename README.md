import tkinter as tk
from tkinter import messagebox
import json
import os

DATA_FILE = "tasks.json"

def load_tasks():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    return []

def save_tasks(tasks):
    with open(DATA_FILE, "w") as f:
        json.dump(tasks, f, indent=4)

def add_task():
    task = entry.get().strip()
    if task == "":
        messagebox.showwarning("Warning", "Task cannot be empty!")
        return
    tasks.append({"task": task, "done": False})
    save_tasks(tasks)
    refresh_list()
    entry.delete(0, tk.END)

def toggle_task():
    selected = listbox.curselection()
    if not selected:
        return
    index = selected[0]
    tasks[index]["done"] = not tasks[index]["done"]
    save_tasks(tasks)
    refresh_list()

def delete_task():
    selected = listbox.curselection()
    if not selected:
        return
    index = selected[0]
    del tasks[index]
    save_tasks(tasks)
    refresh_list()

def refresh_list():
    listbox.delete(0, tk.END)
    for idx, t in enumerate(tasks):
        status = "✅" if t["done"] else "⏳"
        listbox.insert(tk.END, f"{status} {t['task']}")

# Main window
root = tk.Tk()
root.title("Quick To-Do App 📝")
root.geometry("400x400")

tasks = load_tasks()

# Entry + Button
entry = tk.Entry(root, width=30)
entry.pack(pady=10)

add_btn = tk.Button(root, text="Add Task", command=add_task)
add_btn.pack(pady=5)

# Listbox
listbox = tk.Listbox(root, width=50, height=15)
listbox.pack(pady=10)

# Buttons
toggle_btn = tk.Button(root, text="Mark Done / Undo", command=toggle_task)
toggle_btn.pack(pady=5)

delete_btn = tk.Button(root, text="Delete Task", command=delete_task)
delete_btn.pack(pady=5)

refresh_list()
root.mainloop()
