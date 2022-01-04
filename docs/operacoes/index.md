

# API da Bolsa de Documentos
## Descrição
A invocação dos webservices deve ser assíncrona, sendo necessário a implementação de webservices no lado do cliente para a receção de callbacks.

Na mensagem de invocação deve estar especificado o endpoint dos webservices de receção de callbacks no campo “replyTo”. O campo “relatesTo” virá preenchido na resposta do webservice, o qual irá conter o messageID para a correlação de pedidos.

## Integração com LargeFiles
A invocação de alguns webservices como o addDocument e o getDocumentFile implica o envio ou  a receção de ficheiros, sendo o LargeFiles um intermediário em caso de ocorrência de partilha de ficheiros numa troca de mensagens.

O envio e a receção de ficheiros devem ser efetuados via HTTP POST ou GET, e a entidade a enviar o ficheiro deve efetuar um POST com o mesmo em formato Base64 acompanhado por um Id único (GUID). A entidade recetora deve efetuar o download via GET com base esse Id, devendo esse Id ser enviado na troca de mensagem dentro do payload).

XSD Schema do payload a incluir na mensagem na invocação de webservice em caso de envio de ficheiro:

```markdown
<?xml version='1.0' encoding='UTF-
8'?><xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:tns="http://pi.ama.p t/largefiles"targetNamespace="http://pi.ama.pt/largefiles" version="1.0">
   <xs:element name="AttachContext" type="tns:AttachContext"/>
   <xs:complexType name="AttachContext">
      <xs:sequence>
         <xs:element default="30" minOccurs="0" name="TTL" type="xs:int"/>
         <xs:element maxOccurs="unbounded" name="FileGuid" type="xs:string"/>
      </xs:sequence>
   </xs:complexType>
</xs:schema>
```

O “FileGuid” é o Id associado ao ficheiro enviado juntamente com o mesmo via POST.
O “TTL” é o número de dias que o ficheiro deve ficar disponível para download no LargeFiles. Depois desse tempo o ficheiro será removido do LargeFiles mesmo que não tenha ocorrido nenhum download por parte da entidade recetora.

O seguinte diagrama representa um fluxo de troca de mensagens numa invocação de webservice (com iAP já incluído como intermediário na comunicação via webservices), onde a entidade A envia um documento para a entidade B:

![Integração com LargeFiles - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/IntegracaoLargeFiles_diagrama.jpg?raw=true)

# WebServices disponibilizados
## Fluxo de invocação de webservices
O seguinte diagrama representa o fluxo de comunicação esperado, já com iAP como intermediário:

![Invocação de webservices - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/InvocacaoWebservices_diagrama.jpg?raw=true)

## Parâmetros comuns
### Parâmetros de entrada
 - **UserAuthentication:**
 
| Nome do parâmetro | Descrição | Obrigatório |
|--|--|--|
| **EntityNif** | NIF da Entidade | Sim |
| **Username** | Nome de utilizador atribuído à Entidade | Sim |
| **Password** | Palavra-passe associada ao nome de utilizador da Entidade | Sim |


### Parâmetros de saída
 - **Status:**
 
| Nome do parâmetro | Descrição |
|--|--|--|
| **Code** | Código do estado |
| **Description** | Descrição do Estado |

### Possíveis valores de estado na resposta
 
| Código | Descrição |
|--|--|--|
| **Sucess** | Pedido executado com sucesso |
| **Failed** | Pedido não executado |
| **Permission_Denied** | Utilizador sem permissões |
| **Validation_Error** | Erro de validação |
| **Doc_Not_Found** | Documento não encontrado |
| **Doc_Duplicate** | Existe um documento com o mesmo título |
| **Download_Auth_Expired** | A autorização para o descarregamento encontra-se expirada |
| **Area_Storage_Exceeded** | Necessita de mais espaço para a área reservada |

##  AddDocument
Este webservice tem como objetivo adicionar um documento à área reservada da entidade. Na mensagem do pedido deve incluir o payload especificado no cap. “Integração com LargeFiles”, devendo a entidade efetuar o upload do ficheiro do documento para o LargeFiles antes da invocação deste webservice.

O seguinte diagrama representa o fluxo de invocação do webservice:

![AddDocument - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/AddDocument_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **DocumentToAdd:**
 
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Filename** | Nome do ficheiro em formato <nome>.<extensão>. | Sim | -
| **MimeType** | Formato MIME (exemplo: “application/pdf”). | Sim | -
| **FolderPath** | Diretoria dentro da área reservada para onde o documento deve ser armazenado (exemplo: “certificacoes/cidadaos/12345678”). | Não | -
| **ValidityDate** | Data de validade do documento, conforme indicado pelo Produtor do mesmo. Se não for definida na inserção o documento é considerado vitalício. | Não | -
| **Title** | Título do documento. Se não for definido na inserção passa a ser igual ao valor do Filename. | Não |
| **ClassificationCode** | Classificação do recurso atribuída com base numa estrutura classificativa. Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOC” (Documento). | Não | DOC - Documento <br><br> 450.30.002 -Certificação de habilitações ou qualificações <br><br> 450.30.502 -Emissão de declarações comprovativas <br><br> 450.30.003 -Emissão de certidões <br><br> 750.30.602 - Reconhecimento, creditação e validação de competências e qualificações
| **Subject** | Assunto relacionado com o documento. Se não for definido na inserção passa a ser igual ao valor do Filename. | Não | -
| **Language** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Português). | Não | PT (Português) <br><br> EN (Inglês)
| **Tags** | Tags a associar a um documento para facilitar a pesquisa e a categorização do mesmo. | Não | -
| **CollaboratorId** | NIF da Entidade colaboradora na produção do documento. | Não | -
| **CollaboratorDesignation** | Nome da Entidade colaboradora na produção do documento. | Não | -
| **CollaboratorType** | Pode ser definido com uma das chaves na lista de valores. | Não | CITIZEN (Cidadão)<br><br> PRIVATEENTITY (Entidade Privada)<br><br> PUBLICENTITY (Entidade Pública)
| **CreationDate** | Data de criação do documento. | Não | -
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOCPERSONAL” (Documento com informação pessoal). | Não | DOCPERSONAL (Documento com informação pessoal)<br><br> SERVAP (Resultado de serviço da AP)<br><br> SERVOTHER (Resultado de serviço (outro))<br><br> DOCPROFESSIONAL (Documento com informação profissional)
| **Country** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Portugal). | Não | PT (Portugal)
| **SecurityClassification** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “RESERVED” (Reservado). | Não | RESERVED (Reservado)<br><br> NORESTRICTION (Sem restrições de acesso)
| **Price** | Preço associado ao documento. | Não | -
| **ConservationTime** | Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma. Se não for definida na inserção é considerado o valor “5”. | Não | -

