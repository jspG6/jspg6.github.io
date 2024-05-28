## Modificações para Criar um Cubo Giratório Colorido

Para transformar o código original em um programa que exibe um cubo colorido girando continuamente, você precisará fazer as seguintes alterações:

**1. Definição dos Vértices e Cores do Cubo:**

Substitua a definição dos vértices (`vertices`) e cores (`colors`) do triângulo pelos vértices e cores de um cubo. Um cubo possui 8 vértices e cada face tem uma cor diferente.

```c
GLfloat vertices[] = {
    // Frente
    -0.5f, -0.5f, -0.5f, // 0
     0.5f, -0.5f, -0.5f, // 1
     0.5f,  0.5f, -0.5f, // 2
    -0.5f,  0.5f, -0.5f, // 3
    // Trás 
    -0.5f, -0.5f,  0.5f, // 4
     0.5f, -0.5f,  0.5f, // 5
     0.5f,  0.5f,  0.5f, // 6
    -0.5f,  0.5f,  0.5f  // 7
};

GLfloat colors[] = {
    1.0f, 0.0f, 0.0f, // Vermelho
    0.0f, 1.0f, 0.0f, // Verde
    0.0f, 0.0f, 1.0f, // Azul
    1.0f, 1.0f, 0.0f, // Amarelo
    1.0f, 0.0f, 1.0f, // Magenta
    0.0f, 1.0f, 1.0f  // Ciano
};
```

**2. Definição dos Índices das Faces:**

O OpenGL desenha primitivas (pontos, linhas, triângulos). Para formar as faces do cubo, precisamos dividir cada face em dois triângulos. O array `indices` define a ordem em que os vértices serão agrupados para formar esses triângulos. 

Cada grupo de 3 números no array `indices` representa um triângulo. Os números são índices que se referem aos vértices definidos no array `vertices`. Por exemplo, `0, 1, 2` significa que o primeiro triângulo será formado pelos vértices 0, 1 e 2 do array `vertices`.


```c
GLuint indices[] = {
    0, 1, 2, 2, 3, 0, // Face frontal
    4, 5, 6, 6, 7, 4, // Face traseira
    1, 5, 6, 6, 2, 1, // Face direita
    0, 4, 7, 7, 3, 0, // Face esquerda
    3, 2, 6, 6, 7, 3, // Face superior
    0, 1, 5, 5, 4, 0  // Face inferior
};
```

**3. Criação do Element Buffer Object (EBO):**

O EBO é um buffer que armazena os índices das faces do cubo. Ele permite que a GPU renderize o cubo de forma mais eficiente, pois ela só precisa processar os vértices necessários para cada triângulo.

A função `glGenBuffers` cria um novo buffer e armazena seu identificador na variável `EBO`. A função `glBindBuffer` vincula o EBO ao contexto OpenGL, e `glBufferData` copia os dados do array `indices` para o EBO.


```c
GLuint EBO;
glGenBuffers(1, &EBO);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

**4. Rotação Contínua:**

Para criar a animação de rotação, precisamos modificar a matriz de modelo do cubo em cada quadro da renderização. A matriz de modelo define a posição, orientação e escala do objeto.
A variável `angle` armazena o ângulo de rotação atual. Em cada iteração do loop principal, o ângulo é incrementado e a matriz de modelo é atualizada usando `glm_rotate`. A função `glm_rotate` aplica uma rotação à matriz de modelo em torno dos eixos X, Y e Z.


```c
float angle = 0.0f;
while (isRunning) {
    // ... (tratamento de eventos) ...

    angle += 0.5f; // Incrementa o ângulo de rotação

    // Atualiza a matriz de modelo com a nova rotação
    glm_mat4_identity(modelMatrix);
    glm_rotate(modelMatrix, glm_rad(angle), (vec3){1.0f, 1.0f, 1.0f}); // Rotação em X, Y e Z
    // ... (restante da configuração das matrizes) ...

    // ... (limpeza e renderização) ...

    // Renderiza o cubo usando os índices
    glDrawElements(GL_TRIANGLES, 36, GL_UNSIGNED_INT, 0); 

    // ... (atualização da janela) ...
}
```

**5. Renderização com Índices:**

A função `glDrawElements` é usada para renderizar o cubo usando os índices armazenados no EBO. Isso permite que a GPU desenhe apenas os triângulos necessários para formar o cubo, em vez de processar todos os vértices individualmente.
A função `glDrawElements` recebe o tipo de primitiva (GL_TRIANGLES), o número de índices (36, pois temos 12 triângulos com 3 vértices cada), o tipo de dados dos índices (GL_UNSIGNED_INT) e um offset (0, pois começamos a ler os índices do início do EBO).

```c
glDrawElements(GL_TRIANGLES, 36, GL_UNSIGNED_INT, 0); 
```
