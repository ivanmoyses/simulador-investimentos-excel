# Simulador de Investimentos em Excel

## Sobre o projeto

Este projeto apresenta uma ferramenta de **simulação de investimentos e planejamento financeiro desenvolvida em Excel**, construída como parte da minha formação em Excel, análise de dados e construção de soluções aplicadas a problemas de negócio.

A proposta é transformar informações como renda mensal, valor de aporte, prazo e taxas de rendimento em projeções que permitam visualizar a evolução de um patrimônio ao longo do tempo.

A ferramenta também permite selecionar diferentes perfis de investimento e simular a distribuição de um aporte mensal entre diferentes categorias de Fundos de Investimento Imobiliário (FIIs).

> **Importante:** todos os valores utilizados neste projeto são hipotéticos e possuem finalidade exclusivamente educacional. As simulações não constituem recomendação, indicação ou orientação de investimento.

---

## Objetivo da ferramenta

O simulador foi desenvolvido para responder às principais perguntas envolvidas em um planejamento de investimentos:

* Quanto investir por mês?
* Por quantos anos investir?
* Qual taxa de rendimento mensal considerar?
* Qual patrimônio poderá ser acumulado?
* Qual poderá ser o rendimento mensal estimado sobre esse patrimônio?

Além dessas informações, a ferramenta apresenta projeções para diferentes períodos e permite comparar o aporte definido pelo usuário com uma segunda simulação baseada em uma sugestão de aporte equivalente a 30% da renda mensal.

---

# Estrutura do projeto

A pasta de trabalho foi organizada em diferentes abas, separando a interface de utilização, os cálculos e as tabelas de apoio.

Essa estrutura foi pensada para facilitar a utilização por pessoas que não precisam conhecer as fórmulas utilizadas internamente.

## `Simulador de Investimentos`

É a principal interface da ferramenta.

Nessa aba, o usuário pode informar ou alterar:

* Salário mensal;
* Aporte mensal;
* Prazo do investimento;
* Taxa de rendimento mensal;
* Perfil de investimento.

A partir desses parâmetros, a ferramenta calcula:

* Patrimônio acumulado;
* Rendimento mensal estimado;
* Projeções de patrimônio;
* Distribuição do aporte entre categorias de FIIs;
* Uma segunda simulação utilizando a sugestão de aporte de 30% da renda.

### Cenários de evolução

A ferramenta apresenta projeções para:

* 2 anos;
* 5 anos;
* 10 anos;
* 20 anos;
* 30 anos.

Os cenários utilizam referências estruturadas para permitir que a mesma lógica de cálculo seja aplicada aos diferentes períodos.

---

# Função `VF`

Uma das principais funções utilizadas no projeto é a função `VF` — Valor Futuro.

Ela é utilizada para estimar o patrimônio acumulado considerando uma taxa de rendimento, quantidade de períodos e aportes periódicos.

A estrutura conceitual utilizada é:

```excel
=VF(taxa;períodos;pagamento)
```

Como a taxa considerada na simulação é mensal, o prazo informado em anos é convertido para meses.

Exemplo:

```excel
=VF(Taxa_Mensal;Qtd_Anos*12;Aporte*-1)
```

O sinal negativo aplicado ao aporte representa, na lógica financeira do Excel, uma saída de caixa durante o período de investimento. Dessa forma, o resultado final pode ser apresentado como patrimônio positivo.

---

# Projeções e acompanhamento da evolução

A ferramenta foi estruturada para acompanhar a evolução do patrimônio em diferentes períodos.

Nas tabelas de acompanhamento, o patrimônio projetado é utilizado como base para estimar o rendimento mensal correspondente à taxa definida para a carteira.

Essa separação permite diferenciar:

**Taxa utilizada na formação do patrimônio**

da

**Taxa utilizada para estimar o rendimento sobre o patrimônio acumulado.**

Dessa maneira, a ferramenta consegue trabalhar com diferentes premissas de crescimento e rendimento sem alterar a estrutura principal dos cálculos.

---

# Perfis de investimento

A ferramenta trabalha com três perfis utilizados exclusivamente para fins de simulação:

* Conservador;
* Moderado;
* Agressivo.

A seleção do perfil é realizada por meio de uma lista de validação de dados.

Cada perfil possui uma distribuição percentual entre seis categorias de FIIs:

* Papel;
* Tijolo;
* Híbridos;
* FOFs;
* Desenvolvimento;
* Hotelarias.

Quando o usuário altera o perfil, a distribuição do aporte é atualizada automaticamente.

Cada perfil possui distribuição equivalente a **100% do aporte**.

> Os percentuais foram definidos exclusivamente para fins didáticos e de demonstração da funcionalidade da ferramenta. Não representam recomendação de carteira ou orientação financeira.

