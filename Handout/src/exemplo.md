Algoritmo de Karatsuba
======


Na escola aprendemos uma maneira de fazer multiplicações que vamos chamar de algoritmo escolar. O algoritmo escolar consiste em fazer a multiplicação, algarismo por algarismo, dos fatores envolvidos e depois somar os resultados, fazendo os shifts necessários, pra chegar no produto final.

O Algoritmo de Karastuba oferece uma alternativa a essa multiplicação do algoritmo escolar, mas antes de apresenta-lo, para que a utilização dele faça sentido, vamos determinar qual a complexidade de uma multiplicação normal.

??? Atividade

Determine a complexidade de uma multiplicação de dois números de $n$ dígitos feita pelo algoritmo escolar, na notação $O()$, sendo que $n$ é a quantidade de algarismos de cada fator envolvido e a operação base é uma multiplicação com um digito só.  

::: Gabarito
Uma multiplicação de dois número com $n$ dígitos implica em $n \times n$ operações de um dígito, portanto sua complexidade é de $O(n^2)$   
:::

??? 

Uma operação com complexidade $O(n^2)$ pode ser muito custoso em questão de tempo de processamento quando trabalhamos com uma quantidade grande de operações ou operações com muitos algarismos em cada fator, sendo assim, Karatsuba se propõe a diminuir essa complexidade para multiplicações e com isso diminuir o custo dessas operações para os nossos queridos precessadores. Para saber se esse é realmente o caso, precisamos primeiro entender o algoritmo em si.   

Entendendo o Algoritmo de Karatsuba
---------

Ok... temos que uma multiplicação feita usando o algoritmo escolar tem uma complexidade $O(n^2)$ e que Karatsuba diminui essa complexidade, aumentando a velocidade de processamento da mesma operação, porém, não bem a mesma. O algoritmo de Karatsuba pode ser simplificado em cinco passos que apresentaremos a seguir.  

Para entender melhor o Algoritmo de Karatsuba, o jeito mais simples é fazendo as contas na mão e verificando se o resultado bate com o esperado.

Pegue uma calculadora e vamos lá, começaremos com algo simples.

??? Exercício 1

Primeiro, faça a seguinte multiplicalção: $34 \times 17 = \,?$

::: Gabarito
Você deve ter chegado em $578$, se não, tem algo de errado com a sua calculadora...
:::

???

Maravilha, agora anote esse resultado em algum lugar para fazermos o algoritmo.

Primeiro de tudo, vamos relembrar um conceito muito simples mas que é esquecido com frequência, **qualquer** número pode ser representado por uma soma de seus algarismos multiplicados por $10^m$, sendo $m$ a quantidade de casas decimais que faltam para que a primeira parte do número chegue na sua casa decimal inicial. Assim, ao decompormos o um número qualquer $\alpha\beta$, em duas partes obtemos:

$$\theta = \alpha 10^1 + \beta 10^0$$

??? Karatsuba 1
A primeira coisa a se fazer é separar os números $34 \text{ e } 17$ em duas partes: 

* 34 em $a \text{(maior) e } b \text{(menor)}$;
* 17 em $c \text{(maior) e } d \text{(menor)}$;

::: Gabarito
* $a = 3$
* $b = 4$
* $c = 1$
* $d = 7$
:::
???

Agora, iremos realizar a multiplicação destes números compostos para chegar numa equação que nos ajudará a chegar mais prómixos do algoritmo, para isso vamos ver como funcionaria para fazer o produto de $x \text{ e } y$ onde:

$$x = x_1 10^m + x_0$$
$$y = y_1 10^m + y_0$$
$$ $$
$$xy = (x_1 10^m + x_0)(y_1 10^m + y_0)$$

Fazendo a distributiva:

$$x y = x_1 y_1 10^{2m} + (x_1 y_0 + x_0 y_1) 10^m + x_0 y_0$$

Maravilha, mas esse ainda é só o começo, perceba que por enquanto nós temos **4 multiplicações** (as multiplicações por $10^m$ não são consideradas operações de multiplicação pois podem ser atingidas através de shifts) o que não parece muito bom comparado ao que o algoritmo propõe, mas segura firme que logo logo vamos diminuir este número.

