# Otimização de Precificação Dinâmica com Aprendizado por Reforço 📈🛒

Este repositório contém o desenvolvimento de um agente inteligente de precificação dinâmica para e-commerce baseado em **Aprendizado por Reforço (Reinforcement Learning)**. O projeto foi desenvolvido como parte das atividades do **MBA em Ciência de Dados da UNIFOR**, utilizando dados transacionais adaptados do dataset público *Amazon Sale Report*.

O objetivo principal é resolver o clássico dilema do varejo: evitar a perda de margem de lucro em momentos de alta demanda e mitigar a ociosidade logística/custos fixos em momentos de baixa demanda.

---

## 📑 Estrutura do Projeto

*   `trabalho_aprendizado_por_reforco.ipynb`: Jupyter Notebook contendo toda a lógica de extração, tratamento de dados, modelagem do ambiente, implementação do algoritmo Q-Learning e gráficos de convergência.
*   `requirements.txt`: Lista de bibliotecas Python necessárias para rodar o projeto.
*   `README.md`: Documentação do projeto (este arquivo).

---

## 🧠 Modelagem do Problema (Processo de Decisão de Markov)

Para que o agente pudesse aprender a tomar decisões ótimas de preço de forma autônoma, o problema foi modelado como um **MDP (Markov Decision Process)** dentro de um simulador customizado:

### 1. Estados ($S$)
O mercado foi segmentado em 3 níveis de demanda baseados nos quartis de volume de vendas do dataset real:
*   **Demanda Baixa:** Dias com volume de vendas abaixo da média histórica.
*   **Demanda Média:** Dias com volume de vendas estável.
*   **Demanda Alta:** Dias de pico de acessos e compras.

### 2. Ações ($A$)
A cada dia (passo), o agente pode tomar uma de três decisões de precificação:
*   **Preço com Desconto (-15%):** Focado em queimar estoque e atrair volume.
*   **Preço Médio:** Alinhado com a concorrência e o mercado.
*   **Preço Premium (+15%):** Focado em maximizar a margem de lucro.

### 3. Função de Recompensa ($R$)
O coração do aprendizado. A recompensa do agente é o **Lucro Líquido Diário**, calculado por:

$$Recompensa = Receita - Custo\ Fixo\ Operacional$$

Se o agente escolhe um preço muito alto em um dia de demanda baixa, as vendas despencam e o custo fixo pune severamente a recompensa (gerando prejuízo). O agente é forçado a aprender o equilíbrio da elasticidade-preço.

---

## 📊 O Algoritmo: Q-Learning

Foi implementado do zero o algoritmo clássico de **Q-Learning**, que utiliza a Equação de Bellman para atualizar a tabela de qualidade (Q-Table) do agente:

$$Q(s, a) \leftarrow Q(s, a) + \alpha \left( r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right)$$

O agente passou por um treinamento de **2.000 episódios** utilizando uma estratégia $\epsilon$-greedy (epsilon-greedy) para balancear a exploração do ambiente (*exploration*) e a aplicação do conhecimento adquirido (*exploitation*).

### Resultados Obtidos
*   **Fase Inicial:** Alta taxa de exploração, resultando em instabilidade e lucros baixos/negativos.
*   **Convergência:** Após o aprendizado, o agente estabilizou os ganhos no teto possível do ambiente, superando expressivamente a política de baseline (decisões aleatórias).
*   **Estratégia Ótima Aprendida:** Descontos cirúrgicos na demanda baixa para cobrir custos logísticos e preços Premium na demanda alta para extrair a margem máxima.

---

## 🛠️ Tecnologias Utilizadas

*   **Python 3.x**
*   **Pandas & NumPy:** Manipulação e análise dos dados da Amazon.
*   **Matplotlib & Seaborn:** Visualização dos dados e curvas de aprendizado.
*   **Gym/Custom Environment:** Criação do simulador de mercado.

---

## 🚀 Como Executar o Projeto

1. Clone este repositório:
   ```bash
   git clone https://github.com/Emanuel085/datacorp-pricing-rl.git