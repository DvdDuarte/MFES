## Regras

A motherboard MB1 quando combinada com a placa grafica PG1, obriga a utilizacao da RAM1.
``` (MB1 ∧ PG1) -> RAM1 ≡ ¬ (MB1 ∧ PG1) ∨ RAM1```

A placa grafica PG1 precisa do CPU1, excepto quando combinada com uma memoria RAM2.
``` PG1 -> (CPU1 ∨ RAM2) ≡ ¬ PG1 ∨ (CPU1 ∨ RAM2)```

O CPU2 so pode ser instalado na motherboard MB2.
``` CPU2 -> MB2 ≡ ¬ CPU2 ∨ MB2```

O monitor MON1 para poder funcionar precisa da placa grafica PG1 e da memoria RAM2.
``` MON1 -> (PG1 ∧ RAM2) ≡ ¬ MON1 ∨ (PG1 ∧ RAM2)```

O monitor MON2 precisa da memoria RAM2 para poder trabalhar com a placa grafica PG3.
``` (MON2 ∧ PG3) -> RAM2 ≡ ¬ (MON2 ∧ PG3) ∨ RAM2```

