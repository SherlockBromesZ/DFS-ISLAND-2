# Explicação do Algoritmo de Contagem de Ilhas

Este algoritmo simples foi projetado para contar o número de "ilhas" em uma grade 2D. Cada ilha é composta por `1`s conectados vertical e horizontalmente e é cercada por `0`s ou pela borda da grade.

## Como Funciona

A abordagem principal deste algoritmo é usar Busca em Profundidade (DFS) para explorar cada ilha e marcar todas as suas partes, para que não contemos a mesma ilha mais de uma vez. Aqui está uma explicação passo a passo:

### Guia Passo a Passo

1. **Inicializar a Grade**
   - Temos uma grade de inteiros onde cada valor é ou `0` ou `1`. `1` representa terra e `0` representa água.

2. **Array Visitado**
   - Um array booleano 2D `no_visitado` é usado para manter o controle de quais células foram visitadas para evitar contar a mesma parte da terra várias vezes.

3. **Função de Busca em Profundidade (DFS)**
   - `dfs` é uma função que nos ajudará a visitar todas as partes de uma ilha. Ela marca a célula atual como visitada e depois visita recursivamente todas as suas células adjacentes de terra (acima, abaixo, esquerda, direita).
   - **Verificação de Limite:** Antes de acessar uma célula, garantimos que ela esteja dentro dos limites da grade.
   - **Verificação de Terra e Visita:** Só queremos continuar a DFS se a célula for terra (`1`) e ainda não tiver sido visitada.

4. **Contar Ilhas**
   - Iteramos por cada célula na grade.
   - Se encontrarmos uma célula com `1` que não foi visitada, é o início de uma nova ilha. Começamos uma DFS a partir daí.
   - Cada chamada de DFS marcará todas as células dessa ilha como visitadas.
   - Incrementamos nossa contagem de ilhas cada vez que iniciamos uma nova DFS a partir de uma célula de terra não visitada.

### Análise do Código de Exemplo

Aqui está o código fornecido com comentários explicando cada parte:

```cpp
#include<bits/stdc++.h>
using namespace std;

// Função DFS para marcar todas as partes adjacentes de terra de uma ilha
void dfs(int x, int y, vector<vector<int>> &grid, vector<vector<bool>> &no_visitado) {
    // Verifica se a posição atual está fora dos limites da grade
    if(x < 0 || y < 0 || x >= grid.size() || y >= grid[0].size()) return;

    // Verifica se a célula atual é água ou já foi visitada
    if(grid[x][y] == 0 || no_visitado[x][y] == true) return;
    
    // Marca a célula atual como visitada
    no_visitado[x][y] = true;
    
    // Visita recursivamente todas as células adjacentes (acima, abaixo, esquerda, direita)
    dfs(x+1, y, grid, no_visitado);
    dfs(x-1, y, grid, no_visitado);
    dfs(x, y+1, grid, no_visitado);
    dfs(x, y-1, grid, no_visitado);
}

int main() {
    int count = 0;
    vector<vector<int>> grid = {
        {1,1,0,0},
        {1,1,0,0},
        {0,0,1,1},
        {0,0,1,1}
    };

    // Inicializa o array visitado com as mesmas dimensões da grade
    vector<vector<bool>> no_visitado(grid.size(), vector<bool>(grid[0].size(), false));
    
    // Itera sobre cada célula na grade
    for (int i = 0; i < grid.size(); i++) {
        for (int j = 0; j < grid[0].size(); j++) {
            // Se a célula for terra e não visitada, é uma nova ilha
            if (!no_visitado[i][j] && grid[i][j] == 1) {
                dfs(i, j, grid, no_visitado); // Visita todas as partes desta ilha
                count++; // Incrementa a contagem de ilhas
            }
        }
    }
    
    cout << count << endl; // Exibe o número de ilhas
    return 0;
}
```

### Pontos Chave

- **DFS é ideal para este problema** porque visita eficientemente cada componente conectado (ilha) usando uma abordagem baseada em pilha (implicitamente via recursão).
- **Nenhum espaço extra** é utilizado além do array `no_visitado` e da pilha de chamadas do sistema para recursão.
- **Casos limite** são tratados pelas verificações de limite na função DFS.

Este algoritmo é eficiente e direto, tornando-o perfeito para entender rapidamente como os componentes conectados podem ser contados em uma grade 2D.
