from collections import deque
import heapq

labirinto = [
    ['S', '.', '.', '#', '.', '.', '.'],
    ['.', '#', '.', '#', '.', '#', '.'],
    ['.', '#', '.', '.', '.', '#', '.'],
    ['.', '.', '#', '#', '.', '.', '.'],
    ['#', '.', '.', '.', '#', '.', 'E']
]


def bfs(lab):
    linhas, colunas = len(lab), len(lab[0])

    for i in range(linhas):
        for j in range(colunas):
            if lab[i][j] == 'S':
                inicio = (i, j)
            if lab[i][j] == 'E':
                fim = (i, j)

    fila = deque([inicio])
    visitados = set([inicio])
    pais = {}
    nos_expandidos = 0

    movimentos = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    while fila:
        atual = fila.popleft()
        nos_expandidos += 1

        if atual == fim:
            break

        for dx, dy in movimentos:
            nx, ny = atual[0] + dx, atual[1] + dy
            if 0 <= nx < linhas and 0 <= ny < colunas:
                if lab[nx][ny] != '#' and (nx, ny) not in visitados:
                    fila.append((nx, ny))
                    visitados.add((nx, ny))
                    pais[(nx, ny)] = atual

    caminho = []
    no = fim
    while no != inicio:
        caminho.append(no)
        no = pais[no]
    caminho.append(inicio)
    caminho.reverse()

    return caminho, nos_expandidos


def heuristica(a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1])


def a_estrela(lab):
    linhas, colunas = len(lab), len(lab[0])

    for i in range(linhas):
        for j in range(colunas):
            if lab[i][j] == 'S':
                inicio = (i, j)
            if lab[i][j] == 'E':
                fim = (i, j)

    fila = []
    heapq.heappush(fila, (0, inicio))

    g = {inicio: 0}
    pais = {}
    nos_expandidos = 0

    movimentos = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    while fila:
        _, atual = heapq.heappop(fila)
        nos_expandidos += 1

        if atual == fim:
            break

        for dx, dy in movimentos:
            nx, ny = atual[0] + dx, atual[1] + dy
            vizinho = (nx, ny)

            if 0 <= nx < linhas and 0 <= ny < colunas and lab[nx][ny] != '#':
                novo_g = g[atual] + 1

                if vizinho not in g or novo_g < g[vizinho]:
                    g[vizinho] = novo_g
                    f = novo_g + heuristica(vizinho, fim)
                    heapq.heappush(fila, (f, vizinho))
                    pais[vizinho] = atual

    caminho = []
    no = fim
    while no != inicio:
        caminho.append(no)
        no = pais[no]
    caminho.append(inicio)
    caminho.reverse()

    return caminho, nos_expandidos
    
print("Caminho BFS:", caminho_bfs)
print("Nós expandidos BFS:", nos_bfs)

print("\nCaminho A*:", caminho_a)
print("Nós expandidos A*:", nos_a)
