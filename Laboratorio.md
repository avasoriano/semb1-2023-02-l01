# Atividades de Laboratório - Sistemas Embarcados I

# Ava Soriano Rezende - 11921EBI011

## 1. O arquivo **stm32f411-blackpill/src/startup.c** implementa parcialmente a tabela de vetores de interrupção do microcontrolador STM32F411. Utilizando a tabela disponibilizada no manual de referência (***RM0383 Reference manual***) implemente **toda** a tabela de vetores de interrupção do microcontrolador STM32F411.
R:
#include <stdint.h>

// Definição do endereço inicial da pilha
#define SRAM_START  0x20000000U                  /* Inicio da SRAM CORTEX-M */
#define SRAM_SIZE   (128U * 1024U)               /* Tamanho SRAM STM32F411 128K */
#define SRAM_END    ((SRAM_START) + (SRAM_SIZE)) /* Final da SRAM STM32F411 */
#define STACK_START SRAM_END                     /* Inicio da stack */

// Definição do tipo de função para os vetores de interrupção
typedef void (*pFunction)(void);

// Protótipos das funções de exceção
void reset_handler(void);
void nmi_handler(void) _attribute_ ((weak, alias("default_handler")));
void hardfault_handler(void) _attribute_ ((weak, alias("default_handler")));
void memmanage_handler(void) _attribute_ ((weak, alias("default_handler")));
void busfault_handler(void) _attribute_ ((weak, alias("default_handler")));
void usagefault_handler(void) _attribute_ ((weak, alias("default_handler")));
void svc_handler(void) _attribute_ ((weak, alias("default_handler")));
void debugmon_handler(void) _attribute_ ((weak, alias("default_handler")));
void pendsv_handler(void) _attribute_ ((weak, alias("default_handler")));
void systick_handler(void) _attribute_ ((weak, alias("default_handler")));

void wwdg_handler(void) _attribute_ ((weak, alias("default_handler")));
void pvd_handler(void) _attribute_ ((weak, alias("default_handler")));
void tamp_stamp_handler(void) _attribute_ ((weak, alias("default_handler")));
void rtc_wkup_handler(void) _attribute_ ((weak, alias("default_handler")));
void flash_handler(void) _attribute_ ((weak, alias("default_handler")));
void rcc_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti0_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti1_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti2_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti3_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti4_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream0_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream1_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream2_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream3_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream4_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream5_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream6_handler(void) _attribute_ ((weak, alias("default_handler")));
void adc_handler(void) _attribute_ ((weak, alias("default_handler")));
void can1_tx_handler(void) _attribute_ ((weak, alias("default_handler")));
void can1_rx0_handler(void) _attribute_ ((weak, alias("default_handler")));
void can1_rx1_handler(void) _attribute_ ((weak, alias("default_handler")));
void can1_sce_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti9_5_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim1_brk_tim9_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim1_up_tim10_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim1_trg_com_tim11_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim1_cc_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim2_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim3_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim4_handler(void) _attribute_ ((weak, alias("default_handler")));
void i2c1_ev_handler(void) _attribute_ ((weak, alias("default_handler")));
void i2c1_er_handler(void) _attribute_ ((weak, alias("default_handler")));
void i2c2_ev_handler(void) _attribute_ ((weak, alias("default_handler")));
void i2c2_er_handler(void) _attribute_ ((weak, alias("default_handler")));
void spi1_handler(void) _attribute_ ((weak, alias("default_handler")));
void spi2_handler(void) _attribute_ ((weak, alias("default_handler")));
void usart1_handler(void) _attribute_ ((weak, alias("default_handler")));
void usart2_handler(void) _attribute_ ((weak, alias("default_handler")));
void usart3_handler(void) _attribute_ ((weak, alias("default_handler")));
void exti15_10_handler(void) _attribute_ ((weak, alias("default_handler")));
void rtc_alarm_handler(void) _attribute_ ((weak, alias("default_handler")));
void otg_fs_wkup_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim8_brk_tim12_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim8_up_tim13_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim8_trg_com_tim14_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim8_cc_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma1_stream7_handler(void) _attribute_ ((weak, alias("default_handler")));
void fsmc_handler(void) _attribute_ ((weak, alias("default_handler")));
void sdio_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim5_handler(void) _attribute_ ((weak, alias("default_handler")));
void spi3_handler(void) _attribute_ ((weak, alias("default_handler")));
void uart4_handler(void) _attribute_ ((weak, alias("default_handler")));
void uart5_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim6_dac_handler(void) _attribute_ ((weak, alias("default_handler")));
void tim7_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream0_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream1_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream2_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream3_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream4_handler(void) _attribute_ ((weak, alias("default_handler")));
void eth_handler(void) _attribute_ ((weak, alias("default_handler")));
void eth_wkup_handler(void) _attribute_ ((weak, alias("default_handler")));
void can2_tx_handler(void) _attribute_ ((weak, alias("default_handler")));
void can2_rx0_handler(void) _attribute_ ((weak, alias("default_handler")));
void can2_rx1_handler(void) _attribute_ ((weak, alias("default_handler")));
void can2_sce_handler(void) _attribute_ ((weak, alias("default_handler")));
void otg_fs_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream5_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream6_handler(void) _attribute_ ((weak, alias("default_handler")));
void dma2_stream7_handler(void) _attribute_ ((weak, alias("default_handler")));
void usart6_handler(void) _attribute_ ((weak, alias("default_handler")));
void i2c3_ev_handler(void) _attribute_ ((weak, alias("default_handler")));
void i2c3_er_handler(void) _attribute_ ((weak, alias("default_handler")));
void otg_hs_ep1_out_handler(void) _attribute_ ((weak, alias("default_handler")));
void otg_hs_ep1_in_handler(void) _attribute_ ((weak, alias("default_handler")));
void otg_hs_wkup_handler(void) _attribute_ ((weak, alias("default_handler")));
void otg_hs_handler(void) _attribute_ ((weak, alias("default_handler")));
void dcmi_handler(void) _attribute_ ((weak, alias("default_handler")));
void cryp_handler(void) _attribute_ ((weak, alias("default_handler")));
void hash_rng_handler(void) _attribute_ ((weak, alias("default_handler")));
void fpu_handler(void) _attribute_ ((weak, alias("default_handler")));

