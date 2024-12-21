
# Implementação de Padrões de Projeto
## Padrão Criacional: PROTOTYPE
O Prototype é um padrão de projeto criacional que permite copiar objetos existentes sem fazer seu código ficar dependente de suas classes.</br>
Digamos que você tenha um objeto, e você quer criar uma cópia exata dele.</br>
Como você o faria? Primeiro, você tem que criar um novo objeto da mesma classe. Então você terá que ir por todos os campos do objeto original e copiar seus valores para o novo objeto.</br>
#### O problema a ser resolvido:
É que nem todos os objetos podem ser copiados dessa forma porque alguns campos de objeto podem ser privados e não serão visíveis fora do próprio objeto e há ainda mais um problema com a abordagem direta. Uma vez que você precisa saber a classe do objeto para criar uma cópia, seu código se torna dependente daquela classe.</br>
Outro problema a ser levado em consideração que algumas vezes você só sabe a interface que o objeto segue, mas não sua classe concreta, quando, por exemplo, um parâmetro em um método aceita quaisquer objetos que seguem uma interface.</br>
#### A solução é:
O padrão Prototype delega o processo de clonagem para o próprio objeto que está sendo clonado. O padrão declara um interface comum para todos os objetos que suportam clonagem. Essa interface permite que você clone um objeto sem acoplar seu código à classe daquele objeto. Geralmente, tal interface contém apenas um único método clonar.
### Diagrama UML
![UML PROTOTYPE](https://github.com/user-attachments/assets/6701211d-e6ac-4e60-a73a-f667a91252ef)</br>
1. A interface Protótipo declara os métodos de clonagem. Na maioria dos casos é apenas um método clonar.</br>
2. A classe Protótipo Concreta implementa o método de clonagem. Além de copiar os dados do objeto original para o clone, esse método também pode lidar com alguns casos específicos do processo de clonagem relacionados a clonar objetos ligados, desfazendo dependências recursivas, etc.</br>
3. O Cliente pode produzir uma cópia de qualquer objeto que segue a interface do protótipo.</br>
### Explicando o codigo exemplo PROTOTYPE.rb :
#### Classe Prototype
1- Atributos attr_accessor: Define getters e setters para os atributos primitive, component, e circular_reference.</br>
2- Construtor initialize: Inicializa os atributos com nil.</br>
3- Método clone: Realiza a clonagem do objeto. Clona profundamente o component e circular_reference. No caso de circular_reference, ajusta o backreference para que a cópia aponte para o novo objeto clonado em vez do original.</br>
4- Método privado deep_copy: Utiliza serialização (Marshalling) para fazer uma cópia profunda do objeto. Isso é um pouco lento e ineficiente, sendo mais adequado em situações simples ou de exemplo.</br>
#### Classe ComponentWithBackReference
1- Atributo attr_accessor: Define getter e setter para o atributo prototype.</br>
2- Construtor initialize: Inicializa prototype com a instância fornecida de Prototype.</br>
#### Código do Cliente
##### Criação do Protótipo p1:
1- p1 é uma instância de Prototype.</br>
2- Atribui-se o valor 245 ao atributo primitive.</br>
3- Atribui-se o valor Time.now ao atributo component.</br>
4- Cria-se uma instância de ComponentWithBackReference passando p1 e atribui-se a circular_reference.</br>
##### Clonagem do Protótipo p2:
1- p2 é uma cópia de p1 criada usando o método clone.</br>
##### Verificações:
1- Valores Primitivos: Verifica se os valores primitivos foram copiados.</br>
2- Componentes Simples: Verifica se o componente simples foi clonado.</br>
3- Referência Circular: Verifica se a referência circular foi clonada.</br>
4- Referência ao Protótipo: Verifica se a referência circular no clone aponta para o próprio clone.</br>
## Padrão Estrutural: BRIDGE
O Bridge é um padrão de projeto estrutural que permite que você divida uma classe grande ou um conjunto de classes intimamente ligadas em duas hierarquias separadas—abstração e implementação—que podem ser desenvolvidas independentemente umas das outras.</br>
#### O problema a ser resolvido:
Digamos que você tem uma classe Forma geométrica com um par de subclasses: Círculo e Quadrado.</br>
Você quer estender essa hierarquia de classe para incorporar cores, então você planeja criar as subclasses de forma Vermelho e Azul.</br>
Contudo, já que você já tem duas subclasses, você precisa criar quatro combinações de classe tais como CírculoAzul e QuadradoVermelho.</br>
Adicionar novos tipos de forma e cores à hierarquia irá fazê-la crescer exponencialmente.</br>
Por exemplo, para adicionar uma forma de triângulo você vai precisar introduzir duas subclasses, uma para cada cor.</br>
E depois disso, adicionando uma nova cor será necessário três subclasses, uma para cada tipo de forma.</br>
#### A solução é:
Esse problema ocorre porque estamos tentando estender as classes de forma em duas dimensões diferentes: por forma e por cor. Isso é um problema muito comum com herança de classe.</br>
O padrão Bridge tenta resolver esse problema ao trocar de herança para composição do objeto.</br>
Isso significa que você extrai uma das dimensões em uma hierarquia de classe separada, para que as classes originais referenciem um objeto da nova hierarquia, ao invés de ter todos os seus estados e comportamentos dentro de uma classe.</br>
Podemos extrair o código relacionado à cor em sua própria classe com duas subclasses: Vermelho e Azul.</br>
A classe Forma então ganha um campo de referência apontando para um dos objetos de cor.</br>
Agora a forma pode delegar qualquer trabalho referente a cor para o objeto ligado a cor.</br>
Aquela referência vai agir como uma ponte entre as classes Forma e Cor.</br>
De agora em diante, para adicionar novas cores não será necessário mudar a hierarquia da forma e vice versa.</br>
### Diagrama UML
![UML BRIDGE](https://github.com/user-attachments/assets/b4e288b1-cf80-4bb3-bde0-05474f5b1062)
1. A Abstração fornece a lógica de controle de alto nível. Ela depende do objeto de implementação para fazer o verdadeiro trabalho de baixo nível.</br>
2. A Implementação declara a interface que é comum para todas as implementações concretas. Um abstração só pode se comunicar com um objeto de implementação através de métodos que são declarados aqui.
A abstração pode listar os mesmos métodos que a implementação, mas geralmente a abstração declara alguns comportamentos complexos que dependem de uma ampla variedade de operações primitivas declaradas pela implementação.</br>
3. Implementações Concretas contém código plataforma-específicos.</br>
4. Abstrações Refinadas fornecem variantes para controle da lógica. Como seu superior, trabalham com diferentes implementações através da interface geral de implementação.</br>
5. Geralmente o Cliente está apenas interessado em trabalhar com a abstração. Contudo, é trabalho do cliente ligar o objeto de abstração com um dos objetos de implementação.</br>
### Explicando o codigo exemplo BRIDGE.rb :
#### Classe Abstraction
1- Atributo @implementation: Referência a um objeto da hierarquia Implementation.</br>
2- Método initialize: Inicializa a instância de Abstraction com um objeto Implementation.</br>
3- Método operation: Executa uma operação base, delegando a operação real para o método operation_implementation do objeto Implementation.</br>
#### Classe ExtendedAbstraction
1- Herança de Abstraction: ExtendedAbstraction herda de Abstraction.</br>
2- Método operation: Sobrescreve o método operation para fornecer uma operação estendida, ainda delegando a operação real ao método operation_implementation do objeto Implementation.</br>
#### Classe Implementation
1- Interface Implementation: Define a interface para todas as classes de implementação.</br>
2- Método abstrato operation_implementation: Levanta um erro se não for implementado por uma subclasse.</br>
#### Classes Concretas de Implementação: ConcreteImplementationA E ConcreteImplementationB
1- Herança de Implementation: Ambas as classes concretas (ConcreteImplementationA e ConcreteImplementationB) herdam de Implementation.</br>
2- Método operation_implementation: Implementam a interface operation_implementation com o comportamento específico de cada plataforma.</br>
#### Código do Cliente
##### Função client_code: 
Recebe um objeto Abstraction e chama seu método operation.</br>
##### Execução do Código do Cliente:
Cria uma instância de ConcreteImplementationA e uma instância de Abstraction utilizando esta implementação.</br>
Chama client_code passando abstraction.</br>
Repete o processo para ConcreteImplementationB e ExtendedAbstraction.</br>
##### Resumo da explicação
Este código implementa o padrão de projeto Bridge. Ele separa a abstração (Abstraction) da implementação (Implementation), permitindo que ambas as hierarquias evoluam independentemente.</br>
