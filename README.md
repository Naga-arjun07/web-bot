import tkinter as tk
from tkinter import messagebox, scrolledtext
import wikipedia
import requests
import re
from sympy import symbols, Eq, solve, sympify

# === Clean and Convert Math Query ===
def extract_equation(query):
    query = query.lower()
    query = query.replace("equals", "=").replace("equal", "=")
    query = query.replace("plus", "+").replace("minus", "-")
    query = query.replace("times", "*").replace("divided by", "/")
    query = query.replace("to the power of", "**")
    query = re.sub(r'(\d)\s*x', r'\1*x', query)
    return query.strip()

# === Detect if it's a math query ===
def is_math_query(query):
    return bool(re.search(r'[=+\-*/^0-9()]', query))

# === Solve math using SymPy ===
def solve_equation(expr_str):
    try:
        x = symbols("x")
        if "=" in expr_str:
            left, right = expr_str.split("=")
            equation = Eq(sympify(left), sympify(right))
            solution = solve(equation, x)
            return f"x = {solution}"
        else:
            result = sympify(expr_str).evalf()
            return f"The answer is: {result}"
    except Exception as e:
        print("Math error:", e)
        return None

# === Try DuckDuckGo Answer API ===
def get_duckduckgo_answer(query):
    url = "https://api.duckduckgo.com/"
    params = {
        'q': query,
        'format': 'json',
        'no_redirect': 1,
        'no_html': 1
    }
    try:
        res = requests.get(url, params=params)
        data = res.json()
        if data.get("AbstractText"):
            return data["AbstractText"]
        elif data.get("RelatedTopics"):
            for topic in data["RelatedTopics"]:
                if "Text" in topic:
                    return topic["Text"]
    except Exception as e:
        print("DuckDuckGo error:", e)
    return None

# === Main Bot Logic ===
def process_query(query):
    cleaned_query = extract_equation(query)
    print("Cleaned Query:", cleaned_query)

    # Math first
    if is_math_query(cleaned_query):
        result = solve_equation(cleaned_query)
        if result:
            return result

    # Try Wikipedia
    try:
        return wikipedia.summary(query, sentences=2)
    except:
        pass

    # Fallback: DuckDuckGo
    duck_ans = get_duckduckgo_answer(query)
    if duck_ans:
        return duck_ans

    return "Sorry, I couldn't find an answer to that question."

# === On Ask button click ===
def on_submit():
    user_input = input_box.get()
    if not user_input.strip():
        messagebox.showwarning("Input Error", "Please enter a question.")
        return

    if user_input.lower() in ["exit", "quit", "stop"]:
        root.destroy()
        return

    response = process_query(user_input)
    chat_area.config(state='normal')
    chat_area.insert(tk.END, f"You: {user_input}\n", "user")
    chat_area.insert(tk.END, f"iBot: {response}\n\n", "bot")
    chat_area.config(state='disabled')
    chat_area.yview(tk.END)
    input_box.delete(0, tk.END)

# === Button hover effects ===
def on_hover(e):
    submit_button.config(bg="#2e7d32")

def on_leave(e):
    submit_button.config(bg="#388e3c")

# === GUI Dark Mode Colors ===
BG_COLOR = "#1e1e1e"
TEXT_COLOR = "#ffffff"
ENTRY_BG = "#2b2b2b"
CHAT_BG = "#111111"
BTN_COLOR = "#388e3c"
HOVER_COLOR = "#2e7d32"

# === GUI Setup ===
root = tk.Tk()
root.title("iBot - Smart Assistant")
root.geometry("720x520")
root.configure(bg=BG_COLOR)

title_label = tk.Label(root, text="🤖 iBot - Math + Wiki + DuckDuckGo", font=("Helvetica", 16, "bold"),
                       bg=BG_COLOR, fg=TEXT_COLOR)
title_label.pack(pady=10)

chat_area = scrolledtext.ScrolledText(root, font=("Consolas", 12), wrap=tk.WORD,
                                      state='disabled', height=20, width=85,
                                      bg=CHAT_BG, fg=TEXT_COLOR, insertbackground=TEXT_COLOR)
chat_area.pack(padx=15, pady=(0, 10))
chat_area.tag_config("user", foreground="#64b5f6")
chat_area.tag_config("bot", foreground="#81c784")

input_label = tk.Label(root, text="Enter your question below:", font=("Arial", 12, "bold"),
                       bg=BG_COLOR, fg=TEXT_COLOR, anchor="w")
input_label.pack(padx=20, anchor="w")

input_frame = tk.Frame(root, bg=BG_COLOR)
input_frame.pack(padx=20, pady=12, fill=tk.X)

input_box = tk.Entry(input_frame, font=("Arial", 14), width=60, bd=2, relief=tk.GROOVE,
                     bg=ENTRY_BG, fg=TEXT_COLOR, insertbackground=TEXT_COLOR)
input_box.pack(side=tk.LEFT, padx=(0, 10), ipady=6, expand=True, fill=tk.X)

submit_button = tk.Button(input_frame, text="🔍 Ask", font=("Arial", 12, "bold"),
                          bg=BTN_COLOR, fg="white", activebackground=HOVER_COLOR,
                          relief=tk.RAISED, bd=3, padx=20, pady=6, command=on_submit)
submit_button.pack(side=tk.LEFT)
submit_button.bind("<Enter>", on_hover)
submit_button.bind("<Leave>", on_leave)

root.mainloop()











