# Sistema de Planejamento de Rotas de Entrega em Niterói 🚴‍♂️📦

O trabalho consiste em implementar um sistema de planejamento de rotas de entrega utilizando Prolog, cujo objetivo principal é encontrar o caminho mais curto entre dois pontos (cruzamentos) em uma malha urbana representada como um grafo, onde os vértices são cruzamentos e as arestas representam ruas ou avenidas. Esse sistema foi desenvolvido com foco nos bairros de Icaraí e Santa Rosa, em Niterói, RJ.

## Estrutura do Projeto

### 1. Representação do Mapa
O mapa foi modelado como um grafo, onde:
- **Vértices**: Cada cruzamento entre ruas e avenidas é representado por identificadores únicos, como `C1`, `C2`, etc.
- **Arestas**: Cada rua ou avenida que conecta dois cruzamentos é representada por um predicado `conexao(Cruzamento1, Cruzamento2, Nome, Distancia)`.
  - `Cruzamento1` e `Cruzamento2`: Representam os cruzamentos conectados pela via.
  - `Nome`: Nome da rua ou avenida.
  - `Distancia`: Distância em metros entre os dois cruzamentos.

#### Exemplo de Predicado:
```prolog
conexao(C71, C69, "R. Dr. Paulo César", 150).
conexao(C74, C71, "R. Dr. Paulo César", 180).
conexao(C2, C18, "R. Álvares de Azevedo", 170).
conexao(C18, C19, "R. Álvares de Azevedo", 150).
```

<br>&nbsp;<br>

### 2. Algoritmo de Busca
O algoritmo utilizado para encontrar o caminho mais curto entre dois cruzamentos foi o **A***. Esse método combina o custo acumulado com uma estimativa heurística para priorizar os nós mais promissores a cada etapa.

#### Componentes do A*:
- **Custo acumulado** (`g(n)`): A soma das distâncias reais percorridas desde o cruzamento inicial até o atual.
- **Heurística** (`h(n)`): Uma estimativa do custo restante até o destino, baseada no número mínimo de arestas entre os cruzamentos.
- **Custo total** (`f(n)`): A soma do custo acumulado e da heurística:
  
  
  ```f(n) = g(n) + h(n)```
  
#### Estimativa Heurística: Escolha e Justificativa

A escolha da heurística é uma parte crucial na implementação do algoritmo A*, pois ela influencia diretamente o desempenho e a eficiência na busca pelo caminho mais curto. Nesse trabalho, a heurística foi baseada no **número mínimo de arestas** entre dois cruzamentos, multiplicado pelo custo médio de uma aresta no grafo. Essa abordagem foi escolhida por ser prática, genérica e computacionalmente eficiente para um grafo urbano de grande escala, como o que representa os bairros de Icaraí e Santa Rosa.

#### Por que não utilizar a distância em linha reta?
Embora a distância em linha reta entre dois cruzamentos seja uma heurística válida e geralmente mais precisa para estimar o custo restante, implementá-la apresentaria algumas limitações no contexto deste trabalho:

1. **Complexidade na Determinação das Distâncias**: 
   - Para calcular as distâncias em linha reta, seria necessário conhecer as coordenadas geográficas (latitude e longitude) de cada cruzamento no grafo.
   - Com mais de 100 vértices no grafo, isso implicaria a obtenção de um conjunto substancial de dados adicionais, bem como o cálculo das distâncias entre todos os pares de vértices relevantes, aumentando a carga de pré-processamento.

2. **Manutenção e Escalabilidade**:
   - Qualquer modificação no grafo, como a inclusão de novos cruzamentos ou alterações nas conexões existentes, exigiria uma atualização completa das distâncias em linha reta. Isso tornaria o sistema menos flexível e mais difícil de manter.

#### Por que utilizar o número mínimo de arestas?
A heurística baseada no número mínimo de arestas apresenta vantagens importantes:

1. **Eficiência Computacional**:
   - O cálculo do número mínimo de arestas requer apenas a análise da estrutura do grafo e não depende de coordenadas ou cálculos geométricos.

