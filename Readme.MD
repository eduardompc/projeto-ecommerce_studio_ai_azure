# projeto-ecommerce_studio_ai_azure
 Projeto bootcamp

Aprendizado Prático: Armazenamento de Dados de um E-commerce na Azure Cloud

Uma abordagem passo a passo para criar e gerenciar recursos na Azure

Introdução
A tecnologia de computação em nuvem tornou-se essencial para empresas que buscam escalabilidade, segurança e eficiência. Este aprendizado prático explora o armazenamento de dados de um e-commerce utilizando a Microsoft Azure Cloud. O objetivo principal é criar uma estrutura básica envolvendo um banco de dados SQL para dados transacionais, uma Storage Account para arquivos não estruturados e tabelas de produtos enquanto salvamos imagens no Blob Storage.

Passos para Implementar o Projeto

1. Criação de um Resource Group
O primeiro passo na Azure é organizar os recursos em um Resource Group (Grupo de Recursos). O Resource Group serve como um contêiner lógico para gerenciar todos os serviços de forma centralizada.
Etapas:
•	Acesse o portal da Azure (portal.azure.com).
•	Navegue até "Resource Groups" e clique em "Criar".
•	Escolha um nome para o grupo, como "EcommerceGroup".
•	Selecione a região mais próxima do público-alvo do e-commerce para otimizar a latência.
•	Finalize clicando em "Revisar + Criar".
2. Criação e Configuração do SQL Database
O SQL Database armazena dados estruturados, como informações sobre produtos, clientes e pedidos.
Etapas:
•	Dentro do Resource Group, clique em "Criar" e procure por "SQL Database".
•	Escolha um nome para o banco, como "EcommerceDB".
•	Crie ou selecione um servidor SQL. Por exemplo, "EcommerceServer" com autenticação SQL configurada.
•	Selecione o nível de desempenho (DTU ou vCore) com base nas necessidades do sistema. Para um projeto básico, comece com o nível "Basic".
•	Finalize a criação e aguarde alguns minutos para que o banco seja provisionado.
Configuração:
•	Acesse o banco de dados recém-criado e configure as regras de firewall para permitir acesso da máquina local ou de outros serviços.
•	Use o SQL Management Studio ou Azure Data Studio para conectar-se ao banco e criar tabelas.
Exemplo de script para criar uma tabela de produtos:
```sql
CREATE TABLE Produtos (
ProdutoID INT PRIMARY KEY IDENTITY(1,1),
Nome NVARCHAR(100),
Descricao NVARCHAR(255),
Preco DECIMAL(10, 2),
Estoque INT
);
```
3. Criação de uma Storage Account:

Para armazenar imagens dos produtos, criaremos uma Storage Account que disponibiliza o serviço Blob Storage.
Etapas:
•	No Resource Group, clique em "Criar" e procure por "Storage Account".
•	Nomeie a Storage Account, por exemplo, "ecommercestorage".
•	Escolha o tipo de armazenamento "Standard" e o desempenho "Hot" para acesso frequente.
•	Crie o recurso e configure os containers de Blob Storage.
Configuração do Blob Container:
•	Acesse a Storage Account e crie um container chamado "imagensprodutos".
•	Configure o container com permissões de acesso público, se necessário, ou mantenha-o privado para maior segurança.
4. Salvando Imagens no Blob Storage
Depois de configurar o Blob Storage, você pode fazer upload das imagens dos produtos.
Exemplo de upload de arquivo via Azure Portal:
•	Acesse o container "imagensprodutos".
•	Clique em "Carregar" e selecione as imagens a serem enviadas.
•	Recupere o URL gerado para acessar a imagem.
Exemplo de código para upload via SDK Azure em Python:
```python
from azure.storage.blob import BlobServiceClient
# Conexão com a Storage Account
connection_string = "sua_connection_string_aqui"
blob_service_client = BlobServiceClient.from_connection_string(connection_string)
# Upload de uma imagem
blob_client = blob_service_client.get_blob_client(container="imagensprodutos", blob="produto1.jpg")
with open("produto1.jpg", "rb") as file:
blob_client.upload_blob(file)
print("Imagem enviada com sucesso!")
```


5. Integração Banco de Dados e Imagens
Armazene os URLs das imagens no banco de dados SQL para associá-los aos produtos.
Exemplo:
```sql
INSERT INTO Produtos (Nome, Descricao, Preco, Estoque)
VALUES ('Produto 1', 'Descrição do Produto 1', 59.99, 100);
UPDATE Produtos
SET URLImagem = '[URL]'
WHERE ProdutoID = 1;
```
Exemplo Prático:

Imagine um e-commerce de produtos eletrônicos. Após criar o Resource Group "EcommerceGroup" e provisionar os recursos necessários (SQL Database e Storage Account), você pode estruturar as tabelas e começar a popular o banco com informações de produtos. Por exemplo:
•	Produto: "Fone de Ouvido Bluetooth"
•	Descrição: "Fone de ouvido sem fio, bateria de 12h, som estéreo."
•	Preço: R$ 199,99
•	Estoque: 50 unidades
•	Imagem: Enviada para o container "imagensprodutos".
URL da imagem salva:
`[URL]`
Esses dados podem ser integrados ao front-end do e-commerce, permitindo que clientes visualizem informações e imagens dos produtos.


Conclusão:

Este projeto prático demonstra como a Microsoft Azure Cloud pode ser utilizada para criar uma solução robusta e escalável de armazenamento de dados para um e-commerce. O SQL Database fornece um banco estruturado para transações e o Blob Storage oferece um repositório confiável para arquivos não estruturados, como imagens. Essa abordagem não só melhora a organização dos dados, mas também garante alta disponibilidade e segurança. Com as ferramentas da Azure, é possível expandir facilmente o sistema para atender a demandas crescentes, tornando-o uma escolha ideal para negócios online.
Além disso, é possível integrar funcionalidades para exibir imagens diretamente no front-end, melhorando a experiência visual dos clientes. Utilizando o Azure Blob Storage, as imagens dos produtos podem ser carregadas rapidamente, garantindo uma apresentação dinâmica e atrativa. Essa funcionalidade não só valoriza a aparência do e-commerce, mas também facilita a tomada de decisão por parte dos clientes, que podem visualizar os detalhes dos produtos de forma clara e eficiente.

*Anexo Imagens
