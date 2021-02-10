

2. **API da Bolsa de Documentos**
    1. **Descrição**

    A invocação dos webservices deve ser assíncrona, sendo necessário a implementação de webservices no lado do cliente para a receção de callbacks.


    Na mensagem de invocação deve estar especificado o endpoint dos webservices de receção de callbacks no campo “replyTo”. O campo “relatesTo” virá preenchido na resposta do webservice, o qual irá conter o messageID para a correlação de pedidos.:






    2. **Estrutura Fixa**

Existem 2 componentes fixas que fazem parte de todas as mensagens:

	-Parâmetros de Entrada

	-Parâmetros de Saída



        1. **Parâmetros de Entrada – UserAuthentication**

A estrutura contém dados referentes à mensagem:


<table>
  <tr>
   <td>Elemento
   </td>
   <td><strong>UserAuthentication</strong>
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Descrição
   </td>
   <td>Elemento que identifica o utilizador que está a realizar o pedido.
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Observações
   </td>
   <td>Estrutura de preenchimento obrigatório
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Tipo
   </td>
   <td>ComplexType
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Elemento
   </td>
   <td colspan="2" >Descrição
   </td>
   <td>Obrigatório
   </td>
  </tr>
  <tr>
   <td>EntityNif
   </td>
   <td colspan="2" >NIF da Entidade.
   </td>
   <td>Sim
   </td>
  </tr>
  <tr>
   <td>UserName
   </td>
   <td colspan="2" >Nome de utilizador atribuído à Entidade.
   </td>
   <td>Sim
   </td>
  </tr>
  <tr>
   <td>Password
   </td>
   <td colspan="2" >Palavra-passe associado ao nome de utilizador da Entidade.
   </td>
   <td>Sim
   </td>
  </tr>
</table>


**Exemplo:**


```
    <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>

```



        2. **Parâmetros de Saída - Status**

A estrutura contém dados referentes ao pedido:


<table>
  <tr>
   <td>Elemento
   </td>
   <td><strong>Status</strong>
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Descrição
   </td>
   <td>Elemento que caracteriza o resultado da realização do serviço.
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Observações
   </td>
   <td>Estrutura de preenchimento obrigatório
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Tipo
   </td>
   <td>ComplexType
   </td>
   <td colspan="2" >
   </td>
  </tr>
  <tr>
   <td>Elemento
   </td>
   <td colspan="2" >Descrição
   </td>
   <td>Obrigatório
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td colspan="2" ><em>String</em>
   </td>
   <td>Sim
   </td>
  </tr>
  <tr>
   <td>Description
   </td>
   <td colspan="2" ><em>String</em>
   </td>
   <td>Sim
   </td>
  </tr>
</table>


**Exemplo**:


```
    <Status>
               <Code>FAILED</Code>
               <Description>Request execution failed.</Description>
            </Status>

```



        3. **Operações**

A lista de operações permitidas neste serviço são as seguintes:

 



            1. **AddDocument**

Esta operação tem como objetivo adicionar um documento à área reservada da entidade.

Na mensagem do pedido deve incluir o _payload_ especificado no cap. “2.4. Integração com LargeFiles”, devendo a entidade efetuar o _upload_ do ficheiro do documento para o LargeFiles antes da invocação desta operação.

O seguinte diagrama representa o fluxo de invocação do _webservice_:



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


**Exemplo:**

  `&lt;addDocumentInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DocumentToAdd>
               <Filename>?</Filename>
               <MimeType>?</MimeType>
               <!--Optional:-->
               <FolderPath>?</FolderPath>
               <ValidityDate>?</ValidityDate>
               <Title>?</Title>
               <ClassificationCode>?</ClassificationCode>
               <Subject>?</Subject>
               <Language>?</Language>
               <Tags>?</Tags>
               <CollaboratorId>?</CollaboratorId>
               <CollaboratorDesignation>?</CollaboratorDesignation>
               <CollaboratorType>?</CollaboratorType>
               <CreationDate>?</CreationDate>
               <ResourceType>?</ResourceType>
               <Country>?</Country>
               <SecurityClassification>?</SecurityClassification>
               <Price>?</Price>
               <ConservationTime>?</ConservationTime>
            </DocumentToAdd>
            <lar:AttachContext>
               <TTL>30</TTL>
               <FileGuid>?</FileGuid>
            </lar:AttachContext>
         </addDocumentInput>

```



            2. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Descrição
   </td>
   <td><strong>Adicionar Documentos</strong>
   </td>
  </tr>
  <tr>
   <td>Observações
   </td>
   <td> 
   </td>
  </tr>
  <tr>
   <td>Tipo
   </td>
   <td><strong>DocumentToAdd</strong>
   </td>
  </tr>
</table>



<table>
  <tr>
   <td>Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>Filename</strong>
   </td>
   <td>Nome do ficheiro em formato &lt;nome>.&lt;extensão>.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>MimeType</strong>
   </td>
   <td>Formato MIME (exemplo: “application/pdf”).
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPath</strong>
   </td>
   <td>Diretoria dentro da área reservada para onde o documento deve ser armazenado (exemplo: “certificacoes/cidadaos/12345678”).
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ValidityDate</strong>
   </td>
   <td>Data de validade do documento, conforme indicado pelo Produtor do mesmo. Se não for definida na inserção o documento é considerado vitalício.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Title</strong>
   </td>
   <td>Título do documento. Se não for definido na inserção passa a ser igual ao valor do Filename.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ClassificationCode</strong>
   </td>
   <td>Classificação do recurso atribuída com base numa estrutura classificativa. Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOC” (Documento).
   </td>
   <td>Não
   </td>
   <td>Ver tabela
