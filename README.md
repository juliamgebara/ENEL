# Objetivo do estudo

Desde importantes quedas no serviço de energia ocorridas nos últimos anos na cidade de São Paulo e sua região metropolitana — com destaque para os episódios de 2022 a 2025 —, é comum ler nas redes sociais e mesmo ouvir pelas ruas da metrópole quão ineficiente é o serviço da concessionária Enel Distribuição São Paulo. Abundam as acusações de incapacidade de gestão das necessidades da capital.

Diante dessa situação, contudo, é gritante a falta de análises mais profundas das ocorrências e de comparação à concessionária anterior, Eletropaulo. É certo que a gravidade na demora no restabelecimento de energia nas principais ocorrências é evidente para qualquer um que resida na região — não à toa, a empresa já foi notificada pela Agência Nacional de Energia Elétrica (ANEEL) —, mas carecem evidências concretas e quantificadas às acusações e comparações feitas quase cotidianamente no debate público.

Este estudo pretende atacar essa demanda, visando a responder as seguintes questões:

1.  A Enel é, factualmente, pior em número de interrupções e demora de restabelecimento do serviço do que a Eletropaulo?
2.  As condições climáticas de São Paulo se deterioraram no período sob gestão da Enel, o que justificaria a possível piora dos indicadores?
3.  O desempenho da Enel se distribuiu de forma uniforme ao longo da concessão, ou há evidências de deterioração progressiva em regiões específicas da cidade?

# Metodologia

## Fontes de dados

Para fundamentar a análise, utilizamos duas bases de dados principais:

- **Indicadores de continuidade coletivos da ANEEL** (2010–2025): indicadores mensais por conjunto de unidades consumidoras, abrangendo o período sob gestão da Eletropaulo e da ENEL. A data de corte adotada é **1º de junho de 2018**, dado que a ENEL assumiu formalmente a concessão em maio daquele ano. Os indicadores centrais analisados são o **DECXN** (Duração Equivalente de Interrupção por Unidade Consumidora de origem externa e não programada) e o **FECXN** (Frequência Equivalente de Interrupção de origem externa e não programada), que isolam as ocorrências imprevistas causadas por fatores externos à distribuidora — eventos que servem como proxy da resiliência operacional da empresa.

- **Dados meteorológicos do INMET** (2016–2025): séries horárias da estação do Mirante de Santana (zona norte da capital), consolidadas em médias mensais, cobrindo precipitação, temperatura, umidade, velocidade de rajada máxima e demais variáveis climáticas.

## Tratamento e preparo dos dados

O arquivo `preprocessamento.py` realiza o tratamento das bases, incluindo:

- Leitura e concatenação das duas décadas de dados da ANEEL, com padronização de encoding e separadores;
- Mapeamento dos mais de 150 conjuntos de unidades consumidoras para macrorregiões da cidade (zonas norte, sul, leste, oeste, centro e municípios da região metropolitana);
- Criação da variável `Distribuidora` (Eletropaulo ou ENEL) com base no corte temporal;
- Incorporação do número de consumidores por conjunto para cálculo de DEC e FEC ponderados;
- Consolidação dos dados horários do INMET em médias mensais, com tratamento de sentinelas (-9999) e normalização dos nomes das colunas.


## Análise estatística

A análise exploratória e os testes estatísticos são conduzidos no notebook `Indicadores.ipynb`, que emprega:

- Visualizações de série temporal, boxplots e KDE plots para comparação entre períodos;
- **Teste U de Mann-Whitney**: avaliação não paramétrica de diferença entre as distribuições de DEC e FEC nas duas gestões;
- **d de Cohen**: medida do tamanho do efeito entre as médias;
- **Teste de Levene**: verificação de homocedasticidade, utilizado para identificar diferenças de instabilidade (variância) entre as distribuidoras;
- **Correlação de Spearman** e **regressão linear múltipla padronizada**: avaliação da relação entre variáveis climáticas e os indicadores de energia, incluindo lags de 1 mês e variável composta de calor úmido;
- **Análise fatorial por região e por ano** (facet de barras anuais com média móvel de 2 anos): mapeamento da heterogeneidade geográfica da piora ou melhora dos indicadores.