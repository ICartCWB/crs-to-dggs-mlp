# CRS-to-DGGS-MLP: Análise da Transformação de Coordenadas para DGGS com Redes Neurais

Este repositório contém o código e a análise completa da pesquisa que investiga o uso de Redes Neurais (Perceptrons de Múltiplas Camadas - MLPs) para a transformação de coordenadas geográficas (CRS) em índices de Sistemas de Grade Global Discreta (DGGS), com foco em aplicações para Cubos de Dados de Sensoriamento Remoto.

O notebook principal, contendo todo o desenvolvimento, experimentação e análise, pode ser visualizado em:
[artigo_CRS_MLP_DGGS.ipynb](https://github.com/ICartCWB/crs-to-dggs-mlp/blob/main/artigo_CRS_MLP_DGGS.ipynb)

## Resumo (Abstract)

O mapeamento eficiente de coordenadas de diversos Sistemas de Referência de Coordenadas (CRS) para índices hierárquicos de Sistemas de Grade Global Discreta (DGGS) é um desafio chave para Cubos de Dados de sensoriamento remoto. Este estudo investiga Perceptrons de Múltiplas Camadas (MLPs) para esta tarefa de geocodificação, comparando a performance nas grades H3 e rHEALPix. Como a predição perfeita do índice se mostrou inviável para uma MLP monolítica, nós introduzimos a métrica de Resolução Correta Média (MCR) para medir a profundidade hierárquica média que um modelo aprende corretamente. Usando um delineamento em blocos, nós avaliamos os efeitos do volume de treinamento e da faixa de latitude, bloqueando pelo CRS de origem (WGS84/Córrego Alegre). A análise revela que a estrutura regular da grade rHEALPix resulta em uma MCR significativamente maior do que a da grade mais complexa do H3, independentemente dos fatores experimentais. Este trabalho valida a abordagem com MLP, estabelece a MCR como uma ferramenta de avaliação crucial e destaca a superior "aprendibilidade" do rHEALPix para esta tarefa.

## Principais Descobertas (Key Findings)

1. Limites da Arquitetura Monolítica: Uma única MLP multi-head, embora excelente em prever a célula base DGGS (>94% de acurácia), demonstrou ser inadequada para replicar perfeitamente a estrutura hierárquica completa dos índices (0% de acerto perfeito).
2. "Aprendibilidade" Superior do rHEALPix: A análise estatística (ANOVA) e a métrica MCR revelaram que a estrutura regular do rHEALPix é significativamente mais "fácil" de ser aprendida pela MLP do que a geometria mais complexa do H3, que possui singularidades pentagonais.
3. Métrica MCR como Ferramenta de Avaliação: A Resolução Correta Média (MCR) provou ser uma métrica muito mais informativa do que a acurácia binária, quantificando a profundidade prática da precisão de cada modelo.
4. Impacto dos Fatores: O volume de dados de treinamento foi o fator mais significativo para melhorar a performance de todos os modelos. A faixa de latitude mostrou ter um efeito estatisticamente significativo na performance do rHEALPix, indicando uma maior dificuldade de aprendizado nas regiões polares.

## Visão Geral da Metodologia

O projeto foi executado em quatro fases principais:
1. Geração de Dados: Criação de um pipeline para gerar dados sintéticos em "fragmentos de imagem", utilizando Features de Fourier para codificar as coordenadas e amostragem por área de superfície igual para garantir uma distribuição geográfica justa.
2. Otimização de Hiperparâmetros: Um processo de duas fases com Optuna e Validação Cruzada Estratificada (K-Fold) para encontrar a arquitetura de modelo e os parâmetros de treinamento ideais.
3. Treinamento do Modelo: Treinamento de quatro modelos finais (H3/WGS84, H3/Córrego Alegre, rHEALPix/WGS84, rHEALPix/Córrego Alegre) com os melhores hiperparâmetros em datasets de 10k, 100k e 1M de pontos.
4. Análise Estatística: Uso de ANOVA e testes post-hoc de Tukey HSD para avaliar formalmente o impacto dos fatores experimentais (volume de treino, latitude) e do bloco (CRS) na MCR.