<p>
ClassificationCode
   </td>
  </tr>
  <tr>
   <td><strong>Subject</strong>
   </td>
   <td>Assunto relacionado com o documento. Se não for definido na inserção passa a ser igual ao valor do Filename.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Language</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Português).
   </td>
   <td>Não
   </td>
   <td>Ver tabela Language
   </td>
  </tr>
  <tr>
   <td><strong>Tags</strong>
   </td>
   <td>Tags a associar a um documento para facilitar a pesquisa e a categorização do mesmo.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorId</strong>
   </td>
   <td>NIF da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorDesignation</strong>
   </td>
   <td>Nome da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver Tabela CollaboratorType
   </td>
  </tr>
  <tr>
   <td><strong>CreationDate</strong>
   </td>
   <td>Data de criação do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>ResourceType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOCPERSONAL” (Documento com informação pessoal).
   </td>
   <td>Não
   </td>
   <td>Ver Tabela ResourceType
   </td>
  </tr>
  <tr>
   <td><strong>Country</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Portugal).
   </td>
   <td>Não
   </td>
   <td>PT (Portugal)
   </td>
  </tr>
  <tr>
   <td><strong>SecurityClassification</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “RESERVED” (Reservado).
   </td>
   <td>Não
   </td>
   <td>Ver tabela SecurityClassification
   </td>
  </tr>
  <tr>
   <td><strong>Price</strong>
   </td>
   <td>Preço associado ao documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>ConservationTime</strong>
   </td>
   <td>Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma. Se não for definida na inserção é considerado o valor “5”.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
</table>




            3. **Parâmetros de saída**

    Na resposta é devolvido o ID atribuído ao documento na Bolsa de Documentos.






            4. **GetDocument**

Esta operação tem como objetivo retribuir a informação associada ao documento.



            5. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>DocumentId</strong>
   </td>
   <td>ID do documento atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
         <getDocumentInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DocumentId>?</DocumentId>
         </getDocumentInput>

```



            6. **Parâmetros de saída**

Na resposta desta operação é devolvida informação associada ao documento estruturada em conformidade com especificações MIP (Meta informação para interoperabilidade).


    



            7. **GetDocumentList**

Esta operação tem como objetivo retribuir a informação associada a um ou mais documentos presentes na área reservada da Entidade. Pode servir como pesquisa de documentos onde os valores dos parâmetros de entrada funcionam como filtro caso forem definidos. Caso nenhum parâmetro venha preenchido, na resposta é devolvida todos os documentos da área reservada da entidade.





                1. **Parâmetros de entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>MimeType</strong>
   </td>
   <td>Formato MIME (exemplo: “application/pdf”).
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPath</strong>
   </td>
   <td>Diretoria dentro da área reservada para onde os documentos podem estar armazenados (exemplo: “certificacoes/cidadaos/12345678”).
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ValidityDate</strong>
   </td>
   <td>Data de validade dos documentos, conforme indicado pelo Produtor do mesmo.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Title</strong>
   </td>
   <td>Título do documento.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ClassificationCode</strong>
   </td>
   <td>Classificação do recurso atribuída com base numa estrutura classificativa. Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela ClassificationCode
   </td>
  </tr>
  <tr>
   <td><strong>ProducerDesignation</strong>
   </td>
   <td>Designação do produtor do documento.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Subject</strong>
   </td>
   <td>Assunto relacionado com o documento.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Language</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela Language
   </td>
  </tr>
  <tr>
   <td><strong>EditorId</strong>
   </td>
   <td>NIF, NIC, NOA, NCS ou NON da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>EditorDesignation</strong>
   </td>
   <td>Nome da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>EditorType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver Tabela EditorType
   </td>
  </tr>
  <tr>
   <td><strong>Tags</strong>
   </td>
   <td>Tags associadas ao documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorId</strong>
   </td>
   <td>NIF da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorDesignation</strong>
   </td>
   <td>Nome da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver Tabela CollaboratorType
   </td>
  </tr>
  <tr>
   <td><strong>CreationDate</strong>
   </td>
   <td>Data de criação do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>ResourceType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela ResourceType
   </td>
  </tr>
  <tr>
   <td><strong>DataFormat</strong>
   </td>
   <td>Formato/extensão do ficheiro, como por exemplo “PDF”, “PNG”, entre outros.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>Country</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>PT (Portugal)
   </td>
  </tr>
  <tr>
   <td><strong>SecurityClassification</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela SecurityClassification
   </td>
  </tr>
  <tr>
   <td><strong>Price</strong>
   </td>
   <td>Preço associado ao documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>ConservationTime</strong>
   </td>
   <td>Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
</table>


**Exemplo**:

          `&lt;DocumentSearchParameters>`


```
               <MimeType>?</MimeType>
               <FolderPath>?</FolderPath>
               <ValidityDate>?</ValidityDate>
               <Title>?</Title>
               <ClassificationCode>?</ClassificationCode>
               <ProducerDesignation>?</ProducerDesignation>
               <Subject>?</Subject>
               <Language>?</Language>
               <EditorId>?</EditorId>
               <EditorDesignation>?</EditorDesignation>
               <EditorType>?</EditorType>
               <Tags>?</Tags>
               <CollaboratorId>?</CollaboratorId>
            <CollaboratorDesignation>?</CollaboratorDesignation>
               <CollaboratorType>?</CollaboratorType>
               <CreationDate>?</CreationDate>
               <ResourceType>?</ResourceType>
               <DataFormat>?</DataFormat>
               <Country>?</Country>
              <SecurityClassification>?</SecurityClassification>
               <Price>?</Price>
               <ConservationTime>?</ConservationTime>
            </DocumentSearchParameters>

```



                2. **Parâmetros de Saída**

Na resposta desta operação é devolvida informação associada a um ou mais documentos estruturados em conformidade com especificações MIP (Meta informação para interoperabilidade).





            8. **GetDocumentFile**

Esta operação tem como objetivo retribuir o ficheiro de um documento.

Na mensagem de resposta do pedido encontra-se o _payload_ especificado na integração com LargeFiles”, devendo a entidade efetuar o _download_ do ficheiro do documento do LargeFiles após a receção da resposta.

O seguinte diagrama representa o fluxo de invocação da operação:



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")






                3. **Parâmetros de entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>DocumentId</strong>
   </td>
   <td>ID do documento atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
     <getDocumentFileInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DocumentId>?</DocumentId>
         </getDocumentFileInput>

```



                4. **Parâmetros de saída**

Na resposta da operação é incluído o payload com o GUID associado ao ficheiro. Esse GUID deve ser usado para o download do ficheiro disponibilizado no LargeFiles.





            9. **AddDocumentLink**

