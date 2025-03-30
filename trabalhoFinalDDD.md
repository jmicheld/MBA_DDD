
# DDD - Domain Drive Design - Trabalho Final - Turma: 8AOJR

## Professor: Thiago Keller Torquato Vicco

## Alunos

### RM359096 - Francisco Anderson Rodrigues da Silva
### RM359921 - Jean Michel Deschamps
### RM359610 - Marcello Naz�rio De Oliveira Ara�jo

---

## 1. Nome do Projeto

### Alugo

---

## 2. Objetivo Principal do Projeto
Plataforma facilitadora de acesso a itens que possam ser alugados entre pessoas e empresas (B2C), empresas e empresas(B2B) e pessoas e pessoas(C2C).

---

## 3. Identificação dos Subdomínios

| **Subdomínio**              | **Descrição**                                                                                    | **Tipo**         |
|-----------------------------|--------------------------------------------------------------------------------------------------|------------------|
| Itens           			  | Gerencia os itens, agendamento de aluguel.                                      				 | Core Domain      |
| Gestão de Aluguel			  | Gerencia os contratos de aluguel e devolução de itens  		                            		 | Core Domain      |
| Usuários                    | Gerencia o login, cadastro e permissões de locatário e locador.                                  | Supporting       |
| Pagamentos                  | Processa pagamentos e repassa valores para plataforma e usuário.                                 | Generic          |
| Financeiro                  | Controle de informações sobre recebimento e pagamentos.                                          | Supporting       |
| Sistema                     | Realiza as tarefas automatizadas como envio de Email/SMS                                         | Generic          |
---

## 4. Desenho dos Bounded Contexts

| **Bounded Context**           | **Responsabilidade**                                                                                 | **Subdomínios Relacionados** |
|-------------------------------|------------------------------------------------------------------------------------------------------|-----------------------------|
| Contexto de Aluguel           | Gerencia os aluguéis de itens.                                                                       | Gestão de Aluguel           |
| Contexto de Pagamentos        | Processa cobranças de itens locados.                                                                 | Pagamentos                  |
| Contexto de Locatário         | Processa as informações relacionadas ao usuário locatário.                                           | Usuários                    |
| Contexto de Locador           | Processa as informações relacionadas ao usuário locador.                                             | Usuários                    |
| Contexto de Catálago          | Gerencia as informações de itens.                                                                    | Itens                       |
| Contexto de Recebíveis        | Gerencia as informações de valores a receber.                                                        | Itens                       |
| Contexto de Contas a Pagar    | Gerencia as informações de valores a serem pagos.                                                    | Itens                       |
| Contexto de Agenda            | Gerencia as agenda de datas dos itens locados.                                                       | Itens                       |

---

## 5. Comunicação entre os Bounded Contexts

| **De (Origem)**              | **Para (Destino)**          | **Forma de Comunicação**    | **Exemplo de Evento/Chamada**                  |
|------------------------------|-----------------------------|-----------------------------|------------------------------------------------|
| Contexto de Locador          | Contexto de Catálogo        | API                         | Gerenciar informações de itens                 |
| Contexto de Locador          | Contexto de Agenda          | API                         | Configura a agenda de cada item                |
| Contexto de Locador          | Contexto de Aluguel         | API                         | Efetua uma ordem de aluguel de um Item pelo ID |
| Contexto de Locatário        | Contexto de Catálogo        | API                         | Consulta de itens a seren alugados             |
| Contexto de Locatário        | Contexto de Agenda          | API                         | Consulta a agenda de um item pelo ID           | 
| Contexto de Catálogo         | Contexto de Aluguel         | API                         | Criar uma ordem de aluguel de um Item pelo ID  |
| Contexto de Aluguel          | Contexto de Pagamentos      | API                         | Gerar a transação de pagamento                  |
| Contexto de Pagamentos       | Contexto de Recebíveis      | Mensageria                  | Obter informações de pagamentos                |

---

## 6. Definição da Linguagem Ubíqua