### Parâmetros de saída
Na resposta do webservice é devolvido um ID atribuído ao documento na Bolsa de Documentos do Cidadão.

##  GetDocument
Este webservice tem como objetivo retribuir a informação associada ao documento.

### Parâmetros de entrada
 - **Outros parâmetros:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores; (Chave – Descrição)
|--|--|--|-
| **DocumentID** | ID do documento atribuído pela Plataforma de Documentos do Cidadão. | Sim | -

### Parâmetros de saída
Na resposta do webservice é devolvida informação associada ao documento estruturada em conformidade com especificações MIP (Metainformação para interoperabilidade).

##  GetDocumentList
Este webservice tem como objetivo retribuir a informação associada a um ou mais documentos presentes na área reservada da Entidade. Pode servir como pesquisa de documentos onde os valores dos parâmetros de entrada funcionam como filtro caso forem definidos. Caso nenhum parâmetro venha preenchido, na resposta é devolvida todos os documentos da área reservada da entidade.

### Parâmetros de entrada
 - **DocumentSearchParameters:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **MimeType** | Formato MIME (exemplo: “application/pdf”). | Não | - 
| **FolderPath** | Diretoria dentro da área reservada para onde os documentos podem estar armazenados (exemplo: “certificacoes/cidadaos/12345678”). | Não | - 
| **ValidityDate** | Data de validade dos documentos, conforme indicado pelo Produtor do mesmo. | Não | - 
| **Title** | Título do documento. | Não | - 
| **ClassificationCode** | Classificação do recurso atribuída com base numa estrutura classificativa. Pode ser definido com uma das chaves na lista de valores. | Não | DOC - Documento <br><br> 450.30.002 -Certificação de habilitações ou qualificações <br><br> 450.30.502 -Emissão de declarações comprovativas <br><br> 450.30.003 -Emissão de certidões <br><br> 750.30.602 - Reconhecimento, creditação e validação de competências e qualificações 
| **ProducerDesignation** | Designação do produtor do documento. | Não | - 
| **Subject** | Assunto relacionado com o documento. | Não | - 
| **Language** | Pode ser definido com uma das chaves na lista de valores. | Não | PT (Português) <br><br> EN (Inglês) 
| **EditorId** | NIF, NIC, NOA, NCS ou NON da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão. | Não | - 
| **EditorDesignation** | Nome da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão. | Não | - 
| **EditorType** | Pode ser definido com uma das chaves na lista de valores. | Não | CITIZEN (Cidadão)<br><br> PRIVATEENTITY (Entidade Privada)<br><br> PUBLICENTITY (Entidade Pública) 
| **Tags** | Tags associadas ao documento. | Não | - 
| **CollaboratorId** | NIF da Entidade colaboradora na produção do documento. | Não | - 
| **CollaboratorDesignation** | Nome da Entidade colaboradora na produção do documento. | Não | - 
| **CollaboratorType** | Pode ser definido com uma das chaves na lista de valores. | Não | CITIZEN (Cidadão) <br><br>PRIVATEENTITY (Entidade Privada)<br><br>PUBLICENTITY (Entidade Pública) 
| **CreationDate** | Data de criação do documento. | Não | - 
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores. | Não | DOCPERSONAL (Documento com informação pessoal) <br><br>SERVAP (Resultado de serviço da AP) <br><br>SERVOTHER (Resultado de serviço (outro)) <br><br>DOCPROFESSIONAL (Documento com informação profissional) 
| **DataFormat** | Formato/extensão do ficheiro, como por exemplo “PDF”, “PNG”, entre outros. | Não | - 
| **Country** | Pode ser definido com uma das chaves na lista de valores. | Não | PT (Portugal) 
| **SecurityClassification** | Pode ser definido com uma das chaves na lista de valores. | Não | RESERVED (Reservado) <br><br>NORESTRICTION (Sem restrições de acesso) 
| **Price** | Preço associado ao documento. | Não | - 
| **ConservationTime** | Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma | Não | - 

### Parâmetros de saída
Na resposta do webservice é devolvida informação associada a um ou mais documentos estruturada em conformidade com especificações MIP (Metainformação para interoperabilidade).

| Nome do parâmetro | Descrição | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Status** | - | 1..1 
| **Code** | Código erro | - 
| **Description** | Descrição do erro | - 
| **DocumentList** | - | 0.. N 
| **FolderPath** | Diretoria dentro da área reservada para onde os documentos podem estar armazenados (exemplo: “certificacoes/cidadaos/12345678”) | - 
| **Title** | Título do documento | - 
| **IdentifierType** | Tipo de utilizador | - 
| **ResourceIdentifier** | Id do documento na bolsa | - 
| **ClassificationCode** | Classificação do documento, È definido com uma das chaves na lista de valores | - 
| **ProducerDesignation** | Designação do produtor do documento | - 
| **Subject** | Assunto do documento | - 
| **Language** | Língua do documento | - 
| **EditorId** | NIF, NIC, NOA, NCS ou NON da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão | - 
| **EditorDesignation** | Nome da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão | - 
| **Type** | Tipo de editor, É definido com uma das chaves na lista de valores | - 
| **CreationDate** | Data de criação do documento | - 
| **RegistrationDate** | Data de registo do documento | - 
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores | - 
| **Spatial** | País do documento | - 
| **ConservationTime** | Tempo de conservação do documento | - 
| **FinalDestiny** | Tipo de eliminação | -  

