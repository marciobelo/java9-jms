# Modularização em Java

## Introdução

O JPMS (*Java Platform Module System*, antes conhecido como projeto *Jigsaw*) existe desde o Java 9.

Um módulo JPMS é um grupo de pacotes e recursos fortemente relacionados junto com um arquivo descritor de módulo.

## Descritor de módulo

- `Name`: nome do módulo. Sugestão domínio invertido igual ao pacotes.
- `Dependencies`: módulos do qual o módulo corrente depende
- `Public Packages`: lista dos pacotes que ficarão acessíveis externamento por outros pacotes
  - Todos os pacotes do módulo são, por *default*, privados ao módulo
- `Services Offered`: implementações de serviços que podem ser consumidos externamente
- `Services Consumed`: permite o módulo atual consumir um serviço externo
- `Reflection permissions`: explicitamente permite outras classes usar reflexão para acessar membros privados de um pacote

## Tipos de módulos

- `System Modules`: módulos Java SE ou JDK
- `Application Modules`: módulos da minha aplicação explicitamente definidos. Gera um jar onde internamente se encontra um module-info.class.
- `Automatic Modules`: quando adicionado ao `module path` um JAR "legado", recebe o nome do próprio JAR e pode acessar para leitura qualquer outro módulo adicionado ao `module path`.
- `Unamed Modules`: quando um JAR é carregado no `classpath`, mas não no `module path`, ele é automaticamente adicionado ao `Unnamed Module`, que é um módulo dinamicamente criado para onde vão todos os packages que não estão contidos num módulo explícito para compatibilidade retroativa.

## Distribuição

- Pode ser JAR ou expandido
- Um módulo por JAR

## Declaração

- Na raiz dos fontes colocar um arquivo especial module-info.java

```
module meumodulo {

  requires outromodulo; // dependência compile e runtime
                        // java.base é implicitamente requerido
                        // todos os pacotes públicos dentro do outromodulo são acessíveis
                        // clientes não requerem transitivamente
  requires static outromoduloopcional;  // dependência de compilação apenas
                                        // clientes deste módulo nao precisam importar também
  requires transitive outromoduloobrigatorio; // clientes importam as dependências automaticamente

  exports com.meupacote;

  export com.outropacote to outromodulo; // só outromodulo pode acessar o com.outropacote

  uses InterfaceServicoConsumido; // declara-se como consumidor de uma implementação dessa interface

  provides InterfaceServicoProvido with ServicoProvidoImpl;

}
```

## Reflexão

- Antes do java 9, nada era de fato encapsulado. 

- Por reflexão se podia acessar tudo, até membros privados

- Agora precisamos explicitamente autorizar o uso de reflexão

- Para liberar com era antes do java 9:

```
open modulo meumodulo {

}
```

- Restringir no meu modulo o que estará liberado

```
module meumodulo {

  opens pacoteliberareflexao;
}
```

- E além disso restringir para quais módulos

```
module meumodulo {

  opens pacoteliberado to modulo1, modulo2;
}
```

## Referências

- [A Guide to Java 9 Modularity](https://www.baeldung.com/java-9-modularity)

- [Multi-Module Maven Application with Java Modules](https://www.baeldung.com/maven-multi-module-project-java-jpms)