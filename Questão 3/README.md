### Conflitos no pipeline

Considerandos os estágios:
* BI: Busca da Instrução. (2ns)
* DI: Decodificação da Instrução. (1ns)
* EX: Excecução da instrução. (2ns)
* MEM: Leitura/escrita na memória. (2ns)
* ER: escrita no registrador. (1ns)

|                      | CC1 | CC2 | CC3 | CC4 | CC5 | CC6 | CC7 | CC8 | CC9 | CC10|
|----------------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|1. subi $t2, $t2, 4   |  BI |  DI |  EX | ER  |  -  |  -  |  -  |  -  |  -  |  -  |
|2. lw   $t1, 0($t2)   |  -  |  BI |  DI | EX  | MEM | ER  |  -  |  -  |  -  |  -  |
|3. add  $t3, $t1, $t4 |  -  |  -  |  BI | DI  | EX  | ER  |  -  |  -  |  -  |  -  |
|4. add  $t4, $t3, $t3 |  -  |  -  |  -  | BI  | DI  | EX  | ER  |  -  |  -  |  -  |
|5. STALL              |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |
|6. STALL              |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |
|6. sw   $t4, 0($t2)   |  -  |  -  |  -  |  -  |  -  | BI  | DI  | EX  | MEM |  -  |
|7. STALL              |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |  -  |
|8. beq  $t2, $0, loop |  -  |  -  |  -  |  -  |  -  |  -  |  -  | BI  | DI  | EX  |

OBS: 
* Conflito de dados no 3 e 4 resolvido com adiantamento de dados.
* STALL em 5 e 7 devido a conflito de dados.
* STALL em 6 devido a conflito de estrutura (impossivel executar BI e MEM no mesmo ciclo de clock)

Com pipeline: 19ns
Sem pipeline: 38ns
speedUP 38/19 = 2x mais rápida que a versão sem pipeline.