Esta operação tem como objetivo adicionar um _link_ à área reservada da entidade.



                5. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>LinkUrl</strong>
   </td>
   <td>Hiperligação para onde o link redirecionará quando for selecionado.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPath</strong>
   </td>
   <td>Diretoria dentro da área reservada onde os <em>links</em> podem estar armazenados (exemplo: “<em>links</em>/cidadaos/12345678”).
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ValidityDate</strong>
   </td>
   <td>Data de validade dos documentos, conforme indicado pelo Produtor do mesmo.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Title</strong>
   </td>
   <td>Título do <em>link</em>.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorId</strong>
   </td>
   <td>NIF da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorDesignation</strong>
   </td>
   <td>Nome da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela 
<p>
CollaboratorType
   </td>
  </tr>
  <tr>
   <td><strong>CreationDate</strong>
   </td>
   <td>Data de criação do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>ResourceType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela ResourceType
   </td>
  </tr>
  <tr>
   <td><strong>SecurityClassification</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela SecurityClassification
   </td>
  </tr>
  <tr>
   <td><strong>ConservationTime</strong>
   </td>
   <td>Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
</table>


**Exemplo:**


```
<addDocumentLinkInput>
            <UserAuthentication>
     <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DocumentLinkToAdd>
               <LinkUrl>?</LinkUrl>
               <FolderPath>?</FolderPath>
               <ValidityDate>?</ValidityDate>
               <Title>?</Title>
               <CollaboratorId>?</CollaboratorId>
               <CollaboratorDesignation>?</CollaboratorDesignation>
               <CollaboratorType>?</CollaboratorType>
               <CreationDate>?</CreationDate>
               <ResourceType>?</ResourceType>
               <SecurityClassification>?</SecurityClassification>
               <ConservationTime>?</ConservationTime>
            </DocumentLinkToAdd>
         </addDocumentLinkInput>

```



                6. **Parâmetros de Saída**

Na resposta da operação é indicado o identificador interno atribuído ao _link_ que foi criado.





            10. **GetDocumentLink**

Esta operação tem como objetivo retribuir a informação associada ao _link_.



                7. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>DocumentId
   </td>
   <td>ID do link que se pretende obter, atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td>
   </td>
  </tr>
</table>


**Exemplo:**


```
       <getDocumentLinkInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DocumentId>?</DocumentId>
         </getDocumentLinkInput>

```



                8. **Parâmetros de Saída**

Na resposta da operação é devolvida informação associada ao _link_ estruturada em conformidade com especificações MIP (Meta informação para interoperabilidade), com a devida adaptação para utilização nos _links_.





            11. **GetDocumentLinkList**

Esta operação tem como objetivo retribuir a informação associada a um ou mais _links_ presentes na área reservada da Entidade. Pode servir como pesquisa de _links_ onde os valores dos parâmetros de entrada funcionam como filtro caso forem definidos. Caso nenhum parâmetro venha preenchido, na resposta é devolvida todos os documentos da área reservada da entidade.



                9. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>FolderPath</strong>
   </td>
   <td>Diretoria dentro da área reservada para onde os documentos podem estar armazenados (exemplo: “certificacoes/cidadaos/12345678”).
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ValidityDate</strong>
   </td>
   <td>Data de validade dos documentos, conforme indicado pelo Produtor do mesmo.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Title</strong>
   </td>
   <td>Título do documento.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ProducerDesignation</strong>
   </td>
   <td>Designação do produtor do documento.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EditorId</strong>
   </td>
   <td>NIF, NIC, NOA, NCS ou NON da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>EditorDesignation</strong>
   </td>
   <td>Nome da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>EditorType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela EditorType
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorId</strong>
   </td>
   <td>NIF da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorDesignation</strong>
   </td>
   <td>Nome da Entidade colaboradora na produção do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>CollaboratorType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela CollaboratorType
   </td>
  </tr>
  <tr>
   <td><strong>CreationDate</strong>
   </td>
   <td>Data de criação do documento.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>ResourceType</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela ResourceType
   </td>
  </tr>
  <tr>
   <td><strong>DataFormat</strong>
   </td>
   <td>Formato/extensão do ficheiro, como por exemplo “PDF”, “PNG”, entre outros.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td><strong>SecurityClassification</strong>
   </td>
   <td>Pode ser definido com uma das chaves na lista de valores.
   </td>
   <td>Não
   </td>
   <td>Ver tabela SecurityClassification
   </td>
  </tr>
  <tr>
   <td><strong>ConservationTime</strong>
   </td>
   <td>Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
</table>




**Exemplo:**


```
  <DocumentLinkSearchParameters>
               <FolderPath>?</FolderPath>
               <ValidityDate>?</ValidityDate>
               <Title>?</Title>
               <ProducerDesignation>?</ProducerDesignation>
               <EditorId>?</EditorId>
               <EditorDesignation>?</EditorDesignation>
               <EditorType>?</EditorType>
               <CollaboratorId>?</CollaboratorId>
            <CollaboratorDesignation>?</CollaboratorDesignation>
               <CollaboratorType>?</CollaboratorType>
               <CreationDate>?</CreationDate>
               <ResourceType>?</ResourceType>
               <DataFormat>?</DataFormat>
              <SecurityClassification>?</SecurityClassification>
               <ConservationTime>?</ConservationTime>
            </DocumentLinkSearchParameters>
         </getDocumentLinkListInput>

```



                10. **Parâmetros de Saída**

Na resposta é devolvida informação associada a um ou mais _links_ estruturados em conformidade com especificações MIP (Meta informação para interoperabilidade), com a devida adaptação para utilização nos _links_





            12. **SendDocumentToCitizen**

Esta operação tem como objetivo efetuar o envio de documentos ou _links_ presentes na área reservada da Entidade para um Cidadão. O Cidadão conseguirá visualizar e aceitar o envio na sua área de documentos.





                11. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>CitizenNic</strong>
   </td>
   <td>NIC do Cidadão recetor do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CitizenName</strong>
   </td>
   <td>Nome do Cidadão recetor do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CitizenEmail</strong>
   </td>
   <td>Email do Cidadão recetor do envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados ao envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
    <sendDocumentToCitizenInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <CitizenNic>?</CitizenNic>
            <CitizenName>?</CitizenName>
 		<CitizenEmail>?</CitizenEmail>
           <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <Comments>?</Comments>
         </sendDocumentToCitizenInput>

