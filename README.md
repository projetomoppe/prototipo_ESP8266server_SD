# O uso de redes de sensores para monitoramento da bacia hidrográfica do rio Jacaraípe para prediçao de enchentes

O projeto tem como intuito construir um dispositivo para monitoramento de nível d'água que será utilizado no rio Jacaraípe para alertar a população local sobre possíveis enchentes e alagamentos. 

## Introdução

Para que o desenvolvimento e implementação do protótipo acontecesse, foi realizada uma pesquisa de componentes e sistemas embarcados para saber qual seria mais viável para o projeto, e o Arduino foi o que mais atendeu às nossas necessidades.

### Componentes
Arduino Mega 2560;                                                                                                          
2 Sensores ICOS LA16M40;                                                                                                
Sensor ultrassônico HC-SR04;                                                                                    
Módulo GPS GY-NEO6MV2;                                                                                    
Módulo Wifi ESP2866.                                                                                       

## Pinagem dos dispositivos

Sensores ICOS:                                                                                                            
```
sensor1 - pino de dados 4 e 5v.                                           
sensor2 - pino de dados 3 e 5v.
```


Sensor ultrassônico:                                                                                                        
```
pino 12 - ligação do pino ECHO.                                                           
pino 13 - ligação do pino TRIGGER.                                   
pino GND.
pino 5v.
```

Módulo GPS:                                                                                                               
```
portas serial RX3 e TX3 para comunicação. pino RX3 com divisor de tensão para 3.3v, com 2 resistores(4.7k e 10k).
porta GND. 
pino 5v.
```

Módulo Wifi:
```
pino RX direto ao divisor de tensão e pino TX.
pinos RST e CH_PD ligados a um resistor de 1K, que vai para o pino 3.3v.
porta GND.
porta 3.3v.
```
## Sobre o Módulo SD

O Módulo SD será usado com intuito de armazenamento de dados 

### Executando os testes
Serão realizdos os testes ultizando uma placa arduino juntamente com sensores de nível icos e com o sensor ultrasônico.

### Notas adicionais sobre implementação em um sistema

É preciso adicionar algumas bibliotecas especificas para o bom andamento do teste como:

#include <SD.h> (Oferece os recursos básicos para gravar e ler arquivos SD)

#include <SdFat.h> (Inicializa ou imprime um erro)


## Construído com

https://github.com/itead/ITEADLIB_Arduino_WeeESP8266 - weeESP8266 utilizada no módulo wifi.
https://bitbucket.org/teckel12/arduino-new-ping/downloads/ - NewPing utilizada no módulo ultrassônico.
https://github.com/mikalhart/TinyGPSPlus/releases/tag/v0.95 - TinyGPS utilizada no módulo GPS.
https://github.com/projetomoppe/moduloSD/blob/master/exemplo_gps/exemplo_gps.ino - Biblioteca do módulo SD.


### Biblioteca weeESP8266

funções utilizadas:

bool 	setOprToStation (void) : Define o modo de operação para a estação.

bool 	joinAP (String ssid, String pwd) : Entra em um ponto de acesso wifi.

String 	getLocalIP (void) : obtém o endereço IP para o módulo ESP8266.

bool 	enableMUX (void) : Habilita multiplas conexões com o módulo.

bool 	startTCPServer (uint32_t port=333) : inicia o servidor TCP(somente em modo múltipo).

bool 	setTCPServerTimeout (uint32_t timeout=180) : Define o tempo limite do servidor TCP.

bool 	send (uint8_t mux_id, const uint8_t *buffer, uint32_t len) : Envia dados com base em um dos TCP ou UDP construídos já em modo múltiplo.

bool lenght :

bool 	releaseTCP (uint8_t mux_id) : Libere a conexão TCP no modo múltiplo.

String 	getIPStatus (void) : Obtenha o status atual da conexão (UDP e TCP).

#define SSID  "moppe_wireless" : Define o nome do ponto de acesso(wifi) que será utilizado.

#define PASSWORD  "Moppe123JAC12" : Senha do ponto de acesso(wifi).

### Biblioteca NewPing

Funções utilizadas:

NewPing us(TRIG, ECHO); // inicia o sensor nas portas.

dados_su(); // obtencao do nivel dado pelo sensor ultrassonico

us.ping_cm() // conversao para cm.

### Biblioteca TinyGPS Plus

Funções utilizadas:

static const uint32_t GPSB = 9600; // definindo a velocidade de comunicação do módulo GPS.

gps.location.isUpdated() // Se os dados do GPS forem atualizados.

gps.location.isValid() // se localização for válido.

gps.date.isValid() // se data for válida.

gps.time.isValid() // se tempo for válido.

logData(ID_dispositivo, icos_inf, icos_sup, nivel); // registrar os dados do GPS.

### Biblioteca SdFat

#define LOG_FILE_PREFIX "log" // define o prefixo no nome do arquivo.

#define MAX_LOG_FILES 100 // define o limite de arquivos de log.

#define LOG_FILE_SUFIX "csv" // define o sufixo no nome do arquivo.

char logNomeArquivo[10]; // define o tamanho do nome do arquivo.

define LOG_COLUMN_COUNT 8 // define a quantidade de colunas do log.

char* logColunas[LOG_COLUMN_COUNT] = {"ID", "ICOS Inferior", "ICOS Superior", "Nivel Ultrassonico", "Latitude", "Longitude", "Data", "Hora"}; // define as colunas do log.

const unsigned long LOG_RATE = 1000; // define o tempo do loop de log.

if(!sdCard.begin(pinSD,SPI_HALF_SPEED)); sdCard.initErrorHalt(); //Inicializa o modulo SD.

updateFileName(); // Cada vez que começar, crie um novo arquivo e incremente um número.

printHeader(); // Imprime um cabeçalho na parte superior do novo arquivo.


## Funções dos dispositivos

Sensores ICOS: medir nível de acordo com seu estado que varia entre 0 e 1. À medida que o nível sobe, passa pelo sensor, mudando seu estado. O sensor pode estar normalmente aberto(0) ou normalmente fechado(1).

Sensor ultrassônico: medir nível com ondas sonoras que rebatem na água e mostra exatamente a altura do nível. Também é possível medir a velocidade em que o nível sobe para assim realizar uma estitamita de tempo em que o nível subirá até um nível crítico.

Módulo GPS: exibir a localização do protótipo e mostrar dados como data e hora.

Módulo Wifi: enviar os dados lidos pelos dispositivos para um servidor, onde serão exibidos num website para que qualquer pessoa tenha acesso aos dados lidos em tempo real.
