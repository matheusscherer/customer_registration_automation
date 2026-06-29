# customer-registration-automation
Python CLI that automates client registration — CPF validation, duplicate check, UUID ID generation and receipt output. | Senac Tech POA
# ==============================================================================
# SISTEMA DE AUTOMAÇÃO DE CADASTRO - SENAC TECH
# Disciplina: Automação Inteligente para Negócios
# Desenvolvedor: Matheus Farias Scherer
# ==============================================================================

# Banco de dados interno simulado (Lista de dicionários)
# Já inicia com um cliente para podermos testar a validação de duplicidade
banco_de_dados = [
    {
        "id": 1001, 
        "nome": "Ana Souza", 
        "cpf": "12345678901", 
        "rg": "987654321", 
        "endereco": "Av. Alberto Bins, Porto Alegre - RS"
    }
]

# Contador global para gerar IDs únicos automaticamente
proximo_id = 1002

# [Uso do WHILE] Loop principal para manter a automação rodando até o usuário decidir sair
executando = True

while executando:
    print("\n" + "="*40)
    print("      SISTEMA SENAC TECH - MENU      ")
    print("="*40)
    print("1. [Executar] Automação: Cadastro de Cliente")
    print("2. Visualizar Banco de Dados Ativo")
    print("3. Sair do Sistema")
    print("-"*40)
    
    opcao = input("Escolha uma opção (1-3): ").strip()
    
    # 1. Cadastro de Cliente
    if opcao == "1":
        print("\n--- INICIANDO AUTOMAÇÃO DE CADASTRO ---")
        
        # [Entrada] Receber as informações básicas
        nome = input("Digite o nome completo do cliente: ").strip()
        rg = input("Digite o RG do cliente: ").strip()
        endereco = input("Digite o Comprovante de Endereço (Rua, Nº, Cidade): ").strip()
        
        # 2 e 3. [Entrada + Validação com WHILE] Solicitar e validar o CPF
        # O programa não avança enquanto o CPF não estiver no formato correto
        while True:
            cpf = input("Digite o CPF do cliente (apenas os 11 números): ").strip()
            
            # 4. [Processamento] Validação dos dados (Checando se possui 11 dígitos e se são apenas números)
            if len(cpf) == 11 and cpf.isdigit():
                break # CPF válido, sai do loop de validação
            else:
                print("[ERRO DE VALIDAÇÃO] CPF inválido! O CPF deve conter exatamente 11 dígitos numéricos.")
        
        # 5. [Processamento com FOR...ELSE] Consultar banco de dados para evitar duplicidade
        # Esta é uma estrutura elegante do Python: o ELSE do FOR só executa se o loop terminar 
        # sem encontrar nenhum 'break' (ou seja, se nenhuma duplicidade for achada).
        for cliente in banco_de_dados:
            if cliente["cpf"] == cpf:
                print(f"\n[ALERTA DE DUPLICIDADE] O CPF {cpf} já pertence ao cliente ID: {cliente['id']} ({cliente['nome']}).")
                print("Processo de cadastro abortado.")
                break # Interrompe o loop se achar duplicado
        
        else:
            # 6. [Processamento] Vincular o novo perfil a um número de identificação único (ID)
            id_cliente = proximo_id
            proximo_id += 1 # Garante que o próximo ID será diferente
            
            # Estruturando os dados validados em um dicionário
            novo_cliente = {
                "id": id_cliente,
                "nome": nome,
                "cpf": cpf,
                "rg": rg,
                "endereco": endereco
            }
            
            # 7. [Processamento] Salvar definitivamente as informações no banco de dados
            banco_de_dados.append(novo_cliente)
            
            # 8. [Saída] Exibir mensagem de Sucesso e emitir o comprovante na tela
            print("\n" + "*"*45)
            print("  CADASTRO CONCLUÍDO COM SUCESSO!  ")
            print("*"*45)
            print(f" ID GERADO: {novo_cliente['id']}")
            print(f" CLIENTE:   {novo_cliente['nome']}")
            print(f" CPF:       {novo_cliente['cpf']}")
            print(f" RG:        {novo_cliente['rg']}")
            print(f" ENDEREÇO:  {novo_cliente['endereco']}")
            print("*"*45)
            print("[SISTEMA] Comprovante digital emitido e salvo.")

    elif opcao == "2":
        print("\n--- CONSULTANDO BANCO DE DADOS INTERNO ---")
        if not banco_de_dados:
            print("Nenhum cliente cadastrado.")
        else:
            for cliente in banco_de_dados:
                print(f"ID: {cliente['id']} | Nome: {cliente['nome']} | CPF: {cliente['cpf']}")
                
    elif opcao == "3":
        print("\nEncerrando o sistema de automação Senac Tech. Até logo!")
        executando = False # Altera a condição do WHILE principal para fechar o programa
        
    else:
        print("[AVISO] Opção inválida! Digite 1, 2 ou 3.")
