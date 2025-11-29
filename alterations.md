# Alterações no Controller.py

## Resumo

Atualizações realizadas no arquivo `code/controller.py` para adicionar suporte a controle por teclado, modo debug para visualizar JSON recebido e melhor interface de usuário.

---

## Mudanças Detalhadas

### 1. **Importações Adicionadas** (Linha 12)

- Adicionado `from enum import Enum`
- Necessário para criar a enumeração `ModoControle`

### 2. **Novas Variáveis Globais** (Linhas 35-45)

- `class ModoControle(Enum)`: Enum para gerenciar os 3 modos de operação

  - `AUTOMATICO`: Controle por visão (padrão)
  - `TECLADO`: Controle manual por setas
  - `DEBUG`: Modo de visualização do JSON recebido

- `modo_controle_atual = ModoControle.AUTOMATICO`: Armazena o modo atual
- `teclas_pressionadas = {}`: Dicionário para rastrear setas pressionadas
- `ultima_msg_json = None`: Armazena o último JSON recebido para debug

### 3. **Nova Função: `processar_comando_teclado()`** (Linhas 141-153)

Processa os comandos do teclado e retorna velocidades dos motores:

- **Seta ↑ (UP)**: Avança em frente (VEL_FRENTE em ambos os motores)
- **Seta ↓ (DOWN)**: Vai de ré (VEL_RE em ambos os motores)
- **Seta ← (LEFT)**: Vira à esquerda (M1 lento, M2 rápido)
- **Seta → (RIGHT)**: Vira à direita (M1 rápido, M2 lento)

### 4. **Sistema de Eventos de Teclado** (Linhas 180-197)

Expandido o tratamento de eventos pygame para:

- `KEYDOWN`: Rastreia teclas pressionadas
  - **F1**: Alterna para modo AUTOMÁTICO
  - **F2**: Alterna para modo TECLADO
  - **F3**: Alterna para modo DEBUG
- `KEYUP`: Remove teclas do registro de pressionadas

### 5. **Modo Debug Integrado** (Linhas 224-232)

Quando `modo_controle_atual == ModoControle.DEBUG`:

- Exibe o JSON completo recebido em formato indentado
- Mostra estrutura dos dados para validação
- Para motores (envia 0, 0)

### 6. **Modo Teclado Integrado** (Linhas 242-251)

Quando `modo_controle_atual == ModoControle.TECLADO`:

- Processa comandos do teclado via `processar_comando_teclado()`
- Envia velocidades diretamente para fila de comandos
- Atualiza status com velocidades atuais (M1, M2)

### 7. **Interface Pygame Melhorada** (Linhas 309-331)

Atualizações na tela de status:

- Agora exibe o modo atual em tempo real
- Cores dinâmicas:
  - Verde (0, 255, 0): Modo automático
  - Azul (0, 150, 255): Modo teclado ativo
  - Vermelho (255, 50, 50): Modo destravamento
- Adicionadas instruções de teclas na interface:
  "F1: Automático | F2: Teclado | F3: Debug (JSON)"

---

## Como Usar

### Executar o script:

```powershell
cd D:\Projetos\mecathron
.\venv\Scripts\python.exe code\controller.py
```

### Trocar de Modo:

| Tecla  | Modo       | Descrição                         |
| ------ | ---------- | --------------------------------- |
| **F1** | Automático | Controle por visão (segue o alvo) |
| **F2** | Teclado    | Controle manual com setas         |
| **F3** | Debug      | Exibe JSON recebido no console    |

### Movimentos (Modo Teclado - F2):

| Tecla | Ação               |
| ----- | ------------------ |
| **↑** | Avança para frente |
| **↓** | Volta (ré)         |
| **←** | Vira esquerda      |
| **→** | Vira direita       |

---

## Benefícios das Mudanças

1. **Flexibilidade**: Alternar entre controle automático e manual facilmente
2. **Debug**: Visualizar exatamente o que o servidor está enviando
3. **Testes**: Testar movimentos do carrinho sem depender de visão computacional
4. **Interface**: Display melhorado mostra estado atual do sistema
5. **Segurança**: F1 retorna ao modo automático a qualquer momento

---

## Arquivos Afetados

- `d:\Projetos\mecathron\code\controller.py`

## Data das Alterações

29 de Novembro de 2025
