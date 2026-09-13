# Aviso

Deixando claro: o conteúdo abaixo se baseia num curso gratuito e não pode ser redistribuído. Se quiser estudar o tema, procure o curso aberto da trilha de Java com Spring Boot. Se a plataforma retirar o acesso ou passar a cobrar, não depende de mim — mas o material de lá é excelente e quem assina um plano aproveita bem mais.


# Observação inicial

Para os trabalhos que eu, André de Paula Terceiro, fizer em cima deste repositório, consulte o arquivo indicado no projeto.

# Produtividade com stream em Java

- A API de stream trouxe outra alternativa para manipular coleções, apoiada em programação funcional.
- É uma solução poderosa para processar coleções declarativamente, sem a burocracia do estilo imperativo.

```java
public class CestaDeItens {
  //properties
  private List<Item> itensCesta;
  //construtor
  public CestaDeItens() {
    this.itensCesta = new ArrayList<>();
  }
  
  //método para calcular valor total dos itens sem utilizar o Stream API
  public double somarTotal() {
    double soma = 0d;
    if (!itensCesta.isEmpty()) {
      for (Item item : itensCesta) {
        double parcial = item.precoUnitario() * item.quantidade();
        soma += parcial;
      }
      return soma;
    } else {
      throw new RuntimeException("Nenhum item na cesta!");
    }
  }
}
```

- No estilo imperativo, até uma soma simples obriga quem escreve a cuidar tanto do que fazer com cada elemento quanto do jeito de percorrer a coleção.

```java
public class CestaDeItens {
  //properties
  private List<Item> itensCesta;
  //construtor
  public CestaDeItens() {
    this.itensCesta = new ArrayList<>();
  }
  
  //método para calcular valor total dos itens utilizando o Stream API
  public double somarTotal() {
    if (itensCesta.isEmpty()) {
      throw new RuntimeException("Nenhum item na cesta!");
    }
    return itensCesta.stream()
        .mapToDouble(item -> item.precoUnitario() * item.quantidade())
        .sum();
  }
}
```
- Junto com expressões lambda e referências de método, a API oferece outra maneira de lidar com conjuntos de elementos: código mais curto e fácil de manter, com paralelização sem efeitos colaterais em execução.
- As operações se dividem em dois grupos:

1. Intermediárias: devolvem um novo stream e permitem encadear chamadas, formando um fluxo de processamento. São elas:
- `filter`: mantém só os elementos que satisfazem o predicado.
Exemplo: ` stream.filter(n -> n > 5)`
- `map`: aplica a função a cada elemento e devolve um novo stream com os resultados.
Exemplo: `stream.map(s -> s.toUpperCase())`
- `sorted`: ordena os elementos em ordem natural, se forem comparáveis, ou conforme o comparador informado.
Exemplo: `stream.sorted()`
- `distinct`: descarta duplicados, valendo-se da comparação de igualdade da classe.
Exemplo: `stream.distinct()`
- `limit`: corta o fluxo nos primeiros elementos até o limite informado
Exemplo: stream.limit(10)
- `skip`: descarta os primeiros elementos informados e devolve o restante.
Exemplo: `stream.skip(5)`

2. Terminais: encerram o fluxo e produzem um resultado. São elas:
- `forEach`: executa a ação para cada elemento.
Exemplo: `stream.forEach(System.out::println)`
- `toArray`: converte o fluxo em arranjo.
Exemplo: `stream.toArray()`
- `collect`: reúne os elementos numa estrutura escolhida, como lista ou mapa.
Exemplo: stream.collect(Collectors.toList())
- `count`: devolve a quantidade de elementos.
Exemplo: `stream.count()`
- `anyMatch`: diz se algum elemento satisfaz o predicado.
Exemplo: `stream.anyMatch(s -> s.startsWith("A"))`
- `allMatch`: diz se todos os elementos satisfazem o predicado.
Exemplo: `stream.allMatch(n -> n > 0)`
- `noneMatch`: diz se nenhum elemento satisfaz o predicado.
Exemplo: stream.noneMatch(s -> s.isEmpty())
- `min` e `max`: devolvem o menor e o maior elemento conforme o comparador.
Exemplo: `stream.min(Comparator.naturalOrder())` ou `stream.max(Comparator.naturalOrder())`
- `reduce`: combina os elementos com o acumulador informado e devolve o resultado final.
Exemplo: `stream.reduce(0, (a, b) -> a + b)`

### Expressões lambda

- Elas representam interfaces funcionais — aquelas com um único método abstrato — de forma enxuta, abrindo caminho para o estilo funcional.
- Essas interfaces são a base do uso de lambda na linguagem.
- Uma expressão lambda é uma função sem declaração: não precisa de nome, tipo de retorno nem modificador de acesso. A ideia é declarar o método no mesmo ponto em que ele é usado.
- A sintaxe é composta por argumentos, a seta e o corpo.

```java
public class OrdenaPessoas {
  //atributo
  private List<Pessoa> listaPessoas;

  //construtor
  public OrdenaPessoas() {
    this.listaPessoas = new ArrayList<>();
  }

  public List<Pessoa> porAltura() {
    if (!listaPessoas.isEmpty()) {
      List<Pessoa> ordenadasAltura = new ArrayList<>(listaPessoas);
      ordenadasAltura.sort((a1, a2) -> Double.compare(p1.altura(), p2.altura()));
      return ordenadasAltura;
    } else {
      throw new RuntimeException("Nenhum item na cesta!");
    }
  }
}
```

### Referência de método

- É um recurso que permite apontar para um método ou construtor de uma classe de forma funcional, indicando que ele deve ser usado num ponto do código, o que deixa tudo mais simples e legível.
- Para usar, informe a classe ou a referência, dois sinais de dois-pontos e o nome do método, sem parênteses no fim.

```java
public class OrdenaPessoas {
  //atributo
  private List<Pessoa> listaPessoas;

  //construtor
  public OrdenaPessoas() {
    this.listaPessoas = new ArrayList<>();
  }

  public List<Pessoa> porAltura() {
    if (!listaPessoas.isEmpty()) {
      List<Pessoa> ordenadasAltura = new ArrayList<>(listaPessoas);
      ordenadasAltura.sort(Comparator.comparingDouble(Pessoa::altura));
      return ordenadasAltura;
    } else {
      throw new RuntimeException("Nenhum item na cesta!");
    }
  }
}
```

---

### Referências

[1] Artigo técnico da Oracle Brasil sobre a API de stream, disponível em página pública.

[2] Repositório público de exemplos da API de coleções da linguagem, mantido por uma instrutora da plataforma.

[3] Artigo de um portal de desenvolvimento sobre como usar interfaces funcionais na linguagem.