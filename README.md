Feito por Future Makers -> Victor Mattenhauer Lopes Capp (555753)
                           Paulo Akira Okama (556840)
                           Rafael Ferrari (556655)

Criamos um sistema para uma vinheria onde a cada momento diferente de luz é mostrado uma mensagem.
Primeiramente é mostrado a logo da nossa empresa, Future Makers. 

![Capture](https://github.com/VictorMLCapp/Edge-computing/assets/163414336/495af386-4098-4959-b72c-543e7d2524a2)

Após nossa logo, demostra se a temperatura está OK e o LED verde é ativo, mostrando também a temperatura e a umidade do ambiente

![image](https://github.com/VictorMLCapp/Edge-computing/assets/163414336/22505260-a6db-413b-9320-5c5b9b409bfe)

Se o local estiver com meia luz, o LED amarelo é ativo, soando o buzzer e aparecendo no Display a mensagem Ambiente com meia luz e mostrando também a temperatura e a umidade.

![image](https://github.com/VictorMLCapp/Edge-computing/assets/163414336/b0b12c46-2f03-4673-8aac-ffc91aceb363)


Aós é demonstrado com o LED vermelho que o ambiente está com a temperatura elevada e Muita luz, mostrando o quanto de temperatra e umidade o ambiente está.

![image](https://github.com/VictorMLCapp/Edge-computing/assets/163414336/6e094f25-e093-4377-bc7e-4f36d69e153f)


Utilizamos uma buzina através de um Buzzer e LEDs que começam a piscar, temos 3 LEDs, verde, amarelo e vermelho.
Foi utilizado um Arduino, Display, Sensor de Luminosidade, DHT-11 (porém no Wokwi é utilizado o DHT-22) e utilizamos também 5 resistores e 24 fios aproximadamente (foto do projeto desligado a seguir)

![image](https://github.com/VictorMLCapp/Edge-computing/assets/163414336/d2614fcd-4044-4b90-9228-80cbbb474b2d)


Foi utilizado duas bibliotecas, sendo elas a LiquidCrystal e a DHT sensor library. 

![image](https://github.com/VictorMLCapp/Edge-computing/assets/163414336/31a41e83-6480-4235-b2b9-b5e322a26606)


Código comentado utilizado abaixo:


#include "DHT.h" // Inclui a biblioteca DHT para interface com o sensor DHT

#define DHTPIN 2     // Define o pino ao qual o sensor DHT está conectado
#define DHTTYPE DHT22   // Define o tipo de sensor DHT (DHT22 neste caso)

DHT dht(DHTPIN, DHTTYPE); // Cria um objeto DHT para interagir com o sensor DHT

#include <LiquidCrystal.h> // Inclui a biblioteca LiquidCrystal para controle do LCD

const int rs = 12; // Define o pino RS do LCD
const int en = 10; // Define o pino EN do LCD
const int d4 = 5;  // Define o pino D4 do LCD
const int d5 = 4;  // Define o pino D5 do LCD
const int d6 = 3;  // Define o pino D6 do LCD
const int d7 = 2;  // Define o pino D7 do LCD

LiquidCrystal lcd(rs, en, d4, d5, d6, d7); // Cria um objeto LiquidCrystal para o LCD

// Definições dos pinos
const int pinLDR = A0; // Define o pino ao qual o LDR está conectado
const int ledverde = 7; // Define o pino do LED verde
const int ledamarelo = 8; // Define o pino do LED amarelo
const int ledvermelho = 9; // Define o pino do LED vermelho
const int pinBuzzer = 5; // Define o pino do buzzer

void setup() {
  Serial.begin(115200); // Inicia a comunicação serial com a taxa de transmissão de 115200 bps
  Serial.println(F("DHT22 Ativo!")); // Imprime uma mensagem indicando que o sensor DHT22 está ativo

  dht.begin(); // Inicializa o sensor DHT

  // Configuração dos pinos como saída
  pinMode(ledverde, OUTPUT); // Configura o pino do LED verde como saída
  pinMode(ledamarelo, OUTPUT); // Configura o pino do LED amarelo como saída
  pinMode(ledvermelho, OUTPUT); // Configura o pino do LED vermelho como saída
  pinMode(pinLDR, INPUT); // Configura o pino do LDR como entrada
  pinMode(pinBuzzer, OUTPUT); // Configura o pino do buzzer como saída

  Serial.begin(9600); // Inicia a comunicação serial com a taxa de transmissão de 9600 bps

  // Inicializa o LCD
  lcd.begin(16, 2); // Inicializa o LCD com 16 colunas e 2 linhas
  lcd.clear(); // Limpa o LCD

  // Cria caracteres personalizados
  byte name0x5[] = { B00000, B11111, B10000, B10000, B11110, B10000, B10000, B10000 }; // F
  byte name0x6[] = { B00000, B10001, B10001, B10001, B10001, B10001, B10001, B01110 }; // U
  byte name0x7[] = { B00000, B11111, B00100, B00100, B00100, B00100, B00100, B00100 }; // T
  byte name0x8[] = { B00000, B01110, B10001, B10001, B11110, B11000, B10100, B10010 }; // R
  byte name0x10[] = { B00000, B11111, B10000, B10000, B11110, B10000, B10000, B11111 }; // E 
  byte name1x5[] = { B00000, B11011, B10101, B10001, B10001, B10001, B10001, B10001 }; // M
  byte name1x6[] = { B00000, B01110, B10001, B10001, B11111, B10001, B10001, B10001 }; // A
  byte name1x7[] = { B00000, B10010, B10100, B11000, B11000, B10100, B10010, B10001 }; // K 

  lcd.createChar(0, name0x5); // Cria o caractere personalizado 'F'
  lcd.setCursor(5, 0); // Define a posição do cursor no LCD
  lcd.write(byte(0)); // Escreve o caractere personalizado no LCD

  lcd.createChar(1, name0x6); // Cria o caractere personalizado 'U'
  lcd.setCursor(6, 0); // Define a posição do cursor no LCD
  lcd.write(byte(1)); // Escreve o caractere personalizado no LCD

  lcd.createChar(2, name0x7); // Cria o caractere personalizado 'T'
  lcd.setCursor(7, 0); // Define a posição do cursor no LCD
  lcd.write(byte(2)); // Escreve o caractere personalizado no LCD

  lcd.createChar(3, name0x6); // Cria o caractere personalizado 'R'
  lcd.setCursor(8, 0); // Define a posição do cursor no LCD
  lcd.write(byte(3)); // Escreve o caractere personalizado no LCD

  lcd.createChar(4, name0x8); // Cria o caractere personalizado 'E'
  lcd.setCursor(9, 0); // Define a posição do cursor no LCD
  lcd.write(byte(4)); // Escreve o caractere personalizado no LCD

  lcd.createChar(5, name0x10); // Cria o caractere personalizado 'S'
  lcd.setCursor(10, 0); // Define a posição do cursor no LCD
  lcd.write(byte(5)); // Escreve o caractere personalizado no LCD

  lcd.createChar(6, name1x5); // Cria o caractere personalizado 'M'
  lcd.setCursor(7, 1); // Define a posição do cursor no LCD
  lcd.write(byte(6)); // Escreve o caractere personalizado no LCD

  lcd.createChar(7, name1x6); // Cria o caractere personalizado 'A'
  lcd.setCursor(8, 1); // Define a posição do cursor no LCD
  lcd.write(byte(7)); // Escreve o caractere personalizado no LCD

  delay(5000); // Aguarda 5 segundos antes de continuar
}

void loop() {
  // Leitura do valor analógico do LDR
  int valor