??? Karatsuba 2
Agora, calcule $P \text{ e } S$, que são respectivamente: $a \times c \; \;\text{ e } \;\; b \times d$;

::: Gabarito
* $P: \; 3 \times 1 = \; 3$
* $S: \; 4 \times 7 = \; 28$
:::
???

Agora, o resultado final de nossa multiplicação pode ser escrito como $P10^2 + (ad + bc)10^1 + (S)$ neste caso, nosso  $m$ é 1, pois dividimos os números em duas partes, ou seja, $m = n/2$, mas como dito anteriormente, queremos diminuir o número de multiplicações presentes, o seu computador agradece.

É agora que entra o famoso **pulo do gato**! Queremos chegar em $(a \times d) + (b \times c)$ só que sem essas duas multiplicações, para isso, vamos utilizar utilizar uma sacada do nosso querido *Gauss*, primeiro faça o seguinte:  

??? Karatsuba 3
Calcule  $\; T \, = \; (a + b) \times (c + d)$.
::: Gabarito

$T \,$: $\; (3 + 4) \times (1 + 7) = \; 56$
:::
???

Agora, pra chegarmos em $(ad + bc)$ é bem simples! O truque é o seguinte, iremos subtrair os termos $P\text{ e }S$ de $T$. Esse passo elimina uma das multiplicações que seriam necessárias inicialmente.

??? Karatsuba 4
Faça o cálculo: $\; T - S - P$ e chame o resultado de $Q$.

::: Gabarito
$Q \,$: $\; T - S - P = \; 25$
:::
???

Este truque funciona da seguinte forma, ao fazermos a distributiva $(a + b) \times (c + d) \rightarrow T$ e subtrairmos $ac \rightarrow P \text{ e } bd \rightarrow S$, isso nos resulta em:

$$ac + ad + bc + bd - ac - bd$$
$$\quad = ad + bc$$

Isso nos resulta na equação final a seguir:

$$R \, = \; ac \times 10^n + ((a + b) \times (c + d) - P - S) \times 10^{n/2} + bd$$

Note que agora, realizando este passo, nós temos um total de **3 multiplicações** no nosso algoritmo, *now we're talking*.

Agora já estamos com a faca e o queijo na mão, é só partir pro abraço. Lembra daquela equação final que vimos antes, pode aplicar.

??? Karatsuba 5
Realize a seguinte equação e verifique se os resultados batem.

$$R \, = \; P \times 10^n + Q \times 10^{n/2} + S$$

Sendo $n$ o número de algarismos.

::: Gabarito
Você deve ter chegado em:
$$R: \; 3 \times 10^2 + 25 \times 10^{1} + 28 = \; 578$$
Relembrando, o motivo de multiplicarmos $P\text{ por } 10^n$ é para deslocarmos o número para seu local ideal, o mesmo é feito com $Q\times 10^{n/2}$.
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

*"Tá... continuo achando esse processo todo inútil, olha quanta linha pra fazer uma multiplicação comum!"*. Realmente ainda parece improvável que esse método seja mais rápido que uma multiplicação comum, mas não vá tirando conclusões precipitadas, ainda nem calculamos a complexidade do algoritmo!

Calculando a complexidade
---------

Bom, agora só nos resta chegar a complexidade final, para isso, utilizaremos nosso conhecimento adquirido em aula.

??? Complexidade 1

Vamos começar escrevendo a recorrência de nosso pseudocódigo e construindo sua árvore.

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
:::
???

