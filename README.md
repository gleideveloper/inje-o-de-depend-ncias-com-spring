# dependency-injection-with-spring
fonte: https://campuscode.com.br/conteudos/s-o-l-i-d-principio-de-inversao-de-dependencia
## Problema: Exemplo do diagrama abaixo fere os dois princípios do SOLID, que são:

### <a> Os Princípios do SOLID — OCP Princípio aberto-fechado</a>
O princípio de Aberto/Fechado propõe que entidades (classes, funções, módulos, etc.) devem ser abertas para extensão, mas fechadas para modificação.

* Aberto para extensão significa que, ao receber um novo requerimento, é possível adicionar um novo comportamento.
* Fechado para modificação significa que, para introduzir um novo comportamento (extensão), não é necessário modificar o código existente.
### <a>Os Princípios do SOLID — DIP Princípio de inversão de dependência</a>
O Princípio de Inversão de Dependência possui duas definições:
* (1) módulos de alto nível não devem depender de módulos de baixo nível e ambos devem depender de abstrações; e -
* (2) abstrações não devem depender de detalhes, mas detalhes devem depender de abstrações.

### Forte acoplamento entre classes
Exemplo no diagram de corte forte acoplamento entre a clase VendaService com a classe PagSeguroService, não sendo uma boa prática, sem o uso de interface.

![Alt text](src/main/resources/modelagem/AcomplamentoForte.png?raw=true "Forte Acomplamento")

* A classe VendaService é responsável por instanciar suas dependências, causando um forte acoplamento.
* A classe VendaService <b>conhece a dependência concreta</b> PagSeguroService.
* Se a classe concreta mudar, é <b>preciso mudar a classe PagSeguroService.</b>

Nesse exemplo abaixo existem um alto acoplamento entre esse dois serviços, não sendo uma boa prática, sem o uso de interface.
* A classe VendaService é responsável por instanciar suas dependências, causando um forte acoplamento com pontos de alteração.
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
## 1ª Solução: Injeção de dependência por meio do construtor

### Inversão de controle, Injeção de dependência

<b>acoplamento fraco.</b>
* A classe VendaService <b>não conhece a dependência concreta</b>
* Se a classe concreta mudar, <b>a classe VendaService não muda</b>, e só é alterado em um único lugar.

O exemplo abaixo temos um <b>fraco acoplamento</b> entre esse dois serviços, com o uso da <b> interface GatewayPagamento.</b>

```java
public class VendaService {

    private final GatewayPagamento gatewayPagamento;
    //Injeção de dependência usando o construtor
    public VendaService(GatewayPagamento gatewayPagamento) {
        this.gatewayPagamento = gatewayPagamento;
    }

    public void registrar(Venda venda, String numeroCartao){
        BigDecimal valorTotal = venda.getPricoUnitario().multiply(new BigDecimal(venda.getQuantidade()));
        System.out.printf("[Venda] Registrando venda de %s no valor total de %f...\n", venda.getProduto(), valorTotal);

        //Forte acomplamento entre a clase VendaService com a classe PagSeguroService
        //PagSeguroService pagSeguroService = new PagSeguroService("857db3dbbce149ab8943430f4d18bdf3");

        //Fraco acomplamento usando a interface por meio do construtor
        gatewayPagamento.efetuarPagamento(numeroCartao, valorTotal);
    }
}
````

É a injeção de dependência chamada pelo construtor através de um upcasting, onde minha classe mais especifica é instanciada no contrustor da classe VendaService.



Se a própria classe VendaService não deve ser responsável por instanciar suas dependências.

### Inversão de controle

É um padrão de desenvolvimento que consiste em <b>retirar da classe a responsabilidade de instanciar suas dependências.</b>

### Injeção de dependência

É uma forma de <b>realizar a inversão de controle</b>: um componente externo instacia a dependência, que é então injetada no objeto "pai" através desse componente. Pode ser implementada de vários formas:
* Construtor
* Classe de instanciação (builder/factory)
* Container / framework
