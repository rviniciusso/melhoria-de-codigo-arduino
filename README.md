# melhoria-de-codigo-arduino

#define CUSTOM_SETTINGS
#define INCLUDE_GAMEPAD_MODULE
#include <DabbleESP32.h>

// --- Definição dos Pinos ---

#define IN1 14
#define IN2 27
#define IN3 26
#define IN4 25
#define ENA 32
#define ENB 33

// troca de cosnt, uma variavel, para o pino IN1 para 14 quando for digitado, o que foi definido, ao compilar para o arduino, sem consumir ram
// const byte IN1 = 14; 
// const byte IN2 = 27;
// const byte IN3 = 26;
// const byte IN4 = 25;
// const int ENA = 32;
// const int ENB = 33;

// --- Configurações de PWM para o ESP32 ---
const uint16_t freq = 5000;
const uint8_t canalA = 0;
const uint8_t canalB = 1;
const uint8_t resolucao = 8;

uint8_t velocidade = 200; 
// mudança de tipo para ocupar menos memória no arduino

void setup() {
  Serial.begin(115200);
  Dabble.begin("Neftis"); 

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  
  ledcSetup(canalA, freq, resolucao);
  ledcSetup(canalB, freq, resolucao);
  ledcAttachPin(ENA, canalA);
  ledcAttachPin(ENB, canalB);
}

void frente() {
  // FRENTE (Antes ia para esquerda, agora corrigido para frente)
    digitalWrite(IN1, HIGH); digitalWrite(IN2, LOW);
    digitalWrite(IN3, LOW);  digitalWrite(IN4, HIGH);
    ledcWrite(canalA, velocidade);
    ledcWrite(canalB, velocidade);
}

void tras() {
  // TRÁS (Antes girava para direita, agora corrigido para trás)
    digitalWrite(IN1, LOW);  digitalWrite(IN2, HIGH);
    digitalWrite(IN3, HIGH); digitalWrite(IN4, LOW);
    ledcWrite(canalA, velocidade);
    ledcWrite(canalB, velocidade);
}

void esquerda() {
  // ESQUERDA (Antes ia para trás, agora corrigido para girar à esquerda)
    digitalWrite(IN1, LOW);  digitalWrite(IN2, HIGH);
    digitalWrite(IN3, LOW);  digitalWrite(IN4, HIGH);
    ledcWrite(canalA, velocidade);
    ledcWrite(canalB, velocidade);
}

void direita() {
  // DIREITA (Antes ia para frente, agora corrigido para girar à direita)
    digitalWrite(IN1, HIGH); digitalWrite(IN2, LOW);
    digitalWrite(IN3, HIGH); digitalWrite(IN4, LOW);
    ledcWrite(canalA, velocidade);
    ledcWrite(canalB, velocidade);
}

void parar() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  ledcWrite(canalA, 0);
  ledcWrite(canalB, 0);
}

void loop() {
  Dabble.processInput(); 

  // --- LÓGICA CORRIGIDA CONFORME SEU RELATO ---
  // usar funçoes para fazer a mesma lógica que o parar();, deixa o codigo mais limpo e as funçoes melhores definidas.

  if (GamePad.isUpPressed()) {
    frente();
  }
  
  else if (GamePad.isDownPressed()) {
    tras();
  }
  
  else if (GamePad.isLeftPressed()) {
    esquerda();
  }
  
  else if (GamePad.isRightPressed()) {
    direita();
  }
  
  else {
    parar();
  }
}