Lembrando um pouco do passo a passo da receita que vimos na [Aula 6](https://ensino.hashi.pro.br/desprog/aula6/index.html). Ao partirmos da raiz, quando descemos $h-2$ andares, chegamos em $f(a)$ tal que $a>1$, por estarmos no último andar antes da base.

Agora, com isso em mente, resolva o próximo exercício.


??? Complexidade 2

Calcule o limitante para a altura e a "soma de vermelhos" (parte não-recursiva das expansões) de cada andar. 

!!! Atenção
Não é necessário simplificar a soma, por enquanto.
!!!

::: Gabarito

$$\frac{n}{2^{(h-2)}} > 1$$

Disso segue que:

$$h < \log_2{n} + 2$$

Agora, ao longo dos andares temos:

$$(n + n(3/2) + n(9/4) + ... + n(3/2)^{(h-2)}) + 3^{(h-1)} $$

:::
???

Sou só eu, ou essa soma ta com uma cara de PG? Pior que é isso mesmo, tirando o $3^{(h-1)}$ ela é uma soma de PG com:

* primeiro elemento $n$;
* razão $3/2$;
* número de elementos $h-1$.

Que maravilha! Agora você está com a faca e o queijo na mão (**O Retorno**), já sabe o que fazer né? 

??? Complexidade 3

Simplifique a "soma de vermelhos" obtida no exercício anterior.

!!! Dica

Dê uma bisbilhotada na [Cola de Complexidade](https://ensino.hashi.pro.br/desprog/aula9/cola.html).

!!!

::: Gabarito

$$n \times \frac{(3/2)^{(h-1)} - 1}{(3/2)-1} + 3^{(h-1)}$$

$$=n \times \frac{(3/2)^{(h-1)} - 1}{1/2} + 3^{(h-1)}$$

$$=n \times (2\times(3/2)^{(h-1)} - 2) + 3^{(h-1)}$$

$$=2n\times(3/2)^{h} \times (3/2)^{-1} - 2n + 3^{h} \times 3^{-1}$$

$$=2n\times(3/2)^{h} \times (2/3) - 2n + \frac{3^{h}}{3}$$

$$=\frac{4n\times(3/2)^{h}}{3} - 2n + \frac{3^{h}}{3}$$

$$=\frac{4n \times \frac{3^h}{2^h} + 3^h}{3} - 2n$$

:::
???

Essa ai foi moleza, agora é que o bicho vai pegar de vez! Vamos para o último passo para descobrir se esse bendito algoritmo é bom mesmo.

??? Complexidade 4

Utilize a altura obtida em ***Complexidade 1***, substitua-a na equação simplificada do último exercício para encontrar a complexidade.

!!! Dica de Ouro
Use a **mudança de base** duas vezes para inverter a base com o logaritmando!
!!!
::: Gabarito

$$< \frac{4n \times \frac{3^{\log_2{n} + 2}}{2^{\log_2{n} + 2}} + 3^{\log_2{n} + 2}}{3} - 2n$$

$$< \frac{4n \times \frac{3^{\log_2{n}}\times3^2}{n\times2^2} + 3^{\log_2{n}} \times 3^2}{3} - 2n$$

$$< \frac{4n \times \frac{3^{\log_2{n}}\times9}{4n} + 3^{\log_2{n}} \times 9}{3} - 2n$$

$$< \frac{3^{\log_2{n}}\times9 + 3^{\log_2{n}} \times 9}{3} - 2n$$

$$< 6\times3^{\log_2{n}} - 2n$$


Com a mudança de base logaritimica:

$$< 6\times3^{\frac{\log_3{n}}{\log_3{2}}} - 2n$$

$$< 6\times{n}^{\frac{1}{\log_3{2}}} - 2n$$

$$< 6\times{n}^{\frac{1}{\frac{\log_2{2}}{\log_2{3}}}} - 2n$$

$$< 6\times{n}^{\log_2{3}} - 2n$$

Com isso chegamos numa complexidade: 

$$O(n^{\log_2{3}}) \rightarrow O(n^{1.584})$$

???


Olha só, chegamos em $O(n^{1.584})$, e ai, agora você acredita que karatsuba consome menos tempo que uma multiplicação comum? Se ainda tem suas dúvidas, olhe este gráfico o comparando com a multiplicação tradicional.

![](graph.png)

??? Desafio Hardcore

Faça a seguinte multiplicação utilizando o algoritmo: $3408 \times 1735 = \,?$

::: Gabarito
1. Separando os números:

    * $a = 34$
    * $b = 08$
    * $c = 17$
    * $d = 35$

2. Achando $P \; e \; S$:

    * $P: \; 34 \times 17 = \; 578$
    * $S: \; 08 \times 35 = \; 280$

3. Calculando $T \,$: $\; (34 + 08) \times (17 + 35) = \; 2184$

4. Calculando $Q \,$: $\; T - S - P = \; 1326$

5. Resultado final:
$$R: \; 578 \times 10^4 + Q \times 10^{4/2} + S = \; 5.912.880$$

:::

???
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