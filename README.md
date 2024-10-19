## Cenário: Limpeza dos dados e Categorização de Produtos
Neste exemplo, estou lidando com uma tabela de produtos (Products) que contém informações como título do produto e descrição. O objetivo é:

Identificar e categorizar os produtos de acordo com palavras-chave no título e descrição.
Filtrar e padronizar os títulos e descrições, removendo caracteres indesejados.
Detectar padrões que não estão formatados corretamente (exemplo: títulos muito curtos ou mal formatados).

### Criação do banco de dados Loja de Departamentos
    CREATE DATABASE DEPARTMENT_STORE
### Criação da tabela de produtos 
    CREATE TABLE Products(
    
          ProductID INT PRIMARY KEY,
          ProductName NVARCHAR(255),
          ProductDescription NVARCHAR(MAX)
    );
    SELECT * FROM Products
  ### 2. Inserção de dados exemplo
    
    INSERT INTO Products (ProductID, ProductName, ProductDescription)
    VALUES
    (1, 'Smartphone XYZ', 'Novo Smartphone XYZ com tela de 6 polegadas'),
    (2, 'Notebook ABC - Promoção', 'Notebook com 8GB RAM e 256GB SSD'),
    (3, 'Camiseta Azul (GG)', 'Camiseta azul tamanho GG, 100% algodão'),
    (4, 'Smartwatch Pro Edition!', 'Relógio inteligente com múltiplas funções'),
    (5, 'Promoção Liquidificador', 'Liquidificador 500W - Promoção até o fim do mês!'),
    (6, 'TV 50" UltraHD', 'Televisor 4K UltraHD com 50 polegadas');
   
   ### 3.Categorizar produtos com base em palavras-chave no título
       
     CASE
     WHEN ProductName LIKE '%Smartphone%' THEN 'Categoria: Eletrônicos'
     WHEN ProductName LIKE '%Notebook%' THEN 'Categoria: Computadores'
     WHEN ProductName LIKE '%Camiseta%' THEN 'Categoria: Vestuário'
     WHEN ProductName LIKE '%Smartwatch%' THEN 'Categoria: Eletrônicos'
     WHEN ProductName LIKE '%TV%' THEN 'Categoria: Eletrônicos'
     WHEN ProductName LIKE '%Liquidificador%' THEN 'Categoria: Eletrodomésticos'
     ELSE 'Categoria: Outros'
     END AS ProductCategory,

  ### 3.2 Verificar títulos de produto com palavras-chave mal formatadas ou muito curtas
    
      CASE
        WHEN LEN(ProductName) < 10 THEN 'Título Curto'
        WHEN ProductName LIKE '%!%' THEN 'Título Contém Exclamação'
        WHEN ProductName LIKE '%Promoção%' THEN 'Título Contém "Promoção"'
        ELSE 'Título Ok'
    END AS TitleValidation,

  ### 3.3 Padronizar descrições removendo termos desnecessários
    
    TRIM(REPLACE(REPLACE(ProductDescription, '-', ''), '!', '')) AS CleanedDescription,

  ### 3.4 Verificar descrições muito curtas ou sem informações relevantes
    CASE
        WHEN LEN(ProductDescription) < 30 THEN 'Descrição Muito Curta'
        ELSE 'Descrição Ok'
    END AS DescriptionValidation

    FROM Products;  


## Explicação do Script

Criação da Tabela e Inserção de Dados:

A tabela Products contém informações sobre produtos, como o nome e a descrição.
Os dados inseridos incluem exemplos de produtos de diferentes categorias, com alguns títulos e descrições formatados de forma inconsistente (como o uso de exclamações e termos promocionais).
Categorização de Produtos:

A função LIKE dentro de uma cláusula CASE para categorizar os produtos com base em palavras-chave encontradas no título do produto. Se a palavra "Smartphone", "Notebook", "Camiseta", "TV" ou "Liquidificador" estiver presente no nome do produto, ele será categorizado de acordo. Caso contrário, será classificado como "Outros".
Validação de Títulos de Produtos:

O script verifica o título do produto para detectar possíveis problemas. Por exemplo:
Títulos muito curtos (menos de 10 caracteres).
Títulos que contêm exclamações ("!").
Títulos que contêm a palavra "Promoção".
Isso ajuda a identificar entradas mal formatadas ou excessivamente promocionais.
Padronização de Descrições:

A função REPLACE() para remover caracteres indesejados, como hífens ("-") e exclamações ("!"), das descrições dos produtos.
A função TRIM() é usada para remover espaços em branco desnecessários no início ou fim das descrições.
Validação de Descrições:

A função LEN() para verificar se as descrições são curtas demais (menos de 30 caracteres), o que pode indicar falta de informações relevantes.