```



                12. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            13. **SendDocumentToEntity**

Esta operação tem como objetivo efetuar o envio de documentos ou _links_ presentes na área reservada da Entidade para uma outra Entidade. A Entidade recetora conseguirá visualizar e aceitar o envio na sua área reservada.



                13. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>EntityNif</strong>
   </td>
   <td>NIF da Entidade recetora do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EntityName</strong>
   </td>
   <td>Nome da Entidade recetora do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EntityEmail</strong>
   </td>
   <td>Email da Entidade recetora do envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados ao envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
    <sendDocumentToEntityInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <EntityNif>?</EntityNif>
            <EntityName>?</EntityName>
            <EntityEmail>?</EntityEmail>
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <Comments>?</Comments>
         </sendDocumentToEntityInput>

```



                14. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            14. **SendDocumentToLawyer**

Esta operação tem como objetivo efetuar o envio de documentos ou _links_ presentes na área reservada da Entidade para um Advogado. O Advogado conseguirá visualizar e aceitar o envio na sua área de documentos.



                15. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>LawyerNoa
   </td>
   <td>NOA do Advogado recetor do envio.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>LawyerName
   </td>
   <td>Nome do Advogado recetor do envio.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>LawyerEmail
   </td>
   <td>Email do Advogado recetor do envio.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>DocumentIdList
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>Process
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>Comments
   </td>
   <td>Comentários associados ao envio.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
</table>


**Exemplo:**

**		`&lt;sendDocumentToLawyerInput>`**


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <LawyerNoa>?</LawyerNoa>
            <LawyerName>?</LawyerName>
            <LawyerEmail>?</LawyerEmail>
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <Comments>?</Comments>
         </sendDocumentToLawyerInput>

```



                16. **Parâmetros de Entrada**

Na resposta é indicado o estado da execução do pedido.





            15. **SendDocumentToNotary**

Esta operação tem como objetivo efetuar o envio de documentos ou _links_ presentes na área reservada da Entidade para um Notário. O Notário conseguirá visualizar e aceitar o envio na sua área de documentos.



                17. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>NotaryNon</strong>
   </td>
   <td>NON do Notário recetor do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>NotaryName</strong>
   </td>
   <td>Nome do Notário recetor do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>NotaryEmail</strong>
   </td>
   <td>Email do Notário recetor do envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados ao envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
         <sendDocumentToNotaryInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <NotaryNon>?</NotaryNon>
            <NotaryName>?</NotaryName>
            <NotaryEmail>?</NotaryEmail>
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <Comments>?</Comments>
         </sendDocumentToNotaryInput>

```



                18. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            16. **SendDocumentToSolicitor**

Esta operação t como objetivo efetuar o envio de documentos ou _links_ presentes na área reservada da Entidade para um Solicitador. O Solicitador conseguirá visualizar e aceitar o envio na sua área de documentos.



                19. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorNcs</strong>
   </td>
   <td>NCS do Solicitador recetor do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorName</strong>
   </td>
   <td>Nome do Solicitador recetor do envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorEmail</strong>
   </td>
   <td>Email do Solicitador recetor do envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados ao envio.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**

	`&lt;sendDocumentToSolicitorInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <SolicitorNcs>?</SolicitorNcs>
            <SolicitorName>?</SolicitorName>
            <SolicitorEmail>?</SolicitorEmail>
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>            
            <Comments>?</Comments>
         </sendDocumentToSolicitorInput

```



                20. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            17. **ShareDocumentWithCitizen**

Esta operação tem como objetivo efetuar a partilha de documentos ou _links_ presentes na área reservada da Entidade com um Cidadão. O Cidadão conseguirá visualizar e aceitar a partilha na sua área de documentos.



                21. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>CitizenNic</strong>
   </td>
   <td>NIC do Cidadão recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CitizenName</strong>
   </td>
   <td>Nome do Cidadão recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CitizenEmail</strong>
   </td>
   <td>Email do Cidadão recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a partilhar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>hasWritePermission</strong>
   </td>
   <td>Indica se a partilha é de leitura e escrita ou apenas leitura.
   </td>
   <td>Sim
   </td>
   <td>Ver tabela Write Permissions
   </td>
  </tr>
</table>


**Exemplo:**

	`&lt;shareDocumentWithCitizenInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <CitizenNic>?</CitizenNic>
            <CitizenName>?</CitizenName>
            <!--Optional:-->
            <CitizenEmail>?</CitizenEmail>
            <!--1 or more repetitions:-->
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <!--Optional:-->
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
            <HasWritePermission>?</HasWritePermission>
         </shareDocumentWithCitizenInput>

```



                22. **Parâmetros de Saída**

Na resposta do _webservice_ é indicado o estado da execução do pedido.





            18. **ShareDocumentWithEntity**

Esta _operação_ tem como objetivo efetuar a partilha de documentos ou _links_ presentes na área reservada da Entidade com uma outra Entidade. A Entidade recetora conseguirá visualizar e aceitar a partilha na sua área reservada.



                23. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>EntityNif</strong>
   </td>
   <td>NIF da Entidade recetora da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EntityName</strong>
   </td>
   <td>Nome da Entidade recetora da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EntityEmail</strong>
   </td>
   <td>Email da Entidade recetora da partlha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a partilhar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>hasWritePermission</strong>
   </td>
   <td>Indica se a partilha é de leitura e escrita ou apenas leitura.
   </td>
   <td>Sim
   </td>
   <td>Ver tabela Write Permissions
   </td>
  </tr>
</table>


**Exemplo:**


```
<shareDocumentWithEntityInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <EntityNif>?</EntityNif>
            <EntityName>?</EntityName>
            <!--Optional:-->
            <EntityEmail>?</EntityEmail>
            <!--1 or more repetitions:-->
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <!--Optional:-->
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
            <HasWritePermission>?</HasWritePermission>
         </shareDocumentWithEntityInput>

```



                24. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            19. **ShareDocumentWithLawyer**

