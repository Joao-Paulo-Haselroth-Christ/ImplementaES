# ImplementaES
## Implementação de Padrões de Projeto
### Padrão Criacional: PROTOTYPE
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
##### Criação do Protótipo p1:</br>
1- p1 é uma instância de Prototype.</br>
2- Atribui-se o valor 245 ao atributo primitive.</br>
3- Atribui-se o valor Time.now ao atributo component.</br>
4- Cria-se uma instância de ComponentWithBackReference passando p1 e atribui-se a circular_reference.</br>
##### Clonagem do Protótipo p2:</br>
1- p2 é uma cópia de p1 criada usando o método clone.</br>
##### Verificações:</br>
1- Valores Primitivos: Verifica se os valores primitivos foram copiados.</br>
2- Componentes Simples: Verifica se o componente simples foi clonado.</br>
3- Referência Circular: Verifica se a referência circular foi clonada.</br>
4- Referência ao Protótipo: Verifica se a referência circular no clone aponta para o próprio clone.</br>