##  GetDocumentFile
Este webservice tem como objetivo retribuir o ficheiro de um documento.
Na mensagem de resposta do pedido encontra-se o payload especificado no cap. “Integração com LargeFiles”, devendo a entidade efetuar o download do ficheiro do documento do LargeFiles após a receção da resposta deste webservice.

O seguinte diagrama representa o fluxo de invocação do webservice:

![GetDocumentFile - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/GetDocumentFile_diagrama.jpg?raw=true)

### Parâmetros de entrada
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentID** | ID do documento atribuído pela Plataforma de Documentos do Cidadão. | Sim | - 

### Parâmetros de saída
Na resposta do webservice é incluído o payload com o GUID associado ao ficheiro. Esse GUID deve ser usado para o download do ficheiro disponibilizado no LargeFiles.

##  addDocumentLink
Este webservice tem como objetivo adicionar um link à área reservada da entidade.

### Parâmetros de entrada
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **LinkUrl** | Hiperligação para onde o link redirecionará quando for selecionado. | Sim | - 
| **FolderPath** | Diretoria dentro da área reservada onde os links podem estar armazenados (exemplo: “links/cidadaos/12345678”). | Não | - 
| **ValidityDate** | Data de validade dos documentos, conforme indicado pelo Produtor do mesmo. | Não | - 
| **Title** | Título do link. | Sim | - 
| **CollaboratorId** | NIF da Entidade colaboradora na produção do documento. | Não | - 
| **CollaboratorDesignation** | Nome da Entidade colaboradora na produção do documento. | Não | - 
| **CollaboratorType** | Pode ser definido com uma das chaves na lista de valores. | Não | CITIZEN (Cidadão) <br><br>PRIVATEENTITY (Entidade Privada) <br><br>PUBLICENTITY (Entidade Pública) 
| **CreationDate** | Data de criação do documento. | Não | - 
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores. | Não | DOCPERSONAL (Documento com informação pessoal) <br><br>SERVAP (Resultado de serviço da AP) <br><br>SERVOTHER (Resultado de serviço (outro)) <br><br>DOCPROFESSIONAL (Documento com informação profissional) 
| **SecurityClassification** | Pode ser definido com uma das chaves na lista de valores. | Não | RESERVED (Reservado) <br><br> NORESTRICTION (Sem restrições de acesso) 
| **ConservationTime** | Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma. | Não | - 

### Parâmetros de saída
Na resposta do webservice é indicado o identificador interno atribuído ao link que foi criado.

##  getDocumentLink
Este webservice tem como objetivo retribuir a informação associada ao link.

### Parâmetros de entrada
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentID** | ID do link que se pretende obter, atribuído pela Bolsa de Documentos do Cidadão. | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvida informação associada ao link estruturada em conformidade com especificações MIP (Metainformação para interoperabilidade), com a devida adaptação para utilização nos links.

##  getDocumentLinkList
Este webservice tem como objetivo retribuir a informação associada a um ou mais links presentes na área reservada da Entidade. Pode servir como pesquisa de links onde os valores dos parâmetros de entrada funcionam como filtro caso forem definidos. Caso nenhum parâmetro venha preenchido, na resposta é devolvida todos os documentos da área reservada da entidade.

### Parâmetros de entrada
 - **DocumentLinkSearchParameters:**
 
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **FolderPath** | Diretoria dentro da área reservada para onde os documentos podem estar armazenados (exemplo: “certificacoes/cidadaos/12345678”) | Não | - 
| **ValidityDate** | Data de validade dos documentos, conforme indicado pelo Produtor do mesmo | Não | - 
| **Title** | Título do documento | Não | - 
| **ProducerDesignation** | Designação do produtor do documento | Não | - 
| **EditorId** | NIF, NIC, NOA, NCS ou NON da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão | Não | - 
| **EditorDesignation** | Nome da Entidade, Cidadão ou Profissional que publicou o documento na Plataforma de Documentos do Cidadão | Não | - 
| **EditorType** | Pode ser definido com uma das chaves na lista de valores | Não | CITIZEN (Cidadão) <br><br>PRIVATEENTITY (Entidade Privada) <br><br>PUBLICENTITY (Entidade Pública) 
| **CollaboratorId** | NIF da Entidade colaboradora na produção do documento | Não | - 
| **CollaboratorDesignation** | Nome da Entidade colaboradora na produção do documento | Não | - 
| **CollaboratorType** | Pode ser definido com uma das chaves na lista de valores. | Não | CITIZEN (Cidadão) <br><br>PRIVATEENTITY (Entidade Privada) <br><br>PUBLICENTITY (Entidade Pública) 
| **CreationDate** | Data de criação do documento | Não | - 
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores | Não | DOCPERSONAL (Documento com informação pessoal) <br><br>SERVAP (Resultado de serviço da AP) <br><br>SERVOTHER (Resultado de serviço (outro)) <br><br>DOCPROFESSIONAL (Documento com informação profissional) 
| **DataFormat** | Formato/extensão do ficheiro, como por exemplo “PDF”, “PNG”, entre outros | Não | - 
| **SecurityClassification** | Pode ser definido com uma das chaves na lista de valores | Não | RESERVED (Reservado) <br><br>NORESTRICTION (Sem restrições de acesso) 
| **ConservationTime** | Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma | Não | - 

