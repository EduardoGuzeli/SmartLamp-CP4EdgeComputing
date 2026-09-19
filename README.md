# 💡 SmartLamp — Edge Computing

<p align="center">
  <img src="https://img.shields.io/badge/ESP32-Edge%20Computing-blue?style=for-the-badge&logo=espressif" alt="ESP32">
  <img src="https://img.shields.io/badge/IoT-Smart%20Home-00C853?style=for-the-badge" alt="IoT">
  <img src="https://img.shields.io/badge/MQTT-Communication-6600CC?style=for-the-badge&logo=mqtt" alt="MQTT">
  <img src="https://img.shields.io/badge/C%2B%2B-Embedded-00599C?style=for-the-badge&logo=cplusplus" alt="C++">
</p>

<p align="center">
  <strong>Uma solução IoT para controle inteligente de iluminação utilizando Edge Computing, ESP32 e protocolo MQTT.</strong>
</p>

---

## 📌 Sobre o projeto

O **SmartLamp** é um projeto de Internet das Coisas (IoT) desenvolvido para demonstrar a aplicação de **Edge Computing em sistemas de iluminação inteligente**.

A solução utiliza um **ESP32** como dispositivo de borda (*edge device*), responsável pela comunicação via Wi-Fi, integração com um **Broker MQTT**, controle do LED e aquisição de dados de luminosidade.

A arquitetura permite que o dispositivo envie informações de luminosidade e estado da lâmpada para a infraestrutura MQTT, além de receber comandos remotamente para ligar ou desligar a iluminação.

> 🚀 **Objetivo:** demonstrar, de forma prática, como dispositivos IoT podem coletar dados, processá-los localmente e se comunicar com outros componentes de uma arquitetura distribuída.

---

## 🎯 Objetivos

### Objetivo geral

Desenvolver uma solução de iluminação inteligente baseada em **IoT e Edge Computing**, utilizando o ESP32 para aquisição de dados, processamento local e comunicação através do protocolo MQTT.

### Objetivos específicos

* 🔌 Controlar o estado de uma lâmpada/LED através de comandos MQTT;
* 📡 Estabelecer comunicação entre o ESP32 e uma rede Wi-Fi;
* 📨 Enviar informações do dispositivo para um Broker MQTT;
* 📥 Receber comandos através de tópicos MQTT;
* 💡 Monitorar o estado da iluminação;
* ☀️ Realizar a leitura de luminosidade através de entrada analógica;
* ⚡ Converter a leitura do sensor para uma escala percentual;
* 🧠 Aplicar conceitos de Edge Computing em um dispositivo IoT;
* 🔄 Implementar mecanismos de reconexão Wi-Fi e MQTT.

---

## 🧠 Conceito de Edge Computing

O **Edge Computing** consiste em aproximar o processamento e a tomada de decisão da origem dos dados, reduzindo a dependência de processamento centralizado.

No SmartLamp, o **ESP32 atua como dispositivo de borda**, realizando diretamente tarefas como:

```text
┌──────────────────────┐
│      AMBIENTE        │
│                      │
│  Sensor de luminos.  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│        ESP32         │
│                      │
│  • Leitura sensor    │
│  • Processamento     │
│  • Controle LED      │
│  • Wi-Fi             │
│  • MQTT              │
└──────────┬───────────┘
           │
           │ MQTT
           ▼
┌──────────────────────┐
│     MQTT BROKER      │
│                      │
│  Comunicação IoT     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    Aplicação /       │
│    Plataforma IoT    │
└──────────────────────┘
```

Essa abordagem possibilita uma arquitetura mais distribuída e adequada para aplicações que dependem de comunicação eficiente entre dispositivos conectados.

---

## ⚙️ Principais funcionalidades

| Funcionalidade         | Descrição                                   |
| ---------------------- | ------------------------------------------- |
| 📡 Wi-Fi               | Conexão do ESP32 à rede sem fio             |
| 📨 MQTT                | Comunicação com o Broker MQTT               |
| 💡 Controle da lâmpada | Liga/desliga o LED através de comandos      |
| ☀️ Luminosidade        | Leitura de entrada analógica                |
| 📊 Percentual          | Conversão da leitura para escala de 0 a 100 |
| 🔄 Reconexão           | Reconexão automática ao Wi-Fi e MQTT        |
| 📤 Telemetria          | Publicação do estado e luminosidade         |
| 🖥️ Serial Monitor     | Exibição das informações durante a execução |

