import tkinter as tk
import random
import os
import sys
from PIL import Image, ImageTk

# Tentativa de importar pygame para sons
try:
    import pygame
    pygame.mixer.init()
    SOM_ATIVO = True
except ImportError:
    SOM_ATIVO = False
    print("Aviso: pygame não encontrado. Sons desativados.")

# Define o diretório base corretamente
BASE_DIR = os.path.dirname(os.path.abspath(sys.argv[0]))

# Lista de perguntas com imagens e sons apenas para eventos
perguntas = [
    {"nome": "Pikachu", "imagem": "pikachu.png", "opcoes": ["Pikachu", "Charmander", "Bulbasauro"], "correta": "Pikachu"},
    {"nome": "Charmander", "imagem": "charmander.png", "opcoes": ["Squirtle", "Charmander", "Jigglypuff"], "correta": "Charmander"},
    {"nome": "Bulbasauro", "imagem": "bulbasauro.png", "opcoes": ["Bulbasauro", "Meowth", "Eevee"], "correta": "Bulbasauro"},
]

# Função para tocar sons apenas nos momentos corretos
def tocar_som(nome_arquivo):
    if SOM_ATIVO:
        caminho_som = os.path.join(BASE_DIR, "sons", nome_arquivo)
        if os.path.exists(caminho_som):
            pygame.mixer.Sound(caminho_som).play()
        else:
            print(f"Erro: Som {caminho_som} não encontrado")

# Configuração da interface
janela = tk.Tk()
janela.title("Jogo Anime")
janela.geometry("450x550")
janela.configure(bg="#ADD8E6")  # Fundo azul claro mediano

# Canvas para imagem principal
canvas = tk.Canvas(janela, width=180, height=180, bg="#FFF5F5", highlightthickness=3, highlightbackground="#FF69B4")
canvas.pack(pady=20)

resposta_label = tk.Label(janela, text="", font=("Arial", 16, "bold"), bg="#ADD8E6", fg="black")
resposta_label.pack()

botoes = []
pontuacao = 0
pergunta_atual = None
imagem_tk = None

# Função para carregar uma nova pergunta
def carregar_pergunta():
    global imagem_tk, pergunta_atual

    resposta_label.config(text="")
    pergunta_atual = random.choice(perguntas)

    caminho_imagem = os.path.join(BASE_DIR, "imagens", pergunta_atual["imagem"])
    
    if not os.path.exists(caminho_imagem):
        resposta_label.config(text=f"Erro: {pergunta_atual['imagem']} não encontrada!", fg="red")
        return

    try:
        imagem = Image.open(caminho_imagem)
        imagem = imagem.resize((180, 180))
        imagem_tk = ImageTk.PhotoImage(imagem)
        canvas.delete("all")
        canvas.create_image(90, 90, image=imagem_tk)
    except Exception as e:
        resposta_label.config(text=f"Erro ao carregar imagem: {e}", fg="red")
        return

    for widget in botoes:
        widget.destroy()
    botoes.clear()

    for opcao in pergunta_atual["opcoes"]:
        botao = tk.Button(janela, text=opcao, font=("Arial", 14, "bold"),
                          bg="#FF0000", fg="white", activebackground="#0000FF",  # Red and blue design
                          relief="ridge", borderwidth=3, padx=10, pady=5,
                          command=lambda o=opcao: verificar_resposta(o))
        botao.pack(pady=5)
        botoes.append(botao)

# Função para verificar a resposta
def verificar_resposta(resposta):
    global pontuacao
    if resposta == pergunta_atual["correta"]:
        pontuacao += 1
        resposta_label.config(text="✔️ Correto!", fg="green")
        tocar_som("vitoria.wav")
    else:
        resposta_label.config(text=f"❌ Errado! Resposta correta: {pergunta_atual['correta']}", fg="red")
        tocar_som("derrota.wav")
    
    janela.after(2000, carregar_pergunta)

# Função para avançar de fase
def proxima_fase():
    tocar_som("proxima_fase.wav")
    carregar_pergunta()

# Botão para próxima fase
botao_proximo = tk.Button(janela, text="🎮Próxima Fase ▶", font=("Arial", 14, "bold"),
                          bg="#800080", fg="white", activebackground="#DA70D6",
                          relief="ridge", borderwidth=3, padx=10, pady=5,
                          command=proxima_fase)
botao_proximo.pack(pady=20)

# Inicia o jogo
carregar_pergunta()
janela.mainloop()