---

# Chave composta e `PROCV`

Uma das principais estruturas utilizadas no projeto é a combinação de **chave composta + PROCV**.

Para relacionar o perfil selecionado ao tipo de FII, a ferramenta combina as duas informações em uma única chave.

Exemplo:

```text
Conservador-PAPEL
```

A lógica pode ser representada da seguinte maneira:

```text
Perfil selecionado
        ↓
Perfil + Tipo de FII
        ↓
Chave composta
        ↓
PROCV
        ↓
Percentual correspondente
        ↓
Percentual × aporte
        ↓
Valor destinado à categoria
```

O `PROCV` é utilizado com busca exata, evitando que o Excel faça uma correspondência aproximada.

Exemplo conceitual:

```excel
=PROCV(Perfil&"-"&Tipo_FII;Tabela_de_Apoio;4;FALSO)
```

Essa estrutura permite que uma única alteração no perfil atualize automaticamente toda a distribuição do aporte.

---

# Intervalos nomeados

Para facilitar a leitura das fórmulas e reduzir a dependência direta de endereços de células, foram utilizados intervalos nomeados.

Entre os principais nomes utilizados estão:

* `Aporte`
* `Taxa_Mensal`
* `Qtd_Anos`
* `Patrimonio`
* `Salario`
* `Taxa_Rendimento`
* `Sugestao_Investimento`
* `Objetivo_Financeiro`
* `Tempo_para_Carro`
* `Valor_para_Carro`
* `Valor_Acumulado_Carro`
* `Rendimento_Carro`

O uso de intervalos nomeados facilita a manutenção da ferramenta, melhora a interpretação das fórmulas e permite que alterações na estrutura sejam realizadas com menor risco de quebrar os cálculos.

---

# Segunda simulação — sugestão de 30% da renda

A ferramenta possui uma segunda simulação baseada em uma sugestão de aporte equivalente a **30% da renda mensal**.

Por exemplo, considerando uma renda hipotética de:

```text
R$ 8.000,00
```

a sugestão é calculada como:

```text
R$ 8.000 × 30% = R$ 2.400,00
```

Esse valor é utilizado como novo aporte para realizar uma segunda projeção.

A ferramenta permite, portanto, comparar dois cenários:

1. Aporte informado diretamente pelo usuário;
2. Aporte calculado a partir da sugestão de 30% da renda.

Essa funcionalidade foi criada para demonstrar como uma mesma estrutura pode trabalhar com diferentes premissas sem alterar as fórmulas principais da ferramenta.

---

# Acompanhamento da evolução

A estrutura de acompanhamento utiliza o patrimônio projetado como base para estimar o rendimento mensal.

Por exemplo, na simulação relacionada ao planejamento de aquisição de um veículo, o rendimento mensal é obtido a partir do patrimônio acumulado e da taxa de rendimento definida para o cenário.

A mesma lógica é aplicada nos cenários do simulador de investimentos.

Isso permite acompanhar não apenas o patrimônio projetado, mas também uma estimativa do rendimento correspondente à taxa considerada.

---

# Simulador de objetivo financeiro — compra de um carro

Além da ferramenta principal de investimentos, o projeto possui uma simulação voltada ao planejamento de um objetivo financeiro específico: **a aquisição de um veículo**.

A aba permite trabalhar com:

* Valor necessário para o objetivo;
* Aporte mensal;
* Prazo;
* Taxa de rendimento;
* Patrimônio acumulado;
* Rendimento estimado;
* Perfil de distribuição dos investimentos.

A proposta é demonstrar como as mesmas técnicas utilizadas no simulador de investimentos podem ser aplicadas a diferentes problemas de planejamento financeiro.

Nesse cenário, os valores e taxas também são hipotéticos.

---

# Abas de apoio

## `Apoio`

Contém a estrutura de relacionamento entre:

**Perfil → Tipo de FII → Percentual**

Essa tabela é utilizada como fonte para as buscas realizadas pelo `PROCV`.

## `Sugestão de Investimento`

Contém a estrutura utilizada para a segunda simulação baseada na sugestão de aporte equivalente a 30% da renda.

## `FIIs Para comprar Carro`

Contém os parâmetros de distribuição utilizados especificamente na simulação do objetivo de aquisição do veículo.

Essas tabelas ficam separadas da interface principal para manter a ferramenta organizada e facilitar futuras alterações nos parâmetros.

---

# Organização visual e experiência de utilização

A planilha foi reformulada para apresentar uma interface mais próxima de uma ferramenta de utilização prática, evitando que o usuário precise navegar diretamente pela estrutura das fórmulas.