Esta operação tem como objetivo efetuar a partilha de documentos ou _links_ presentes na área reservada da Entidade com um Advogado. O Advogado conseguirá visualizar e aceitar a partilha na sua área de documentos.



                25. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>LawyerNoa</strong>
   </td>
   <td>NOA do Advogado recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>LawyerName</strong>
   </td>
   <td>Nome do Advogado recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>LawyerEmail</strong>
   </td>
   <td>Email do Advogado recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a partilhar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>hasWritePermission</strong>
   </td>
   <td>Indica se a partilha é de leitura e escrita ou apenas leitura.
   </td>
   <td>Sim
   </td>
   <td>Ver tabela Write Permissions
   </td>
  </tr>
</table>


**Exemplo:**


```
	 <shareDocumentWithLawyerInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <LawyerNoa>?</LawyerNoa>
            <LawyerName>?</LawyerName>
            <LawyerEmail>?</LawyerEmail>
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
            <HasWritePermission>?</HasWritePermission>
         </shareDocumentWithLawyerInput>

```



                26. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            20. **ShareDocumentWithNotary**

Esta operação tem como objetivo efetuar a partilha de documentos ou _links_ presentes na área reservada da Entidade com um Notário. O Notário conseguirá visualizar e aceitar a partilha na sua área de documentos.



                27. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>NotaryNon</strong>
   </td>
   <td>NON do Notário recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>NotaryName</strong>
   </td>
   <td>Nome do Notário recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>NotaryEmail</strong>
   </td>
   <td>Email do Notário recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a partilhar.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>hasWritePermission</strong>
   </td>
   <td>Indica se a partilha é de leitura e escrita ou apenas leitura.
   </td>
   <td>Sim
   </td>
   <td>Ver tabela Write Permissions
   </td>
  </tr>
</table>


**Exemplo:**

**	`&lt;shareDocumentWithNotaryInput>`**


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <NotaryNon>?</NotaryNon>
            <NotaryName>?</NotaryName>
            <NotaryEmail>?</NotaryEmail>
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
            <HasWritePermission>?</HasWritePermission>
         </shareDocumentWithNotaryInput>

```



                28. **Parâmetros de Saída**

Na resposta do é indicado o estado da execução do pedido.





            21. **ShareDocumentWithSolicitor**

Esta operação tem como objetivo efetuar a partilha de documentos ou _links_ presentes na área reservada da Entidade com um Solicitador. O Solicitador conseguirá visualizar e aceitar a partilha na sua área de documentos.



                29. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorNcs</strong>
   </td>
   <td>NCS do Solicitador recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorName</strong>
   </td>
   <td>Nome do Solicitador recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorEmail</strong>
   </td>
   <td>Email do Solicitador recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>DocumentIdList</strong>
   </td>
   <td>Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>hasWritePermission</strong>
   </td>
   <td>Indica se a partilha é de leitura e escrita ou apenas leitura.
   </td>
   <td>Sim
   </td>
   <td>Ver tabela Write Permissions
   </td>
  </tr>
</table>


**Exemplo:**


```
<shareDocumentWithSolicitorInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <SolicitorNcs>?</SolicitorNcs>
            <SolicitorName>?</SolicitorName>
            <SolicitorEmail>?</SolicitorEmail>
            <!--1 or more repetitions:-->
            <DocumentIdList>?</DocumentIdList>
            <Process>?</Process>
           <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
            <HasWritePermission>?</HasWritePermission>
         </shareDocumentWithSolicitorInput>

```



                30. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            22. **ShareDocumentWithEntity**

Esta operação_ _tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com uma outra Entidade. A Entidade recetora conseguirá visualizar e aceitar a partilha na sua área reservada.



                31. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>EntityNif</strong>
   </td>
   <td>NIF da Entidade recetora da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EntityName</strong>
   </td>
   <td>Nome da Entidade recetora da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>EntityEmail</strong>
   </td>
   <td>Email da Entidade recetora da partlha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPathList</strong>
   </td>
   <td>Lista de nomes das pastas a serem partilhadas com o recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
<shareFolderWithEntityInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <EntityNif>?</EntityNif>
            <EntityName>?</EntityName>
            <EntityEmail>?</EntityEmail>
            <FolderPathList>?</FolderPathList>
            <Process>?</Process>
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
         </shareFolderWithEntityInput>

```



                32. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.



            23. **ShareDocumentWithCitizen**

Esta operação tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um Cidadão. O Cidadão conseguirá visualizar e aceitar a partilha na sua área de documentos.



                33. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>CitizenNic</strong>
   </td>
   <td>NIC do Cidadão recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CitizenName</strong>
   </td>
   <td>Nome do Cidadão recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>CitizenEmail</strong>
   </td>
   <td>Email do Cidadão recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPathList</strong>
   </td>
   <td>Lista de pastas a serem partilhadas com o recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**

	`&lt;shareFolderWithCitizenInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <CitizenNic>?</CitizenNic>
            <CitizenName>?</CitizenName>
            <!--Optional:-->
            <CitizenEmail>?</CitizenEmail>


            <FolderPathList>?</FolderPathList>
            <Process>?</Process>
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
         </shareFolderWithCitizenInput>

```



                34. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            24. **ShareFolderWIthLawyer**

Esta operação tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um advogado. Este profissional conseguirá visualizar e aceitar a partilha na sua área de documentos.



                35. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>LawyerNoa
   </td>
   <td>NOA do advogado recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>LawyerName
   </td>
   <td>Nome do advogado recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>LawyerEmail
   </td>
   <td>Email do advogado recetor da partilha.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>FolderPathList
   </td>
   <td>Lista de pastas a serem partilhadas com o recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>Process
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>Comments
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>ExpirationDate
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
</table>


**Exemplo:**

	`&lt;shareFolderWithLawyerInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <LawyerNoa>?</LawyerNoa>
            <LawyerName>?</LawyerName>
            <!--Optional:-->
            <LawyerEmail>?</LawyerEmail>
            <!--1 or more repetitions:-->
            <FolderPathList>?</FolderPathList>
            <Process>?</Process>
            <!--Optional:-->
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
         </shareFolderWithLawyerInput>

```



                36. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            25. **ShareDocumentWithSolicitor**

