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

Algoritmo C.2: Função de Configuração Inicial (código em C)

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

Algoritmo C.3: Função de aquisição (código em C)

void Adquirir(void)
{
Show_Status_Aq( 0 ) ; // indicador de aquisição
if(Flag_Button) // se o botão teste for pressionado
{
Flag_Button = 0;
Copy_DataTest( ); // se dados de testes são usados
}
else
{
REGDrive (ON) ; // c i r c u i t o de po t enc i a e s ene r g i z ado
HAL_Delay (500);
Get_IV_data ( ); // obtemos as medições de tensão e corrente
HAL_Delay (200);
REGDrive (OFF); // o circuito de potência é desenergizado
}
Process_Data( );
SaveData( );
Show_Status_Aq ( 1 ) ; // indicador que terminou a aquisição
}


Algoritmo C.4: Função para obter as medições de corrente e tensão (código em C)
void Get_IV_data(void)
{
static uint16_t k;
static float powIGBT;
for (k=0; k<VDACmax; k++) // obtém 4095 medições
{
DACspi_tx (k);
Read_ADC_IV ( );
Datos_I [ k ] = Data1 ;
Datos_V [ k ] = Data2 ;
powIGBT = ( uint16_t ) ( ( (float)Data1*(float)Data2)/10000);
// se os valores estiverem dentro dos limites (1kW o 10 ,1A o 680,4V)
if ((powIGBT > 57654) j j (Data1 > 63000) j j (Data2 > 63000))
{
DACspi_tx(0) ; // zera a tensão do DAC
Flag Alarm = 1 ;
break;
}
}
DACspi_tx (0); // zera a tensão do DAC
}
~~~


