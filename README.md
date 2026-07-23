# 🏎️ Cronômetro Digital para Pista de Carrinho Seguidor de Linha

> Sistema de cronometragem de precisão alimentado por **ESP32**, integrado com barreira infravermelha (IR) e interface Web acessível via Wi-Fi interno.

---

## 📌 Sumário

- [Sobre o Projeto](#-sobre-o-projeto)
- [Estrutura Física e Modelagem 3D](#-estrutura-física-e-modelagem-3d)
- [Como Configurar e Executar](#-como-configurar-e-executar)
- [Interface Web](#-interface-web)

---

## 📖 Sobre o Projeto

Este projeto consiste em um cronômetro automatizado desenvolvido para medir o tempo de volta de carrinhos arduinos seguidores de linha em pistas de competição. 

[cite_start]O sistema utiliza um microcontrolador **ESP32** atuando como Ponto de Acesso (Access Point - AP) e servidor HTTP local[cite: 1, 4]. [cite_start]Ao interromper o feixe óptico infravermelho (IR)[cite: 1, 3], o tempo é disparado/pausado automaticamente, enviando os dados em tempo real para qualquer dispositivo conectado na rede local (computadores, tablets ou smartphones).

---

## 🛠️ Componentes do Hardware

| Componente | Quantidade | Função |
| :--- | :---: | :--- |
| **ESP32** | 1 | [cite_start]Microcontrolador principal, gerador do Wi-Fi e Servidor Web [cite: 1, 4] |
| **LED Emissor IR** | 1 | [cite_start]Emite o feixe óptico infravermelho modulado [cite: 1] |
| **Sensor Receptor IR** | 1 | [cite_start]Detecta a presença/interrupção do feixe (GPIO 15) [cite: 1] |
| **Módulo Step-Down lm2596** | 1 | Regulador de tensão para alimentação do circuito |
| **Baterias Li-Ion / LiPo** | 2 | Alimentação portátil do sistema |
| **Chave Gangorra (2 pinos)** | 1 | Botão de Liga/Desliga principal |

---

## 📐 Estrutura Física e Modelagem 3D

Toda a eletrônica foi embutida dentro de um portal (pórtico) prototipado e dimensionado no **Tinkercad 3D**, projetado sob medida para abrigar os componentes eletrônicos e permitir o fluxo contínuo da pista.

* **Altura e Largura Personalizadas:** Garantem espaço ideal para a passagem livre dos carrinhos seguidores de linha sobre a pista sem risco de colisão.
* **Alinhamento dos Sensores:** Suportes internos posicionam o LED emissor e o receptor IR alinhados nas laterais do portal.

---

### 🔨 Processo de Montagem da Estrutura

#### Passo 1: Prototipagem e Modelagem 3D (Tinkercad)
O projeto do portal foi modelado digitalmente para validar as dimensões físicas, o alojamento dos componentes e o vão livre de passagem dos carrinhos.

![Modelagem 3D no Tinkercad](https://i.postimg.cc/bN0dtV5K/Captura-de-tela-2026-07-23-170659.jpg)

<div align="center">
  <img src="https://i.postimg.cc/bN0dtV5K/Captura-de-tela-2026-07-23-170659.jpg" alt="Descrição da Imagem" width="50%">
</div>

#### Passo 2: Alojamento da Eletrônica e Alimentação
Dentro do corpo da estrutura, foram reservados compartimentos para acomodar as duas baterias, o módulo Step-Down LM2596, a placa ESP32 e a chave gangorra (Liga/Desliga).

![Montagem Interna do Circuito e Alimentação](https://i.postimg.cc/0y1004ZH/shared-image-(2).jpg)

#### Passo 3: Fixação e Alinhamento dos Sensores IR
O LED emissor IR e o fotorreceptor foram instalados nas laterais internas do portal. O alinhamento óptico preciso entre ambos é essencial para garantir a detecção sem falhas ao interromper o feixe.

![Posicionamento dos Sensores IR no Portal](https://i.postimg.cc/3xQFFq9c/shared-image-(3).jpg)
![Posicionamento dos Sensores IR no Portal](https://i.postimg.cc/2SN77tT9/shared-image-(5).jpg)

#### Passo 4: Estrutura Finalizada na Pista
A estrutura montada e selada é posicionada sobre a linha de chegada/largada da pista de competição, pronta para operar via Wi-Fi.

![Visão Geral do Portal Montado](https://i.postimg.cc/MpJmmLts/shared-image-(4).jpg)

## 💻 Arquitetura e Funcionamento do Código

O firmware foi desenvolvido em **C++ / Arduino IDE** para ESP32:

1. [cite_start]**Rede Wi-Fi Interna:** O ESP32 cria uma rede Wi-Fi própria (`SSID: CronometroPistaSATC`) sem necessidade de roteadores externos[cite: 1, 4].
2. [cite_start]**WebServer:** Um servidor HTTP escuta requisições na porta `80` e entrega a página HTML interativa.
3. **Leitura da Barreira IR:**
   * [cite_start]Utiliza um LED IR com sinal gerado via PWM interno a `38 kHz`[cite: 1].
   * [cite_start]Quando o carrinho passa, a barreira óptica é quebrada (transição de `HIGH` para `LOW` no sensor)[cite: 2, 3].
   * [cite_start]Um filtro de *cooldown* de $1000\text{ ms}$ previne múltiplos disparos em uma mesma passagem[cite: 3].
4. [cite_start]**Comunicação Web:** A interface web realiza requisições periódicas via Fetch/AJAX para as rotas `/tempo` e `/estado`, garantindo precisão na atualização do display em tempo real.

---

## 🚀 Como Configurar e Executar

### 1. Gravação do Código
1. [cite_start]Abra o arquivo `Projeto_cronometro.ino` na **Arduino IDE**[cite: 50].
2. Certifique-se de selecionar a placa **ESP32 Dev Module**.
3. Compile e faça o upload do código para o ESP32.

### 2. Conexão e Uso
1. Ligue o circuito acionando o **botão liga/desliga de 2 pinos**.
2. No seu computador ou celular, procure pelas redes Wi-Fi disponíveis e conecte-se em:
   * [cite_start]**SSID:** `CronometroPistaSATC` [cite: 4]
   * [cite_start]**Senha:** `12345678` [cite: 4]
3. Abra o seu navegador web (Chrome, Firefox, Edge, etc.) e digite o endereço IP do ESP32:
   ```text
   [http://192.168.4.1](http://192.168.4.1)