Esta operação tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um solicitador. Este profissional conseguirá visualizar e aceitar a partilha na sua área de documentos.



                37. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorNcs</strong>
   </td>
   <td>NCS do solicitador recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorName</strong>
   </td>
   <td>Nome do solicitador recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>SolicitorEmail</strong>
   </td>
   <td>Email do solicitador recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPathList</strong>
   </td>
   <td>Lista de pastas a serem partilhadas com o recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
<shareFolderWithSolicitorInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <SolicitorNcs>?</SolicitorNcs>
            <SolicitorName>?</SolicitorName>
            <!--Optional:-->
            <SolicitorEmail>?</SolicitorEmail>
            <!--1 or more repetitions:-->
            <FolderPathList>?</FolderPathList>
            <Process>?</Process>
            <!--Optional:-->
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
         </shareFolderWithSolicitorInput>

```



                38. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            26. **ShareDocumentWithNotary**

Esta operação tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um notário. Este profissional conseguirá visualizar e aceitar a partilha na sua área de documentos.



                39. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>NotaryNon</strong>
   </td>
   <td>NON do notário recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>NotaryName</strong>
   </td>
   <td>Nome do notário recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>NotaryEmail</strong>
   </td>
   <td>Email do notário recetor da partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>FolderPathList</strong>
   </td>
   <td>Lista de pastas a serem partilhadas com o recetor da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Comments</strong>
   </td>
   <td>Comentários associados à partilha.
   </td>
   <td>Não
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração da partilha.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**

	`&lt;shareFolderWithNotaryInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <NotaryNon>?</NotaryNon>
            <NotaryName>?</NotaryName>
            <NotaryEmail>?</NotaryEmail>


            <FolderPathList>?</FolderPathList>
            <Process>?</Process>
            <Comments>?</Comments>
            <ExpirationDate>?</ExpirationDate>
         </shareFolderWithNotaryInput>

```



                40. **Parâmetros de Saída**

Na resposta é indicado o estado da execução do pedido.





            27. **AcceptSend**

Esta operação tem como objetivo aceitar um envio de documentos que a Entidade recebeu.



                41. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>SendId
   </td>
   <td>ID do envio atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
</table>


Exemplo:


```
 	 <acceptSendInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <SendId>?</SendId>
         </acceptSendInput>

```



                42. **Parâmetros de Saída**

Na resposta é devolvido o ID do documento associado.





            28. **AcceptShare**

Esta operação tem como objetivo aceitar uma partilha de documentos que a Entidade recebeu.



                43. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>ShareId
   </td>
   <td>ID da partilha atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
</table>


Exemplo:


```
 	 <acceptShareInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <ShareId>?</ShareId>
         </acceptShareInput>

```



                44. **Parâmetros de Saída**

Na resposta é devolvido o ID do documento associado.





            29. **GetProcessDownloadAuthorizationList**

Esta operação tem como objetivo retribuir as autorizações de _download_ de ficheiros de documentos concebidos por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.



                45. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
<strong>:</strong> Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**

	`&lt;getProcessDownloadAuthorizationListInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <Process>?</Process>
         </getProcessDownloadAuthorizationListInput>

```



                46. **Parâmetros de Saída**

Na resposta é devolvido a lista de IDs de autorizações de _download_.





            30. **GetProcessSendList**

Esta operação tem como objetivo retribuir os envios efetuados por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.



                47. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>Process
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**


```
<getProcessSendListInput>
            <UserAuthentication>
               <EntityNif>1</EntityNif>
               <Username>1</Username>
               <Password>1</Password>
            </UserAuthentication>
            <Process>?</Process>
         </getProcessSendListInput>

```



                48. **Parâmetros de Saída**

Na resposta é devolvido a lista de IDs de envio.





            31. **GetProcessShareList**

Esta operação tem como objetivo retribuir as partilhas efetuadas por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.


<table>
  <tr>
   <td>Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>Process</strong>
   </td>
   <td>Identificador único disponibilizado pela Entidade para identificar o processo associado.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>




                49. **Parâmetros de Entrada**

Exemplo:


```
<getProcessShareListInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <Process>?</Process>
         </getProcessShareListInput>

```



                50. **Parâmetros de Saída**

Na resposta é devolvida a lista de IDs de partilha.





            32. **GetDownloadAuthorization**

Esta operação tem como objetivo retribuir o ficheiro de documento cuja autorização de _download_ foi concebida por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.





                51. **Parâmetros de Entrada**

	


<table>
  <tr>
   <td>Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>DownloadAuthorizationId
   </td>
   <td>Identificador de autorização de download atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
</table>


**Exemplo:**


```
<getDownloadAuthorizationInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DownloadAuthorizationId>?</DownloadAuthorizationId>
         </getDownloadAuthorizationInput>

```



                52. **Parâmetros de Saída**

Na resposta é incluído um payload com o GUID associado a um ficheiro de documento. Esse GUID deve ser utilizado para o descarregamento do ficheiro disponibilizado no LargeFiles.





            33. **GenerateDocumentQRCode**

Esta operação tem como objetivo gerar um QR code de um documento (apenas aplicável a formatos PDF). Na aplicação será gerada um novo ficheiro PDF do documento com um cabeçalho que inclui um QR Code. 



                53. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td><strong>DocumentId</strong>
   </td>
   <td>ID do documento atribuído pela Plataforma de Documentos do Cidadão.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
  <tr>
   <td><strong>ExpirationDate</strong>
   </td>
   <td>Data de expiração do QR Code gerado.
   </td>
   <td>Sim
   </td>
   <td><strong>-</strong>
   </td>
  </tr>
</table>


**Exemplo:**

   	` &lt;generateDocumentQRCodeInput>`


```
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <DocumentId>?</DocumentId>
            <ExpirationDate>?</ExpirationDate>
         </generateDocumentQRCodeInput>