// Vetor de tabela de exceção
_attribute_((section(".isr_vector"))) pFunction __isr_vectors[] = {
    (pFunction)((uint32_t)STACK_START),      // Endereço de inicialização de pilha
    reset_handler,
    nmi_handler,
    hardfault_handler,
    memmanage_handler,
    busfault_handler,
    usagefault_handler,
    0,                                      // Reservado
    0,                                      // Reservado
    0,                                      // Reservado
    0,                                      // Reservado
    svc_handler,
    debugmon_handler,
    0,                                      // Reservado
    pendsv_handler,
    systick_handler,

// Vetores de interrupção específicos do STM32F411
    wwdg_handler,
    pvd_handler,
    tamp_stamp_handler,
    rtc_wkup_handler,
    flash_handler,
    rcc_handler,
    exti0_handler,
    exti1_handler,
    exti2_handler,
    exti3_handler,
    exti4_handler,
    dma1_stream0_handler,
    dma1_stream1_handler,
    dma1_stream2_handler,
    dma1_stream3_handler,
    dma1_stream4_handler,
    dma1_stream5_handler,
    dma1_stream6_handler,
    adc_handler,
    can1_tx_handler,
    can1_rx0_handler,
    can1_rx1_handler,
    can1_sce_handler,
    exti9_5_handler,
    tim1_brk_tim9_handler,
    tim1_up_tim10_handler,
    tim1_trg_com_tim11_handler,
    tim1_cc_handler,
    tim2_handler,
    tim3_handler,
    tim4_handler,
    i2c1_ev_handler,
    i2c1_er_handler,
    i2c2_ev_handler,
    i2c2_er_handler,
    spi1_handler,
    spi2_handler,
    usart1_handler,
    usart2_handler,
    usart3_handler,
    exti15_10_handler,
    rtc_alarm_handler,
    otg_fs_wkup_handler,
    tim8_brk_tim12_handler,
    tim8_up_tim13_handler,
    tim8_trg_com_tim14_handler,
    tim8_cc_handler,
    dma1_stream7_handler,
    fsmc_handler,
    sdio_handler,
    tim5_handler,
    spi3_handler,
    uart4_handler,
    uart5_handler,
    tim6_dac_handler,
    tim7_handler,
    dma2_stream0_handler,
    dma2_stream1_handler,
    dma2_stream2_handler,
    dma2_stream3_handler,
    dma2_stream4_handler,
    eth_handler,
    eth_wkup_handler,
    can2_tx_handler,
    can2_rx0_handler,
    can2_rx1_handler,
    can2_sce_handler,
    otg_fs_handler,
    dma2_stream5_handler,
    dma2_stream6_handler,
    dma2_stream7_handler,
    usart6_handler,
    i2c3_ev_handler,
    i2c3_er_handler,
    otg_hs_ep1_out_handler,
    otg_hs_ep1_in_handler,
    otg_hs_wkup_handler,
    otg_hs_handler,
    dcmi_handler,
    cryp_handler,
    hash_rng_handler,
    fpu_handler
};

