# Model Adapter

## Por que usar um Model Adapter?

Imagine que sua aplicação utiliza a propriedade title em 100 lugares diferentes. Se, por algum motivo, o backend mudar o nome da propriedade de title para subject, você precisaria modificar manualmente todos esses 100 lugares no frontend para refletir a mudança.

Isso seria trabalhoso, propenso a erros e difícil de manter.

Com um Model Adapter, essa mudança pode ser feita em um único lugar. O adapter mapeia a nova propriedade subject do backend para a propriedade title usada no frontend, garantindo que todas as partes do código continuem funcionando sem precisar modificar cada uma delas individualmente.
Exemplo de Implementação do Model Adapter no Angular

### 1. Criando a Interface do Modelo do Frontend

```typescript
    export interface Expense {
    id: number;
    title: string;
    amount: number;
    }
```

### 2. Criando o Adapter (expense.adapter.ts)

```typescript
    export class ExpenseAdapter {
    static fromApi(apiExpense: any): Expense {
        return {
        id: apiExpense.id,
        title: apiExpense.subject, // Mapeando 'subject' do backend para 'title' no frontend
        amount: apiExpense.value,
        };
    }
    }
```

### 3. Usando o Adapter ao Receber os Dados da API

```bash
    import { HttpClient } from '@angular/common/http';
    import { Injectable } from '@angular/core';
    import { Observable } from 'rxjs';
    import { map } from 'rxjs/operators';
    import { Expense } from '../models/expense.model';
    import { ExpenseAdapter } from '../adapters/expense.adapter';

    @Injectable({
    providedIn: 'root',
    })
    export class ExpensesService {
    private apiUrl = 'https://api.example.com/expenses';

    constructor(private http: HttpClient) {}

    getExpenses(): Observable<Expense[]> {
        return this.http.get<any[]>(this.apiUrl).pipe(
        map((apiExpenses) => apiExpenses.map(ExpenseAdapter.fromApi)) // Convertendo os dados
        );
    }
    }
```

### Benefícios do Model Adapter

✅ Menos impacto com mudanças no backend → Se o backend mudar, basta ajustar o adapter, sem precisar modificar várias partes do código.

✅ Maior consistência → O frontend mantém sua estrutura de dados coerente, independentemente de como o backend retorna os dados.

✅ Código mais limpo e organizado → Os componentes e serviços não precisam lidar com transformações repetitivas, pois a conversão acontece no adapter.

Essa abordagem facilita a manutenção e escalabilidade do código, garantindo que futuras mudanças no backend tenham impacto mínimo na aplicação. 🚀