```



                54. **Parâmetros de Saída**

Na resposta do é devolvido o QRKey do QR Code gerado.





            34. **GetQRCodeDocumentFile**

Esta operação tem como objetivo retribuir o ficheiro de um documento associado a um QR Code.

Na mensagem de resposta do pedido encontra-se o _payload_ especificado na integração com LargeFiles, devendo a entidade efetuar o _download_ do ficheiro do documento do LargeFiles após a receção da resposta desta operação.

O seguinte diagrama representa o fluxo de invocação do _webservice_:



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")




                55. **Parâmetros de Entrada**

<table>
  <tr>
   <td>
Nome do parâmetro
   </td>
   <td>Descrição
   </td>
   <td>Obrigatório
   </td>
   <td>Lista de valores
<p>
(Chave - Descrição)
   </td>
  </tr>
  <tr>
   <td>Qrkey
   </td>
   <td>Chave do QR Code.
   </td>
   <td>Sim
   </td>
   <td>-
   </td>
  </tr>
</table>


**Exemplo:**


```
<getQRCodeDocumentFileInput>
            <UserAuthentication>
               <EntityNif>?</EntityNif>
               <Username>?</Username>
               <Password>?</Password>
            </UserAuthentication>
            <Qrkey>?</Qrkey>
         </getQRCodeDocumentFileInput>

