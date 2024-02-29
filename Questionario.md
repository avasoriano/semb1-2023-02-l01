# Questionário Sistemas Embarcados I

# Ava Soriano Rezende - 11921EBI011

## 1. Explique brevemente o que é compilação cruzada (***cross-compiling***) e para que ela serve.
R: O cross-compiling é o processo de criação de um código executável para uma diferente plataforma daquela em que o compilador está sendo executado, ou seja, assim ela serve para executar o código em outro sistena operacional ou em outra arquitetura. Por exemplo, compilar para Windows a partir do Linux ou para Arm64 a partir do x64.


## 2. O que é um código de inicialização ou ***startup*** e qual sua finalidade?
R: O código de inicialização é o ponto de partida crítico para o sistema operacional (antes do main). Suas principais funções incluem: configurar a memória, iniciar as variáveis globais (localizar e carregar o sistema operacional), configuração do processador e preparação para executar o código principal do programa. 


## 3. Sobre o utilitário **make** e o arquivo **Makefile responda**:

#### (a) Explique com suas palavras o que é e para que serve o **Makefile**.
R: É um arquivo texto que contém regras usado para automatizar a compilação e execução de programas escritos em C/C++, ou seja, serve para descrever como os arquivos-fonte devem ser compilados e vinculados para criar um código executável / programa. 

#### (b) Descreva brevemente o processo realizado pelo utilitário **make** para compilar um programa.
R: O utilitário make vai analisar todo o código descrito no Makefile e compila os arquivos-fontes necessários para criar um arquivo objeto executável. 

#### (c) Qual é a sintaxe utilizada para criar um novo **target**?
R: Target são as dependências, ou seja, refere-se ao resultado que se quer obter ao executar uma regra específica. Por exemplo, se você quisesse criar um alvo chamado “compilar” que depende do arquivo-fonte main.c, você poderia escrever o seguinte no seu Makefile:
compilar: main.c
    gcc -o meu_programa main.c

#### (d) Como são definidas as dependências de um **target**, para que elas são utilizadas?
R: As dependências de um target em um Makefile são especificadas para indicar quais arquivos ou outros alvos devem estar atualizados antes que o alvo em questão possa ser construído.

#### (e) O que são as regras do **Makefile**, qual a diferença entre regras implícitas e explícitas?
R: As regras são instruções que especificam a automatização para compilar e vincular os arquivos. As regras explícitas informam ao make quais arquivos dependem de outros arquivos e os comandos necessários para compilar um arquivo específico. Já as regras implícitas são semelhantes às explícitas, mas indicam os comandos a serem executados com base nas extensões dos arquivos, o make usa essas regras para determinar quais comandos executar automaticamente.


## 4. Sobre a arquitetura **ARM Cortex-M** responda:

### (a) Explique o conjunto de instruções ***Thumb*** e suas principais vantagens na arquitetura ARM. Como o conjunto de instruções ***Thumb*** opera em conjunto com o conjunto de instruções ARM?
R: O conjunto Thumb (16 bytes - tamanho reduzido) é um subconjunto do conjunto de instrções ARM, projetado para otimizar o tamanho do código e melhorar a densidade de instruções, ou seja, melhora o desempenho de sistemas embarcados. Durante a execução, as instruções Thumb de 16 bits são descomprimidas em instruções ARM completas de 32 bits em tempo real, sem perda de desempenho, isso permite que o mesmo código-fonte contenha instruções Thumb e ARM, otimizando o tamanho e o desempenho, permitindo que o processador execute ambos. 

### (b) Explique as diferenças entre as arquiteturas ***ARM Load/Store*** e ***Register/Register***.
R: Na arquitetura ARM Load/Store todas as operações lógicas e aritméticas são executadas entre registradores primeiro para que esses dados sejam executados na memória, ou seja, apenas instruções de carga (load - leitura) e armazenamento (store - escrita) podem acessar a memória. Na arquitetura Register/Register todas as operações são executadas diretamente entre registradores, ou seja, as intruções já operam nos próprios registradores da CPU sem a necessidade de acessar a memória, com isso, torna essa arquitetura mais eficiente, principalmente em termos de desempenho e em operações que envolvem muitos dados.

