import pywhatkit as kit
import pyautogui
import time
import tkinter as tk
from tkinter import ttk, messagebox

class WhatsAppBotApp:
    def __init__(self, root):
        self.root = root
        self.root.title("WhatsApp Bot")
        self.root.geometry("500x400")
        self.root.configure(bg="#f0f0f0")

        # Estilo personalizado para os botões
        self.style = ttk.Style()
        self.style.configure("TButton",
                             font=("Arial", 12, "bold"),
                             padding=10,
                             relief="flat",
                             background="#4CAF50",
                             foreground="white")

        # Criando widgets
        self.label = tk.Label(root, text="Número do Destinatário (1-5):", font=("Arial", 12), bg="#f0f0f0")
        self.label.pack(pady=10)

        # Entradas para os números (permitindo até 5 números)
        self.numero_entries = []
        for i in range(5):
            entry = tk.Entry(root, font=("Arial", 12), justify="center", width=20)
            entry.pack(pady=5)
            self.numero_entries.append(entry)
        
        # Inserir números padrão para testes
        self.numero_entries[0].insert(0, "+55XXXXXXXXXXX")
        self.numero_entries[1].insert(0, "+55XXXXXXXXXXX")
        self.numero_entries[2].insert(0, "+55XXXXXXXXXXX")
        self.numero_entries[3].insert(0, "+55XXXXXXXXXXX")
        self.numero_entries[4].insert(0, "+55XXXXXXXXXXX")

        self.label_mensagem = tk.Label(root, text="Mensagem:", font=("Arial", 12), bg="#f0f0f0")
        self.label_mensagem.pack(pady=10)

        self.entry_mensagem = tk.Entry(root, font=("Arial", 12), justify="center", width=30)
        self.entry_mensagem.pack(pady=5)
        self.entry_mensagem.insert(0, "Olá! Esta é uma mensagem automática do bot.")

        # Botão de envio
        self.botao_enviar = tk.Button(root, text="Enviar Mensagens", font=("Arial", 12, "bold"),
                                      bg="#4CAF50", fg="white", relief="flat", bd=0,
                                      activebackground="#45a049", cursor="hand2",
                                      command=self.enviar_mensagens)
        self.botao_enviar.pack(pady=20)

        # Estilização de botões com efeito pulsante
        self.botao_enviar.bind("<Enter>", self.pulsar_botao)
        self.botao_enviar.bind("<Leave>", self.reverter_botao)

    def enviar_mensagens(self):
        # Obter a mensagem
        mensagem = self.entry_mensagem.get()

        # Verificar se há pelo menos um número
        numeros_destinatarios = [entry.get() for entry in self.numero_entries if entry.get()]
        
        if len(numeros_destinatarios) == 0:
            messagebox.showwarning("Erro", "Por favor, insira pelo menos um número de destinatário.")
            return
        
        for numero_destinatario in numeros_destinatarios:
            if not numero_destinatario.startswith("+") or len(numero_destinatario) < 10:
                messagebox.showwarning("Erro", f"Número inválido: {numero_destinatario}. Use o formato +55XXXXXXXXXXX")
                return

            # Enviar mensagem para cada número
            kit.sendwhatmsg_instantly(numero_destinatario, mensagem)

            # Tempo para carregar o WhatsApp Web antes de enviar
            time.sleep(5)

            # Aperta "Enter" para enviar
            pyautogui.press("enter")
        
        messagebox.showinfo("Sucesso", f"Mensagem enviada para {len(numeros_destinatarios)} números com sucesso!")

    def pulsar_botao(self, event):
        self.botao_enviar.config(bg="#45a049", font=("Arial", 13, "bold"))

    def reverter_botao(self, event):
        self.botao_enviar.config(bg="#4CAF50", font=("Arial", 12, "bold"))

if __name__ == "__main__":
    root = tk.Tk()
    app = WhatsAppBotApp(root)
    root.mainloop()
