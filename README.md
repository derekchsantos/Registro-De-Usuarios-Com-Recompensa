# Registro de Usuários com Recompensa

Um contrato inteligente didático desenvolvido em **Solidity** para a rede Ethereum. O projeto demonstra como criar um sistema de cadastro de usuários e distribuição de recompensas em tokens simulados, aplicando conceitos fundamentais de Web3.

## Sobre o Projeto

Este Smart Contract permite que usuários se registrem na blockchain associando seu nome ao seu endereço de carteira (`address`). Ao se registrar e manter-se ativo, o usuário pode receber recompensas em uma simulação de economia de tokens.

### Funcionalidades principais:
- **Registro de Usuários:** Armazena nome, data de registro e status.
- **Prevenção de Duplicidade:** Um endereço de carteira só pode se registrar uma única vez.
- **Sistema de Recompensas:** Distribuição de um valor fixo (`100 tokens`) para usuários ativos.
- **Gestão de Conta:** Funções para consultar dados e desativar contas.
- **Transparência:** Emissão de eventos (logs) para todas as ações importantes.

## Conceitos Explorados

O código contém comentários explicativos sobre pilares da rede Ethereum:
*   **GAS:** Entenda o custo computacional e por que pagamos transações.
*   **EVM (Ethereum Virtual Machine):** O motor que executa este código de forma descentralizada.
*   **Imutabilidade:** A diferença entre contratos tradicionais e Smart Contracts.

## Tecnologias e Ferramentas

- **Linguagem:** Solidity ^0.8.0
- **Ambiente de Desenvolvimento:** [Remix IDE](https://ethereum.org)
- **Padrões:** Conceitos básicos inspirados no ERC-20 (mappings de saldo).

## Como testar

1. Acesse o [Remix IDE](https://ethereum.org).
2. Crie um novo arquivo chamado `RegistroDeUsuarios.sol` e cole o código do contrato.
3. No menu lateral, vá em **Solidity Compiler** e clique em **Compile**.
4. Vá em **Deploy & Run Transactions**, escolha o ambiente `Remix VM` e clique em **Deploy**.
5. No painel abaixo de "Deployed Contracts", você poderá testar as funções:
   - Digite um nome em `registrarUsuario` e clique no botão.
   - Use seu endereço (copiado do topo do Remix) para testar o `recompensarUsuario`.
   - Consulte o saldo e os dados em `consultarUsuario`.

## Estrutura de Dados

O contrato utiliza estruturas eficientes para otimizar o consumo de Gas:
- `mapping`: Para busca rápida de saldos e registros.
- `struct`: Para agrupar informações complexas do usuário.
- `constant`: Para definir valores fixos de recompensa sem gasto excessivo de memória.

---
*Este projeto tem fins educacionais para demonstrar a lógica de negócios dentro da blockchain.*
