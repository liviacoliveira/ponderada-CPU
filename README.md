# CPU de 8 bits

Este repositório contém a implementação de uma Unidade Central de Processamento (CPU) de 8 bits, desenvolvida de forma modular no simulador Digital. O projeto foca na elaboração do circuito de controle capaz de gerenciar os clicos de busca e de execução, e no conjunto de memórias para armazenar as instruções e as variáveis;

## Estrutura de Arquivos e Módulos

O projeto foi construído de forma hierárquica para garantir organização e reuso de componentes:

1. **ALU.dig** (Unidade Lógica e Aritmética)
É o núcleo de cálculo da CPU. Recebe dois operandos de 8 bits e um código de seleção (OpCode).

2. **registrador8bits.dig**
Componente de memória volátil síncrona. Utilizado para instanciar os registradores:

* **AC** (Acumulador): Armazena resultados imediatos.
* **MQ** (Multiplier/Quotient): Registrador auxiliar para operações complexas.
* **N** (Operando): Armazena o valor de entrada vindo do barramento.

3. **CPU.dig** 
O arquivo mestre que integra todos os módulos. Ele contém o Barramento Único (BUS) de 8 bits e as conexões de controle vindas da EEPROM.

## Unidade de Controle e Ciclo de Máquina

A inteligência da CPU é baseada em Microprogramação, o que permite que a máquina execute passos complexos automaticamente.

### Contador + Splitter

Um Contador de 3 bits atua como o ponteiro de micro-instruções.

O Splitter une o sinal do contador com as chaves de seleção de operação do usuário, formando o endereço completo para a EEPROM.

### Fluxo de Execução (Exemplo: Soma)

* **Passo de Busca (T0)**: O contador envia o primeiro endereço. A EEPROM ativa o sinal Load_N. O dado da entrada é salvo no Registrador N.
* **Passo de Execução (T1)**: O contador incrementa. A EEPROM configura a ALU para "Soma" e ativa Load_AC. O resultado é salvo no Acumulador.

Saída: O resultado final é decodificado e exibido nos Displays de 7 Segmentos em formato Hexadecimal.

## Tabela de Operações Suportadas

| OpCode | Operação | Saída AC | Saída MQ |
|--------|----------|----------|----------|
| 000 | Soma | Resultado | - |
| 110 | Multiplicação | LSB (8 bits baixos) | MSB (8 bits altos) |
| 100 | Divisão | Resto | Quociente |
| 101 | Shift Lógico | Dado deslocado | - |

## Visualização final da CPU

![cpu](cpu.png)

## Vídeo explicativo da solução