| **Termo**                    | **Descrição**                                                                                 |
|------------------------------|-----------------------------------------------------------------------------------------------|
| Aluguel                      | Contrato firmado entre locador e locatário para aluguel de um item                            |
| Locador                      | Usuário que disponibilizada algum item para locação                                           |
| Locatário                    | Usuário que efetua o alguem de algum item                                                     |
| Item                         | Qualquer objeto disponível para aluguel na plataforma                                         |
| Agenda                       | Disponibilidade de data para aluguel de um item                                               |
---

## 7. Estratégia de Desenvolvimento

| **Subdomínio**              | **Estratégia**                         | **Ferramentas ou Serviços (se aplicável)** |
|-----------------------------|--------------------------------------------|----------------------------------------|
| Itens           			  | Desenvolvimento interno                    |                                        |
| Usuários                    | Interno com uso de autenticação para login | Keycloack                              |
| Pagamentos                  | Terceirizar usando gateway de pagamentos   | Stone                                  |
| Gestão de Aluguel		      | Desenvolvimento interno                    |                                        |
| Financeiro                  | Módulo Tercerizado                         | Omie / e-Gestor                        |

---

## 8. Entidades vs. Objetos de Valor

| **Elemento**                | **Entidade ou Value Object?** | **Justificativa** |
|-----------------------------|------------------------------|-------------------|
| **Locador**                 | Entidade                     | Possui identidade única e pode atualizar seus dados ao longo do tempo. |
| **Locatário**               | Entidade                     | Tem um ID único e pode mudar seus dados pessoais. |
| **Item**                    | Entidade                     | Cada item possui uma identidade única na plataforma. |
| **Aluguel**                 | Entidade                     | Representa uma transação única entre locador e locatário. |
| **Agenda**                  | Entidade                     | Possui um conjunto de datas específicas associadas a um item. |
| **Pagamento**               | Entidade                     | Cada pagamento tem um identificador único e pode ser atualizado com status de aprovação. |
| **Contrato de Aluguel**      | Entidade                     | Representa o vínculo formal entre locador e locatário e pode ter alterações. |
| **Endereço**                | Value Object                 | Se um usuário mudar de endereço, o antigo é substituído, não modificado. |
| **CPF/CNPJ**                | Value Object                 | Sempre pertence a apenas um usuário e não muda. |
| **Valor do Aluguel**        | Value Object                 | Representa um montante específico sem identidade própria. |
| **Período de Aluguel**      | Value Object                 | Um intervalo de tempo que pode ser substituído, mas não alterado. |

---

# 9. Agregados e Aggregate Root

## 📌 Agregados Definidos

### **Agregado: Aluguel**  
**Aggregate Root:** Aluguel  
- **Locador** (Entidade)  
- **Locatário** (Entidade)  
- **Item** (Entidade)  
- **Período de Aluguel** (Value Object)  
- **Valor do Aluguel** (Value Object)  
- **Contrato de Aluguel** (Entidade)  

### **Agregado: Pagamento**  
**Aggregate Root:** Pagamento  
- **Aluguel** (Entidade)  
- **Valor do Aluguel** (Value Object)  
- **Status do Pagamento** (Value Object)  

### **Agregado: Catálogo de Itens**  
**Aggregate Root:** Item  
- **Locador** (Entidade)  
- **Agenda** (Entidade)  
- **Descrição do Item** (Value Object)  

---

## 📌 Regras de Negócio nos Agregados  

### **Agregado: Aluguel**  
✅ Um item só pode estar associado a um aluguel ativo por vez.  
✅ Um aluguel finalizado não pode ser alterado.  

### **Agregado: Pagamento**  
✅ Um pagamento só pode ser gerado para um aluguel válido.  
✅ Após um pagamento ser processado com sucesso, ele não pode ser modificado.  

### **Agregado: Catálogo de Itens**  
✅ O locador pode gerenciar os itens disponíveis para aluguel.  
✅ A agenda de um item não pode ter datas sobrepostas para aluguéis diferentes.  


---

## 10. Repositórios

## 📌 Interfaces de Repositório

### **Repositório para Aluguel**

```csharp
public interface IAluguelRepository
{
    Aluguel ObterPorId(Guid id);
    void Salvar(Aluguel aluguel);
}
```

