# PG50315 David - Alexandre Ferreira Duarte | Mestrado em Engenharia Informática

2 Modelos CPU  CPU1 CPU2                     </br>
2 Modelos RAM RAM1 RAM2                      </br>
2 Modelos MB (motherboard) MB1 MB2           </br>
3 Modelos PG (placas gráficas) PG1 PG2 PG3   </br>
3 Modelos MON (monitores) MON1 MON2 MON3     </br>

```
CPU1 = 1
CPU2 = 2
RAM1 = 3
RAM2 = 4
MB1 = 5
MB2 = 6
PG1 = 7
PG2 = 8
PG3 = 9
MON1 = 10
MON2 = 11
MON3 = 12
```

Cada computador tem que ter obrigatoriamente uma única motherboard, um unico CPU, uma única placa gráfica e uma única memória RAM. O computador poderá ter ou não ter monitores.

Uma única motherboard </br>
`` (MB1 ∨ MB2) ∧ (MB1 → ¬ MB2) ∧ (MB2 → ¬ MB1) ≡ (MB1 ∨ MB2) ∧ ( ¬ MB1 ∨ ¬ MB2)``

Um único CPU </br>
`` (CPU1 ∨ CPU2) ∧ (CPU1 → ¬ CPU2) ∧ (CPU2 → ¬ CPU1) ≡ (CPU1 ∨ CPU2) ∧ ( ¬ CPU1 ∨ ¬ CPU2)``

Uma única placa gráfica </br>
``` 
(PG1 ∨ PG2 ∨ PG3) ∧ (PG1 → (¬ PG2 ∧ ¬ PG3)) ∧ (PG2 → (¬ PG1 ∧ ¬ PG3)) ∧ (PG3 → (¬ PG1 ∧ ¬ PG2)) ≡
≡ (PG1 ∨ PG2 ∨ PG3) ∧ ( ¬ PG1 ∨ ¬ PG2) ∧ ( ¬ PG1 ∨ ¬ PG3) ∧ ( ¬ PG2 ∨ ¬ PG3)
```

Uma única memória RAM </br>
`` (RAM1 ∨ RAM2) ∧ (RAM1 → ¬ RAM2) ∧ (RAM2 → ¬ RAM1) ≡ (RAM1 ∨ RAM2) ∧ ( ¬ RAM1 ∨ ¬ RAM2) ``

Poderá ter ou não ter monitores

# REGRAS

A motherboard MB1 quando combinada com a placa gráfica PG1, obriga à utilização da RAM1. </br>
`` (MB1 ∧ PG1) → RAM1 ≡ ¬ (MB1 ∧ PG1) ∨ RAM1 ≡ ¬ MB1 ∨ PG1 ∨ RAM1``

A placa gráfica PG1 precisa do CPU1, excepto quando combinada com uma memória RAM2. </br>
`` PG1 → (CPU1 ∨ RAM2) ≡ ¬ PG1 ∨ (CPU1 ∨ RAM2)``

O CPU2 só pode ser instalado na motherboard MB2. </br>
`` CPU2 → MB2 ≡ ¬ CPU2 ∨ MB2``

O monitor MON1 para poder funcionar precisa da placa gráfica PG1 e da memória RAM2. </br>
`` MON1 → (PG1 ∧ RAM2) ≡ ¬ MON1 ∨ (PG1 ∧ RAM2) ≡ (¬ MON1 ∨ PG1) ∧ (¬ MON1 ∨ RAM2)``

O monitor MON2 precisa da memória RAM2 para poder trabalhar com a placa gráfica PG3. </br>
`` (MON2 ∧ PG3) → RAM2 ≡ ¬ (MON2 ∧ PG3) ∨ RAM2 ≡ ¬ MON2 ∨ PG3 ∨ RAM2 ``


# PERGUNTAS

## a) O monitor MON1 so poderá ser usado com uma motherboard MB1?

## b) Um cliente pode personalizar o seu computador da seguinte forma: uma motherboard MB1, o CPU1, a placa gráfica PG2 e a memória RAM1?

## c) É possivel combinar a motherboard MB2, a placa gráfica PG3 e a RAM1 num mesmo computador ?

## d) Para combinarmos a placa gráfica PG2 e a RAM1 temos que usar o CPU2?

# Codificação do Problema

```Python
!pip install python-sat[pblib,aiger]
```


```Python
from pysat.solvers import Minisat22

s = Minisat22()                # cria o solver s

# Um único CPU (CPU1 = 1, CPU2 = 2): (CPU1 ∨ CPU2) ∧ ( ¬ CPU1 ∨ ¬ CPU2)
s.add_clause([1, 2])         
s.add_clause([-1, -2])

# Uma única memória RAM (RAM1 = 3, RAM2 = 4):(RAM1 ∨ RAM2) ∧ ( ¬ RAM1 ∨ ¬ RAM2)
s.add_clause([3, 4])         
s.add_clause([-3, -4])

# Uma única motherboard (MB1 = 5, MB2 = 6):(MB1 ∨ MB2) ∧ ( ¬ MB1 ∨ ¬ MB2)
s.add_clause([5, 6])         
s.add_clause([-5, -6])

# Uma única placa gráfica (PG1 = 7, PG2 = 8, PG3 = 9): (PG1 ∨ PG2 ∨ PG3) ∧ ( ¬ PG1 ∨ ¬ PG2) ∧ ( ¬ PG1 ∨ ¬ PG3) ∧ ( ¬ PG2 ∨ ¬ PG3)
s.add_clause([7, 8, 9])         
s.add_clause([-7, -8])
s.add_clause([-7, -9])
s.add_clause([-8, -9])

# Poderá ter ou não ter monitores (MON1 = 10, MON2 = 11, MON3 = 12)


# ==============================
# REGRAS
# ==============================

# A motherboard MB1 quando combinada com a placa gráfica PG1, obriga à utilização da RAM1: ¬ MB1 ∨ PG1 ∨ RAM1
s.add_clause([-5, 7, 3])

# A placa gráfica PG1 precisa do CPU1, excepto quando combinada com uma memória RAM2: ¬ PG1 ∨ (CPU1 ∨ RAM2)
s.add_clause([-7, 1, 4])

# O CPU2 só pode ser instalado na motherboard MB2: ¬ CPU2 ∨ MB2
s.add_clause([-2, -6])

# O monitor MON1 para poder funcionar precisa da placa gráfica PG1 e da memória RAM2: (¬ MON1 ∨ PG1) ∧ (¬ MON1 ∨ RAM2)
s.add_clause([-10, 7])
s.add_clause([-10, 4])

# O monitor MON2 precisa da memória RAM2 para poder trabalhar com a placa gráfica PG3: ¬ MON2 ∨ PG3 ∨ RAM2 
s.add_clause([-11, 9, 4])


if s.solve():                  # testa a satisfatibilidade
    print("SAT")
    print(s.get_model())       # imprime o modelo
else: 
    print("UNSAT")

s.delete()                     # apaga o solver s
```
