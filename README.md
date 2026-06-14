<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/PySide6-Qt-green?style=for-the-badge&logo=qt&logoColor=white"/>
  <img src="https://img.shields.io/badge/Status-Em%20Desenvolvimento-orange?style=for-the-badge"/>
</p>

<h1 align="center">hein IDE</h1>
<p align="center">IDE de código Python desenvolvida com PySide6 — produtividade, integração GitHub e experiência fluida.</p>

---

## Índice

- [Requisitos](#requisitos)
- [Como Iniciar](#como-iniciar)
- [Interface](#interface)
- [Editor de Código](#editor-de-código)
- [Sistema de Abas](#sistema-de-abas)
- [Auto-Save e File Watcher](#auto-save-e-file-watcher)
- [Terminais](#terminais)
- [Painel de Arquivos](#painel-de-arquivos)
- [Git e GitHub](#git-e-github)
- [Outline](#outline)
- [Indexador](#indexador)
- [Atalhos de Teclado](#atalhos-de-teclado)
- [Configuração e Temas](#configuração-e-temas)
- [Estrutura do Projeto](#estrutura-do-projeto)

---

## Requisitos

- Python 3.10 ou superior
- Git instalado no PATH do sistema → [Download](https://git-scm.com/download/win)

```bash
pip install PySide6 jedi
```

---

## Como Iniciar

```bash
python main.py
```

---

## Interface

```
┌─────────────────┬───────────────────────────┬──────────────┐
│  COLUNA 1       │  COLUNA 2                 │  COLUNA 3    │
│                 │                           │              │
│  Menu lateral   │  Editor de código (abas)  │  Outline     │
│  Explorer       │  ─────────────────────    │  ─────────   │
│  Painel Git     │  Terminal (PS / Saída)    │  Painel IA   │
└─────────────────┴───────────────────────────┴──────────────┘
```

---

## Editor de Código

Editor principal baseado em `QPlainTextEdit`.

### Edição

| Funcionalidade | Descrição |
|---|---|
| Realce de sintaxe | Python completo com cores por tipo de token |
| Highlight da linha atual | Linha do cursor destacada |
| Números de linha | Calha lateral com numeração dinâmica |
| Fechamento automático | `(`, `[`, `{`, `"`, `'` — fecha automaticamente |
| Indentação automática | Enter após `:` indenta o próximo bloco |
| Comentar/descomentar | `Ctrl+/` — alterna `#` na seleção |
| Mover linhas | `Alt+↑` / `Alt+↓` |
| Deletar linha | `Ctrl+Shift+K` |
| Duplicar linha | `Ctrl+Shift+D` |

### Múltiplos Cursores

| Ação | Atalho |
|---|---|
| Adicionar cursor acima | `Ctrl+Alt+↑` |
| Adicionar cursor abaixo | `Ctrl+Alt+↓` |
| Selecionar próxima ocorrência | `Ctrl+D` |
| Cursor com clique | `Alt+Click` |
| Cancelar múltiplos cursores | `Esc` |

### Dobramento de Blocos

| Ação | Atalho |
|---|---|
| Recolher bloco atual | `Ctrl+Alt++` |
| Expandir bloco atual | `Ctrl+Alt+-` |
| Recolher tudo | `Ctrl+Alt+Shift++` |
| Expandir tudo | `Ctrl+Alt+Shift+-` |

Funciona em: `def`, `async def`, `class`, `if`, `elif`, `else`, `for`, `while`, `try`, `except`, `finally`, `with`.

### Busca e Substituição

| Ação | Atalho |
|---|---|
| Abrir busca | `Ctrl+F` |
| Abrir substituição | `Ctrl+H` |
| Buscar no projeto | `Ctrl+Shift+F` |
| Fechar painel | `Esc` |

O Find in Files roda em thread separada e suporta regex e case-sensitive. Resultados aparecem em tempo real.

### Autocomplete

Powered by **Jedi** — autocomplete real baseado no código do projeto.

| Ação | Comportamento |
|---|---|
| Digitar | Sugestões aparecem automaticamente |
| `Tab` ou `Enter` | Confirma a sugestão |
| `Esc` | Fecha o autocomplete |
| `(` | Mostra assinatura da função (parâmetros) |

O Jedi roda em subprocess separado — não trava a IDE. Enxerga os pacotes instalados na venv do projeto do usuário.

### Ir Para Definição

| Ação | Atalho |
|---|---|
| Ir para definição | `F12` ou `Ctrl+Click` |
| Voltar | `Alt+←` |
| Find References | `Shift+F12` |

- Hover com `Ctrl`: sublinha a palavra navegável em azul
- Ao navegar: linha de destino fica destacada por 1.5s
- Resultado exibido na status bar
- Funciona entre arquivos usando o banco SQLite do Indexador

### Validador de Imports

Detecta imports inválidos e sublinha com linha ondulada vermelha — igual ao VS Code.

```python
from modulo_inexistente import teste   # sublinhado vermelho
from os import path                    # OK (stdlib)
from PySide6.QtWidgets import QWidget  # OK (instalado)
```

### Calha de Números (Gutter)

A calha lateral tem 7 recursos visuais integrados:

```
[BP zone] [pad] [número] [seta fold] [git bar]
```

| Recurso | Descrição |
|---|---|
| Números de linha | Linha atual em negrito + azul |
| Code Folding | Setas ▼▶ — clique para recolher/expandir |
| CodeLens | "N uso(s)" acima de cada `def`/`class` |
| Breakpoints | Bolinha vermelha — clique na zona esquerda |
| Bookmarks | Diamante azul ◆ — `Ctrl+F2` / `F2` / `Shift+F2` |
| Indicador de Erros | Dot vermelho (erro) ou amarelo (aviso) |
| Git change bar | Verde = adicionado, Amarelo = modificado, Vermelho = deletado |

### Fonte

| Ação | Atalho |
|---|---|
| Aumentar fonte | `Ctrl+=` |
| Diminuir fonte | `Ctrl+-` |
| Resetar fonte | `Ctrl+0` |

Fonte padrão: Consolas 11pt. Range: 6pt a 32pt.

### Encoding

- Leitura com fallback automático: `utf-8` → `utf-8-sig` → `cp1252` → `latin-1`
- Todo arquivo salvo pelo editor é gravado em **UTF-8**
- Novos projetos criados com `# -*- coding: utf-8 -*-`

---

## Sistema de Abas

- Múltiplos arquivos abertos simultaneamente
- Deduplicação: clicar em arquivo já aberto foca a aba existente
- Título mostra `*nome.py` quando há alterações não salvas

---

## Auto-Save e File Watcher

Gerenciado por `GerenciadorAbas_AutoSalve.py`. Funciona automaticamente, sem configuração.

### Auto-Save

Após **3 segundos** sem digitar, o arquivo ativo é salvo silenciosamente.

- Sem botão, sem pergunta, sem asterisco
- Não destrói o histórico de undo (`Ctrl+Z` continua funcionando)
- `Ctrl+S` continua funcionando para quem prefere salvar manualmente

### File Watcher

Monitora todos os arquivos abertos via `QFileSystemWatcher`. Quando qualquer programa alterar um arquivo aberto:

- `git restore`, `git pull`, `git checkout`
- Editor externo (VS Code, Bloco de Notas, etc.)

→ A aba **recarrega automaticamente** com o conteúdo atualizado.

---

## Terminais

### PowerShell

- Terminal completo integrado
- Inicia automaticamente na pasta do projeto aberto
- Múltiplas abas (botão `+`)
- Suporte a cores ANSI
- Botão 🧹 para limpar

### Saída (Run Output)

- Terminal de execução do código Python
- Aparece automaticamente ao executar (`F5`)
- Exibe `stdout` e `stderr` do script
- Mostra código de saída ao finalizar
- Usa a **venv do projeto do usuário** — nunca a venv da IDE

---

## Painel de Arquivos

Explorer lateral com árvore de arquivos do projeto.

| Funcionalidade | Descrição |
|---|---|
| Navegar pastas | Clique para expandir/recolher |
| Abrir arquivo | Clique simples |
| Contador | Total de arquivos no projeto |

Usa `QFileSystemModel` — reflete mudanças no disco em tempo real.

---

## Git e GitHub

Integração completa com Git local e GitHub via painel lateral.

### Autenticação — GitHub Device Flow

1. Clique **Entrar com GitHub**
2. A IDE gera um código (ex: `ABCD-1234`)
3. Browser abre em `github.com/login/device`
4. Cole o código e autorize
5. Login mantido entre sessões

### Painel Git — 5 Abas

**⬡ Status**
- Campo de commit + botão ✓
- Árvore de arquivos: Staged / Não staged / Não rastreado / Conflito
- Menu de contexto: Stage, Unstage, Descartar, Stage e Commit rápido
- Double-click → abre arquivo no editor

**◎ Log**
- Histórico (últimos 80 commits, todas as branches)
- HEAD em azul, branch atual em verde, tags em amarelo
- Filtro de texto em tempo real
- Clique → mostra diff do commit

**⑂ Branches**
- Locais e Remotas
- Tracking info (ahead/behind)
- Menu: Checkout, Merge, Deletar
- Top 10 contribuidores

**✦ Stash**
- Lista de stashes
- Guardar, Aplicar (pop / sem remover), Deletar

**± Diff**
- Diff colorido do arquivo atual vs HEAD ou staged
- Atualiza ao trocar de aba

### Barra de Alertas (sempre visível)

| Cor | Situação |
|---|---|
| 🔴 Erro | HEAD detached, conflitos de merge |
| 🟡 Aviso | Sem remote, atrás do remote em N commits |
| 🔵 Info | +5 commits não enviados, +20 arquivos não rastreados |
| 🟢 OK | Tudo saudável |

### Operações

| Botão | Ação |
|---|---|
| 💾 Commit | Cria versão local |
| ☁ Push | Envia para o GitHub |
| ☁↓ Pull | Baixa do GitHub |
| 📋 Histórico | Lista versões com opção de restaurar |
| Publicar | Cria repositório no GitHub |
| Clonar | Baixa repositório e abre na IDE |

> Ao restaurar uma versão, as abas abertas no editor **recarregam automaticamente**.

---

## Outline

Painel lateral com a estrutura do arquivo ou projeto em árvore.

**Modo Arquivo:**
```
📄 arquivo.py
  🔷 MinhaClasse
    🔹 __init__(self)
    🔹 metodo(self, x)
  🔸 funcao_global(a, b)
```

**Modo Projeto:**
```
📁 Editor_Terminal
  📁 _EDITOR_
    📄 Editor.py
      🔷 CodeEditor
```

- `Ctrl+Shift+O` — abrir/fechar
- Clique no item → navega para a linha no editor
- Filtro em tempo real
- Atualiza automaticamente ao trocar de aba ou editar

---

## Indexador

Sistema que varre todos os `.py` do projeto e mantém um banco SQLite.

- **Local do banco:** `<projeto>/.hein/index.db`
- **Indexação incremental:** hash MD5 por arquivo — só reindexar o que mudou
- **Roda em QThread** — não trava a IDE
- **Alimenta:** Ir para Definição, Find References, CodeLens, Validador de Imports

Tabelas: `files`, `symbols`, `usages`, `imports`

---

## Atalhos de Teclado

### Arquivo

| Ação | Atalho |
|---|---|
| Novo Projeto | `Ctrl+Shift+N` |
| Abrir Projeto | `Ctrl+Shift+O` |
| Salvar | `Ctrl+S` |
| Executar | `F5` |
| Tela cheia | `F11` |

### Editor

| Ação | Atalho |
|---|---|
| Ir para definição | `F12` / `Ctrl+Click` |
| Voltar da definição | `Alt+←` |
| Find References | `Shift+F12` |
| Buscar no arquivo | `Ctrl+F` |
| Substituir | `Ctrl+H` |
| Buscar no projeto | `Ctrl+Shift+F` |
| Comentar/descomentar | `Ctrl+/` |
| Mover linha acima | `Alt+↑` |
| Mover linha abaixo | `Alt+↓` |
| Deletar linha | `Ctrl+Shift+K` |
| Duplicar linha | `Ctrl+Shift+D` |
| Selecionar próxima ocorrência | `Ctrl+D` |
| Cursor acima | `Ctrl+Alt+↑` |
| Cursor abaixo | `Ctrl+Alt+↓` |
| Cursor com mouse | `Alt+Click` |
| Aumentar fonte | `Ctrl+=` |
| Diminuir fonte | `Ctrl+-` |
| Resetar fonte | `Ctrl+0` |
| Ir para linha | `Ctrl+G` |
| Início do arquivo | `Ctrl+Home` |
| Fim do arquivo | `Ctrl+End` |

### Bookmarks

| Ação | Atalho |
|---|---|
| Toggle bookmark | `Ctrl+F2` |
| Próximo bookmark | `F2` |
| Bookmark anterior | `Shift+F2` |

### Dobramento

| Ação | Atalho |
|---|---|
| Recolher bloco | `Ctrl+Alt++` |
| Expandir bloco | `Ctrl+Alt+-` |
| Recolher tudo | `Ctrl+Alt+Shift++` |
| Expandir tudo | `Ctrl+Alt+Shift+-` |

### Configuração

| Ação | Atalho |
|---|---|
| Editar atalhos | `Ctrl+2` |
| Editar tema | `Ctrl+3` |
| Editar configurações | `Ctrl+6` |
| Editar autocomplete/snippets | `Ctrl+5` |

> Todos os atalhos são configuráveis via `_CONFIGURA_/atalhos.json`. Edite e salve — aplicado na hora, sem reiniciar.

---

## Configuração e Temas

### thema.json — Tema Visual (`Ctrl+3`)

Controla todas as cores da IDE. Seções: `editor`, `interface`, `terminal`, `terminal_run`, `painel_outline`, `painel_arquivos`, `painel_git`, `painel_ia`.

Hot-reload via `QFileSystemWatcher` — salve e veja a mudança na hora.

### configurar.json — Fonte e Layout (`Ctrl+6`)

```json
"editor":          { "fonte": "Consolas", "tamanho_fonte": 11 },
"terminal":        { "fonte": "Consolas", "tamanho_fonte": 11 },
"outline":         { "fonte": "Segoe UI", "tamanho_fonte": 11 },
"painel_arquivos": { "fonte": "Segoe UI", "tamanho_fonte": 11 }
```

Outras opções: `wrap_linha`, `destacar_linha`, `fechar_pares`, `tamanho_minimo`, `tamanho_maximo`.

### autocomplete.json — Snippets (`Ctrl+5`)

```json
{
  "ambiente": { "pasta_venv": ".venv" },
  "snippets": {
    "ifmain": "if __name__ == '__main__':\n    ",
    "funcao": "def nome_funcao():\n    ",
    "classe": "class NomeClasse:\n    def __init__(self):\n        ",
    "tentar": "try:\n    \nexcept Exception as e:\n    print(e)"
  }
}
```

Digite o nome curto no editor e pressione `Tab` ou `Enter` para expandir.

---

## Estrutura do Projeto

```
Editor_Terminal/
├── main.py                             ← ponto de entrada
├── log.py
├── _APP_/
│   ├── main_window.py                  ← MainWindow
│   ├── tab_widget.py                   ← TabWidget
│   └── terminal_tab_widget.py          ← TerminalTabWidget
├── _CONFIGURA_/
│   ├── Fontes_hein/                   ← fontes .ttf próprias
│   ├── __init__.py                     ← constantes globais
│   ├── fontes.py
│   ├── gerenciador_atalhos.py
│   ├── gerenciador_configurar.py
│   ├── gerenciador_tema.py
│   ├── log.py
│   ├── atalhos.json
│   ├── autocomplete.json
│   ├── configurar.json
│   └── thema.json
├── _EDITOR_/
│   ├── Editor.py                       ← CodeEditor
│   ├── GerenciadorAbas_AutoSalve.py              ← auto-save + file watcher
│   ├── Autocomplete/                   ← Jedi via subprocess
│   ├── Core/                           ← EstadoEditor, GerenciadorArquivo
│   ├── Funcionalidades/                ← busca, sintaxe, cursores, indentação
│   ├── Indexador/                      ← SQLite index
│   └── Ui/                             ← calha de números, métricas
├── _TERMINAIS_/
│   ├── terminal_powershell.py          ← PowerShellTerminalWidget
│   └── terminal_run.py                 ← RunOutputWidget
├── _PAINEL_DIREITO_/
│   ├── outline.py                      ← PainelOutline
│   └── painel_ia.py                    ← em desenvolvimento
└── _PAINEL_ESQUERDO_/
    ├── FileTree.py                     ← PainelArquivos
    └── Git/                            ← integração GitHub completa
```

### Dados salvos pelo sistema

| Dado | Local |
|---|---|
| Token GitHub | `%APPDATA%\hein\github_config.json` |
| Tarefas globais | `%APPDATA%\hein\tarefas.json` |
| Tarefas do projeto | `<projeto>\.hein\tarefas.json` |
| Índice do projeto | `<projeto>\.hein\index.db` |

---

<p align="center">
  Desenvolvido por <strong>HenriqLs</strong> com PySide6 e Python
</p>