📌 Por que o repositório trabalha apenas com Aluguel?

Porque Aluguel é o Aggregate Root, então Locador, Locatário, Item e Contrato de Aluguel são gerenciados por ele.

### **Repositório para Pagamento**

```csharp
public interface IPagamentoRepository
{
    Pagamento ObterPorId(Guid id);
    void Salvar(Pagamento pagamento);
}
```

📌Por que o repositório trabalha apenas com Pagamento?

Porque Pagamento é o Aggregate Root, então Valor do Aluguel e Status do Pagamento são gerenciados por ele.

### **Repositório para Catálogo de Itens**

```csharp
public interface IItemRepository
{
    Item ObterPorId(Guid id);
    void Salvar(Item item);
}
```

📌Por que o repositório trabalha apenas com Item?
Porque Item é o Aggregate Root, então Locador, Agenda e Descrição do Item são gerenciados por ele.

---

# 📌 Event Storming para o Projeto "Alugo"

## **Processo Central Escolhido**
**Processo:** Aluguel de um item na plataforma.  

---

## **Mapeamento de Eventos de Domínio**
**Pergunta-chave:** "O que acontece no processo?"  

### Eventos principais:
- **Item Disponibilizado para Aluguel**
- **Solicitação de Aluguel Criada**
- **Pagamento Efetuado**
- **Aluguel Confirmado**
- **Item Retirado**
- **Item Devolvido**
- **Pagamento Repasse Efetuado ao Locador**

---

## **Identificação de Comandos e Atores**

**Pergunta-chave:** "O que causou esse evento?"  

| **Comando**                     | **Ator**          | **Evento Gerado**                    |
|----------------------------------|------------------|--------------------------------------|
| Publicar Item para Aluguel       | Locador          | Item Disponibilizado para Aluguel   |
| Solicitar Aluguel                | Locatário        | Solicitação de Aluguel Criada       |
| Efetuar Pagamento                | Locatário        | Pagamento Efetuado                  |
| Aprovar Aluguel                  | Sistema          | Aluguel Confirmado                  |
| Retirar Item                     | Locatário        | Item Retirado                        |
| Devolver Item                    | Locatário        | Item Devolvido                       |
| Efetuar Repasse ao Locador        | Sistema          | Pagamento Repasse Efetuado ao Locador |

---

## **Descobrindo Regras e Políticas de Negócio**

**Pergunta-chave:** "Quais regras precisam ser seguidas nesse processo?"  

### Políticas:
✅ **O item só pode ser alugado se estiver disponível no catálogo.**  
✅ **O aluguel só é confirmado após o pagamento aprovado.**  
✅ **O locatário tem um prazo máximo para retirar o item, senão o aluguel é cancelado.**  
✅ **Se o item não for devolvido dentro do prazo, uma multa será aplicada automaticamente.**  
✅ **Após a devolução do item, o sistema processa o repasse do valor ao locador.**  

### Integrações externas:
🔗 **Gateway de Pagamento** (ex: Stone) para processar transações.  
🔗 **Serviço de Notificações** para envio de e-mails e SMS.  

---

## **Identificação dos Bounded Contexts**
**Pergunta-chave:** "Quem é responsável por cada parte do processo?"  

| **Contexto**           | **Eventos Relacionados**                     |
|------------------------|---------------------------------------------|
| **Contexto de Catálogo**   | Item Disponibilizado para Aluguel         |
| **Contexto de Aluguel**    | Solicitação de Aluguel Criada, Aluguel Confirmado, Item Retirado, Item Devolvido |
| **Contexto de Pagamentos** | Pagamento Efetuado, Pagamento Repasse Efetuado ao Locador |
| **Contexto de Agenda**     | Verificação de Disponibilidade do Item    |


---
## 12. Diagramas Visuais

![Diagrama 1](https://raw.githubusercontent.com/jmicheld/MBA_DDD/refs/heads/main/DDD-1.png) 
![Diagrama 2](https://raw.githubusercontent.com/jmicheld/MBA_DDD/refs/heads/main/DDD-2.png)

---