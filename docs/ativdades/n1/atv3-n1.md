# UPM CC - Computação Visual 07N (2024.1)

[Página Inicial](../../index.md)

## Semana 3: Online - Teoria (Formatos de imagem)

### Enunciado

Nessa semana, vimos que os processos de amostragem e quantização são realizados para converter dados contínuos (capturados por sensores de aquisição de imagens) em dados discretos, resultando na geração de imagens digitais.

A discretização converte uma função contínua *f(s, t)* em uma matriz bidimensional *f(x, y)* com *MxN* elementos, sendo:
- *M*: Número de linhas da imagem.
- *N*: Número de colunas da imagem.
- *(x, y)*: Coordenadas discretas.
- *x* = 0, 1, 2, ..., *M* – 1.
- *y* = 0, 1, 2, ..., *N* – 1.

Cada elemento da matriz bidimensional (e da imagem digital) corresponde a um pixel.

No entanto, quando trabalhamos com arquivos de imagem (isto é, o conteúdo desses arquivos é interpretado como a representação de uma imagem digital), eles raramente armazenam apenas as informações dos pixels que formam a imagem.

**Sabendo disso, escolha um formato de arquivo de imagem e descreva como o conteúdo do arquivo é organizado. Na descrição, detalhe as principais seções do formato de arquivo de imagem escolhido.**

Lembre-se de incluir todas as referências consultadas (livros, links de artigos, vídeos, etc.) e identificar todas as pessoas do grupo.

----
## Resposta

Estrutura de um arquivo escolhida:  [ JPEG ]

Cabeçalho (Header):

    Inicia o arquivo e contém informações básicas sobre o arquivo, como o tipo de arquivo e os parâmetros da imagem.

Segmentos de Dados (Data Segments):

- DQT (Define Quantization Table): Tabela de quantização que afeta a compressão da imagem.

- SOF (Start of Frame): Contém informações sobre a imagem, como tamanho, número de componentes, etc.

- DHT (Define Huffman Table): Tabelas de Huffman para a compressão sem perdas.

- SOS (Start of Scan): Indica o início dos dados da imagem.

- Compressed Image Data: Os próprios dados da imagem, comprimidos usando as tabelas de quantização e Huffman.


Thumbnail (Miniatura):

    Alguns arquivos JPEG podem incluir uma miniatura da imagem principal, permitindo uma visualização rápida sem abrir o arquivo completo.

Comentários:

    Espaço opcional para incluir comentários ou metadados sobre a imagem.

Trailer:

    Marca o final do arquivo, indicando que não há mais dados a serem lidos.


Compactação:

- A parte central do arquivo JPEG é a seção de dados comprimidos. Aqui, os blocos de pixels da imagem são transformados usando a Transformada de Cosine Discreta (DCT) e depois quantizados.
- A quantização reduz a precisão dos valores dos pixels, o que permite uma compressão significativa sem perda perceptível de qualidade.

Tabelas de Huffman:

- As tabelas de Huffman são usadas para codificar os valores quantizados.
- Os valores mais comuns são representados por códigos de comprimento menor, enquanto os menos comuns por códigos de comprimento maior, ajudando na compactação.


Decodificação:

- Quando um visualizador abre o arquivo JPEG, ele decodifica os dados, desfazendo o processo de compactação.
- Isso envolve ler os segmentos de dados, aplicar as tabelas de Huffman reversamente e aplicar a DCT inversa para reconstruir a imagem.



# Referências 
-----




Especificação do Padrão JPEG: https://jpeg.org/jpeg/

Formato de Arquivo JPEG: https://www.adobe.com/cl/creativecloud/file-types/image/raster/jpeg-file.html

Compressão JPEG: https://compressjpeg.com/

Vantagens e Desvantagens do JPEG: https://www.alefotografo.com.br/blog/jpeg-ou-png-vantagens-e-desvantagens-dos-dois-formatos-de-imagens
