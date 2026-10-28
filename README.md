# BakeML

**BakeML** é uma *Domain-Specific Modeling Language* (DSML) para a modelagem estrutural de bolos e de suas composições internas e externas. Foi desenvolvida na ferramenta **Modelio** para a disciplina de **Engenharia de Software Orientada a Modelos**, do Instituto de Ciência e Tecnologia da **Universidade Federal Fluminense (UFF)**.

## Autores

| Nome | E-mail |
|------|--------|
| Luis Filipe Lamonica Salvador Caliman | luisflsc@id.uff.br |
| Gabriel Moura dos Santos Barreto | moura_gabriel@id.uff.br |

## Motivação

Na confeitaria moderna, os bolos têm estruturas, camadas, sabores e composições cada vez mais complexos. Hoje, esses elementos costumam ser descritos apenas em linguagem natural, o que gera ambiguidades entre confeiteiros e clientes.

A BakeML oferece conceitos próprios do domínio, como Massa, Recheio, Cobertura e Bolo. Com eles, o confeiteiro cria modelos visuais padronizados e pode validá-los com o cliente antes de produzir o bolo.

## Ferramenta

A linguagem foi implementada como um **perfil UML** no [Modelio](https://www.modelio.org/), que oferece:

- definição de **estereótipos** sobre metaclasses UML;
- **tagged values** para atributos específicos dos estereótipos;
- **validação automática** por regras de restrição (OCL);
- **aplicação visual** dos estereótipos, com ícones e marcações;
- criação de **módulos** instaláveis, que permitem encapsular e distribuir a DSML.

## Sintaxe abstrata

A BakeML estende as metaclasses `Package`, `Class` e `Association` da UML.

| Elemento | Metaclasse | Descrição |
|----------|------------|-----------|
| **Caixa** | `Package` | Conjunto de bolos (uma entrega). |
| **Bolo** | `Class` | Representa os andares de um bolo. Contém `Massa [1..*]`, `Recheio [0..*]` e `Cobertura [0..*]`. |
| **Massa** | `Class` | Estrutura mínima de um bolo. Atributos: `Tipo` (sabor), `CamadaInferior [0..1]` e `CamadaSuperior [0..1]`. |
| **Recheio** | `Class` | Intercala duas massas. Atributos: `Sabor`, `MassaInferior` e `MassaSuperior`. |
| **Cobertura** | `Class` | Fica no topo do bolo. Atributos: `Sabor` e `MassaBase`. |

## Exemplos de uso

- **Caixa com dois bolos:** o Bolo1 tem 2 massas, um recheio entre elas e uma cobertura. O Bolo2 tem apenas uma massa, a estrutura mínima válida.
- **Duas entregas:** a Caixa1 contém um bolo simples, com massa e cobertura. A Caixa2 contém um bolo com massa e recheio, sem cobertura.
- **Bolo de dois andares:** a linguagem permite quantos andares forem necessários.

## Regras de transformação (PIM → PSM)

As transformações foram descritas em **QVT** (Query/View/Transformation), do modelo independente de plataforma (PIM) para o específico de plataforma (PSM):

1. **`BoloToPSMBolo`:** estrutura a hierarquia de camadas, identificando a massa base e percorrendo as conexões superiores até o topo.
2. **`LevelForMassa`:** cada Massa gera um `Level` no PSM, com sabor e altura padrão.
3. **`ConnectLevels`:** conecta cada nível ao anterior e ao próximo, formando a cadeia estrutural do bolo.
4. **`RecheioToFilling`:** posiciona cada Recheio entre duas massas consecutivas. Se não houver massas suficientes, registra um *issue* de "recheio órfão".
5. **`ValidateMassa`:** gera diagnósticos para loops nas camadas e para inconsistências entre `CamadaInferior` e `CamadaSuperior`.
6. **`MassaTipoNormalization`:** converte o `Tipo` (string) da Massa em um enum `Flavor` padronizado (`CHOCOLATE`, `VANILLA` ou `CUSTOM`).

## Considerações sobre o Modelio

**Pontos positivos**
- Amplo suporte à definição de perfis UML (estereótipos, tagged values etc.).
- Interface que facilita a organização do perfil e a aplicação dos estereótipos.
- Módulos permitem empacotar a DSML e instalá-la no ambiente do usuário final.

**Pontos negativos**
- Complexidade e curva de aprendizado alta no início.
- A criação de módulos exige familiaridade com a estrutura de arquivos da ferramenta.
- Algumas funcionalidades exigem conhecimento de linguagens externas, como Java.
- Documentação nem sempre clara para quem cria uma DSML pela primeira vez.

**Sugestões de melhoria:** interface mais objetiva e intuitiva, documentação mais clara sobre as funcionalidades suportadas, e uma nova implementação.

## Arquivos

| Arquivo | Descrição |
|---------|-----------|
| `Relatório_BakeML.docx` | Relatório de desenvolvimento da BakeML |
| `README.md` | Este arquivo |
