### Conflitos no pipeline

Considerandos os estágios:
* BI: Busca da Instrução.
* DI: Decodificação da Instrução.
* EX: Excecução da instrução.
* MEM: Leitura/escrita na memória.
* ER: escrita no registrador.

                         C1 | C2 | C3 | C4  | C5  | C6  | C7  | C8  | C9  |
1. subi $t2, $t2, 4    | BI | DI | EX | ER  |
2. lw   $t1, 0($t2)         | BI | DI | EX  | MEM | ER  |
3. add  $t3, $t1, $t4            | BI | DI  | EX  | ER  |
4. add  $t4, $t3, $t3                 | BI  | DI  | EX  | ER  |
5. STALL (Conflito de dados)
6. sw   $t4, 0($t2)                         | BI  | DI  | EX  | MEM |
7. STALL
8. beq  $t2, $0, loop                                   | BI  | DI  | EX  |

Conflito de dados no 3 e 4 resolvido com adiantamento de dados.