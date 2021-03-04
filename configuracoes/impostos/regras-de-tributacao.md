---
title: Regras de tributação
description: Veja como configurar regras de tributação para que o sistema calcule os impostos automaticamente
published: true
date: 2021-03-04T22:09:45.909Z
tags: impostos, configurações
editor: markdown
dateCreated: 2021-03-04T22:09:45.909Z
---

# Regras de tributação{#regras}

Configuração de impostos é geralmente uma tarefa confusa, difícil de entender e muito trabalhosa. Essa parte do GDOOR WEB foi desenvolvida tendo como foco principal a praticidade, para que o usuário possa fazer isso sem muita dor de cabeça. Ainda é importante que isso seja feito com a ajuda de um responsável pela contabilidade da empresa, pois documentos com a declaração de impostos incorreta podem acarretar em pagamento de imposto indevido, multa, ou pode ser necessário depois de um tempo, pagar impostos atrasados.

O GDOOR WEB já vem com uma configuração genérica pré-definida, que pode ser alterada e/ou incrementada. Os impostos abrangidos pelo serviço, são os incidentes sobre a circulação de mercadorias: [ICMS](/glossario#icms), [ICMS ST](/glossario#icms-st), [FCP](/glossario#fcp), [IPI](/glossario#ipi), [PIS](/glossario#pis) e [COFINS](/glossario#cofins).

Acesse o módulo de [Impostos](/configuracoes/impostos) sob a seção **Configurações** no menu principal do sistema:

[![Acessar configurações de impostos](/config/impostos/acessar-config-imposto.png)](/config/impostos/acessar-config-imposto.png){.fancybox}

Primariamente, o sistema vem com algumas configurações pré-definidas que podem ser alteradas. Dentre elas, o sistema tem uma para cada operação onde a incidência do imposto pode variar: **venda de produtos importados**, **venda para consumidor final** e **venda para revenda**, além de uma **geral**, que será aplicada quando não houver configuração específica. O vínculo entre a configuração de imposto e o produto pode ser a [natureza da operação](/cadastros/operacoes) ou a [NCM](/glossario#ncm), você informa a faixa de NCM que a regra abrange, e os produtos cuja NCM estiverem dentro dessa faixa, serão vinculados a esta regra. O gráfico abaixo representa como o vínculo é feito: há 3 regras disponíveis, cada uma abrangendo uma faixa de NCM e o produto possui uma NCM que está dentro da faixa abrangida por uma regra.

![Como é feito o vínculo entre produto e tributação do GDOOR WEB](/config/impostos/vinculo-produto-imposto.png =800x)

Deste modo, 4 das configurações iniciais abrangem todos os produtos que forem cadastrados com NCM, pois elas abrangem a faixa de NCM de **0000.00.01** a **9999.99.99**.

De modo semelhante ao vínculo por NCM, o sistema também faz um vínculo pelo [CEST](/glossario#cest). Esse vínculo é uma exceção para especificar a configuração de [Substituição Tributária](/glossario#icms-st).

## Vínculo pela natureza da operação

É possível vincular uma regra de tributação diretamente na [natureza da operação](/cadastros/operacoes). Quando uma natureza da operação vinculada com uma regra for utilizada em uma NF-e ou NFC-e, esta regra terá **prioridade** sobre qualquer outra quando o sistema buscar a regra automaticamente. No entanto, você ainda pode alterar manualmente a tributação de cada item.

## Criando uma regra

Para criar uma regra personalizada de tributação, clique no botão de adição no canto inferior direito da tela e será direcionado para o formulário de adição de regra. O GDOOR WEB possui um assistente para criação de regras para facilitar esse processo, abaixo iremos detalhar cada um dos passos desse assistente, que são:

- [Identificação](#identificacao)
- [Vínculos](#vinculos)
- [Tipo de imposto](#tipo-de-imposto)
- [Impostos](#impostos)
- [Revisão](#revisao)

![Assistente de configuração de regra](/config/impostos/formulario.png)

### Identificação

No primeiro passo você precisa definir um **nome** para identificar a regra e o tipo de **operação** que ela vai abranger. No campo **descrição** você pode detalhar o objetivo da regra e em que tipo de situação ela vai se encaixar; é um campo opcional e apenas informativo. As opções de operação são aplicáveis nas seguintes situações, em sua respectiva ordem de prioridade:

- **Produtos importados**: Quando o produto tem origem estrangeira. Isto é indicado pelo campo **Origem** no [cadastro do produto](/cadastros/produtos).
- **Consumidor final**: Quando a NF-e está marcada como operação para consumidor final.
- **Revenda**: Quando a NF-e **não** está marcada como operação para consumidor final.
- **Geral**: Em qualquer operação, <u>desde que não tenha encontrado uma regra específica antes</u>.

> Caso o produto seja uma exceção dentro de uma faixa de NCM, é possível vinculá-lo diretamente a uma regra. Este vínculo direto é feito no cadastro do produto e tem prioridade sobre as outras operações.
{.is-info .gw .gw-note}

### Vínculos

![Vínculos possíveis para a regra](/config/impostos/regra-vinculos.gif)

Neste passo você pode definir se esta regra será vinculada aos produtos por **NCM**, por **CEST** ou se vai deixar **sem vínculo**. Em qualquer uma das 3 opções, a regra ainda pode ser vinculada diretamente ao produto. Caso você escolha vínculo por NCM ou CEST, deve informar quais NCM/CEST vão direcionar para esta regra. Por exemplo, caso você escolha o vínculo por NCM, clique no botão <span class=mat-button>Vincular NCM</span> e, no diálogo que aparecer, informe o código específico ou a faixa de NCM.

![Adicionar vínculo por NCM](/config/impostos/modal-vinculo-ncm.png =400x)

#### Exceções{#excecao}

Quando houver uma exceção dentro de uma faixa de código, você pode adicionar essa exceção clicando no botão <span class=mat-button>Exceção</span>. Assim, o vínculo já existente será recriado contornando essa exceção. Por exemplo: Suponhamos que haja uma regra que compreenda 2 grupos de NCM consecutivos: 24 e 25. Então, a faixa de NCM seria **2400.00.00** a **2599.99.99**. Aí você adiciona uma exceção, que é o **2501.00.00**. O novo vinculo ficará com duas faixas: **2400.00.00** a **2500.99.99**, e também **2501.00.01** a **2599.99.99**.

![Adicionar exceção](/config/impostos/excecao.png)

Ao adicionar um código ou faixa para o vínculo – seja NCM ou CEST – o sistema verificará se não há conflito com outra regra para a mesma operação. Por exemplo: supondo que a **Regra 1**, para operações com **produtos importados** abrangesse a faixa de NCM **0101.00.00** até **0201.99.99**. E a **Regra 2**, também para **produtos importados** abrangesse de **0200.00.00** até **0299.99.99**. Se um determinado produto tivesse a NCM **0200.01.01**, o sistema não saberia qual regra aplicar porque há duas conflitando. Por este motivo, o sistema não pode aceitar que duas regras para a mesma operação dentro da mesma faixa de NCM.

Assim, se duas faixas em regras diferentes para uma mesma operação se interceptarem, será mostrada uma confirmação para adicionar exceção na outra regra, para que não haja conflito. No caso do exemplo citado acima, a mensagem seria assim:

![Confirmar adição de exceção](/config/impostos/conflito.png)

Em caso de confirmação neste exemplo, o que aconteceria seria o seguinte: A **Regra 2** agora abrange a faixa de NCM **0200.00.00** a **0299.99.99**. A **Regra 1** que abrangia a faixa de NCM _~~0101.00.00~~_ a _~~0201.99.99~~_, agora abrange uma faixa menor, **0101.00.00** a **0199.99.99**. Abaixo, veja a mudança em gráfico:

![Exceção na faixa de NCM](/config/impostos/excecao.gif)

#### Vínculo por CEST

Configurar uma regra e vincular pelo CEST é considerado uma exceção. Isso porque a tributação é definida pela NCM. O CEST é um código que especifica o produto quanto à **substituição tributária**, portanto, uma NCM pode ter vários CEST vinculados a ela. Assim sendo, quando você escolher o vínculo por CEST, apenas a configuração de ICMS ST ([veja mais abaixo](#icms-st)) será habilitada. O padrão de vínculo é o mesmo utilizado para a NCM.

### Tipo de imposto

![Tipo de impostos a serem configurados nesta regra](/config/impostos/tipo-de-imposto.png)

Este passo estará disponível caso você escolha o vínculo por NCM ou Sem vínculo. Nele você deve escolher quais impostos vai configurar para que o faça no próximo passo.

### Impostos

![Passo onde você configura os impostos](/config/impostos/passo-impostos.png)

Neste passo serão mostradas uma aba para cada imposto que você escolheu no passo anterior, ou apenas a de ICMS ST, caso tenha escolhido vínculo por CEST. Abaixo, veremos os detalhes de cada configuração.

#### ICMS

Primeiro, você informa o [CST](/glossario#cst)/[CSOSN](/glossario#csosn) padrão para esta regra e com base nesse código serão apresentados os campos que podem ser preenchidos. A imagem a seguir mostra duas opções diferentes de configuração com seus respectivos campos a serem configurados:

![Campos para configuração do ICMS](/config/impostos/config-icms-campos.png)

A tabela de alíquotas possui uma linha para cada UF porque o cenário da tributação pode mudar de acordo com as UF envolvidas, inclusive de acordo com a direção da operação. Não é necessário preencher *todas* as linhas – preencha apenas para as linhas cuja UF sua empresa comercializa. Ainda, se você vai preencher para várias UF e os valores para cada uma forem iguais, ou pelo menos para grande parte iguais, não é necessário preencher linha a linha, você pode preencher uma linha e replicar o valor para todas as outras, ou para as que ainda não foram preenchidas.

![Replicar informação](/config/impostos/icms-replicate.gif)

> A coluna **CFOP** não é de informação obrigatória. Na verdade, o CFOP não é definido pela regra, mas varia de acordo com a operação que estiver sendo realizada. No entanto, a configuração dele na regra facilita o preenchimento automático para que você não precise especificar o CFOP a cada item adicionado a uma NF-e, por exemplo.
{.is-info .gw .gw-note}

#### ICMS ST

Esta aba será mostrada caso o [CST](/glossario#cst)/[CSOSN](/glossario#csosn) informado na aba **ICMS** indicar que há cobrança de ICMS por **Substituição Tributária**, ou o vínculo escolhido for o **CEST**.

Assim como na configuração de ICMS, a configuração de ICMS ST tem uma tabela com uma linha para cada UF. Por padrão, o sistema já preenche as alíquotas internas de cada UF conforme disposto pela [CONFAZ](/glossario#confaz):

![Tabela de alíquotas do ICMS](/config/impostos/tabela-icms.png)

E esta tabela pode ser alterada do mesmo modo como explicado sobre a tabela de configuração do ICMS. As colunas **% BC ICMS** e **% Alíq. ICMS** serão exibidas no caso do vínculo por CEST, para que você possa definir o ICMS próprio do produto, que é necessário para o cálculo da Substituição Tributária. Caso o vínculo não seja o CEST, o ICMS próprio deve ser configurado na aba [ICMS](#icms).

![Campos para configuração do ICMS ST](/config/impostos/config-icms-st-campos.png)

> O sistema traz alguns valores pré-preenchidos para facilitar a configuração. No entanto, é de extrema importância que você consulte a **contabilidade** da empresa para auxiliar nesse processo.
{.is-warning .gw .gw-important}

#### IPI

![Campos para configuração do IPI](/config/impostos/config-ipi.png)

Nesta tela você configura um [CST](/glossario#cst) para as operações de entrada e outro para as operações de saída. Se o código indicar isenção de IPI, será necessário informar o código do enquadramento legal do IPI, que classifica a incidência do imposto. Para obter esse código, você pode buscar uma tabela na internet ou consultar a contabilidade.

Diferentemente do ICMS, que é um imposto **estadual**, o IPI é **federal**, portanto, é o mesmo em todo o país, não havendo necessidade de configurar por UF. Assim, é só preencher a alíquota que pode ser um percentual e que considera o valor da mercadoria, ou no caso de algumas mercadorias, usa-se a **alíquota específica**, que é um valor fixo por unidade do produto.

#### PIS/COFINS

![Campos para configuração de PIS e COFINS](/config/impostos/config-pis-cofins.png)

Nesta tela você configura um [CST](/glossario#cst) para as operações de entrada e outro para as operações de saída. A base de cálculo é a mesma para as duas contribuições, mas a alíquota é específica de cada uma.

### Revisão

O último passo contém todos os dados como você configurou nos passos anteriores. Isso pode ajudá-lo a ter uma visão geral da configuração para checar se as informações estão corretas sem ter que rever cada passo. No título de cada seção há um botão **Ajustar** que direciona você para a seção em questão para que possa ajustar alguma informação.

![Revisão da configuração da regra](/config/impostos/revisao.png)