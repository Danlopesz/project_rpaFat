<div align="center">

# 🚗 RPA Pós-Faturamento — Processador de Faturas

**Automação Inteligente para Extração e Processamento de Demonstrativos de Faturamento**

![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium-WebDriver-43B02A?style=for-the-badge&logo=selenium&logoColor=white)
![Tkinter](https://img.shields.io/badge/Tkinter-GUI-FF6F00?style=for-the-badge&logo=python&logoColor=white)
![Status](https://img.shields.io/badge/Status-Em%20Produção-brightgreen?style=for-the-badge)

<br>

*RPA com interface gráfica que automatiza o fluxo de pós-faturamento, realizando download de demonstrativos, extração de dados via parsing de PDF e geração de planilha formatada para importação em sistema de gestão.*

---

</div>

## 📑 Índice

- [Visão Geral](#-visão-geral)
- [Funcionalidades](#-funcionalidades)
- [Arquitetura & Fluxo](#-arquitetura--fluxo)
- [Tech Stack](#-tech-stack)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Pré-requisitos](#-pré-requisitos)
- [Instalação](#-instalação)
- [Configuração](#%EF%B8%8F-configuração)
- [Como Usar](#-como-usar)
- [Saídas Geradas](#-saídas-geradas)
- [Troubleshooting](#-troubleshooting)
- [Autor](#-autor)
- [Licença](#-licença)

---

## 🎯 Visão Geral

O **RPA Pós-Faturamento** automatiza o processo de extração e tratamento de faturas em uma operação de gestão de frotas. Ele substitui o trabalho manual de:

1. Consultar faturas individualmente em um portal web;
2. Baixar demonstrativos em PDF;
3. Extrair informações-chave de cada documento (placa, condutor, valores);
4. Compilar os dados em uma planilha padronizada para importação no sistema de gestão.

O que antes levava **horas de trabalho repetitivo** agora é executado em minutos com acompanhamento em tempo real via interface gráfica.

---

## ✨ Funcionalidades

| Funcionalidade | Descrição |
|---|---|
| 🖥️ **Interface Gráfica** | GUI moderna com tema customizado, barra de progresso, terminal de log em tempo real e contadores de processamento |
| 🌐 **Automação Web** | Navegação automatizada em portal de faturamento via Selenium WebDriver com tratamento de esperas e timeouts |
| 📄 **Download de PDFs** | Download automático de demonstrativos com detecção de conclusão, renomeação padronizada e controle de timeout |
| 🔍 **Parsing Inteligente de PDF** | Extração de dados estruturados de PDFs não-tabulares usando `pdfplumber` com múltiplos padrões regex e fallbacks |
| 📊 **Geração de Excel** | Planilha formatada como tabela real (`openpyxl`), com estilos e filtros, pronta para importação |
| ☁️ **Sincronização com Nuvem** | Cópia automática dos artefatos gerados para diretório sincronizado com a nuvem corporativa |
| 🔄 **Normalização de Dados** | Tratamento robusto de documentos (formatação com zeros à esquerda) e valores monetários em BRL |
| ⚡ **Resiliência** | Aceita variações de cabeçalho nos dados de entrada, múltiplos fallbacks na extração e tratamento de erros por registro |

---

## 🏗 Arquitetura & Fluxo

```
┌─────────────────────────────────────────────────────────────────────┐
│                        INTERFACE GRÁFICA (Tkinter)                  │
│   ┌──────────────┐  ┌──────────────────┐  ┌─────────────────────┐  │
│   │ Seleção Excel │  │ Barra Progresso  │  │   Terminal de Log   │  │
│   └──────┬───────┘  └────────┬─────────┘  └──────────┬──────────┘  │
└──────────┼───────────────────┼───────────────────────┼──────────────┘
           │                   │                       │
           ▼                   ▼                       ▼
┌──────────────────────────────────────────────────────────────────────┐
│                         FLUXO DE EXECUÇÃO                            │
│                                                                      │
│  1️⃣  Leitura da planilha de entrada                                  │
│      └─ Deduplicação · Normalização de campos · Validação            │
│                          │                                           │
│  2️⃣  Loop por registro   ▼                                           │
│      ┌─────────────────────────────────────┐                         │
│      │  Selenium → Portal de Faturamento   │                         │
│      │  • Seleciona parâmetros de busca    │                         │
│      │  • Preenche filtros dinamicamente   │                         │
│      │  • Captura metadados da fatura      │                         │
│      │  • Download do demonstrativo (PDF)  │                         │
│      └──────────────┬──────────────────────┘                         │
│                     ▼                                                │
│      ┌─────────────────────────────────────┐                         │
│      │  pdfplumber → Parsing do PDF        │                         │
│      │  • Extrai dados com regex           │                         │
│      │  • Múltiplos padrões + fallbacks    │                         │
│      │  • Normalização de valores (BRL)    │                         │
│      └──────────────┬──────────────────────┘                         │
│                     ▼                                                │
│  3️⃣  Geração do Excel final (tabela formatada com openpyxl)          │
│                     │                                                │
│  4️⃣  Cópia dos artefatos para diretório de nuvem sincronizado        │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 🛠 Tech Stack

| Tecnologia | Uso |
|---|---|
| **Python 3.12+** | Linguagem principal |
| **Tkinter** | Interface gráfica nativa com tema customizado |
| **Selenium** | Automação web (Chrome WebDriver) |
| **pdfplumber** | Extração de texto estruturado de PDFs |
| **pandas** | Leitura e manipulação de dados tabulares |
| **openpyxl** | Geração de Excel com tabelas e estilos formatados |
| **regex (re)** | Extração de padrões (placas veiculares, valores monetários, nomes) |
| **threading** | Execução não-bloqueante para manter a UI responsiva |

---

## 📂 Estrutura do Projeto

```
project_rpaFat/
│
├── .gitignore              # Regras de exclusão do Git
├── requirements.txt        # Dependências do projeto
├── README.md               # Documentação (este arquivo)
├── extractRpa.py           # Script principal (GUI + lógica RPA)
│
├── .venv/                  # Ambiente virtual Python (não versionado)
│
└── Processamento_Faturas/  # Gerado em tempo de execução (não versionado)
    ├── PDFs/               # Demonstrativos baixados e renomeados
    └── output.xlsx         # Planilha final para importação
```

---

## 📋 Pré-requisitos

- [x] **Python 3.12+** → [Download](https://www.python.org/downloads/)
- [x] **Google Chrome** → atualizado na última versão
- [x] **ChromeDriver** → compatível com a versão do Chrome ([Download](https://chromedriver.chromium.org/downloads))
- [x] **Acesso ao portal de faturamento** → credenciais válidas e rede autorizada

> ⚠️ O ChromeDriver deve estar no `PATH` do sistema ou na mesma pasta do script.

---

## 🚀 Instalação

**1. Clone o repositório:**

```bash
git clone https://github.com/Danlopesz/project_rpaFat.git
cd project_rpaFat
```

**2. Crie o ambiente virtual:**

```powershell
python -m venv .venv
```

**3. Ative o ambiente virtual:**

```powershell
# PowerShell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
.\.venv\Scripts\Activate.ps1
```

```bash
# CMD
.venv\Scripts\activate.bat
```

**4. Instale as dependências:**

```powershell
pip install -r requirements.txt
```

---

## ⚙️ Configuração

O script utiliza variáveis de configuração no início do arquivo. Antes de rodar, ajuste:

| Configuração | O que alterar |
|---|---|
| **Diretório de saída** | Caminho onde os PDFs e Excel serão copiados automaticamente |
| **URL do portal** | Endereço do portal de faturamento |
| **Mapeamento de filtros** | Dicionário de parâmetros para seleção no portal |

---

## 🖥 Como Usar

**1.** Execute o script:

```powershell
python extractRpa.py
```

**2.** Na interface gráfica:

| Passo | Ação |
|---|---|
| 📂 | Clique em **"Selecionar Excel"** e escolha a planilha de entrada |
| ▶️ | Clique em **"Executar Processo"** para iniciar a automação |
| 👀 | Acompanhe o progresso pela barra, contadores e terminal de log |

**3.** Ao finalizar, os artefatos estarão disponíveis no diretório de saída configurado.

> 💡 **Dica:** Não interaja com o navegador Chrome durante a execução. A automação controla a navegação automaticamente.

---

## 📤 Saídas Geradas

### Planilha Excel

Gerada como **tabela formatada** (com filtros e estilo) contendo os campos consolidados:
- Dados da planilha de entrada (cliente, contrato, origem)
- Metadados capturados do portal (vencimento, natureza)
- Dados extraídos dos PDFs (placa, valor, condutor)

### PDFs

Cada registro gera um PDF renomeado no padrão:

```
{PREFIXO}_{CONTROLE}.pdf
```

---

## 🔧 Troubleshooting

<details>
<summary><b>❌ ModuleNotFoundError: No module named 'pdfplumber'</b></summary>

O pacote não está instalado no ambiente virtual. Ative o `.venv` e instale:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

</details>

<details>
<summary><b>❌ Fatal error in launcher: Unable to create process</b></summary>

O `.venv` está corrompido (foi criado a partir de outro venv ou o Python foi movido). Recrie:

```powershell
deactivate
Remove-Item -Recurse -Force .\.venv
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

</details>

<details>
<summary><b>❌ WebDriverException: ChromeDriver not found</b></summary>

O ChromeDriver não está instalado ou é incompatível com o Chrome. Verifique as versões:

```powershell
# Versão do Chrome: Menu → Ajuda → Sobre o Google Chrome
# Baixe o ChromeDriver compatível e adicione ao PATH
```

Alternativa com `webdriver-manager`:

```powershell
pip install webdriver-manager
```

```python
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=options)
```

</details>

<details>
<summary><b>⚠️ Dados não encontrados no PDF</b></summary>

O layout do PDF pode ter mudado. Abra o PDF com `pdfplumber` e inspecione o texto extraído:

```python
import pdfplumber

with pdfplumber.open("arquivo.pdf") as pdf:
    for page in pdf.pages:
        print(page.extract_text())
```

Ajuste os padrões regex conforme necessário.

</details>

<details>
<summary><b>⚠️ Timeout no download do PDF</b></summary>

Se a rede estiver lenta, aumente o timeout no script:

```python
timeout = 120  # segundos (padrão: 60)
```

</details>

---

## 🧠 Destaques Técnicos

Este projeto demonstra competências em:

- **RPA (Robotic Process Automation)** — automação end-to-end de processo operacional
- **Web Scraping** — navegação automatizada com Selenium, tratamento de esperas dinâmicas e captura de dados do DOM
- **PDF Parsing** — extração de dados não-estruturados com regex e múltiplas estratégias de fallback
- **Data Engineering** — normalização de dados monetários (BRL), documentos e deduplicação
- **GUI Development** — interface rica com Tkinter, threading para não-bloqueio, e feedback visual em tempo real
- **Automação de Excel** — criação de tabelas formatadas com openpyxl prontas para integração com outros sistemas

---

## 👤 Autor

**Daniel Almeida Lopes**

[![GitHub](https://img.shields.io/badge/GitHub-Danlopesz-181717?style=flat-square&logo=github)](https://github.com/Danlopesz)

---

## 📝 Licença

Este projeto é de uso **interno/educacional**.
O código-fonte não está disponível publicamente por conter lógica de negócio proprietária.

---