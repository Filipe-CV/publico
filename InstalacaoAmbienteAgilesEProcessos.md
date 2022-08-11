## Instalações

- <a id="r">JDK</a>
  - v. 1.6 - para o projeto  
    > deverá ser [configurado](#a) no eclipse para o build.

  - v. 1.8 (64bit) - para \[abrir\] o eclipse compartilhado (v. Neon.3 Release (4.6.3)) 
    > O eclipse compartilhado é para arquitetura 64bit o que requer um JDK para a mesma arquitetura para ser usado.

- Criação da partição D:\  
  ```
     Um caminho para criar a partição:  
       1. Diminiuir o volume (C:\)  
          https://answers.microsoft.com/pt-br/windows/forum/all/gerenciamento-de-disco-diminuir-volume/20fd5e3a-cf7e-486a-81ab-1fb0903347a6  
          
       2. "Para criar e formatar uma nova partição (volume)"  
          https://support.microsoft.com/pt-br/windows/criar-e-formatar-uma-parti%C3%A7%C3%A3o-de-disco-r%C3%ADgido-bbb8e185-1bda-ecd1-3465-c9728f7d7d2e
         
   ```  
   
  > Devido o fato em que nos arquivos de configuração do projeto e fontes estão apontando para este diretório, para minimizar a necessidade de mais modificações, é interessante deixar o material em D:\\.   

- Tomcat v. 6.0 (consta no material)  
- Maven 3.0.1 (consta no material)
- Banco de dados PostgreSQL 9.6 (instalador consta no material)  
- Firefox 51.0.1
  > Para vizualização dos desenhos de processo através de execução de applet  
  -  No navegador  
  ```
   (url:) about:preferences#advanced > Atualizações do Firefox > (x) Nunca verificar
  ```  
  - Permitir que a VM Java execute os applets 
  ```
   Painel de Controle > Java > aba 'Segurança' > Lista de Exceções de Sites > Editar lista de sites... > Lista de 
   adicionar os sites https://bpm.recife.pe.gov.br/ e https://bpmh.recife.pe.gov.br/
  ```  

<!-- - DBeaver -->
 
---
## importações

- do Projeto
  - Pelo controle de versão Subrversion (SVN)
    - Requisita usuário para o projeto <http://versao.recife/svn/bpm-pcr-urbanistico>
      > É um servidor local, acredito que é preciso acesso por vpn.  

      > Usar a versão da pasta Trunk.

- do Banco de dados - backup \["do banco de homologação" ~13GB\]
    esquemas  `-` e `-`
    
        C:\"Program Files"\PostgreSQL\9.6\bin\>psql -U postgres -f "caminho"\"arqBackup".sql  
        
   <!-- Em notas.txt ln 1636 há o código SQL para criação da tabela blobdata & criação do schema agiles -->


---
## Configurações

- Eclipse
  - <a id="a">jdk 1.6</a> &nbsp; [:arrow_up:](#r)
    ```
     Eclipse, aba Window > Perferences > Java > Installed JREs > ... :
      jdk1.6.0_24
    ```  

- Tomcat
  - usar JDK 1.6.0_24
    ```
     Eclipse, aba Window > Perferences > Server > Runtime Environment > ... > JRE: jdk 1.6.0_xx
    ```  
  - argumentos:  

    ```
     Show view (aba), Servers 'duplo click' > Open launch configurations > Arguments > VM arguments:  
     
     -Dcatalina.base="D:\desenvolvimento\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0"
     -Dcatalina.home="D:\desenvolvimento\apache-tomcat-6.0.26" 
     -Dwtp.deploy="D:\desenvolvimento\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps"
     -Djava.endorsed.dirs="D:\desenvolvimento\apache-tomcat-6.0.26\endorsed"
     -Dagiles.dir="D:\desenvolvimento\AgilesConfigs\jboss\config"
     -Djava.net.preferIPv4Stack=true
     -Xms512M -Xmx512M -XX:PermSize=256m -XX:MaxPermSize=512m
    ```
    
- Maven
  > Usar a pasta .m2 do material compartilhado como repositório local  
   ` Eclipse, aba Window > Perferences > Maven > user settings > local repository `

- Banco de Dados  
  - Banco `-`  
    - Esquema `-`  

  - Banco `-`  
    - Esquema `-`
      - com usuário `-` e password `-` para Create e Delete  

  - Em \[ C:\Program Files\ \]\PostgreSQL\9.6\datsa\postgresql.conf > linha 546:  
      de:  ...  
      para: `byte_output = 'escape'  # hex, escape`  
       > Essa configuração é para mostrar o desenho dos processos.

- (Re)configurar:
  - *persistence.xml*  
     (linha 4):
      ```xml

         <non-jta-data-source>java:${pcr-urbanistico.data-source-prefix}${pcr-urbanistico.data-source-name}</non-jta-data-source>
         <!-- <non-jta-data-source>java:${pcr-urbanistico.data-source-name}</non-jta-data-source> -->

      ```  
      <!-- O com os valores, descorbetos por Charles, ao invés de variáveis consta em notas.txt data 10/08/22 -->
  - context.xml  <!-- Servers -->  
     (linha 37 a 40):
      ```xml

         url="jdbc:postgresql://127.0.0.1:5432/<nomeDoBanco2>"
         username="-"
         password="-"

      ```  
      Ou (acrescentar ao final):
      ```xml

         <Resource name="jdbc/<nomeDoBanco>-ds"
              auth="Container"
              type="javax.sql.DataSource"
              driverClassName="org.postgresql.Driver"
              url="jdbc:postgresql://127.0.0.1:5432/<nomeDoBanco>"
              username="-"
              password="-"
              maxActive="20" />

      ```  
      <!-- no do wtzp está como url="jdbc:postgresql://127.0.0.1:5432/<nomeDoBanco2>" -->
  <!-- - Server.xml -->

- Licença do Ágiles: `Agileslicense.xml`
  >  É preciso o cadastramento da placa de rede para a licença do uso da ferramenta Ágiles.  
  - Solicitação por email informando o MAC \[para Tarcísio\]  
    ```
      C:\WINDOWS\system32>ipconfig /all
      [...]
      Adaptador Ethernet Ethernet:
         [...]
         Endereço Físico.....: XX-XX-XX-XX-XX-XX
    ```  
  - Substituir `Agileslicense.xml` que consta em `...\desenvovimento\AgilesConfig\jboss\config` pelo o solicitado.
    
---
## Levantar projeto

- Materialização do projeto  
  `botãoD no projeto [pai] > Import... > Maven/Existing Maven Project ...`  
  
    > criará a estrutura dos outros projetos (controller, function, model, util e view).  
- mvn clean install  
- acesso a página
   `http://127.0.0.1:8080/pcr-urbanistico/externo/solicitacaologin/solicitacao-login.action`
