from datetime import datetime,timedelta
import pytz

class Cliente:
     def __init__(self):
        self.dados={}

     def cadastrar(self):
          
          
          if self.dados:
            print("Usuario já cadastrado")
            return False
          print("Informe as seguintes informações para a realização do cadastro:")
          nome = input("insira seu nome: ")
          self.dados["NOME"] = nome
          nascimento = input("insira a sua data de nascimento: ")
          self.dados["data de nascimento"] = nascimento
          cpf = input("seu cpf (somente numeros): ")
          self.dados["CPF"] = cpf
          print("insira respectivamente os seguintes dados")
          endereco = input("logradouro / bairro/ cidade / estado: ")
          self.dados["endereço"] = endereco
          print("CADASTROS PREENCHIDOS COM SUCESSO !")

     def exibir_dados(self):
          if not self.dados:
              print("Nenhuma informação encontrada ! Realize um cadastro")
              return False
          
          print("DADOS CADASTRADOS:")
          print(""" CONTA: 
        AGENCIA 001 --- CONTA 25154 ---- DIGITO 02""")
          print("nome", self.dados["NOME"])
          print("nascimento:", self.dados["data de nascimento"])
          print("cpf:", self.dados["CPF"])
          print("Endereço:", self.dados["endereço"])
          return True
     def remover_cadastro(self):
         self.dados.clear()
         print("Dados cadastrais apagados com sucesso !!")
         return
     def tem_cadastro(self):
         return bool(self.dados)  # verifica se o dicionario esta vazio ou nao em resumo aplciando uma boleana de true (cheio) ou false( se estiver vazio)
     


class Conta():     
    def __init__(self,cliente):
        self.data_inicial=datetime.now(pytz.timezone("America/Sao_Paulo"))
        self.saldo= 0  
        self.numero_saque=0
        self.limite_saque=3
        self.extrato= ""
        self.limite_diario=0
        self.cliente=cliente
        
    def verificar_novo_dia(self):
          hoje=datetime.now(pytz.timezone("America/Sao_Paulo"))
          if hoje.date()!=self.data_inicial.date():
               self.limite_diario=0
               self.limite_saque=3
               self.data_inicial=hoje

    def verificar_cliente(self):
        if not self.cliente.tem_cadastro():
            print("realize um cadastro para efetuar operações")
            return False
        return True      # sao basicamentes condicioes que atendendem o metodo da booleana acima ! 

    
    def depositar(self):
        if not self.verificar_cliente():
            return
        data=datetime.now(pytz.timezone("America/Sao_Paulo")).strftime("%d/%m/%Y %H:%M")
        solic_deposito=int(input("Selecione o valor que deseja depositar e insira as cedulas"))
        if solic_deposito >=5:
            self.extrato+= f"Deposito R$ +{solic_deposito:.2f} {data}\n"
            self.saldo+=solic_deposito
            self.limite_diario+=1
            print(f"Deposito de R${solic_deposito} feito com sucesso")
            print("Obrigado por utilizar os nossos serviços !!")
            return 
        else:
            print("Valor minimo de deposito é de R$ 5 reais")
            return 
    
    def sacar(self):
        if not self.verificar_cliente():
            return
        
        if self.numero_saque >=self.limite_saque:
                print("Limite de saques diarios atingidos")
                return 
        
        data=datetime.now(pytz.timezone("America/Sao_Paulo")).strftime("%d/%m/%Y %H:%M")

        valor_saque=int(input("Qual o valor do saque ?"))
        
        if valor_saque >500:
            print("o limite é de R$ 500 reais por saque")
            return 
        elif valor_saque > self.saldo:
            print(f"saldo insuficiente")
            return 
        else:
            self.numero_saque+=1
            self.limite_diario+=1
            self.extrato+=f"Saque - R$ {valor_saque:.2f} {data} \n"
            self.saldo -=valor_saque
            print(f"saque de R${valor_saque} realizado com sucesso! ")
            print("Obrigado por utilizar os nossos serviços !!")
            return self.extrato,self.saldo,self.limite_diario,self.numero_saque
        
    def historico(self):
        if not self.verificar_cliente():
            return
            
     
        print(f"=======EXTRATO=======")
     
        if self.extrato == "":
                print(" Nenhuma operação realizada")
                return 
        else:
                
            print(f"Saldo: {self.saldo}")

            print(self.extrato)

            print("Obrigado por utilizar os nossos serviços !!")
            return
    def criar_Conta(self):
        if not self.verificar_cliente():
            return
        
        
        while True:
            try:
                vinculacao = int(input("[1] CRIAR CONTA CORRENTE [2] EXCLUIR DADOS CADASTRAIS: "))
                if vinculacao not in [1, 2]: 
                    print("selecione uma opção valida")
                    continue
                break    
            except ValueError:
                print("digite um valor valido")
                continue
                
        if vinculacao == 1:
            self.saldo = 0
            print("Conta Efetivada com Sucesso! ")
            print("deposite para começar autilizar nossos serviços ")
        elif vinculacao == 2:
            self.cliente.remover_cadastro()

def menu():
    cliente =Cliente()
    conta = Conta(cliente)
    while True:
        conta.verificar_novo_dia()

        try:
            opcoes = int(input("[1] DEPOSITO \\ [2] SAQUE\\ [3] EXTRATO\\[4] CADASTRO E EFETIVAÇÃO DE CONTA\\ [5] INFORMAÇÕES DA CONTA: "))
            
            if opcoes not in [1, 2, 3, 4, 5]:
                print("insira um valor válido")
                continue
                
            if opcoes in [1, 2] and conta.limite_diario >= 10:
                print("Limite de 10 operações esgotadas no dia")
                continue
            if opcoes==1:
                conta.depositar()
            elif opcoes==2:
                conta.sacar()
            elif opcoes==3:
                conta.historico()
            elif opcoes==4:
                cliente.cadastrar()
                conta.criar_Conta()
            elif opcoes==5:
                cliente.exibir_dados()
            if opcoes in [1,2,3,5]:
                    
                    saida = int(input(" [1] retornar as opções\\ [2] sair: "))
                    if saida == 1:
                        continue
                    elif saida == 2:
                        break
        except ValueError:
            print("DIGITE UM VALOR VALIDO")



menu()