// Função de Reset
void reset_handler(void) {
    // Lógica para tratamento de reset
}

// Manipulador de interrupção padrão
void default_handler(void) {
    while (1) {}
}

// Implemente outras funções de manipuladores de interrupção conforme necessário


## 2. Siga o roteiro disponibilizado no [laboratório 02](https://github.com/daniel-p-carvalho/ufu-semb1-lab-02.git) e implemente no arquivo **stm32f411-blackpill/Makefile** um script para automatizar o processo de compilação. Este script deverá ser capaz de gerar os arquivos objetos realocáveis e gerenciar automaticamente as dependências dos arquivos fonte do projeto.
R: 
Ferramentas do toolchain:
CC = arm-none-eabi-gcc
RM = rm -rf

Diretorios arquivos objeto e de lista de dependências serão salvos:
OBJDIR = build
DEPDIR = .deps

Arquivos a serem compilados:
SRCS = src/startup.c \
       src/main.c

Flags do compilador:
CFLAGS = -g -mcpu=cortex-m4 -mthumb -Wall -O0 -I./inc
DEPFLAGS = -MMD -MP -MF $(DEPDIR)/$*.d

Gera lista de arquivos objeto e cria diretório onde serão salvos:
OBJS = $(patsubst %, $(OBJDIR)/%.o, $(basename $(SRCS)))

Gera lista de arquivos de lista dependência e cria diretório onde serão salvos:
DEPS = $(patsubst %, $(DEPDIR)/%.d, $(basename $(SRCS)))

Diretivas especiais para que não dê erro caso o diretório já exista:
$(shell mkdir -p $(OBJDIR) > /dev/null)
$(shell mkdir -p $(DEPDIR) > /dev/null)

Target principal:
all: $(OBJDIR)/$(TARGET).elf

Compilação dos objetos:
$(OBJDIR)/%.o: %.c $(DEPDIR)/%.d
	$(CC) -c $(CFLAGS) $(DEPFLAGS) $< -o $@

Inclui arquivos de dependência:
-include $(DEPS)

Cria um novo target para cada arquivo de dependência possível:
$(DEPS):

Linkagem:
$(OBJDIR)/$(TARGET).elf: $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) $^ -o $@

.PHONY: clean
clean:
	$(RM) $(OBJDIR) $(DEPDIR)

OBS: Neste script: OBJDIR e DEPDIR: essas variáveis especificam os diretórios onde os arquivos objeto e as dependências serão salvos, respectivamente. / SRCS: essa lista contém os arquivos fonte que serão compilados. / CFLAGS: contém as flags de compilação. / DEPFLAGS: são as flags para gerar as dependências. / OBJS e DEPS: são as listas de arquivos objetos e dependências, respectivamente. A regra all é o alvo padrão, responsável por gerar o executável. A regra para compilar os objetos é similar à que você já tinha, mas atualizada para usar o diretório OBJDIR. DEPS é incluído para garantir que as dependências sejam geradas e incluídas corretamente. O alvo clean é utilizado para limpar os diretórios de objetos e dependências.
