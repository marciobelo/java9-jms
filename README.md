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

## Referências

[A Guide to Java 9 Modularity](https://www.baeldung.com/java-9-modularity)