### (c) Os processadores **ARM Cortex-M** oferecem diversos recursos que podem ser explorados por sistemas baseados em **RTOS** (***Real Time Operating Systems***). Por exemplo, a separação da execução do código em níveis de acesso e diferentes modos de operação. Explique detalhadamente como funciona os níveis de acesso de execução de código e os modos de operação nos processadores **ARM Cortex-M**.
R: Os processadores ARM Cortex-M operam em vários níveis, os dois modos principais de acesso são: os privilegiados e os não privilegiados. No privilegiado (PAL) o código tem total acesso aos recursos do processador e aos registradores restritos, isso é usado principalmente pelo sistema operacional e pelo código de baixo nível que precisa de acesso direto ao hardware. No modo não privilegiado (NPAL) o código não tem acesso aos registradores de forma limitada garantindo a segurança do sistema.

O processador ARM Cortex-M3 possui dois modos de operação (determinam como o processador responde às interrupções e eventos do sistema) principais: o Modo de Operação Thread e o Modo de Operação Handler. No primeiro modo, permite a CPU executar o código normal do programa e rodar nos dois níveis de acesso (PAL e NPAL). Já no segundo modo, sempre vai rodar em PAL, é usado para tratamento de interrupções e exceções.

### (d) Explique como os processadores ARM tratam as exceções e as interrupções. Quais são os diferentes tipos de exceção e como elas são priorizadas? Descreva a estratégia de **group priority** e **sub-priority** presente nesse processo.
R: Exceções e interrupções são eventos inesperados que ocorrem durante a execução do programa e interrompem o fluxo normal de execução da CPU requer uma resposta imediata do processador. As exceções são eventos gerados pelo próprio processador, enquanto as interrupções são eventos causados por dispositivos externos. Isso pode incluir eventos como uma solicitação de entrada/sáida, erros de execução ou condições de erro.
Tipos de Exceções:
- Exceções de Acesso à Memória (Abort): geradas quando ocorre um acesso inválido à memória, como desalinhamento de palavra ou falta de página.
- Exceções de Instrução Indefinida: disparadas quando o processador tenta executar uma instrução inválida.
- Exceções de Software (SWI): instruções específicas que podem gerar uma inconformidade, como uma divisão por zero.
- Exceções de Instrução Inválida: ocorrem quando uma instrução não contém um código válido de instrução.

Group Priority (Prioridade do Grupo): divide as interrupções em grupos com diferentes níveis de prioridade. Sub Priority (Subprioridade): usada para resolver disputas dentro do mesmo grupo de prioridade, ou seja, dentro de cada grupo as interrupções tem seus subníveis de prioridades.

### (e) Qual a diferença entre os registradores **CPSR** (***Current Program Status Register***) e **SPSR** (***Saved Program Status Register***)?
R: o CPSR (ou PSR) é o registro que mantém o estado atual do processador, enquanto o SPSR (ou PSR) armazena temporariamente o estado do processador durante exceções e interrupções.

### (f) Qual a finalidade do **LR** (***Link Register***)?
R: O LR armazena o endereço de retorno para onde o fluxo de execução deve voltar após a conclusão de uma chamada de sub-rotina. Isso é quando uma função é chamada, o endereço de retorno (geralmente o endereço da próxima instrução após a chamada) é armazenado no LR, permitindo que a função retorne ao ponto de origem após sua execução.

### (g) Qual o propósito do Program Status Register (PSR) nos processadores ARM?
R: O Program Status Register (PSR), também conhecido como CPSR (Current Program Status Register) é utilizado para armazenar informações cruciais sobre o estado atual do processador. Assim, ele reflete o resultado de operações lógicas e aritméticas, controla interrupções e gerencia o modo de operação do processador.

### (h) O que é a tabela de vetores de interrupção?
R: O vetor de interrupções é uma tabela de endereços de memória que aponta para as rotinas de tratamento de interrupção. Quando uma interrupção é gerada, o processador salva seu estado atual e começa a executar o tratamento de interrupção apontado pelo vetor. Isso garante que nenhuma das tarefas executadas pelo sistema operacional entre em conflito.