---

## 🛠️ Tecnologias utilizadas

### Hardware

* **ESP32**
* LED onboard
* Entrada analógica para leitura de luminosidade
* Potenciômetro/sensor analógico para simulação ou medição de luminosidade

### Software

* **C++**
* **Arduino Framework**
* **Wi-Fi**
* **MQTT**
* Biblioteca `WiFi.h`
* Biblioteca `PubSubClient.h`

---

## 📡 Comunicação MQTT

A comunicação entre o ESP32 e o Broker MQTT é realizada utilizando tópicos específicos.

### 📥 Tópico de comandos

```text
/TEF/lamp001/cmd
```

É utilizado para receber comandos destinados à lâmpada.

### 📤 Tópico de estado

```text
/TEF/lamp001/attrs
```

Responsável pelo envio do estado atual da iluminação.

Exemplos:

```text
s|on
```

```text
s|off
```

### 📤 Tópico de luminosidade

```text
/TEF/lamp001/attrs/l
```

Responsável pelo envio do valor de luminosidade obtido pelo dispositivo.

O valor analógico é convertido para uma escala de **0 a 100**.

---

## 🔄 Fluxo de funcionamento

O funcionamento do sistema pode ser representado da seguinte maneira:

```text
                 ┌──────────────┐
                 │    ESP32     │
                 └──────┬───────┘
                        │
              ┌─────────▼─────────┐
              │ Conecta no Wi-Fi  │
              └─────────┬─────────┘
                        │
              ┌─────────▼─────────┐
              │ Conecta no MQTT   │
              └─────────┬─────────┘
                        │
                 ┌──────▼──────┐
                 │ Loop sistema│
                 └──────┬──────┘
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
    ┌──────────┐  ┌──────────┐  ┌──────────┐
    │ Verifica │  │ Publica  │  │   Lê     │
    │ conexões │  │  estado  │  │luminosid.│
    └──────────┘  └──────────┘  └──────────┘
          │             │             │
          └─────────────┼─────────────┘
                        │
                        ▼
                  MQTT Broker
```

---

## 💡 Controle da iluminação

O ESP32 permanece inscrito no tópico de comandos MQTT e interpreta as mensagens recebidas.

### Ligar

```text
lamp001@on|
```

Resultado:

```text
LED → ON
Estado → 1
```

### Desligar

```text
lamp001@off|
```

Resultado:

```text
LED → OFF
Estado → 0
```

---

## ☀️ Leitura de luminosidade

A luminosidade é obtida através de uma entrada analógica do ESP32.

O código utiliza:

```cpp
const int potPin = 34;
```

A leitura do ADC, que varia de `0` a `4095`, é convertida para uma escala percentual:

```text
0     → 0%
2048  → ~50%
4095  → 100%
```

Essa informação é então publicada no Broker MQTT.

---

## 🔌 Configuração do projeto

Antes de executar o projeto, configure as informações de rede e MQTT no código:

```cpp
const char *default_SSID = "SUA_REDE";
const char *default_PASSWORD = "SUA_SENHA";

const char *default_BROKER_MQTT = "IP_DO_BROKER";
const int default_BROKER_PORT = 1883;
```

> ⚠️ **Importante:** não recomendamos versionar senhas, credenciais ou informações privadas diretamente no código-fonte. Para uma implementação real, utilize variáveis de ambiente, arquivos de configuração seguros ou mecanismos apropriados de gerenciamento de credenciais.

---

## 🚀 Como executar

### 1. Clone o repositório

```bash
git clone https://github.com/EduardoGuzeli/SmartLamp-CP4EdgeComputing.git
```

### 2. Abra o projeto

Abra o arquivo:

```text
SmartLamp.c++
```

em uma IDE compatível com desenvolvimento para ESP32, como a Arduino IDE.

### 3. Instale as bibliotecas

O projeto utiliza:

```cpp
#include <WiFi.h>
#include <PubSubClient.h>
```

Certifique-se de que o suporte à placa ESP32 e a biblioteca **PubSubClient** estejam instalados.

### 4. Configure a rede

Informe:

* SSID da rede Wi-Fi;
* senha da rede;
* endereço IP do Broker MQTT;
* porta do Broker MQTT.

### 5. Conecte o ESP32

Selecione a placa ESP32 e a porta serial correspondente.

### 6. Faça o upload

Compile e envie o programa para o ESP32.

