
# 🚀 Painel de Gerenciamento de Bots no Termux

Este é um painel interativo para o gerenciamento de bots no Termux, utilizando tmux. Com ele, você pode iniciar, parar e monitorar bots desenvolvidos em diversas linguagens, como Python, JavaScript, Shell Script e Java. Ele também inclui uma validação de segurança com uma arte ASCII personalizada para proteger o acesso ao painel. 🎨


---

# 📌 Funcionalidades

✅ Gerenciamento fácil de múltiplos bots no Termux.

✅ Inicia, para e verifica o status de bots em execução.

✅ Validador de segurança com arte ASCII exclusiva.

✅ Instalação automática de dependências.

✅ Compatível com bots desenvolvidos em Python, JavaScript, Shell Script e Java.

✅ Interface de fácil navegação no Terminal.



---

# 📥 Instalação e Uso

1️⃣ Criar o Arquivo do Painel

Abra o Termux e crie um arquivo de script com o nome painel.sh:

nano painel.sh

Agora cole o código abaixo no arquivo e salve (CTRL + X, Y, Enter):

#!/bin/bash

# Instalar dependências automaticamente
install_dependencies() {
    echo "Instalando dependências..."
    pkg update -y && pkg upgrade -y
    pkg install -y lolcat tmux python nodejs openjdk-17 git
    pip install --upgrade pip
    echo "Dependências instaladas com sucesso!"
}

# Verificar e instalar dependências
if ! command -v lolcat &> /dev/null || ! command -v tmux &> /dev/null; then
    install_dependencies
fi

# Arte ASCII com validador
show_validator() {
    clear
    echo -e "\e[34m"
    cat << 'EOF'
 __  __                        ___                         __
/\ \/\ \                 __  /'___\ __                    /\ \
\ \ \ \ \     __   _ __ /\_\/\ \__//\_\    ___     __     \_\ \    ___
 \ \ \ \ \  /'__`\/\`'__\/\ \ \ ,__\/\ \  /'___\ /'__`\   /'_` \  / __`\
  \ \ \_/ \/\  __/\ \ \/ \ \ \ \_/\ \ \/\ \__//\ \L\.\_/\ \L\ \/\ \L\ \
   \ `\___/\ \____\\ \_\  \ \_\ \_\  \ \_\ \____\ \__/.\_\ \___,_\ \____/
    `\/__/  \/____/ \/_/   \/_/\/_/   \/_/\/____/\/__/\/_/\/__,_ /\/___/


 ___                    ___
/\_ \                  /\_ \
\//\ \      __     __  \//\ \    ____    __  _
  \ \ \   /'__`\ /'__`\  \ \ \  /\_ ,`\ /\ \/'\
   \_\ \_/\  __//\ \L\.\_ \_\ \_\/_/  /_\/>  </
   /\____\ \____\ \__/.\_\/\____\ /\____\/\_/\_\
   \/____/\/____/\/__/\/_/\/____/ \/____/\//\/_/
EOF
    echo -e "\e[0m"
    echo -e "\e[34mAproveite o painel!\e[0m"
    sleep 5
}

# Nome da sessão do tmux
SESSION_NAME="discord_bot"

# Função para iniciar um bot
start_bot() {
    read -p "Digite o nome do bot: " bot_name
    read -p "Digite o nome do arquivo principal (ex: main.py, index.js): " bot_file
    
    if [ ! -f "./$bot_file" ]; then
        echo "Arquivo '$bot_file' não encontrado."
        return
    fi

    case "$bot_file" in
        *.py)   cmd="python $bot_file" ;;
        *.js)   cmd="node $bot_file" ;;
        *.sh)   cmd="bash $bot_file" ;;
        *.java) cmd="javac $bot_file" ;;
        *)      echo "Tipo de bot não suportado"; return ;;
    esac

    tmux new-window -t $SESSION_NAME -n "$bot_name" "$cmd"
}

# Função para parar um bot
stop_bot() {
    read -p "Digite o nome do bot: " bot_name
    tmux kill-window -t $SESSION_NAME:"$bot_name" 2>/dev/null
    echo "Bot '$bot_name' parado."
}

# Função para verificar o status dos bots
check_bots() {
    echo "Bots em execução:"
    tmux list-windows -t $SESSION_NAME
}