### (i) Qual a finalidade do NVIC (**Nested Vectored Interrupt Controller**) nos microcontroladores ARM e como ele pode ser utilizado em aplicações de tempo real?
R: O Nested Vectored Interrupt Controller (NVIC) é um componente periférico essencial nos processadores ARM Cortex-M e desempenha um papel crucial no tratamento de interrupções (no gerenciamento delas). Assim, ele permite que o processador lide com várias interrupções de maneira eficiente e priorize eventos importantes. Em sistemas de tempo real, o NVIC é crucial para garantir que as interrupções sejam tratadas de forma previsível e rápida. Aplicações de tempo real como controle de dispositivos, sistemas embarcados e automação industrial, dependem do NVIC para responder a eventos externos sem atrasos excessivos. 

### (j) Em modo de execução normal, o Cortex-M pode fazer uma chamada de função usando a instrução **BL**, que muda o **PC** para o endereço de destino e salva o ponto de execução atual no registador **LR**. Ao final da função, é possível recuperar esse contexto usando uma instrução **BX LR**, por exemplo, que atualiza o **PC** para o ponto anterior. No entanto, quando acontece uma interrupção, o **LR** é preenchido com um valor completamente  diferente,  chamado  de  **EXC_RETURN**.  Explique  o  funcionamento  desse  mecanismo  e especifique como o **Cortex-M** consegue fazer o retorno da interrupção. 
R: o EXC_RETURN é um valor especial usado pelo Cortex-M para gerenciar o fluxo de exceções e garantir que o contexto seja restaurado adequadamente após o tratamento de interrupções. Quando a rotina de tratamento de exceção (como um IRQ handler) está prestes a retornar, o processador usa o valor do EXC_RETURN para restaurar o contexto correto. O processador verifica o EXC_RETURN para determinar se deve voltar ao modo Thread ou Handler, qual pilha usar (MSP ou PSP) e outras configurações específicas.

### (k) Qual  a  diferença  no  salvamento  de  contexto,  durante  a  chegada  de  uma  interrupção,  entre  os processadores Cortex-M3 e Cortex M4F (com ponto flutuante)? Descreva em termos de tempo e também de uso da pilha. Explique também o que é ***lazy stack*** e como ele é configurado. 
R: As diferenças no salvamento de contexto durante a chegada de uma interrupção entre os dois processadores são: no tempo de salvamento de contexto, no uso da pilha e no Lazy Stack. O Cortex-M4F é mais eficiente em termos de tempo de salvamento de contexto durante interrupções, isso ocorre porque o Cortex-M4F possui registradores adicionais para armazenar os estados de ponto flutuante (FPU), enquanto o Cortex-M3 não possui suporte nativo a ponto flutuante. Em relação ao uso da pilha, ambos os processadores utilizam dela para salvar o contexto durante uma interrupção, porém o Cortex-M4F tem a vantagem de salvar os registradores de ponto flutuante em uma área separada da pilha, reduzindo o impacto no uso geral da pilha, enquanto o Cortex-M3, todos os registradores (incluindo ponto flutuante) são salvos na mesma pilha. O lazy stack refere-se à estratégia de salvar apenas os registradores necessários durante uma interrupção. O lazy stack é configurado por meio de opções de configuração específicas no compilador


## Referências

### Básicas

[1] [STM32F411xC Datasheet](https://www.st.com/resource/en/datasheet/stm32f411ce.pdf)

[2] [RM0383 Reference Manual](https://www.st.com/resource/en/reference_manual/rm0383-stm32f411xce-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

[3] [Using the GNU Compiler Collection (GCC)](https://gcc.gnu.org/onlinedocs/gcc/index.html)

[4] [GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

### Vídeos Microsoft Teams

[5] [05 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[6] [06 - Arquitetura Cortex-M Parte 1/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[7] [07 - Arquitetura Cortex-M Parte 2/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[8] [08 - Microcontroladores STM32](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[9] [10 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

### Material Suplementar

[5] [A General Overview of What Happens Before main()](https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/)
 
[6] [Bare metal embedded lecture-1: Build process](https://youtu.be/qWqlkCLmZoE?si=mn5yDnJYudQ1PpZH)
 
[7] [Bare metal embedded lecture-2: Makefile and analyzing relocatable obj file](https://youtu.be/Bsq6P1B8JqI?si=yuNLPj3JQ-2IT1yo)
 
[8] [Bare metal embedded lecture-3: Writing MCU startup file from scratch](https://youtu.be/2Hm8eEHsgls?si=c27MpZ47ApiMSwHR)
 
[9] [Lecture 15: Booting Process](https://youtu.be/3brOzLJmeek?si=MsHRUEJP8zofjwJQ)
