# Strategy
README Técnico — Refatoração de Código Legado (SRP e OCP)

📌 Objetivo

Este projeto tem como objetivo refatorar um código legado que apresentava violações dos princípios SRP (Single Responsibility Principle) e OCP (Open/Closed Principle), a fim de torná-lo mais modular, flexível e de fácil manutenção.
A refatoração foi realizada aplicando padrões de projeto adequados — principalmente Strategy e Factory/Adapter.


---

⚙️ Problemas Identificados

Durante a análise do código original, foram observadas as seguintes violações dos princípios SOLID:

🚫 Violação do SRP

O SRP (Princípio da Responsabilidade Única) afirma que uma classe deve ter apenas uma responsabilidade e um único motivo para mudar.
No código legado, algumas classes acumulavam múltiplas funções.

Exemplo:

class RelatorioService {
    public void gerarRelatorio(String tipo) {
        // 1. Gera dados do relatório
        // 2. Formata o conteúdo
        // 3. Salva em arquivo
    }
}

👉 Problema: a mesma classe era responsável por gerar, formatar e salvar o relatório, o que dificulta manutenção e testes.


---

🚫 Violação do OCP

O OCP (Princípio do Aberto/Fechado) diz que o código deve estar aberto para extensão, mas fechado para modificação.
No código original, a lógica de cálculo de descontos dependia de condicionais, exigindo alteração a cada novo tipo de cliente.

Exemplo:

class CalculadoraDesconto {
    public double calcular(String tipoCliente, double valor) {
        if (tipoCliente.equals("VIP")) return valor * 0.9;
        if (tipoCliente.equals("ESTUDANTE")) return valor * 0.95;
        return valor;
    }
}

👉 Problema: sempre que um novo tipo de cliente é adicionado, o método precisa ser modificado.


---

💡 Soluções Aplicadas

🧠 Padrão Strategy (para resolver violação de OCP)

O padrão Strategy foi aplicado para encapsular as regras de desconto em classes separadas, tornando o sistema extensível sem alterações no código existente.

Após a refatoração:

interface DescontoStrategy {
    double aplicarDesconto(double valor);
}

class DescontoVip implements DescontoStrategy {
    public double aplicarDesconto(double valor) {
        return valor * 0.9;
    }
}

class DescontoEstudante implements DescontoStrategy {
    public double aplicarDesconto(double valor) {
        return valor * 0.95;
    }
}

class CalculadoraDesconto {
    private DescontoStrategy desconto;

    public CalculadoraDesconto(DescontoStrategy desconto) {
        this.desconto = desconto;
    }

    public double calcular(double valor) {
        return desconto.aplicarDesconto(valor);
    }
}

✅ Benefício: agora é possível criar novos tipos de desconto sem modificar o código da CalculadoraDesconto, atendendo ao OCP.


---

🧩 Padrão Factory + SRP (para resolver violação do SRP)

As responsabilidades de geração, formatação e gravação de relatórios foram divididas em classes específicas, aplicando o SRP e utilizando uma fábrica para criar os objetos necessários.

Após a refatoração:

class GeradorRelatorio {
    public String gerar() { return "dados do relatório"; }
}

class FormatadorRelatorio {
    public String formatar(String dados) { return "Relatório formatado: " + dados; }
}

class GravadorArquivo {
    public void salvar(String conteudo) { /* salva no disco */ }
}

class RelatorioService {
    private GeradorRelatorio gerador;
    private FormatadorRelatorio formatador;
    private GravadorArquivo gravador;

    public RelatorioService(GeradorRelatorio g, FormatadorRelatorio f, GravadorArquivo gr) {
        this.gerador = g;
        this.formatador = f;
        this.gravador = gr;
    }

    public void processar() {
        String dados = gerador.gerar();
        String conteudo = formatador.formatar(dados);
        gravador.salvar(conteudo);
    }
}

✅ Benefício: cada classe possui uma única função, facilitando testes, manutenção e evolução do sistema (aderência ao SRP).


---

🧮 Resultados da Refatoração

Critério	Situação Antes	Situação Depois

Responsabilidade única (SRP)	❌ Violada	✅ Corrigida
Aberto/Fechado (OCP)	❌ Violada	✅ Corrigida
Clareza de código	❌ Baixa	✅ Alta
Facilidade de manutenção	❌ Difícil	✅ Fácil
Extensibilidade	❌ Limitada	✅ Alta



---

🧰 Tecnologias Utilizadas

Linguagem: Java (versão X)

Paradigma: Orientação a Objetos (OO)

Padrões de Projeto: Strategy, Factory, Adapter

Princípios SOLID: SRP, OCP



---

📚 Conclusão

A refatoração trouxe benefícios significativos:

O código tornou-se modular e extensível, atendendo ao OCP.

Cada componente passou a ter uma responsabilidade clara, em conformidade com o SRP.

O sistema ficou mais fácil de testar, compreender e manter.

Novas funcionalidades podem ser adicionadas sem impactar o código existente.


Essas melhorias demonstram a importância prática dos princípios SOLID e dos padrões de projeto no desenvolvimento de software escalável e sustentável.


---

✍️ Autor

[Andri Santana Bezerra]
Disciplina: Padrões de Projeto / Engenharia de Software
Data: 22 de Outubro de 2025
