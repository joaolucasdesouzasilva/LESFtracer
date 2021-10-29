# Traçador de Curvas I-V, código para TMS


~~~
Algoritmo C.1: Função principal (código em C)
#include "main.h"
uint8_t estado;
int main ( void)
{
Config Module ( ); // configuração
while ( 1 )
{
estado = Select_State( );  //status atual
Change_State(estado);  //atualização de status
Notifications( ); // atualização de notificações
HAL_Delay(100);
}
}

Algoritmo C.2: Função de Configuração Inicial (codigo em C)

void Config_Module(void)
{
CPU_CACHE_Enable( );
HAL Init ( );
SystemClock_Conf ig ( );
GPIO_Module ( );
BSP_QSPI_Init ( );
SPI_2_Init ( );
ADC_Init ( );
LCD_Config ( );
TIMER3_Init ( );
MountMemory ( );
~~~