```



                56. **Parâmetros de Saída**

Na resposta são incluídos a data de expiração do QR Code e o payload com o GUID associado ao ficheiro. Esse GUID deve ser usado para o download do ficheiro disponibilizado no LargeFiles.



        4. **Envio do ficheiro**

A invocação de alguns _webservices_ como o addDocument e o getDocumentFile implica o envio ou a receção de ficheiros, sendo o LargeFiles um intermediário em caso de ocorrência de partilha de ficheiros numa troca de mensagens.

O envio e a receção de ficheiros devem ser efetuados via HTTP POST ou GET, e a entidade a enviar o ficheiro deve efetuar um POST com o mesmo em formato Base64 acompanhado por um Id único (GUID). A entidade recetora deve efetuar o _download_ via GET com base esse Id, devendo esse Id ser enviado na troca de mensagem dentro do _payload_).

Especificações:



*   Endereço de upload: [http://172.31.201.82/LargeFiles/Default.ashx](http://172.31.201.82/LargeFiles/Default.ashx) 
*   Upload: HTTP POST com parâmetros “File” (ficheiro em format Base64) e “Id” (GUID único)
*   Endereço de download: [http://172.31.201.82/LargeFiles/Download.aspx](http://172.31.201.82/LargeFiles/Download.aspx)
*   Download: HTTP GET com parâmetro “Id”

XSD Schema do _payload_ a incluir na mensagem na invocação de _webservice_ em caso de envio de ficheiro:


```
<?xml version='1.0' encoding='UTF-8'?><xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:tns="http://pi.ama.pt/largefiles"targetNamespace="http://pi.ama.pt/largefiles" version="1.0">
<xs:element name="AttachContext" type="tns:AttachContext"/>
<xs:complexType name="AttachContext">
<xs:sequence>
<xs:element default="30" minOccurs="0" name="TTL" type="xs:int"/>
<xs:element maxOccurs="unbounded" name="FileGuid" type="xs:string"/>
</xs:sequence>
</xs:complexType> 
</xs:schema>
```


O “FileGuid” é o Id associado ao ficheiro enviado juntamente com o mesmo via POST.

O “TTL” é o número de dias que o ficheiro deve ficar disponível para _download_ no LargeFiles. Depois desse tempo o ficheiro será removido do LargeFiles mesmo que não tenha ocorrido nenhum _download_ por parte da entidade recetora.

O seguinte diagrama representa um fluxo de troca de mensagens numa invocação de _webservice _(com iAP já incluído como intermediário na comunicação via _webservices_), onde a entidade A envia um documento para a entidade B:



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")


Se o payload da mensagem contem o elemento AttachContext formatado corretamente, a mensagem só será entregue ao sistema destino quando todos os documentos referidos na lista de elementos **FileGuid **tiverem sido submetidos para a Plataforma de Interoperabilidade

Para que seja corretamente anexado a uma mensagem específica, o pedido http deve conter os seguintes headers

Este serviço permite o envio e receção de ficheiros recorrendo à estrutura fixa anteriormente apresentada attachContext.

Esta estrutura apenas apresenta os dados referentes a ficheiros que já foram previamente enviados. Assim, torna-se necessário efetuar o upload dos ficheiros antes do envio da mensagem.

Para efetuar o upload é necessário aceder ao endereço de upload de acordo com as instruções presentes.

Depois de efetuado o upload a estrutura attachContext poderá ser preenchida na mensagem com os dados do ficheiro que foi enviado


<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: Definition &darr;&darr; outside of definition list. Missing preceding term(s)? </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


:


<table>
  <tr>
   <td>Campo
   </td>
   <td>Descrição
   </td>
  </tr>
  <tr>
   <td>Hash
   </td>
   <td>A hash em base64 da encriptação SHA1 do conteúdo do ficheiro
   </td>
  </tr>
  <tr>
   <td>ID
   </td>
   <td>Identificador do ficheiro no formato GUID (12345678-1234-1234-1234-123456789098)
   </td>
  </tr>
</table>


Não é possível a substituição de ficheiros.



        5. **Receção do Ficheiro**

Após a receção da mensagem de negócio pela entidade recetora, os ficheiros estão disponíveis através do respectivo endereço de download.

Para aceder ao seu conteúdo, deve ser enviado na query string do pedido http um parâmetro com o nome”Id”, contendo o GUID do ficheiro pretendido.

Quando se receciona uma mensagem com a estrutura attachContext preenchida torna-se necessário efetuar o download do ficheiro por forma a aceder ao mesmo. Para tal deverá aceder ao endereço respetivo de acordo com as instruções presentes.





        6. **Endereços**

<table>
  <tr>
   <td>
Ambiente
   </td>
   <td>Ação
   </td>
   <td>Endereço
   </td>
  </tr>
  <tr>
   <td>Testes
   </td>
   <td>Upload
   </td>
   <td><a href="http://172.31.201.82/LargeFiles/Default.ashx">http://172.31.201.82/LargeFiles/Default.ashx</a>
   </td>
  </tr>
  <tr>
   <td>Testes
   </td>
   <td>Dowload
   </td>
   <td><a href="http://172.31.201.82/LargeFiles/Download.aspx">http://172.31.201.82/LargeFiles/Download.aspx</a><span style="text-decoration:underline;">?id=</span>
   </td>
  </tr>
  <tr>
   <td>Produção 
   </td>
   <td>Upload
   </td>
   <td><a href="http://172.31.201.13/Files/Default.ashx">http://172.31.201.13/Files/Default.ashx</a>
   </td>
  </tr>
  <tr>
   <td>Produção
   </td>
   <td>Dowload
   </td>
   <td><a href="http://172.31.201.13/Files/Download.aspx?id">http://172.31.201.13/Files/Download.aspx?id</a>=
   </td>
  </tr>
</table>






    3. **Tabela Estado da resposta - Status**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
<strong>SUCCESS</strong>
   </td>
   <td>Pedido executado em sucesso.
   </td>
  </tr>
  <tr>
   <td><strong>FAILED</strong>
   </td>
   <td>Pedido não executado.
   </td>
  </tr>
  <tr>
   <td><strong>PERMISSION_DENIED</strong>
   </td>
   <td>Utilizador sem permissões.
   </td>
  </tr>
  <tr>
   <td><strong>VALIDATION_ERROR</strong>
   </td>
   <td>Erro de validação.
   </td>
  </tr>
  <tr>
   <td><strong>DOC_NOT_FOUND</strong>
   </td>
   <td>Documento não encontrado.
   </td>
  </tr>
</table>




    4. **Tabela Language**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
<strong>PT</strong>
   </td>
   <td>Português
   </td>
  </tr>
  <tr>
   <td><strong>EN</strong>
   </td>
   <td>Inglês
   </td>
  </tr>
</table>






    5. ** Tabela ClassificationCode**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
TITLE
   </td>
   <td>Título
   </td>
  </tr>
  <tr>
   <td>PROC
   </td>
   <td>Procuração
   </td>
  </tr>
  <tr>
   <td>CERT
   </td>
   <td>Certidão
   </td>
  </tr>
  <tr>
   <td>CAD
   </td>
   <td>Caderneta
   </td>
  </tr>
  <tr>
   <td>DEC
   </td>
   <td>Declaração
   </td>
  </tr>
  <tr>
   <td>CERT2
   </td>
   <td>Certificado
   </td>
  </tr>
  <tr>
   <td>CEDM
   </td>
   <td>Cédula Militar
   </td>
  </tr>
  <tr>
   <td>CARD
   </td>
   <td>Cartão
   </td>
  </tr>
  <tr>
   <td>CESS
   </td>
   <td>Cessação
   </td>
  </tr>
  <tr>
   <td>COMP
   </td>
   <td>Comprovativo
   </td>
  </tr>
  <tr>
   <td>CONT
   </td>
   <td>Contrato
   </td>
  </tr>
  <tr>
   <td>CURR
   </td>
   <td>Curriculum Vitae
   </td>
  </tr>
  <tr>
   <td>DOC
   </td>
   <td>Documento
   </td>
  </tr>
  <tr>
   <td>ESC
   </td>
   <td>Escritura
   </td>
  </tr>
  <tr>
   <td>FIC
   </td>
   <td>Ficha
   </td>
  </tr>
  <tr>
   <td>LIV
   </td>
   <td>Livrete
   </td>
  </tr>
  <tr>
   <td>BOOK
   </td>
   <td>Livro
   </td>
  </tr>
  <tr>
   <td>PART 
   </td>
   <td>Participação
   </td>
  </tr>
  <tr>
   <td>PASS 
   </td>
   <td>Passaporte
   </td>
  </tr>
  <tr>
   <td>PLA
   </td>
   <td>Plano
   </td>
  </tr>
  <tr>
   <td>PLANT
   </td>
   <td>Planta
   </td>
  </tr>
  <tr>
   <td>RECEIPT
   </td>
   <td>Recibo
   </td>
  </tr>
  <tr>
   <td>SEC
   </td>
   <td>Seguro
   </td>
  </tr>
  <tr>
   <td>TERM
   </td>
   <td>Termo
   </td>
  </tr>
  <tr>
   <td>TEST
   </td>
   <td>Testamento
   </td>
  </tr>
</table>




    6. ** Tabela CollaboratorType**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
CITIZEN
   </td>
   <td>Cidadão
   </td>
  </tr>
  <tr>
   <td>PRIVATEENTITY
   </td>
   <td>Entidade Privada
   </td>
  </tr>
  <tr>
   <td>PUBLICENTITY
   </td>
   <td>Entidade Pública
   </td>
  </tr>
</table>




    7. **Tabela ResourceType**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
DOCPERSONAL
   </td>
   <td>Documento com informação pessoal
   </td>
  </tr>
  <tr>
   <td>SERVAP
   </td>
   <td>Resultado de serviço da AP
   </td>
  </tr>
  <tr>
   <td>SERVOTHER
   </td>
   <td>Resultado de serviço (outro
   </td>
  </tr>
  <tr>
   <td>DOCPROFESSIONAL
   </td>
   <td>Documento com informação profissional)
   </td>
  </tr>
</table>




    8. **Tabela SecurityClassification**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
RESERVED
   </td>
   <td>Reservado
   </td>
  </tr>
  <tr>
   <td>NORESTRICTION
   </td>
   <td>Sem restrições de acesso
   </td>
  </tr>
</table>




    9. **Tabela EditorType**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
CITIZEN
   </td>
   <td>Cidadão
   </td>
  </tr>
  <tr>
   <td>PRIVATEENTITY
   </td>
   <td>Entidade Privada
   </td>
  </tr>
  <tr>
   <td>PUBLICENTITY
   </td>
   <td>Entidade Pública
   </td>
  </tr>
</table>




    10. **Tabela ResourceType**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
DOCPERSONAL
   </td>
   <td>Documento com informação pessoal
   </td>
  </tr>
  <tr>
   <td>SERVAP
   </td>
   <td>Resultado de serviço da AP
   </td>
  </tr>
  <tr>
   <td>SERVOTHER
   </td>
   <td>Resultado de serviço (outro)
   </td>
  </tr>
  <tr>
   <td>DOCPROFESSIONAL
   </td>
   <td>Documento com informação profissional
   </td>
  </tr>
</table>




    11. **Tabela WritePermissions**

<table>
  <tr>
   <td>
    ID
   </td>
   <td>
    Value
   </td>
  </tr>
  <tr>
   <td>
FALSE
   </td>
   <td>Só de leitura
   </td>
  </tr>
  <tr>
   <td>TRUE
   </td>
   <td>Leitura e escrita
   </td>
  </tr>
</table>