### Parâmetros de saída
Na resposta do webservice é devolvida informação associada a um ou mais links estruturados em conformidade com especificações MIP (Metainformação para interoperabilidade), com a devida adaptação para utilização nos links.

##  SendDocumentToCitizen
Este webservice tem como objetivo efetuar o envio de documentos ou links presentes na área reservada da Entidade para um Cidadão. O Cidadão conseguirá visualizar e aceitar o envio na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **CitizenNic** | NIC do cidadão recetor do envio | Sim | - 
| **CitizenName** | Nome do cidadão recetor do envio | Sim | - 
| **CitizenEmail** | Email do Cidadão recetor do envio | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a enviar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio | Sim | - 
| **Comments** | Comentários associados ao envio | Não | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  SendDocumentToEntity
Este webservice tem como objetivo efetuar o envio de documentos ou links presentes na área reservada da Entidade para uma outra Entidade. A Entidade recetora conseguirá visualizar e aceitar o envio na sua área reservada.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityNif** | NIF da Entidade recetora do envio | Sim | - 
| **EntityName** | Nome da Entidade recetora do envio | Sim | - 
| **EntityEmail** | Email da Entidade recetora do envio | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a enviar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  SendDocumentToLawyer
Este webservice tem como objetivo efetuar o envio de documentos ou links presentes na área reservada da Entidade para um Advogado. O Advogado conseguirá visualizar e aceitar o envio na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **LawyerNoa** | NOA do Advogado recetor do envio | Sim | - 
| **LawyerName** | Nome do Advogado recetor do envio | Sim | - 
| **LawyerEmail** | Email do Advogado recetor do envio | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a enviar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio | Sim | - 
| **Comments** | Comentários associados ao envio | Não | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  SendDocumentToNotary
Este webservice tem como objetivo efetuar o envio de documentos ou links presentes na área reservada da Entidade para um Notário. O Notário conseguirá visualizar e aceitar o envio na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **NotaryNon** | NON do Notário recetor do envio | Sim | - 
| **NotaryName** | Nome do Notário recetor do envio | Sim | - 
| **NotaryEmail** | Email do Notário recetor do envio | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio | Sim | - 
| **Comments** | Comentários associados ao envio | Não | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  SendDocumentToSolicitor
Este webservice tem como objetivo efetuar o envio de documentos ou links presentes na área reservada da Entidade para um Solicitador. O Solicitador conseguirá visualizar e aceitar o envio na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **SolicitorNcs** | NCS do Solicitador recetor do envio | Sim | - 
| **SolicitorName** | Nome do Solicitador recetor do envio | Sim | - 
| **SolicitorEmail** | Email do Solicitador recetor do envio | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Plataforma de Documentos do Cidadão) dos documentos a enviar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado ao envio | Sim | - 
| **Comments** | Comentários associados ao envio | Não | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  ShareDocumentWithCitizen
Este webservice tem como objetivo efetuar a partilha de documentos ou links presentes na área reservada da Entidade com um Cidadão. O Cidadão conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **CitizenNic** | NIC do cidadão recetor da partilha | Sim | - 
| **CitizenName** | Nome do cidadão recetor da partilha | Sim | - 
| **CitizenEmail** | Email do Cidadão recetor da partilha | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a partilhar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 
| **hasWritePermission** | Indica se a partilha é de leitura e escrita ou apenas leitura | Sim | False = só de leitura <br><br>True = também escrita
 
### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  ShareDocumentWithEntity
Este webservice tem como objetivo efetuar a partilha de documentos ou links presentes na área reservada da Entidade com uma outra Entidade. A Entidade recetora conseguirá visualizar e aceitar a partilha na sua área reservada.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityNif** | NIF da Entidade recetora da partilha | Sim | - 
| **EntityName** | Nome da Entidade recetora da partilha | Sim | - 
| **EntityEmail** | Email da Entidade recetora da partilha | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a partilhar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 
| **hasWritePermission** | Indica se a partilha é de leitura e escrita ou apenas leitura | Sim | False = só de leitura <br><br>True = também escrita
 
### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  ShareDocumentWithLawyer
Este webservice tem como objetivo efetuar a partilha de documentos ou links presentes na área reservada da Entidade com um Advogado. O Advogado conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **LawyerNoa** | NOA do Advogado recetor da partilha | Sim | - 
| **LawyerName** | Nome do Advogado recetor da partilha | Sim | - 
| **LawyerEmail** | Email do Advogado recetor da partilha | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa deDocumentos do Cidadão) dos documentos a partilhar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 
| **hasWritePermission** | Indica se a partilha é de leitura e escrita ou apenas leitura | Sim | False = só de leitura <br><br>True = também escrita
 
### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  ShareDocumentWithNotary
Este webservice tem como objetivo efetuar a partilha de documentos ou links presentes na área reservada da Entidade com um Notário. O Notário conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **NotaryNon** | NON do Notário recetor da partilha | Sim | - 
| **NotaryName** | Nome do Notário recetor da partilha | Sim | - 
| **NotaryEmail** | Email do Notário recetor da partilha | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a partilhar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 
| **hasWritePermission** | Indica se a partilha é de leitura e escrita ou apenas leitura | Sim | False = só de leitura <br><br>True = também escrita
 
### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  ShareDocumentWithSolicitor
Este webservice tem como objetivo efetuar a partilha de documentos ou links presentes na área reservada da Entidade com um Solicitador. O Solicitador conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **SolicitorNcs** | NCS do Solicitador recetor da partilha | Sim | - 
| **SolicitorName** | Nome do Solicitador recetor da partilha | Sim | - 
| **SolicitorEmail** | Email do Solicitador recetor da partilha | Não | - 
| **DocumentIdList** | Lista de IDs (atribuídos pela Bolsa de Documentos do Cidadão) dos documentos a partilhar | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 
| **hasWritePermission** | Indica se a partilha é de leitura e escrita ou apenas leitura | Sim | False = só de leitura <br><br>True = também escrita
 
### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  shareFolderWithEntity
Este webservice tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com uma outra Entidade.  A Entidade recetora conseguirá visualizar e aceitar a partilha na sua área reservada.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityNif** | NIF da Entidade recetora da partilha | Sim | - 
| **EntityName** | Nome da Entidade recetora da partilha | Sim | - 
| **EntityEmail** | Email da Entidade recetora da partilha | Não | - 
| **FolderPathList** | Lista de nomes das pastas a serem partilhadas com o recetor da partilha | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  shareFolderWithCitizen
Este webservice tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um Cidadão. O Cidadão conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **CitizenNic** | NIC do cidadão recetor da partilha | Sim | - 
| **CitizenName** | Nome do cidadão recetor da partilha | Sim | - 
| **CitizenEmail** | Email do Cidadão recetor da partilha | Não | - 
| **FolderPathList** | Lista de pastas a serem partilhadas com o recetor da partilha | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  shareFolderWithLawyer
Este webservice tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um advogado. Este profissional conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **LawyerNoa** | NOA do Advogado recetor da partilha | Sim | - 
| **LawyerName** | Nome do Advogado recetor da partilha | Sim | - 
| **LawyerEmail** | Email do Advogado recetor da partilha | Não | - 
| **FolderPathList** | Lista de pastas a serem partilhadas com o recetor da partilha | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  shareFolderWithSolicitor
Este webservice tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um solicitador. Este profissional conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **SolicitorNcs** | NCS do Solicitador recetor da partilha | Sim | - 
| **SolicitorName** | Nome do Solicitador recetor da partilha | Sim | - 
| **SolicitorEmail** | Email do Solicitador recetor da partilha | Não | - 
| **FolderPathList** | Lista de pastas a serem partilhadas com o recetor da partilha | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  shareFolderWithNotary
Este webservice tem como objetivo efetuar a partilha de pastas presentes na área reservada da Entidade com um notário. Este profissional conseguirá visualizar e aceitar a partilha na sua área de documentos.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **NotaryNon** | NON do Notário recetor da partilha | Sim | - 
| **NotaryName** | Nome do Notário recetor da partilha | Sim | - 
| **NotaryEmail** | Email do Notário recetor da partilha | Não | - 
| **FolderPathList** | Lista de pastas a serem partilhadas com o recetor da partilha | Sim | - 
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado à partilha | Sim | - 
| **Comments** | Comentários associados à partilha | Não | - 
| **ExpirationDate** | Data de expiração de partilha | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

##  AcceptSend
Este webservice tem como objetivo aceitar um envio de documentos que a Entidade recebeu.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **SendId** | ID do envio atribuído pela Plataforma de Documentos do Cidadão | Sim | -  

### Parâmetros de saída
Na resposta do webservice é devolvido o ID do documento associado.

##  AcceptShare
Este webservice tem como objetivo aceitar uma partilha de documentos que a Entidade recebeu.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **ShareId** | ID da partilha atribuído pela Bolsa de Documentos do Cidadão | Sim | -  

### Parâmetros de saída
Na resposta do webservice é devolvido o ID do documento associado.

##  GetProcessDownloadAuthorizationList
Este webservice tem como objetivo retribuir as autorizações de download de ficheiros de documentos concebidos por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvido a lista de IDs de autorizações de download.

##  GetProcessSendList
Este webservice tem como objetivo retribuir os envios efetuados por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvido a lista de IDs de envio.

##  GetProcessShareList
Este webservice tem como objetivo retribuir as partilhas efetuadas por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Process** | Identificador único disponibilizado pela Entidade para identificar o processo associado | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvido a lista de IDs de partilha.

##  GetDownloadAuthorization
Este webservice tem como objetivo retribuir os ficheiros de documento cujo autorização de download foi concebido por um Cidadão ou Profissional (Advogado, Notário e Solicitador) no âmbito de um processo.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DownloadAuthorizationId** | Identificador de autorização de download atribuído pela Plataforma de Documentos do Cidadão | Sim | - 

### Parâmetros de saída
Na resposta do webservice é incluído um payload com o GUID associado a um ficheiro de documento. Esse GUID deve ser utilizado para o descarregamento do ficheiro disponibilizado no LargeFiles.

##  GenerateDocumentQRCode
Este webservice tem como objetivo gerar um QR code de um documento (apenas aplicável a formatos PDF). Na aplicação será gerada um novo ficheiro PDF do documento com um cabeçalho que inclui um QR Code.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentId** | ID do documento atribuído pela Plataforma de Bolsa de Documentos. | Sim | - 
| **ExpirationDate** | Data de expiração do QR Code gerado | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvido o QRKey do QR Code gerado.

##  GetQRCodeDocumentFile
Este webservice tem como objetivo retribuir o ficheiro de um documento associado a um QR Code.
Na mensagem de resposta do pedido encontra-se o payload especificado no cap. “Integração com LargeFiles”, devendo a entidade efetuar o download do ficheiro do documento do LargeFiles após a receção da resposta deste webservice.

O seguinte diagrama representa o fluxo de invocação do webservice:

![GetQRCodeDocumentFile - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/GetQRCodeDocumentFile_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Orkey** | Chave do QR Code | Sim | - 

### Parâmetros de saída
Na resposta do webservice são incluídos a data de expiração do QR Code e o payload com o GUID associado ao ficheiro. Esse GUID deve ser usado para o download do ficheiro disponibilizado no LargeFiles.

##  AddDocumentAsCitizen
Este webservice tem como objetivo adicionar um documento à área reservada de um cidadão em nome dele. Aplica-se nos casos em que uma App adiciona um documento a pedido do utilizador.

Tal como no caso do AddDocument, na mensagem do pedido deve incluir o payload especificado no cap. “Integração com LargeFiles”, devendo a entidade efetuar o upload do ficheiro do documento para o LargeFiles antes da invocação deste webservice.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **CitizenNic** | NIC do Cidadão em nome de quem estamos a adicionar o documento | Sim | - 
| **CitizenName** | Nome do Cidadão | Sim | - 
| **CitizenEmail** | Email do Cidadão | Sim | - 
| **DocumentIdList** | Lista de Ids do documento no LargeFiles | Sim | - 
| **Process** | Processo | Não | - 
| **Comments** | Comentários | Não | - 
| **Filename** | Nome do ficheiro em formato <nome>.<extensão> | Sim | - 
| **imeType** | Formato MIME (exemplo: “application/pdf”) | Sim | - 
| **FolderPath** | Diretoria dentro da área reservada para onde o documento deve ser armazenado (exemplo: “certificacoes/cidadaos/12345678”) | Não | - 
| **ValidityDate** | Data de validade do documento, conforme indicado pelo Produtor do mesmo. Se não for definida na inserção o documento é considerado vitalício | Não | - 
| **Title** | Título do documento. Se não for definido na inserção passa a ser igual ao valor do Filename | Não | - 
| **ClassificationCode** | Classificação do recurso atribuída com base numa estrutura classificativa. Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOC” (Documento) | Não | DOC - Documento <br><br> 450.30.002 -Certificação de habilitações ou qualificações <br><br> 450.30.502 -Emissão de declarações comprovativas <br><br> 450.30.003 -Emissão de certidões <br><br> 750.30.602 - Reconhecimento, creditação e validação de competências e qualificações 
| **Subject** | Assunto relacionado com o documento. Se não for definido na inserção passa a ser igual ao valor do Filename | Não | - 
| **Language** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Português) | Não | PT (Português) <br><br> EN (Inglês) 
| **Tags** | Tags a associar a um documento para facilitar a pesquisa e a categorização do mesmo | Não | - 
| **CollaboratorId** | NIF da Entidade colaboradora na produção do documento | Não | - 
| **CollaboratorDesignation** | Nome da Entidade colaboradora na produção do documento | Não | - 
| **CollaboratorType** | Pode ser definido com uma das chaves na lista de valores | Não | CITIZEN (Cidadão) <br><br> PRIVATEENTITY (Entidade Privada) <br><br> PUBLICENTITY (Entidade Pública) 
| **CreationDate** | Data de criação do documento | Não | - 
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOCPERSONAL” (Documento com informação pessoal) | Não | DOCPERSONAL (Documento com informação pessoal) <br><br> SERVAP (Resultado de serviço da AP) <br><br>SERVOTHER (Resultado de serviço (outro)) <br><br> DOCPROFESSIONAL (Documento com informação profissional)
| **Country** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Portugal) | Não | PT (Portugal) 
| **SecurityClassification** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “RESERVED” (Reservado) | Não | RESERVED (Reservado) <br><br> NORESTRICTION (Sem restrições de acesso)
| **Price** | Preço associado ao documento | Não | - 
| **ConservationTime** | Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma. Se não for definida na inserção é considerado o valor “5” | Não | - 

### Parâmetros de saída
Na resposta do webservice é devolvido um ID atribuído ao documento na Plataforma de Documentos do Cidadão.

##  GetDocumentFromShareLink
Este webservice, que tem um funcionamento semelhante ao do GetDocumentFile, tem como objetivo retribuir o ficheiro de um documento que foi partilhado por um cidadão com uma entidade da AP através de link.

Na mensagem de resposta do pedido encontra-se o payload especificado no cap. “Integração com LargeFiles”, devendo a entidade efetuar o download do ficheiro do documento do LargeFiles após a receção da resposta deste webservice.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentLink** | ID do documento atribuído pela Plataforma de Bolsa de Documentos | Sim | - 

### Parâmetros de saída
Na resposta do webservice é incluído o payload com o GUID associado ao ficheiro. Esse GUID deve ser usado para o download do ficheiro disponibilizado no LargeFiles.

##  GetAllAcknowledge
Este webservice tem como objetivo prestar informações sobre o estado da informação de um cidadão na Bolsa para apresentar na Área Reservada do ePortugal.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Nic** | NIC do Cidadão | Sim | - 
| **LanguageCode** | Língua selecionada | Sim | Códigos de java util.Locale 
| **SearchName** | Padrão para pesquisa | Sim | - 
| **Status** | Estado para filtro | Não | Reservado para uso futuro 
| **Start** | Registo inicial (paginação) | Sim | - 
| **End** | Registo final (paginação) | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvida uma lista de documentos da Bolsa do Cidadão com as informações seguintes:

- Type – Tipo de objecto
- TypeTitle – nome do objecto
- Comment - Comentários
- DestinationName - Destino
- OriginName - Origem
- OriginType – Tipo de origem
- Status - Estado
- ShareActionType - Tipo de partilha
- CreationDate – Data de criação

##  GetAllAcknowledgeSize
Este webservice tem como objetivo prestar informações sobre a contagem de documentos pendentes de um cidadão na Bolsa para apresentar na Área Reservada do ePortugal.

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Nic** | NIC do Cidadão | Sim | - 

### Parâmetros de saída
Na resposta do webservice é devolvido um número inteiro com a contagem de documentos pendentes.

##  GetDocumentByEntityRequest
Este webservice tem como objetivo obter os documentos que o cidadão selecionou para fornecer á entidade na janela de integração.

O seguinte diagrama representa o fluxo de invocação do webservice:

