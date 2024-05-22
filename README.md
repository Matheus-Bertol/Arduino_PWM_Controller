# Controle de Motor PWM com Arduino

## Índice
1. [Introdução ao PWM](#introdução-ao-pwm)
2. [Componentes necessários](#componentes-necessários)
3. [Esquemático](#esquemático)
4. [Código-fonte](#código-fonte)
5. [Instruções de montagem](#instruções-de-montagem)
6. [Funcionamento do projeto](#funcionamento-do-projeto)

## Introdução ao PWM
O PWM (Pulse Width Modulation) é uma técnica utilizada para controlar a potência fornecida a dispositivos elétricos, como motores. Nesta aplicação, utilizaremos um Arduino para controlar a velocidade de um motor através de um botão.

## Componentes necessários
- 1 Arduino Nano
- 1 Motor DC
- 1 Driver de motor L293D
- 1 Botão
- 1 Resistência de 10k ohms
- Fios de conexão
- Fonte de alimentação (5V)

## Esquemático
![Esquemático](./schematics/pwm_controller_schematic.pdf)

## Código-fonte
```cpp
#include <Arduino.h>

const int motorPin = 9;
const int buttonPin = 2;
int motorSpeed[] = {0, 50, 100, 150, 200, 250}; // Array de velocidades
int buttonState = 0;
int lastButtonState = 0;
int pressCount = 0;

void setup() {
  pinMode(motorPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState != lastButtonState) {
    if (buttonState == LOW) {
      pressCount++;
      if (pressCount > 5) {
        pressCount = 0; // Reseta o contador após 5 pressionamentos
      }
      analogWrite(motorPin, motorSpeed[pressCount]);
    }
    delay(100); // Debounce do botão
  }

  lastButtonState = buttonState;
}
