# crud_java_jpa_hibernate_mysql

Pequena introdução: 

Um problema de produtividade começou a ser notado no desenvolvimento de aplicações Web Java. Os desenvolvedores perceberam que a maior parte do tempo era gasto com queries SQL através do JDBC. Um outro problema percebido era a mudança de paradigma. A programação orientada a objetos (ex: Java) é diferente do esquema entidade relacional (ex: SGBDs tradiconais), sendo necessário esquematizar dois modelos para um mesmo sistema. Como solução para esses dois problemas, foi proposto um modelo de mapeamento chamado Mapeamento Objeto Relacional (conhecido como ORM) para representar tabelas de um banco de dados relacional através de classes Java.

Exemplos de mapeamentos: 

Tabela <---> Classe
Coluna <---> Atributo
Registro <---> Objeto

Para padronizar as interfaces das implementações ORM (Mapeamento Objeto Relacional) foi criada uma ESPECIFICAÇÃO oficial chamada JPA (ou Java Persistence API). Ela descreve como deve ser o comportamento dos frameworks Java ORM que desejarem IMPLEMENTAR a sua especificação. Logo SOMENTE com a ESPECIFICAÇÃO JPA NÃO será possível executar operações entre a aplicação e o banco de dados. Apesar de ser SOMENTE a especificação, o JPA possui algumas classes, interfaces e anotações que ajudam o desenvolvedor a abstrair o código.

Esses artefatos estão presentes no pacote javax.persistence que ajudam a manter o código independente da implementação utilizada. Lembrando que para persistir dados com JPA, é preciso escolher uma implementação que irá executar todo o trabalho

Entre os principais artefatos do JPA, podem ser destacados: Anotação @Entity – Indica a aplicação que os OBJETOS da classe especificada serão persistidos no banco de dados. Também podem ser utilizadas outras anotações para auxiliar no mapeamento da classe, tais como: @id, @column, @table, @OneToMany e @ManyToOne.

Interface EntityManager – É utilizada para gerenciar o ciclo de vida das entidades. Os principais métodos utilizados são find, persist e remove. As principais anotações utilizadas junto com a annotation @Entity são: @Table – É uma annotation opcional. Por padrão o NOME da entidade é usado para realizar o mapeamento com o nome da TABELA do banco de dados. Essa annotation será necessária caso o nome da entidade seja diferente do nome da tabela no banco de dados. @Column – É uma annotation opcional. Por padrão o ATRIBUTO da entidade é usado para realizar o mapeamento com o nome da COLUNA do banco de dados. Essa annotation será necessária caso os atributos da entidade sejam diferentes das colunas do banco de dados. @Id – É OBRIGATÓRIO especificar ao menos uma ID para a entidade.

Também existem as annotations de relacionamento que são utilizadas para representar os relacionamentos entre TABELAS do banco de dados (através das chaves estrangeiras no banco de dados) em uma aplicação que esteja utilizando o JPA. As principais annotations são @ManyToMany, @ManyToOne, @OneToMany e @OneToOne.

Na aplicação utilizando JPA, é possível realizar relacionamento unidirecionais e bidirecionais. No unidirecional é possível chegar de uma instância A para uma instância B facilmente, porém o caminho contrário é dificultado. Na bidirecional, tanto do A para o B, quanto do B para o A o acesso é facilitado.

Nas annotations de relacionamento, a propriedade “fetch” exige atenção especial do desenvolvedor. Seus possíveis valores são eager (ansioso) ou lazy (preguiçoso). Suas características são: Eager – A entidade mapeada com esse atributo SEMPRE será carregada na aplicação quando a entidade que está MAPEANDO for consultada, mesmo que nunca seja utilizada durante a execução da aplicação. Lazy – A entidade mapeada com esse atributo SOMENTE será carregada na aplicação quando esta for EXPLICITAMENTE consultada pela entidade que está mapeando (É o mais aconselhável de usar caso não se saiba, em um primeiro momento, o real número de frequência de consultas).

Para persistir dados com as entidades mapeadas, é OBRIGATÓRIO iniciar uma transação. Para manipular transações, é necessário utilizar o seguinte método do entityManager: getTransaction – Retorna uma EntityTransaction, sendo obrigatório o seu uso quando utilizar algum método que realize alterações no banco de dados. Pode utilizar os seguintes métodos:

begin – Inicia uma transação;
commit – Finaliza uma transação, persistindo todos os dados que foram modificados desde o inicio da transação;
rollback – Finaliza uma transação, revertendo todos os dados que foram modificados desde o inicio da transação.

Os principais métodos do entityManager para interagir com as entidades são:

find – Retorna a entidade que está persistida no banco de dados através da sua chave primária;
persist – Persiste a entidade no banco de dados (É necessário ter iniciado uma transação);
remove – Apaga a entidade do banco de dados (É necessário ter iniciado uma transação).

Para configurar uma aplicação JAVA para interagir com o banco de dados usando as especificações do JPA, será necessário configurar o arquivo persistence.xml. Nele é possível especificar qual framework de implementação será utilizado. Eu utilizei nesse projeto o HIBERNATE.

______Passos para utilizar o JPA na sua aplicação:____________

1 - Realizar download do Java Persistence API (JPA) e do driver JDBC para o BD MySQL. É possível baixar manualmente ou através do Gradle ou Maven https://mvnrepository.com/artifact/javax.persistence/javax.persistence-api
https://mvnrepository.com/artifact/mysql/mysql-connector-java

2 - Criar o arquivo persistence.xml e configurar os seguintes parâmetros: URL da string de conexão (driver, endereço do BD e nome do BD), usuário do BD, senha do BD, driver e classes que serão mapeadas para serem usadas pelo JPA.

3 - Utilizar as annotations nas classes que serão mapeadas para uso do Hibernate.

4 - Configurar o entityManager
    
 MINHAS TABELA NO BANCO digital_innovation_one
================================================

1 - Acessar banco de dados. Pode ser workbench ou linha de comando (conforme o comando abaixo)
mysql -u root -p
(Enter password:) password

2 - (CASO NÂO ESTEJA NO BANCO DE DADOS) Mudar para o banco digital_innovation_one (rodar no prompt do MySQL OU no MySQL workbench)
USE digital_innovation_one;

3 - Criar uma tabela no banco de dados (rodar no prompt do MySQL OU no MySQL workbench)

CREATE TABLE aluno (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
     idade INTEGER NOT NULL,
     nome VARCHAR(80) NOT NULL,
     estado_id integer,
     constraint fkEstAluno foreign key (id) references estado (id)
    
CREATE TABLE estado (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL,
    sigla INTEGER NOT NULL,
);
