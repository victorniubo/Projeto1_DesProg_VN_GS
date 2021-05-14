Algoritmo de Karatsuba
======

;present

Entendendo o Algoritmo
---------

Para entender o Algoritmo de Karatsuba, o jeito mais simples é fazendo as contas na mão e verificando se o resultado bate com o esperado.

Pegue uma calculadora e vamos lá, começaremos com algo simples.

??? Exercício 1

Primeiro, faça a seguinte multiplicalção: $3408 \times 1735 = \,?$

::: Gabarito
Você deve ter chegado em $5.912.880$, se não, tem algo de errado com a sua calculadora...
:::

???

Maravilha, agora anote esse resultado em algum lugar para fazermos o algoritimo.

??? Karatsuba 1
A primeira coisa a se fazer é dividir os números $3408 \text{ e } 1735$ em duas partes iguais

* Primeiro número em $a \text{ e } b$;
* Segundo número em $c \text{ e } d$.

::: Gabarito
* $a = 34$
* $b = 08$
* $c = 17$
* $d = 35$
:::
???

Fazemos isso para podermos simplificar as chamadas nos próximos passos.

??? Karatsuba 2
Agora, calcule $P \text{ e } S$, que são respectivamente: $a \times c \; \; e \; \; b \times d$;

::: Gabarito
* $P: \; 34 \times 17 = \; 578$
* $S: \; 08 \times 35 = \; 280$
:::
???

Note que multiplicamos as partes maiores entre si, e o mesmo é feito com as partes menores, isso não é em vão, o motivo pelo qual isso é feito ficará mais claro logo logo.

??? Karatsuba 3
Calcule  $\; T \, = \; (a + b) \times (c + d)$.
::: Gabarito

$T \,$: $\; (34 + 08) \times (17 + 35) = \; 2184$
:::
???

Agora, o nós teriamos que calcular $(ad + bc)$, porém, isso não parece muito viável, visto que adicionaria mais duas multiplicações para o nosso algoritimo. Portanto, usaremos utilizaremos um pequeno truque para nos ajudar.

O truque é o seguinte, iremos subtrair os termos $P\text{ e }S$ de $T$. Fazemos isso para trabalharmos apenas com subtrações.

??? Karatsuba 4
Faça o cálculo: $\; T - S - P$ e chame o resultado de $Q$.

::: Gabarito
$Q \,$: $\; T - S - P = \; 1326$
:::
???

A razão desse truque funcionar é que $T = ac + ad + bc + bd \text{ e } P=ac \text{, } S=bd$ e quanto fazemos a subtração, isso nos deixa apenas com os termos $(ad + bc)$ desejados. Coisa de doido, né?

Calma lá que já estamos no fim, nesse último passo tudo ficará mais claro!

??? Karatsuba 5
Realize a seguinte equação e verifique se os resultados batem.

$$R \, = \; P \times 10^n + Q \times 10^{n/2} + S$$

Sendo $n$ o número de algarismos.

::: Gabarito
Você deve ter chegado em:
$$R: \; 578 \times 10^4 + 1326 \times 10^{4/2} + 280 = \; 5.912.880$$
O motivo de multiplicarmos $P\text{ por } 10^n$ é para deslocarmos o número para seu local ideal, o mesmo é feito com $Q\times 10^{n/2}$.
:::
???

Agora você deve estar pensando *"esse método é inútil... faço muito mais rápido na calculadora"*, mas não é exatamente assim que seu computador pensa! Antes de entender porque o algoritmo de karatsuba é mais rápido no computador precisamos analisar como ele seria construido em código.

Veja o pseudocódigo a seguir e analise-o:

``` py
def karatsuba(num1, num2):

    # Retorna a multiplicação caso o n seja menor que 1
    if (num1 < 10) or (num2 < 10)
        return num1 × num2
    
    # Calcula o tamanho dos números. 
    n = min(size_base10(num1), size_base10(num2))
    # Pega o menor número de algarismos e divide por 2 
    n2 = floor(n / 2) 
    
    # Separa os números em duas partes. 
    a, b = split_at(num1, n2)
    c, d = split_at(num2, n2)
    
    # 3 chamas recursivas feitas para números de aproximadamente metade do tamanho (n/2). 
    S = karatsuba(b, d)
    Q = karatsuba((b + a), (d + c))
    P = karatsuba(a, c)
    
    return (P × 10**(n2 × 2)) + ((Q - P - S) × 10**n2) + S
``` 

*"Tá... continuo achando esse processo todo inútil, olha quanta linha pra fazer uma multiplicação comum!"*. Realmente ainda parece improvável que esse método seja mais rápido que uma multiplicação comum, mas não vá tirando conclusões precipitadas, ainda nem calculamos a complexidade do algoritimo!

??? Exercício

Vamos calcular a complexidade do algoritmo de karatsuba, lembra como faz? 

!!! Dica
São três chamadas recursivas que recebem $\frac{n}{2}$ como parâmetro.
!!!

::: Gabarito

$$f(n) =
  \begin{cases}
    1       & \quad \text{se } n \leq 1\text{;}\\
    3f(n/2) + n  & \quad \text{se } n > 1\text{.}\
  \end{cases}
$$

![](tree.png)

Segundo a receita de complexidade vista em aula:

$$\frac{n}{2^{(h-2)}} > 1$$

Disso segue que:

$$h < \log_2{n} + 2$$

Agora, ao longo dos andares temos:

$$(n + n(3/2) + n(9/4) + ... + n(3/2)^{(h-2)}) + 3^{(h-1)} $$

O primeiro parentesês é uma soma de PG com:

* primeiro elemento $n$;
* razão $3/2$;
* número de elementos $h-1$.

Resultando em:

$$n * \frac{(3/2)^{(h-1)} - 1}{(3/2)-1} + 3^{(h-1)}$$

Simplicando, temos:

$$\frac{4n * \frac{3^h}{2^h} + 3^h}{3} - 2n$$

Agora, substituindo o $h$ pelo encontrado anteriormente:

$$< \frac{4n * \frac{3^{\log_2{n} + 2}}{2^{\log_2{n} + 2}} + 3^{\log_2{n} + 2}}{3} - 2n$$

$$< \frac{4n * \frac{3^{\log_2{n}}*3^2}{n*2^2} + 3^{\log_2{n}} * 3^2}{3} - 2n$$

$$< \frac{4n * \frac{3^{\log_2{n}}*9}{4n} + 3^{\log_2{n}} * 9}{3} - 2n$$

$$< \frac{3^{\log_2{n}}*9 + 3^{\log_2{n}} * 9}{3} - 2n$$

$$< 6*3^{\log_2{n}} - 2n$$


Com a mudança de base logaritimica:

$$< 6*3^{\frac{\log_3{n}}{\log_3{2}}} - 2n$$

$$< 6*n^{\frac{1}{\log_3{2}}} - 2n$$

$$< 6*n^{\frac{1}{\frac{\log_2{2}}{\log_2{3}}}} - 2n$$

$$< 6*n^{\log_2{3}} - 2n$$

Com isso chegamos numa complexidade: 

$$O(n^{\log_2{3}}) \rightarrow O(n^{1.584})$$

:::

???

Olha só, chegamos em $O(n^{1.584})$, acredita em mim agora que karatsuba consome menos tempo que uma multiplicação comum?

------------------------------------------------------------------------
<!--1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md ;` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

;bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
--!>