Foram aplicados recursos como:

* Ocultação das linhas de grade;
* Organização visual das áreas de entrada e resultado;
* Diferenciação entre campos editáveis e campos calculados;
* Estruturação das tabelas de apoio em abas separadas;
* Ocultação de elementos relacionados às fórmulas na versão final;
* Organização das informações para facilitar a leitura e utilização.

O objetivo é permitir que o usuário utilize a ferramenta sem precisar conhecer sua estrutura interna.

Ao mesmo tempo, as fórmulas e tabelas de apoio permanecem estruturadas no arquivo para demonstrar como a solução foi construída.

---

# Fundos de Investimento Imobiliário — FIIs

Os Fundos de Investimento Imobiliário são utilizados neste projeto como exemplo de uma classe de ativos que pode distribuir rendimentos aos seus cotistas.

A ferramenta utiliza diferentes categorias de FIIs apenas para demonstrar como uma tabela de parâmetros pode controlar automaticamente uma distribuição de aportes.

Os percentuais utilizados são hipotéticos e não representam uma carteira recomendada.

O projeto não busca determinar qual fundo deve ser adquirido, mas demonstrar a aplicação de recursos do Excel na construção de uma ferramenta de simulação.

---

# Exemplo de simulação

Como exemplo de funcionamento, a ferramenta pode utilizar parâmetros hipotéticos como:

| Parâmetro                          |     Exemplo |
| ---------------------------------- | ----------: |
| Salário mensal                     | R$ 8.000,00 |
| Aporte mensal                      |   R$ 500,00 |
| Prazo                              |      5 anos |
| Taxa mensal utilizada na simulação |      1,078% |
| Taxa de rendimento da carteira     |       0,89% |

A partir desses parâmetros, o Excel calcula o patrimônio projetado por meio da função `VF` e utiliza a taxa de rendimento definida para estimar o rendimento mensal.

Os valores apresentados são exclusivamente demonstrativos.

---

# Principais recursos utilizados

O desenvolvimento do projeto permitiu aplicar conceitos de:

* Excel;
* Função `VF`;
* `PROCV`;
* Chaves compostas;
* Intervalos nomeados;
* Validação de dados;
* Tabelas de apoio;
* Juros compostos;
* Projeções financeiras;
* Simulação de cenários;
* Organização de dados;
* Construção de interface;
* Automatização de cálculos;
* Estruturação de ferramentas para problemas de negócio.

---

# O que aprendi com o projeto

O principal objetivo deste projeto não foi apenas construir uma calculadora financeira.

A proposta foi compreender como transformar uma necessidade de negócio em uma ferramenta estruturada.

Durante o desenvolvimento, a lógica foi organizada em:

```text
ENTRADAS
   ↓
REGRAS
   ↓
DADOS DE APOIO
   ↓
CÁLCULOS
   ↓
PROJEÇÕES
   ↓
RESULTADOS
```

Essa abordagem permite que o mesmo raciocínio seja aplicado posteriormente a outros problemas de planejamento e análise de dados.

---

# Possíveis evoluções

Entre as possibilidades de evolução da ferramenta estão:

* Inclusão de novos cenários;
* Novos objetivos financeiros;
* Comparação entre diferentes taxas;
* Novos gráficos;
* Expansão das tabelas de apoio;
* Inclusão de novos parâmetros;
* Comparação visual entre perfis;
* Novos modelos de planejamento financeiro;
* Evolução da interface e experiência do usuário.

---

# Como utilizar

1. Abra o arquivo `.xlsx` disponível neste repositório.
2. Acesse a aba **Simulador de Investimentos**.
3. Informe ou altere os parâmetros disponíveis.
4. Selecione o perfil desejado.
5. Observe os resultados calculados automaticamente.
6. Compare os cenários de prazo.
7. Utilize a segunda simulação para analisar o cenário baseado em 30% da renda.
8. Acesse a aba de compra do carro para utilizar a ferramenta em um objetivo financeiro específico.

Os campos destinados à entrada de dados podem ser alterados sem a necessidade de modificar as fórmulas estruturais.

---

# Aviso

Todos os valores financeiros apresentados neste projeto são **fictícios e utilizados exclusivamente para fins educacionais**.

As taxas utilizadas nas simulações não representam garantia de rentabilidade futura.

Os percentuais de distribuição entre categorias de FIIs também são hipotéticos e não representam recomendação de investimento.

Este projeto não substitui a análise de um profissional habilitado e não constitui recomendação de compra, venda ou composição de carteira.

---

# Autor

**Ivan Moyses Ramos**

Projeto desenvolvido como parte da minha jornada de formação em Excel, análise de dados e construção de soluções aplicadas a problemas de negócio.
