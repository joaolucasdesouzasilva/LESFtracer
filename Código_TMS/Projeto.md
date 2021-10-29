# Traçador de Curvas I-V
## Desenvolvimento e Códigos - Laboratório de Energia e Sistemas Fotovoltaicos da UNICAMP (LESF)
### Pesquisador Principal: Rafael Espino Campos
### Pesquisador Campus Sustentável: Elson Yoiti Sakô
### Pesquisador Campus Sustentável: João Lucas de Souza Silva
### Pesquisadora Campus Sustentável: Karen Barbosa de Melo
### Orientador: Marcelo Gradella Villalva

Esse repositório faz parte do projeto "Desenvolvimento e construção de um protótipo de traçador eletrônico de curva I-V para a análise de módulos e strings fotovoltaicos", elaborado na dissertação de Rafael Espino Campos. A dissertação completa também está disponível na pasta do GitHub. O mesmo também apresenta uma licença para uso da tecnologia delineada no projeto Campus Sustentável. 

Este trabalho foi desenvolvido através do programa de Pesquisa e Desenvolvimento do Setor Elétrico PD-00063- 3032/2017 – PA3032: “Desenvolvimento de um modelo de Campus Sustentável na UNICAMP – Laboratório vivo de aplicações de mini geração renovável, eficiência energética, monitoramento e gestão do consumo de energia”, regulado pela Agência Nacional de Energia Elétrica – ANEEL, em parceria com a empresa CPFL Brasil.

A estrutura geral deste repositório é apresentada a seguir. 

~~~
├── Projeto.md          <- apresentação do projeto
│
├── Tese
│   └── Tese            <- dados originais sem modificações
│
│
├── Circuitos                <- Circuitos do traçador de curva
│
└── Códigos            <- Códigos do Projeto
~~~


# Projeto `<Desenvolvimento e construção de um protótipo de traçador eletrônico de curva I-V para a análise de módulos e strings fotovoltaicos>`
# Project `<Development and construction of an electronic I-V curve tracer prototype for the analysis of photovoltaic modules and strings>`

# Descrição Resumida do Projeto

O rápido crescimento do mercado de energia solar requer a implantação de equipamentos capazes de fornecer medições precisas dos módulos fotovoltaicos, seja para verificar o funcionamento da instalação ou encontrar problemas ocasionais. Neste cenário, destaca-se o traçador de curva I-V (curva de corrente versus tensão), utilizado para descrever o comportamento elétrico do sistema fotovoltaico através de todas as possibilidades de operação de corrente e tensão. No entanto, esse equipamento tem um custo elevado para pequenos instaladores. Neste trabalho, são apresentados o desenvolvimento e os resultados experimentais de um traçador de curva I-V capaz de fazer medições de corrente, tensão e potência de até 10 A, 600 V e 1 kW respectivamente, com boa precisão (1 % em toda a faixa de medição para cada um desses parâmetros elétricos), utilizado para examinar e caracterizar m´módulos e strings fotovoltaicos, contribuindo com um equipamento para testes de instalações fotovoltaicas de geração distribuída de energia elétrica. Para tanto, são apresentadas as características principais de uma fonte fotovoltaica, uma revisão bibliográfica dos m´todos mais comuns para obter as curvas I-V e uma comparação entre esses métodos. Posteriormente, foram definidas as características que devem ter o equipamento proposto, o qual está baseado em uma carga eletrônica. As simulações realizadas para o controle da carga variável são apresentadas, em conjunto, com os circuitos de aquisição e de potência projetados e construídos. Além disso, foi implementado o software do sistema embarcado, que inclui a interface homem-máquina e os procedimentos para tratar os dados adquiridos. Foram feitos testes de funcionamento, calibração e testes de campo sob diferentes condições, utilizando como referência um equipamento comercial. Os resultados obtidos pelo protótipo desenvolvido foram favoráveis, pois o erro de comparação no ponto da potência máxima para os dados obtidos depois do processamento (transferido para a condição de teste padrão) foi inferior a 0,9 %. Finalmente, também são propostas as melhorias necessárias para futuros desenvolvimentos.


# Abstract in English

The rapid growth of the solar energy market requires the insertion of equipment capable of providing accurate measurements of the photovoltaic modules, either to verify the operation of the installation or to find occasional problems. In this scenario, the I-V curve tracer (current versus voltage curve) is used to describe the electrical behavior of the photovoltaic system through all possibilities of current and voltage operation. However, this equipment has a high cost for small installers. This work presents the development and experimental results of an I-V curve tracer, capable of making current, voltage and power measurements up to 10 A, 600 V and 1 kW respectively, with good accuracy (1 % over the entire measurement range for each of these electrical parameters), used to examine and characterize photovoltaic modules and strings, contributing with equipment to test photovoltaic installations of distributed generation of electric energy. For this, the main characteristics of a photovoltaic source, a bibliographical review of the most common methods to obtain the I-V curves and a comparison between these methods are presented. Subsequently, the characteristics of the proposed equipment, which is based on an electronic load, were defined. The simulations performed for variable load control are presented, together, the acquisition and power circuits designed and built. In addition, embedded software has been implemented, which includes the human-machine interface and the procedures for processing the acquired data. Functioning, calibration and field tests were performed under different conditions, using commercial equipment as a reference. The results obtained by the developed prototype were favorable because the comparison error at the maximum power point for the data obtained after processing (moved to standard test condition) was less than 0.9 %. Finally, we also proposed the improvements needed for future developments.