### 7. Monitore a execução

Abra o Serial Monitor utilizando:

```text
115200 baud
```

Você poderá acompanhar informações como:

```text
Conectando-se na rede...
Conectado com sucesso
IP obtido...
Conectado com sucesso ao broker MQTT!
Led Ligado
Led Desligado
Valor da luminosidade...
```

---

## 📂 Estrutura do projeto

Atualmente, o projeto possui uma estrutura enxuta e focada no firmware do dispositivo:

```text
SmartLamp-CP4EdgeComputing/
│
├── SmartLamp.c++
│
└── README.md
```

O arquivo `SmartLamp.c++` concentra a lógica embarcada responsável pela comunicação, controle da iluminação, leitura de luminosidade e integração MQTT.

---

## 🏗️ Arquitetura da solução

```text
                 SMARTLAMP
                     │
                     ▼
              ┌─────────────┐
              │    ESP32    │
              │             │
              │ Edge Device │
              └──────┬──────┘
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
    ┌───────────┐         ┌───────────┐
    │ Luminosid.│         │    LED    │
    │  Sensor   │         │  Control  │
    └─────┬─────┘         └─────▲─────┘
          │                     │
          ▼                     │
    ┌───────────────────────────┴─┐
    │          MQTT                │
    │         Broker               │
    └─────────────────────────────┘
```

---

## 📈 Possíveis evoluções

O projeto pode ser expandido futuramente para incorporar novos recursos, como:

* 🤖 Automação baseada em luminosidade;
* 📱 Aplicativo mobile para controle;
* 🌐 Dashboard web;
* 🏠 Integração com Smart Home;
* 📊 Histórico dos níveis de luminosidade;
* 🔐 Autenticação no Broker MQTT;
* 🔒 Comunicação MQTT com TLS;
* 🧠 Regras inteligentes executadas no Edge;
* 💡 Controle de múltiplas lâmpadas;
* 📈 Monitoramento em tempo real;
* ☁️ Integração com plataformas de IoT e Cloud.

---

## 👥 Equipe — Loucos por Arduíno

- [@dicaio](https://github.com/diego-caio) — Diego Caio de Ulhôa Augusto
- [@dudu](https://github.com/EduardoGuzeli) — Eduardo Guzeli Nogueira
- [@gbzambo](https://github.com/gbzambo) — Gabriel Torres Zambo
- [@Lucas](https://github.com/luczss) — Lucas dos Santos Oliveira
- [@Octavio](https://github.com/OctavioMello) — Octávio Mello Covre de Sousa
- Enzo Leme Gomes

---

## 🎓 Contexto acadêmico

Este projeto foi desenvolvido como parte das atividades acadêmicas relacionadas a **Edge Computing, Internet das Coisas (IoT) e sistemas embarcados**, com o objetivo de aplicar conceitos de comunicação entre dispositivos, processamento na borda e automação inteligente.

A proposta combina hardware, software e comunicação em rede para representar um cenário real de **iluminação inteligente conectada**.

---

## 📚 Conceitos aplicados

Durante o desenvolvimento são trabalhados conceitos como:

* **Internet of Things (IoT)**
* **Edge Computing**
* **Sistemas embarcados**
* **Microcontroladores**
* **ESP32**
* **Protocolos de comunicação**
* **MQTT**
* **Wi-Fi**
* **Telemetria**
* **Automação**
* **Sensoriamento**
* **Arquitetura distribuída**

---

## 🔐 Boas práticas de segurança

Para utilização em um ambiente real, recomenda-se:

* Não armazenar senhas diretamente no código;
* Utilizar autenticação no Broker MQTT;
* Utilizar MQTT sobre TLS quando aplicável;
* Restringir o acesso ao Broker;
* Utilizar credenciais individuais;
* Evitar expor o Broker diretamente à Internet;
* Alterar credenciais padrão antes de uma implantação real.

---

## 📜 Licença

Este projeto foi desenvolvido para fins acadêmicos e educacionais.

Consulte o repositório para informações adicionais sobre a licença e condições de utilização.

---

## ⭐ Projeto

Se este projeto foi útil para você ou ajudou em seus estudos sobre **IoT e Edge Computing**, considere deixar uma ⭐ no repositório!

<p align="center">

### 💡 SmartLamp

**IoT + ESP32 + MQTT + Edge Computing**

Desenvolvido com 💙 pela equipe **Loucos por Arduíno**

</p>