2. **Generalidade**:
   - Essa abordagem é aplicável independentemente da geografia específica da cidade, tornando-a uma solução mais genérica para o problema.

#### Outra Abordagem Possível: Algoritmo com Atualização Dinâmica de Pesos

Uma alternativa para otimizar o sistema seria implementar um algoritmo com **atualização dinâmica de pesos**, semelhante ao usado por aplicativos como o Waze. Nesse caso, as **arestas do grafo** (ruas e avenidas) teriam seus pesos (distâncias) ajustados dinamicamente com base em condições reais de trânsito, como congestionamentos, obras ou acidentes.

#### Como Funcionaria?
1. **Dados em Tempo Real**:
   - O sistema seria alimentado por informações atualizadas sobre o fluxo de tráfego, coletadas por sensores, GPS de entregadores, ou dados fornecidos por APIs externas (como Google Maps).

2. **Atualização dos Pesos**:
   - As distâncias estáticas representadas nos predicados `conexao(Cruzamento1, Cruzamento2, Nome, Distancia)` seriam substituídas por **pesos variáveis**, que representariam o tempo estimado para percorrer cada trecho.
   - Por exemplo:
     ```prolog
     conexao(C71, C69, "R. Dr. Paulo César", TempoAtualizado).
     ```

3. **Heurística Baseada em Tempo**:
   - A heurística do algoritmo A* deixaria de usar distâncias médias ou mínimas e passaria a estimar o **tempo restante**, considerando condições de tráfego em tempo real.

#### Benefício:
  - Ao priorizar ruas com menor tráfego, o sistema encontraria rotas mais rápidas, mesmo que envolvam distâncias maiores.

#### Limitações:
- **Coleta de Dados**:
  - A implementação desse método exige infraestrutura para obter informações de tráfego em tempo real, o que pode não ser viável em todas as cidades.

- **Complexidade Adicional**:
  - Incorporar pesos dinâmicos aumenta a complexidade computacional do algoritmo e a necessidade de integração com fontes externas.

<br>&nbsp;<br>
### 3. Fluxo de Implementação

#### Passo 1: Modelagem do Grafo
1. Definir os cruzamentos (`C1`, `C2`, etc.) e as conexões entre eles.
2. Representar as ruas/avenidas e suas distâncias em predicados `conexao/4`.

#### Passo 2: Implementação do Algoritmo A*
1. Implementar o cálculo do custo acumulado (`g(n)`).
2. Implementar a função heurística baseada no número mínimo de arestas.
3. Implementar o A* para calcular o caminho mais curto e o custo total.

#### Passo 3: Testes
1. Criar cenários de teste para rotas curtas e longas dentro dos bairros de Icaraí e Santa Rosa.
2. Validar os resultados verificando se o caminho retornado é de fato o mais curto.

<br>&nbsp;<br>
### 4. Estrutura do Código

#### Predicados Principais:
- **`conexao/4`**: Representa as conexões entre cruzamentos.
  - Exemplo: `conexao(C1, C2, "Rua das Flores", 100).`

- **`rota_mais_curta/4`**: Calcula a rota mais curta entre dois cruzamentos usando o A*.
  - Parâmetros:
    1. Cruzamento inicial.
    2. Cruzamento final.
    3. Lista com os cruzamentos na rota.
    4. Custo total da rota.

#### Função Heurística:
- Baseada no número mínimo de arestas multiplicado pelo custo médio das arestas.
  - Exemplo:
    ```prolog
    heuristica(C1, C5, H) :- numero_minimo_arestas(C1, C5, Arestas), custo_medio(CM), H is Arestas * CM.
    ```

<br>&nbsp;<br>
### 5. Uso do Sistema

#### Exemplo de Consulta:
Para encontrar a rota mais curta entre `C1` e `C10`:
```prolog
?- rota_mais_curta(C1, C10, Caminho, Custo).
Caminho = [C1, C3, C7, C10]
Custo = 450.
```

<br>&nbsp;<br>
### 6. Visualização do Grafo
Para facilitar a compreensão do mapa, foi criada uma representação visual do grafo, mostrando os cruzamentos e as conexões.
