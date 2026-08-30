# Trilha 10 — Ciência de Dados com Kof

> **Trilha isolada.** Pré-requisito: Trilha 00, 01, 02. Ao terminar, você
> implementa estatística descritiva, correlação, regressão e classificação
> **em Kof puro** — sem bibliotecas mágicas. A filosofia aplicada a dados:
> *a complexidade da matemática fica no seu código; a plataforma cuida da
> coleção, do arquivo e do fluxo*.

## O que você vai dominar

| Aula | Tema |
|------|------|
| [01-estatistica.md](01-estatistica.md) | média, mediana, moda, variância, desvio padrão |
| [02-correlacao-regressao-knn.md](02-correlacao-regressao-knn.md) | Pearson, regressão linear, normalização, k-NN |

## O que existe na plataforma (estado real)

| Recurso | Estado |
|---------|--------|
| `List<T>`, arrays, `for-in` | ✅ base |
| `Map<K,V>`, `Set<T>` | ✅ |
| `kof.io` (ler CSV de arquivo) | ✅ |
| `json.encode/decode` (dados) | ✅ |
| `now()` (medir) | ✅ |
| `kof.observability` (`counter`/`gauge`/`health`) | ✅ JVM/Native/JS |
| Bibliotecas de ML (pandas/numpy-like) | não existem — **implementamos** |

**Por que isso é Kof:** sem bibliotecas externas, você *entende* o algoritmo.
Dados = `List<Double>`/`List<record>`; a filosofia é representar o domínio.

## Como estudar

1. Leia as aulas na ordem. Rode os exemplos.
2. Faça os [exercícios](exercicios.md).
3. Confira as [soluções](solucoes/).
4. Passe no checkpoint.

## Checkpoint

Com um dataset de alturas e pesos (10 pontos), em um programa:
1. Calcule média, mediana e desvio padrão de cada variável.
2. Calcule a correlação de Pearson.
3. Ajuste a regressão linear `peso = a*altura + b` e **preveja** o peso de uma altura nova.
4. Classifique um novo ponto com k-NN (k=3).

`kof run` sem erros → trilha concluída.