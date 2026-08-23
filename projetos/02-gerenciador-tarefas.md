# Projeto 2 — Gerenciador de Tarefas (CLI + arquivo)

> **Objetivo:** gerenciador de tarefas persistente em arquivo (`kof.io`),
> com filtros, ordenação e conclusão. Puro CLI.

## Requisitos

- Módulos: 00, 01 (busca/ordenação), `kof.io` (persistência).
- Tarefas salvas em `tarefas.txt` (uma por linha, formato simples).
- Comandos: listar, adicionar, concluir, remover, filtrar por status.

## Modelo

```kof
record Tarefa(Int id, String titulo, Bool concluida)
```

Persistência simples: `id|titulo|true` por linha.

## Passo a passo

1. `carregar()` — lê `tarefas.txt` e devolve `List<Tarefa>`.
2. `salvar(lista)` — escreve todas as linhas.
3. `adicionar(titulo)` — novo id = maior id + 1.
4. `concluir(id)` — marca como concluída.
5. `remover(id)` — remove da lista.
6. `listar()` — imprime com status; opção de filtrar pendentes/concluídas.
7. `ordenarPorTitulo()` — adapte o insertion/merge sort do módulo 01 para
   ordenar por `titulo`.

## Esqueleto

```kof
record Tarefa(Int id, String titulo, Bool concluida)

String linhaDe(Tarefa t) {
    return t.id + "|" + t.titulo + "|" + t.concluida
}

Tarefa tarefaDe(String linha) {
    // split("|") e construa a Tarefa
}

List<Tarefa> carregar() {
    var lista = listOf<Tarefa>()
    var conteudo = readFile("tarefas.txt")
    if (conteudo != null) {
        for (var linha in conteudo.split("\n")) {
            lista.add(tarefaDe(linha))
        }
    }
    return lista
}

void salvar(List<Tarefa> lista) {
    var texto = ""
    for (var t in lista) {
        texto = texto + linhaDe(t) + "\n"
    }
    writeFile("tarefas.txt", texto)
}

main() {
    var tarefas = carregar()
    // interprete args do programa: add/concluir/remover/listar/filtrar
}
```

> **Nota:** o `split("\n")` e a conversão `String → Bool`/`Int` dependem do
> compilador. Verifique e adapte — o importante é o fluxo completo.

## Critérios de aceite

- [ ] Adicionar e persistir (relance o programa e a tarefa continua lá)
- [ ] Concluir marca status
- [ ] Filtrar pendentes vs concluídas
- [ ] Ordenar por título
- [ ] Arquivo corrompido (linha inválida) não quebra o programa
- [ ] Código idiomático (funções top-level, records, exceções)

## Extensões

- Prioridade (alta/média/baixa) e ordenação por prioridade.
- "Desfazer" com a pilha (última ação).
- Estatísticas: quantas concluídas hoje.