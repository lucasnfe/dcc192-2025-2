---
layout: page
title:  TP1 - Pong
permalink: /avaliacoes/tp1-pong
nav_exclude: true
---

# TP1: Pong

#### Entrega: 15/09/2025 às 11:59h

## Introdução

O Pong, desenvolvido pela Atari em 1972, é um dos primeiros e mais populares jogos da era do arcade. Ele  simula um jogo de tênis de mesa, onde cada jogador controla verticalmente uma raquete posicionada em uma das extremidades da tela, com o objetivo de rebater uma bola de tal maneira que o oponente não consiga rebater de volta. Cada vez que um jogador não conseguir rebater a bola, o oponente receberá um ponto. O jogo termina quando um dos jogadores completar 11 pontos. Tanto as raquetes e a bola quanto as marcações de meio de campo e de pontuação são representados por retângulos brancos. O video a seguir mostra um *gameplay* do jogo original:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/e4VRgY3tkh0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="">
    </iframe>
</div>

## Objetivo

Nesse projeto, você irá desenvolver uma versão de 1 jogador do jogo Pong em C++/OpenGL. Nessa versão, o jogador controla a raquete com o objetivo de rebater a bola contra a parede o maior número de vezes possível. Primeiro, você irá implementar um renderizador (*renderer*) em OpenGL e GLSL. Em seguida, irá criar o laço principal do jogo (*game loop*) com uma taxa de quadros (*framerate*) fixa, que processa entradas do teclado, atualiza os objetos do jogo e renderiza os quadros. Por fim, irá implementar os objetos de jogo de forma que eles consigam ser desenhados e atualizados independentes uns dos outros. O video a seguir mostra um gameplay da versão que você irá implementar:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/rdvG8ZS2C_I?si=5q_fK6GvFFT9vXoR" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

## Código-base

Siga as instruções abaixo para baixar o código-base desse trabalho prático:

1. Aceite o projeto *tp1-pong* no GitHub classroom [[nesse link]](https://classroom.github.com/a/tSVPjUqj)
2. Clone o seu novo repositório no seu computador:

    ```
    # Substitua <GITHUB_USERNAME> pelo seu usuário do GitHub
    git clone https://github.com/ufmg-dcc192/tp1-pong-<GITHUB_USERNAME>.git
    ```

3. Abra o projeto *tp1-pong* na CLion e, antes de começar a sua implementação, verique com cuidado as definições de métodos e atributos de cada classe:

    - **Renderer**

        Cria a janela do jogo, inicializar o contexto OpenGL, instanciar os shaders e renderizar objetos na tela.

    - **Shader**

        Classe que encapsula as funções básicas de shaders, como carregamento, compilação, ativação, descarregamento e a alteração de variáveis uniforms.

    - **VertexArray**

        Estrutura de dados para facilitar a criação de buffers na GPU.

    - **Game**

        Classe responsável por inicializar, gerenciar o laço principal e finalizar o jogo. 

    - **Actor**

        Classe base para todos os objetos do jogo, contendo atributos para transformação (translação, rotação e escala) e métodos para processamento de eventos de entrada, atualização e desenho. 

    - **Ball**

        Classe que estende Actor para representar a bola do jogo Pong.
        
    - **Paddle**

        Classe que estende Actor para representar a raquete do jogo Pong.

    - **Math**

        Classes e funções implementando todas as operações matemáticas necessárias para o desenvolvimento dos jogos dessa disciplina.

    - **Random**

        Classe para geração de números pseudo-aleatórios.

## Instruções

### **Parte 1: Game Loop**

Na primeira parte, você irá implementar o laço principal do jogo utilizando uma abordagem de taxa de quadros fixa. Ao final dessa etapa, você será capaz de abrir uma janela SDL mas sem conseguir desenhar nada nela ainda.

1. **Game.cpp**

    1. Implemente o método `Game::Initialize()` para inicializar o subsistema de vídeo da SDL e criar uma janela. O tamanho da janela está especificado nas variáveis estáticas `WINDOW_WIDTH` e `WINDOW_HEIGHT`. Armazene o ponteiro para essa janela na variável `mWindow`. Note que variáveis começando com a letra m representam atributos, ou membros, da classe. Se qualquer uma dessas etapas falhar, imprima uma mensagem de erro na tela com a função [SDL_GetError](https://wiki.libsdl.org/SDL2/SDL_GetError) e retorne `false`. Se tudo correr bem, inicalize o atributo `mTicksCount` com o tempo em  milissegundos decorridos desde a inicialização da biblioteca SDL. Esse tempo pode ser obtido com a função [SDL_GetTicks](https://wiki.libsdl.org/SDL2/SDL_GetTicks). 

    2. Implemente o método `Game::RunLoop()` para executar o laço principal do jogo enquanto o atributo `mIsRunning` for verdadeiro. Dentro do laço, execute os métodos `ProcessInput()`, `Update()` e `GenerateOutput()` nessa ordem.

    3. Implemente o método `Game::ProcessInput()` para processar os eventos de entrada do jogador. Nesse momento, basta usar um loop while para processar os eventos de entrada com a função `SDL_PollEvent` e verificar se algum evento do tipo `SDL_QUIT` aconteceu. Se acontecer, alterar o atributo `mIsRunning` para `false`. Lembre-se que é ela que controla o laço do jogo. Em seguida, use a função `SDL_GetKeyboardState` para acessar o estado do teclado. Utilize esse estado para verificar se o jogador apertou a teclar `ESC` e em caso positivo, altera o atributo `mIsRunning` para `false` também.

    4. Implemente o método `Game::Update()` para controlar a taxa de atualização de quadros. Utilize a função [SDL_TICKS_PASSED](https://wiki.libsdl.org/SDL2/SDL_TICKS_PASSED) em um loop while para garantir que pelo menos 16 milissegundos tenham se passado desde o último quadro. Lembre-se que o a variável `mTicksCount` armazena o tempo de término do último quadro. Em seguida, calcule o tempo em segundos entre o quadro atual e o passado, armazenando o resultado em uma variável `float deltaTime`. 
    Verifique se deltaTime é superior a 0.05 segundos e, se for, limite-a para 0.05 segundos. Essa verificação estabiliza a execução do jogo em situações de debug com break points. Por fim, atualizar o contador de tempo `mTicksCount` com a função `SDL_GetTicks()`, como na etapa 1.

Ao final dessa etapa, você deverá ver uma janela com fundo preto que pode ser fechada com o ícone na barra superior ou com a tecla `ESC`. Além disso, se você imprimir (`SDL_Log`) o valor da variável `deltaTime` dentro da função `Game::Update()`, você deverá ver valores próximos a `0.016`, como ilustram as imagens a seguir:

<img src="{{ 'assets/images/tp1/1-result.png' | relative_url }}" alt="tp1-pong-1" width="560"/>

### **Parte 2: Renderizador**

Na segunda parte, você irá implementar o renderizador para desenhar objetos na tela, começando pelas classes auxiliares `Shader` e `VertexArray`, que serão utilizadas na classe `Renderer` para gerenciamento de shaders e criação de buffers na GPU, respectivamente. Os shaders que usaremos nesse TP estão definitos em arquivos externos no diretório `Shaders` do projeto CLion.

1. **Shader.cpp**

    1. Implemente a função `Shader::Load()` para criar um programa na GPU que irá rodar os shaders de  vértice e fragmento. Como vimos em sala, para criar um programa na GPU, você terá que compilar os shaders. Para isso, a função `CompileShader` já foi implementada para você. Ela carrega o código dos shaders de arquivos de texto externos, assumindo que os shaders de vértice e fragmento tem o mesmo nome, mudando apenas a extensão: `.vert` e `.frag`. Além disso, note que o último parâmetro `CompileShader` é o endereço da variável que irá armazenar o ID do shader compilado. Armazene os IDs dos shaders nos atributos `mVertexShader` e `mFragShader` da classe. Após compilar os shaders, crie um programa na GPU com a função `glCreateProgram()` e armazena o ID desse programa no atributo `mShaderProgram`. Em seguida, anexo os shaders `glAttachShader()` e ligue o programa `glLinkProgram()`.

    2. Implemente a função `Shader::Unload()` para deletar o programa atualmente carregado e deletar os shaders associados. Isso pode ser feito com as função `glDeleteProgram()` e `glDeleteShader()`, respectivamente. Além disso, atribua o valor zero (0) aos atributos `mShaderProgram`, `mVertexShader` e `mFragShader`.

    3. Implementa a função `Shader::SetActive()` para ativar o programa atualmente carregado na GPU. Isso pode ser feito com a função `glUseProgram()`.    

    4. Implemente a função `Shader::SetMatrixUniform()` para alterar o valor de uma variável uniform nos shaders. Isso pode ser feito com as funções `glGetUniformLocation()` e `glUniformMatrix4fv()`. 

2. **VertexArray.cpp**

    1. Implemente o construtor da classe para alocar e ativar os VAO, VBO e EBO na GPU como visto em aula. 
    Utilize as variáveis `mVertexArray`, `mVertexBuffer` e `mIndexBuffer` para armazenar as referências dessas regiões de memória na GPU. Lembre-se de respeitar a ordem da criação dessas regiões de memória vista em sala de aula. Ou seja, primeiro aloque `glGenVertexArrays()` e ligue `glBindVertexArray()` o VAO, depois aloque `glGenBuffers()`, ligue `glBindBuffer()` e tranfira `glBufferData()` os dados dos buffers VBO e EBO. Ao final, habilite o VAO `glEnableVertexAttribArray()` e o configure `glVertexAttribPointer()` para especificar para a GPU como os vértices devem ser lidos pelo vertex shader. Dica: siga os slides 8-11 da aula 4.

    2. Implemente o destrutor da classe para deletar os VAO, VBO e EBO. Isso pode ser feito com as funções `glDeleteVertexArrays()` e `glDeleteBuffers()`.

    3. Implemente a função auxiliar `VertexArray::SetActive()` para ativar o VAO e o EBO, representados pelos atributos `mVertexArray` e `mIndexBuffer`. Isso pode ser feito com as funções `glBindVertexArray()` e
    `glBindBuffer()`.

3. **Renderer.cpp**

    Agora que classes auxiliares `Shader` e `VertexArray` estão implementadas, você pode finalmente implementar a classe do renderizador. Vamos começar pelas funções auxiliares, que usaremos depois para inicializar o renderizador. Ao final, iremos chamar as funções de renderização do `Renreder` no loop do jogo.

    1. Implemente a função auxiliar `Renderer::LoadShaders()` para instanciar os shaders de vértice e fragmento. Para isso, crie um objeto da classe `Shader` e o armazene no atributo `mBaseShader`. Depois chame a função `Load` desse objeto passando uma único caminho para ambos os shaders (lembre-se que eles possuem o mesmo nome, apenas extensões diferentes). 
    
    2. Implemente a função auxiliar `Renderer::CreateRectVertices()` para criar um retângulo que será utilizados para desenhar todos os objetos do pong (raquetes e bolinha). Para isso, você terá que alocar um arranjo com oito floats representando as coordenadas x e y dos 4 vértices do retângulo. Defina os vértices seguindo o sentido horário. Em seguida, crie um arranjo de inteiros para definir os índices dos dois triângulos que compõe o retângulo. Especifique os índices em ordem crescente. Por fim, crie um novo objeto da classe `VertexArray` para armazenar esses vértices e índices. Guarde o endereço desse objeto no atributo `mRectVertices`.
    
    3. Complete a função `Renderer::Initialize()` para criar um contexto OpenGL (`SDL_GL_CreateContext`), inicializar a biblioteca GLEW na janela passada para o construtor (`glewInit()`) e carregar os shaders ( `LoadShaders`). Essas operações devem ser feitas exatamente nesse ordem. Em seguida, crie os vértices de retângulos (`CreateRectVertices`) e os ative, lembrando que eles estão armazenados no atributo `mRectVertices`. Em seguida, defina a cor de fundo da tela para preto com a função `glClearColor`. Isso tudo deve ser feito antes da criação da matriz de projeção ortográfica `Matrix4::CreateOrtho`. Iremos falar mais sobre ela nas próximas aulas.

    4. Implemente a função `Renderer::Clear()` para limpar a tela. Basta chamar a função `glClear()` passando a máscara `GL_COLOR_BUFFER_BIT`.

    5. Implemente a função `Renderer::Draw()` para desenhar um objeto na tela. Note que ela recebe como parâmetro a matriz do modelo. Assim, chame a função `SetMatrixUniform` do shader ativo na GPU (`mBaseShader`) para alterar a variável uniform no shader que representa essa matriz. Em seguida, utilize a função `glDrawElements` para desenhar o objeto usando os vértices ativos na GPU, representados pelo atributo `mRectVertices`. Note que esse atributo possui um método auxiliar `GetNumIndices()` que retorna o número de vértices.

    6. Implemente a função `Renderer::Present()` para trocar o back buffer com o front buffer (double buffering), mostrando os objetos desenhados na tela.

    7. Implemente a função `Renderer::Shutdown()` para descarregar os dados do renderizador. Para isso, chame a função `Unload` do shader ativo `mBaseShader` e em seguida delete esse objeto. Delete também o objeto que armazena os vértices e depois destrua o contexto OpenGL (`SDL_GL_DeleteContext`) e destrua a janela (`SDL_DestroyWindow`).

4. **Game.cpp**

    Agora que temos o renderer pronto, podemos instanciar um objeto desse tipo como atributo da classe Game e chamá-lo no laço principal do jogo para desenhar objetos. 

    1. No método `Game::Initialize()`, instancie um objeto do tipo `Renderer` depois da criação da janela e antes da inicialização do relógio (`mTicksCount`). Armazene o endereço desse objeto no atributo `mRenderer`. Em seguiga, inicialize essa  nova instância com a função `mRenderer->Initialize()`.

    2. Como vimos em aula, a cada quadro, o jogo limpa a tela, desenha os objetos do jogo no back buffer e depois troca esse buffer como front buffer. Implemente esse fluxo no método `GenerateOutput()`. Basta chamar a função `mRenderer->Clear()` do renderer, seguida da função `mRenderer->Present()`.

    3. No método `Game::Shutdown()`, destrua o renderizador chamando seu o método `mRenderer->Shutdown()`. Em seguida, delete a instância do renderizador e atribua NULL ao seu ponteiro.

Ao final dessa etapa, você deveria ser capaz de desenhar retângulos na tela. Para testar a sua implementação, crie uma matriz de modelo e chame a função Draw do renderizador entre as funções `mRenderer->Clear()` e `mRenderer->Present()`, por exemplo:

```
mRenderer->Clear();

Vector3 pos = Vector3(WINDOW_WIDTH/2.0f, WINDOW_HEIGHT/2.0f, 0.0f);
Matrix4 model = Matrix4::CreateScale(100, 100, 1.0f) *
                Matrix4::CreateTranslation(pos);

mRenderer->Draw(model);
mRenderer->Present();
```

Esse trecho cria um quadrado de lado 100 pixels centralizado na tela. Portanto, o resultado deveria ser o seguinte:

<img src="{{ 'assets/images/tp1/2-result.png' | relative_url }}" alt="tp1-pong-1" width="560"/>

Note que o renderizador está usando um sistema de coordenadas diferente do que o sistema de coordenadas padrão da OpenGL. No sistema padrão, a origem do sistema `(0,0)` é o centro da tela e os cantos da tela estão a distância 1 do centro. Esse novo sistema é definido em relação ao tamanho da tela (em pixels). Portanto, a origem do sistema `(0,0)` é o canto esquerdo superior da tela e o canto direito superior é a coordenada `(SCREEN_WIDTH, SCREEN_HEIGHT)`. Em desenvolvimento de jogos, é mais comum trabalhar nesse sistema de coordenadas da tela, pois ele facilita a implementação das mecânicas do jogo. Essa transformação de sistemas de coordenadas é realizada pela matriz `uOrthoProj` do shader de vértices. Estudaremos como essa matriz é construída ela em aulas seguintes.
    
### **Parte 3: Objetos do Jogo**

Na terceira etapa, você irá implementar as classes que representam os objetos do jogo: `Paddle` (raquete) e `Ball` (bolinha). Para isso, iremos criar uma classe abstrata chamada `Actor`, contendo atributos e métodos que geralmente são comuns a todos os objetos: processamento de entrada, atualização de estado e desenho na tela. Essa classe também será responsável pela transformação geométrica dos objetos.  

1. **Actor.cpp**

    1. Implemente a função `Actor::ProcessInput()` para acionar a função da classe filha que implementa a lógica de processamento de entrada desse objeto. As classes filhas (ex. `Paddle`) devem implementar a lógica de entrada na função `OnProcessInput()`, que só deve ser acionada caso o estado do objeto (`mState`) for ativo (`ActorState::Active`).  

    2. Implemente a função `Actor::Update()` para acionar a função da classe filha que implementa a lógica de atualização do estado desse objeto. De maneira similar ao `ProcessInput`, As classes filhas (ex. `Paddle`) devem implementar a lógica de atualização de estado na função `OnUpdate`.

    3. Implemente a função `Actor::GetModelMatrix()` para calcular a matriz de modelo para esse objeto. Lembre-se que você deve executar as multiplicações de matrizes na seguinte ordem: S X R X T. Note que a classe Actor possui atributos `mScale`, `mRotation` e `mPosition` que armazenam a escala, rotação e posição atuais do objeto. Note também os atributos `mWidth` e `mWidth`, que devem ser multiplicados pelo fator de escala na hora de criar a matriz de escala.

    4. Implemente a função `Actor::Draw` para desenhar o objeto na tela. Aqui, basta chamar o método `Renderer::Draw()` do renderer passando a função `Actor::GetModelMatrix()` como argumento.

2. **Paddle.cpp**

    1. Implemente os métodos `Paddle::OnProcessInput()` e `Paddle::OnUpdate()` para mover a raquete  verticalmente com base nas teclas `W` e `S`. A posição vertical da raquete deve ser limitada para garantir que o jogador não consiga movê-la para fora da tela. Sinta-se à vontade para pode criar atributos auxiliares para implementar esse mecânica de movimento.
    
3. **Ball.cpp**

    1. Implemente o métodos `Ball::OnUpdate()` para mover a bolinha na quadra, de forma que quando ela colidir com os limites superior, inferior e direito da tela, a sua direção seja refletida. Quando ela passar pelo limite esquerdo, o jogo deve fechar (veja a função `Quit()` da classe `Game`). Quando ela colidir com a raquete, a sua direção também deve ser reletida. A bolinha deve começar se movendo para a direita e aumentar a sua velocidade toda vez que ela colidir com a raquete.

4. **Game.cpp**

    1. Implemente o método `Game::LoadData()` para instanciar e inicilizar (ex. posição) objetos para a bolinha e a raquete. Armazene os endereços desses objetos em atributos da classe `Game`. Chame esse método durnte a inicialização do jogo `Game::Initialize()` antes da inicialização do relógio `mTicksCount`.
    
    2. No método `Game::ProcessInput()`, chame a função `Paddle::ProcessInput()` do objeto da raquete.
    
    3. No método `Game::Update()`, chame as funções `Paddle::Update()` e `Ball::Update()` dos objetos da raquete e da bolinha, respectivamente. Essas chamadas devem ser feitas ao final da atualização do jogo. 

    4. No método `Game::GenerateOutput()`, chame as funções `Paddle::Draw()` `Ball::Draw()` dos objetos da raquete e da bolinha, respectivamente. Essas chamadas devem estar entre a limpeza da tela e a troca dos buffers. 
    
    5. Implemente o método `Game::UnloadData()` para deletar os objetos do jogo, setando seus ponteiros para NULL.

Essa etapa conclui as implementações básicas do jogo, que deveria ter um resultado como o mostrado no vídeo do início do roteiro.

### **Parte 4: Customização**

Na quarta, e última etapa, você irá customizar a implementação básica para criar uma versão exclusiva do Pong.

1. Adicione uma raquete do lado direito para ser o segundo jogador. Ela deve ser controlada com as setas de cima e baixo do teclado. Modifique a lógica da bolinha para ela não refletir sua direção quando colidir com o lado direito da tela.

2. Modifique as cores da bolinha e das raquetes. Elas devem obrigatoriamente ter cores diferentes. Isso deve ser feito adicionando uma variável uniform no shader de fragmento.

3. Modifique o formato da bolinha para que ela seja um círculo e não um retângulo. Dica: para isso, você precisará manter dois conjuntos de vértices na GPU e alternar a ativação deles durante a renderização.  

4. Reinicie a partida ao invés de fechar o jogo quando a bolinha passar pelos cantos horizontais da tela.

## Submissão

Para submeter o seu trabalho, basta fazer o commit e o push das suas alterações no repositório que foi criado para você no GitHub classroom.

Observação: você pode realizar quantos commits quiser. Isso é inclusive recomendado para dividir o problema em partes menores. Além disso, as mensagens nos commits não precisam ser necessariamente "Submissão TP1". Você pode também criar branches se quiser, mas **apenas o último commit da branch `main` será avaliado**.


```
git add .
git commit -m 'Submissão TP1'
git push
```

## Barema

- Parte 1: Game Loop (10%)
- Parte 2: Renderizador (40%)
- Parte 3: Objetos do Jogo (20%)
- Parte 4: Customização (30%)

## Referências

- Parte 1: 
    - [Game Programming in C++, Cap 1: The Game Loop and Game Class](https://learning.oreilly.com/library/view/game-programming-in/9780134598185/ch01.xhtml#ch01lev2sec3)
    - [Game Programming in C++, Cap 1: Updating the Game](https://learning.oreilly.com/library/view/game-programming-in/9780134598185/ch01.xhtml#ch01lev2sec11)
    - [Game Programming in C++, Cap 2: Game Objects](https://learning.oreilly.com/library/view/game-programming-in/9780134598185/ch02.xhtml#ch02lev2sec1)    
