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

Algoritmo C.5: Algoritmo para tratar os dados nos condições de operação (Código em Matlab)
clear; clc; close all
%% Obtencao de dados
RvarA = 'DadosAdq1.csv' ;
MA = csvread(RvarA, 1, 0);
Data_V = MA( : , 1 );
Data_I = MA( : , 2 );
N_dados = length (Data V );
Data_Va= zeros ( 1 , N dados );
Data_Ia = zeros ( 1 , N dados );
for i = 1:N dados
Data_Va ( i ) = Data V( i );
Data_Ia ( i ) = Data I( i );

end

%% Descartar pontos incongruentes de corrente e tensão
for x = 2 : N_dados
% diferença superior a 11 V entre dados consecutivos
if (abs(Data_Va (x) - Data_Va (x-1)) > 1000)
Data_Va (x) = Data_Va (x-1);
end
% diferença superior a 200mA entre dados consecutivos
if (abs(Data_Ia(x) - Data_Ia (x-1)) > 1000)
Data_Ia (x) = Data_Ia (x-1);
end
% a tensão tem que diminuir
i f Data Va (x) > Data Va (x􀀀1)
Data Va (x) = Data Va (x􀀀1);
end
% a corrente tem que incrementer
if Data_Ia (x) < Data_Ia (x-1)
Data_Ia (x) = Data_Ia (x-1);
end
end

%% Descartar os pontos quando a corrente é zero no começo das medições
m = 50 ; Va_prm = 0 ; h= m/2 +1; Izero = 1 ;
while h < round(N_dados/3)-m/2
temporal = 0;
for k = 0 : m %procura por grupos de 50 dados
temporal = temporal + Data Ia (h-m/2+k ) ;
end
if round(temporal/m) < 5 %se a corrente é inferior a 1 mA
Izero = h; % quantidade de dados descartados
end
h = h + m;
end
for h=1 : Izero
Va_prm = Va_prm + Data_Va(h);
end
% só obtem um valor de tensão dos dados descartados
Va_prm = round(Va_prm/Izero);
N_dados = N_dados- Izero+1; % nova quantidade de pontos uteis
DadosV = zeros (1,N dados);
DadosI = zeros (1,N dados);
DadosV(1) = Va prm;
DadosI(1) = 0 ;
for h=2:N dados
DadosV(h) = Data_Va (h + Izero-1);
DadosI (h) = Data_Ia (h + Izero-1);
end
%% Filtrar dados de tensão e corrente
pasoV = 47; % aproximadamente 0.5V de diferença
pasoI = 617; % aproximadamente 100mA de diferença
nd = 300;
n=0 ; j=1;
while j < N dados
Var V = 0 ; Var I = 0 ; kpmd = 0 ;
for k =1 : nd
if(abs(DadosV(j) - DadosV(j+k)) < pasoV) &&
(abs(DadosI (j) - DadosI (j+k) ) < pasoI)
Var_V = DadosV(j+k) + Var_V;
Var_I = DadosI(j+k) + Var_I;
kpmd = kpmd+1;
end
end
n = n + 1;
i f kpmd == 0
Filt_V (n) = DadosV( j ) ;
Filt_I (n) = DadosI( j ) ;
kpmd = 1 ;
el se % promedia os dados com muito pouca variação de corrente e tensão
FiltV(n) = round(VarV/kpmd);
FiltI(n) = round(VarI/kpmd);
end
j = j + kpmd;
if (N_dados - j ) < nd
nd = N_dados - j ;
end
end
%% Tirar extremos fora da faixa de tensão
for j=1 : n
if Filt_V (j) > 178 % aproximadamente 1.9 V
LimV = j ;
end
if Filt_I ( j ) < 62 % aproximadamente 10 mA
LimI = j ;
end
end
N_dados = LimV - LimI + 1 ; % nova quantidade de dados uteis
Filt_V2 = zeros ( 1 , N_dados ) ;
Filt_I2 = zeros ( 1 , N_dados ) ;
Filt_V2(1)= Filt_V (LimI) ;
Filt_I2 (1)= 0 ;
for j=2 : N_dados
Filt_V2(j)= Filt_V (j+LimI-1);
Filt_I2 (j) = Filt_I (j+LimI-1);
end
%% Pegar os dados válidos e extender até o ponto de curto circuito
% os dados são convertidos a V e A
for i = 1 : length ( Filt_V2 )
Filt_V2 ( i ) = Filt_V2 ( i ) * 0.0107 + 0.03 ;
Fit_I2 ( i ) = Filt_I2 ( i )*0.0001621 + 0.0015 ;
end
% extrapolação linear com os 2 últimos dados
Vp = 0 ; Ip = 0 ; n = 2 ;
for j=1 : n
Vp = Vp + Filt_V2 (N_dados-j +1);
Ip = Ip + Filt_I2 (N_dados-j +1);
end
Vp = Vp/n ; Ip = Ip /n ;
r = 0 ; s = 0 ;
for j=1 : n
r = r + ( Filt_V2 (N_dados-j+1) - Vp) * ( Filt_I2(N_dados-j+1) - Ip) ;
s = s + pow2( Filt_V2 (N_dados-j+1) - Vp ) ;
end
m = r / s ;
b = Ip-m*Vp;
n0=round( Filt_V2 (N _dados) ) ;
~~~


