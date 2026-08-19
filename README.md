# TUTORIAL_INSTALACAO_E_CONFIGURACAO_ESP32_IDF_LINUX
Manual de como fazer a instalação e configuração do ESP32 IDF no ambiente linux. Embora sistemas baseados em Unix tenham complexidades diversas, dependendo do passo a passo pode demorar horas até mesmo utilizando IA.


# Guia Definitivo: Instalação e Configuração do ESP-IDF no Linux + VS Code
Passo 1: Instalação do ESP-IDF no Linux
Instalar as dependências do sistema:
Abra o terminal do Linux e rode:

# Bash
sudo apt update
sudo apt install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0-dev
Baixar o SDK do ESP-IDF (Versão v5.2.2):

# Bash
mkdir -p ~/esp
cd ~/esp
git clone -b v5.2.2 --recursive https://github.com/espressif/esp-idf.git
Executar o instalador das ferramentas:

Bash
cd ~/esp/esp-idf
./install.sh esp32
Passo 2: Preparar o VS Code e Extensões
Abra o VS Code.

Abra a aba de Extensões (Ctrl + Shift + X).

Instale as extensões:

C/C++ (da Microsoft).

ESP-IDF (da Espressif).

Passo 3: Corrigir o IntelliSense (Linhas Vermelhas nos #include)
Para o VS Code reconhecer os cabeçalhos (freertos/FreeRTOS.h, driver/gpio.h, etc.):

No painel de arquivos, abra .vscode/c_cpp_properties.json.

Substitua o conteúdo do arquivo por:

# JSON

{
    "configurations": [
        {
            "name": "ESP-IDF",
            "includePath": [
                "${workspaceFolder}/**",
                "/home/USARIO/esp/esp-idf/components/**"
            ],
            "compilerPath": "/home/USARIO/.espressif/tools/xtensa-esp-elf/esp-13.2.0_20230928/xtensa-esp-elf/bin/xtensa-esp32-elf-gcc",
            "cStandard": "c11",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}
Salve o arquivo (Ctrl + S).

Passo 4: Escrever o Código Mínimo (main/main.c)
Abra o arquivo main/main.c.

Cole o código para piscar o LED da placa (GPIO 2):

C
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"

void app_main(void)
{
    // Configura o pino 2 (LED embutido) como saída digital
    gpio_set_direction(2, GPIO_MODE_OUTPUT);

    while (1) {
        gpio_set_level(2, 1);           // Liga o LED
        vTaskDelay(pdMS_TO_TICKS(500));  // Aguarda 500ms
        gpio_set_level(2, 0);           // Desliga o LED
        vTaskDelay(pdMS_TO_TICKS(500));  // Aguarda 500ms
    }
}
Salve o arquivo (Ctrl + S).

Passo 5: Configurar os Atalhos no ~/.bashrc
Para automatizar o carregamento do ambiente Python e os comandos de gravação e monitoramento:

Abra o arquivo no nano:

# Bash
nano ~/.bashrc
Vá até a última linha do arquivo e cole os atalhos:

# Bash
# Atalhos para ESP-IDF
alias gravar='. $HOME/esp/esp-idf/export.sh && idf.py -p /dev/ttyUSB0 flash'
alias monitorar='. $HOME/esp/esp-idf/export.sh && idf.py -p /dev/ttyUSB0 flash monitor'
Salve e saia do nano:

Aperte Ctrl + O e confirme com Enter.

Aperte Ctrl + X.

Recarregue as configurações do terminal:

# Bash
source ~/.bashrc
Passo 6: Gravando e Usando no Dia a Dia
Para gravar na placa:

Digite apenas no terminal:

# Bash
gravar
Para gravar e abrir o monitor serial ao mesmo tempo:

Bash
monitorar