![GetDocumentByEntityRequest - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/GetDocumentByEntityRequest_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityRequestId** | Id processo de pedido de documentos por uma entidade externa | Sim | - 

### Parâmetros de saída
Na resposta do webservice é incluído o payload com o GUID associado ao ficheiro. Esse GUID deve ser usado para o download do ficheiro disponibilizado no LargeFiles.

### Exemplo
```markdown
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsdl="http://ama.pt/wsdl">
   <soapenv:Header/>
   <soapenv:Body>
      <wsdl:getDocumentByEntityRequest>
         <!--Optional:-->
         <getDocumentByEntityRequestInput>
             <UserAuthentication>
               <EntityNif>508184509</EntityNif>
               <Username>entidadeexterna</Username>
               <Password>123456</Password>
            </UserAuthentication>
            <EntityRequestId>908737</EntityRequestId>
         </getDocumentByEntityRequestInput>
      </wsdl:getDocumentByEntityRequest>
   </soapenv:Body>
</soapenv:Envelope>
```

##  SendDocumentToEntityAsEntity
Este webservice tem como objetivo enviar documentos entre entidades.

O seguinte diagrama representa o fluxo de invocação do webservice:

![SendDocumentToEntityAsEntity - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/SendDocumentToEntityAsEntity_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **To:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityId** | Nipc da entidade | Sim | - 
| **EntityName** | Nome da entidade | Sim | - 
| **EntityEmail** | Email da entidade | Sim | - 

 - **Outros parâmetros:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentId** | Id do documento | Sim | - 
| **DeleteDocumentFromOrigin** | Se pretende apagar ficheiro apos enviar | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

### Exemplo
```markdown
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsdl="http://ama.pt/wsdl">
   <soapenv:Header/>
   <soapenv:Body>
      <wsdl:sendDocumentToCitizenAsCitizen>
         <!--Optional:-->
         <sendDocumentToCitizenAsCitizenInput>
             <UserAuthentication>
               <Username>entidadeexterna</Username>
               <Password>123456</Password>
            </UserAuthentication>
            <DocumentIdList>
               <DocumentId>86329</DocumentId>
               <DeleteDocumentFromOrigin>False</DeleteDocumentFromOrigin>
            </DocumentIdList>
            <from>
               <UserId>00000001</UserId>
               <UserType>NIC</UserType>
            </from>
            <to>
               <UserId>00000002</UserId>
               <UserName>sadad</UserName>
               <UserEmail>asdasda@sapo.pt</UserEmail>
               <UserType>NIC</UserType>
            </to>
         </sendDocumentToCitizenAsCitizenInput>
      </wsdl:sendDocumentToCitizenAsCitizen>
   </soapenv:Body>
</soapenv:Envelope>
```

##  addDocumentToEntityAsCitizen
Este webservice tem como objetivo adicionar um documento à área reservada da entidade. Na mensagem do pedido deve incluir o payload especificado no cap. “Integração com LargeFiles”, devendo a entidade efetuar o upload do ficheiro do documento para o LargeFiles antes da invocação deste webservice.

O seguinte diagrama representa o fluxo de invocação do webservice:

![addDocumentToEntityAsCitizen - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/addDocumentToEntityAsCitizen_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **From:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **UserType** | Tipo de utilizador | Sim | NIC, NON, NCS, NIPC, NOA 
| **UserId** | Id utilizador | Sim | - 

- **To:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityId** | Nipc da entidade | Sim | - 
| **EntityName** | Nome da entidade | Sim | - 
| **EntityEmail** | Email da entidade | Sim | - 

- **DocumentToAdd:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **Filename** | Nome do ficheiro em formato <nome>.<extensão>. | Sim | - 
| **MimeType** | Formato MIME (exemplo: “application/pdf”). | Sim | - 
| **FolderPath** | Diretoria dentro da área reservada para onde o documento deve ser armazenado (exemplo: “certificacoes/cidadaos/12345678”). | Não | - 
| **ValidityDate** | Data de validade do documento, conforme indicado pelo Produtor do mesmo. Se não for definida na inserção o documento é considerado vitalício. | Não | - 
| **Title** | Título do documento. Se não for definido na inserção passa a ser igual ao valor do Filename. | Não | - 
| **ClassificationCode** | Classificação do recurso atribuída com base numa estrutura classificativa. Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOC” (Documento). | Não | DOC - Documento <br><br> 450.30.002 -Certificação de habilitações ou qualificações <br><br> 450.30.502 -Emissão de declarações comprovativas <br><br> 450.30.003 -Emissão de certidões <br><br> 750.30.602 - Reconhecimento, creditação e validação de competências e qualificações 
| **Subject** | Assunto relacionado com o documento. Se não for definido na inserção passa a ser igual ao valor do Filename. | Não | - 
| **Language** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Português). | Não | PT (Português) <br><br> EN (Inglês) 
| **Tags** | Tags a associar a um documento para facilitar a pesquisa e a categorização do mesmo. | Não | - 
| **CollaboratorId** | NIF da Entidade colaboradora na produção do documento. | Não | - 
| **CollaboratorDesignation** | Nome da Entidade colaboradora na produção do documento. | Não | - 
| **CollaboratorType** | Pode ser definido com uma das chaves na lista de valores. | Não | CITIZEN (Cidadão) <br><br> PRIVATEENTITY (Entidade Privada) <br><br> PUBLICENTITY (Entidade Pública) 
| **CreationDate** | Data de criação do documento. | Não | - 
| **ResourceType** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “DOCPERSONAL” (Documento com informação pessoal). | Não | DOCPERSONAL (Documento com informação pessoal) <br><br> SERVAP (Resultado de serviço da AP) <br><br> SERVOTHER (Resultado de serviço (outro)) <br><br> DOCPROFESSIONAL (Documento com informação profissional)
| **Country** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “PT” (Portugal). | Não | PT (Portugal) 
| **SecurityClassification** | Pode ser definido com uma das chaves na lista de valores. Se não for definida na inserção é considerado o valor “RESERVED” (Reservado). | Não | RESERVED (Reservado) <br><br> NORESTRICTION (Sem restrições de acesso)
| **Price** | Preço associado ao documento. | Não | - 
| **ConservationTime** | Deve ser um valor numérico que representa o número de anos que o documento pode ser conservado na plataforma. Se não for definida na inserção é considerado o valor “5”. | Não | - 

### Parâmetros de saída
Na resposta do webservice é devolvido um ID atribuído ao documento na Plataforma Bolsa de Documentos do Cidadão.

### Exemplo
```markdown
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsdl="http://ama.pt/wsdl" xmlns:lar="http://pi.ama.pt/largefiles">
   <soapenv:Header/>
   <soapenv:Body>
      <wsdl:addDocumentToEntityAsCitizen>
         <addDocumentToEntityAsCitizenInput>
            <UserAuthentication>
              <Username>entidadeexterna</Username>
              <Password>123456</Password>
            </UserAuthentication>
            <DocumentToAdd>
               <Filename>teste</Filename>
               <MimeType>pdf</MimeType>
            </DocumentToAdd>
            <lar:AttachContext>
               <TTL>30</TTL>
               <FileGuid>3bb4d3bd-4aa5-4057-ba39-47fbc377b369</FileGuid>
            </lar:AttachContext>
            <from>
               <UserId>0000001</UserId>
               <UserType>NIC</UserType>
            </from>
            <to>
               <EntityId>508184509</EntityId>
               <EntityName>sdsda</EntityName>
               <EntityEmail>asdasda@sapo.pt</EntityEmail>
            </to>
         </addDocumentToEntityAsCitizenInput>
      </wsdl:addDocumentToEntityAsCitizen>
   </soapenv:Body>
</soapenv:Envelope>
```

##  ShareDocumentToEntityAsEntity
Este webservice tem como objetivo partilhar documentos entre entidades.

O seguinte diagrama representa o fluxo de invocação do webservice:

![ShareDocumentToEntityAsEntity - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/ShareDocumentToEntityAsEntity_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **To:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **EntityId** | Nipc da entidade | Sim | - 
| **EntityName** | Nome da entidade | Sim | - 
| **EntityEmail** | Email da entidade | Sim | - 

- **Outros parâmetros:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentIdList** | Id do documento | Sim | - 
| **HasWritePermission** | Permissão de escrita no documento | Não | True/false 
| **Process** | Se pretende apagar ficheiro apos enviar | Não | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

### Exemplo
```markdown
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsdl="http://ama.pt/wsdl">
   <soapenv:Header/>
   <soapenv:Body>
      <wsdl:shareDocumentToEntityAsEntity>
         <shareDocumentToEntityAsEntityInput>
             <UserAuthentication>
			  <EntityNif>508184509</EntityNif>
              <Username>entidadeexterna</Username>
              <Password>123456</Password>
            </UserAuthentication>
            <to>
               <EntityId>510150047</EntityId>
               <EntityName>dasas</EntityName>
               <EntityEmail>dasss@sapo.pt</EntityEmail>
            </to>
            <!--1 or more repetitions:-->
            <DocumentIdList>76465</DocumentIdList>
            <Process></Process>
            <HasWritePermission>true</HasWritePermission>
         </shareDocumentToEntityAsEntityInput>
      </wsdl:shareDocumentToEntityAsEntity>
   </soapenv:Body>
</soapenv:Envelope>
```

##  SendDocumentToCitizenAsCitizen
Este webservice tem como objetivo enviar documentos entre cidadãos.

O seguinte diagrama representa o fluxo de invocação do webservice:

![SendDocumentToCitizenAsCitizen - Diagrama](https://github.com/amagovpt/bolsadocumentos/blob/master/assets/images/SendDocumentToCitizenAsCitizen_diagrama.jpg?raw=true)

### Parâmetros de entrada
 - **From:**

| Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **UserType** | Tipo de utilizador | Sim | NIC, NON, NCS, NIPC, NOA 
| **UserId** | Id utilizador | Sim | - 

 - **To:**
 
 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **UserType** | Tipo de utilizador | Sim | NIC, NON, NCS, NIPC, NOA 
| **UserId** | Id utilizador | Sim | - 
| **UserEmail** | Email da entidade | Sim | - 
| **UserName** | Nome do utilizador | Sim | - 

- **Outros parâmetros:**

 | Nome do parâmetro | Descrição | Obrigatório | Lista de Valores (Chave – Descrição)
|--|--|--|-
| **DocumentId** | Id do documento | Sim | - 
| **DeleteDocumentFromOrigin** | Se pretende apagar ficheiro apos enviar | Sim | - 

### Parâmetros de saída
Na resposta do webservice é indicado o estado da execução do pedido.

### Exemplo
```markdown
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsdl="http://ama.pt/wsdl">
   <soapenv:Header/>
   <soapenv:Body>
      <wsdl:sendDocumentToCitizenAsCitizen>
         <!--Optional:-->
         <sendDocumentToCitizenAsCitizenInput>
             <UserAuthentication>
               <Username>entidadeexterna</Username>
               <Password>123456</Password>
            </UserAuthentication>
            <DocumentIdList>
               <DocumentId>86329</DocumentId>
               <DeleteDocumentFromOrigin>False</DeleteDocumentFromOrigin>
            </DocumentIdList>
            <from>
               <UserId>00000001</UserId>
               <UserType>NIC</UserType>
            </from>
            <to>
               <UserId>00000002</UserId>
               <UserName>sadad</UserName>
               <UserEmail>asdasda@sapo.pt</UserEmail>
               <UserType>NIC</UserType>
            </to>
         </sendDocumentToCitizenAsCitizenInput>
      </wsdl:sendDocumentToCitizenAsCitizen>
   </soapenv:Body>
</soapenv:Envelope>
```
