# dependency-injection-with-spring

## O princípio aberto/fechado (S.O.L.I.D)
<p>O princípio diz que as entidades de software (classes, módulos, funções, etc.)
<b>devem estar abertas para extensão, mas fechadas para modificação, ou seja, deve ser possível estender o comportamento de uma classe, mas não a modificar</b></p>

## Inversão de controle, Injeção de dependência
### Forte acoplamento entre classes
Exemplo no diagram de corte forte acoplamento entre a clase VendaService com a classe PagSeguroService, não sendo uma boa prática, sem o uso de interface.

![Alt text](src/main/resources/modelagem/AcomplamentoForte.png?raw=true "Forte Acomplamento")

* A classe VendaService é responsável por instanciar suas dependências, causando um forte acoplamento.
```java
public class VendaService {

    public void registrar(Venda venda, String numeroCartao){
        BigDecimal valorTotal = venda.getPricoUnitario().multiply(new BigDecimal(venda.getQuantidade()));
        System.out.print("[Venda] Registrando venda de %s no valor total de %f...\n");

        //Forte acomplamento entre a clase VendaService com a classe PagSeguroService
        PagSeguroService pagSeguroService = new PagSeguroService("857db3dbbce149ab8943430f4d18bdf3");
        pagSeguroService.efetuarPagamento(numeroCartao, valorTotal);
    }
}
````