# Função para validar a segurança do painel
validate_security() {
    clear
    echo -e "\e[34m"
    cat << 'EOF'
 __  __                        ___                         __
/\ \/\ \                 __  /'___\ __                    /\ \
\ \ \ \ \     __   _ __ /\_\/\ \__//\_\    ___     __     \_\ \    ___
 \ \ \ \ \  /'__`\/\`'__\/\ \ \ ,__\/\ \  /'___\ /'__`\   /'_` \  / __`\
  \ \ \_/ \/\  __/\ \ \/ \ \ \ \_/\ \ \/\ \__//\ \L\.\_/\ \L\ \/\ \L\ \
   \ `\___/\ \____\\ \_\  \ \_\ \_\  \ \_\ \____\ \__/.\_\ \___,_\ \____/
    `\/__/  \/____/ \/_/   \/_/\/_/   \/_/\/____/\/__/\/_/\/__,_ /\/___/


 ___                    ___
/\_ \                  /\_ \
\//\ \      __     __  \//\ \    ____    __  _
  \ \ \   /'__`\ /'__`\  \ \ \  /\_ ,`\ /\ \/'\
   \_\ \_/\  __//\ \L\.\_ \_\ \_\/_/  /_\/>  </
   /\____\ \____\ \__/.\_\/\____\ /\____\/\_/\_\
   \/____/\/____/\/__/\/_/\/____/ \/____/\//\/_/
EOF
    echo -e "\e[0m"
    echo -e "\e[34mAproveite o painel!\e[0m"
    sleep 5
}

# Verifica se a sessão do tmux já existe
if ! tmux has-session -t $SESSION_NAME 2>/dev/null; then
    echo "Criando nova sessão do tmux '$SESSION_NAME'..."
    tmux new-session -d -s $SESSION_NAME
fi

# Exibe a arte ASCII
clear
show_validator

# Loop do menu
while true; do
    clear
    show_validator
    echo -e "\e[31m╔════════════════════════════════════════════╗\e[0m"
    echo -e "\e[31m║               MENU DE OPÇÕES               ║\e[0m"
    echo -e "\e[31m╠════════════════════════════════════════════╣\e[0m"
    echo -e "\e[31m║ 1. Ligar Bot                               ║\e[0m"
    echo -e "\e[31m║ 2. Parar Bot                               ║\e[0m"
    echo -e "\e[31m║ 3. Ver Status dos Bots                     ║\e[0m"
    echo -e "\e[31m║ 4. Sair                                    ║\e[0m"
    echo -e "\e[31m║ 000. Validar Segurança do Painel           ║\e[0m"
    echo -e "\e[31m╚════════════════════════════════════════════╝\e[0m"
    read -p "Escolha uma opção: " choice
    case $choice in
        1) start_bot ;;
        2) stop_bot ;;
        3) check_bots ;;
        4) echo "Saindo..."; break ;;
        000) validate_security ;;
        *) echo "Opção inválida."; sleep 1 ;;
    esac
done


---

 # 2️⃣ Definir Permissões de Execução

Após criar o arquivo, torne-o executável com o comando:

chmod +x painel.sh

3️⃣ Executar o Painel

Agora, para rodar o painel, execute:

./painel.sh


---

# ⚙️ Dependências

Este painel requer as seguintes dependências, que serão instaladas automaticamente ao rodar o script:

lolcat: Para exibir a arte ASCII com efeito de texto divertido.

tmux: Para gerenciamento de sessões de terminal.

python, nodejs, openjdk-17: Para execução de bots em diversas linguagens.

git: Para controle de versões e atualizações.



---

# 📄 Como Funciona?

Iniciar o Bot: Você pode iniciar um bot de acordo com o arquivo (Python, JavaScript, Shell Script ou Java).

Parar o Bot: Basta fornecer o nome do bot para parar a sessão no tmux.

Ver Status: Veja os bots que estão em execução no momento.

Validação de Segurança: Exibe uma arte ASCII especial e verifica se o painel está seguro para uso.



---

# 👨‍💻 Contribuições

Sinta-se à vontade para contribuir! Abra um pull request ou issue no repositório.


---

💬 Contato

Se tiver dúvidas ou sugestões, entre em contato No discord em 

https://discord.gg/5QkPgNwYqR
---

Uma dica Não clone o repositório!!!

