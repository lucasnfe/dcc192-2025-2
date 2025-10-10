---
layout: page
title:  TP3 - Super Mario Bros 1-1
permalink: /avaliacoes/tp3-smb
nav_exclude: true
---

# TP3: Super Mario Bros 1-1

#### Entrega: 24/10/2025 às 23:59h

## Introdução

O Super Mario Bros (SMB), lançado pela Nintendo em 1985, foi um dos jogos mais populares da era dos consoles 8 bits. SMB é um jogo de plataforma onde o objetivo do jogador é se mover para a direita para chegar a um mastro de bandeira no final de cada nível. O jogador controla o Mario, protagonista da série. O irmão de Mario, Luigi, é controlado pelo segundo jogador no modo multijogador e assume o mesmo papel e funcionalidade do Mario. Na narrativa do jogo, o mundo é chamado de Reino do Cogumelo e o Mario está atravessando-o para salvar a Princesa Peach do antagonista Bowser. O video a seguir mostra um *gameplay* do jogo original:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/7qirrV8w5SQ?si=uaXtyaT-f2IL-16m" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Objetivo

O objetivo desse projeto é praticar a implementação de integração com editores externos, movimentação de câmera, detecção de colisão com AABBs e animações 2D. Para isso, você irá implementar as mecânicas básicas de correr, pular, e acertar inimigos no primeiro nível do SMB. Primeiro, você irá implementar um parser para carregar objetos do jogo a partir de arquivos csv exportados do editor de mapas [[Tiled]](https://www.mapeditor.org/). Como parte dessa tarefa, você irá implementar o componente `AnimatorComponent`, que inicialmente irá desenhar apenas sprites estáticos (não animados) na tela. Segundo, você irá implementar a movimentação de câmera, garantindo que o personagem sempre fique a frente da câmera. Terceiro, você irá implementar o componente `AABBColliderComponent` para detectar colisões entre caixas delimitadoras alinhadas com os eixos (AABBs). Quarto, você irá incrementar o componente `AnimatorComponent` para desenhar animações 2D utilizando sprite sheets gerados pela [[FreeTexturePacker]](https://free-tex-packer.com/app/). Por fim, você irá implementar os goombas, incluindo as mecânicas de matá-los quando o jogador pula em cima deles e de matar o jogador quando eles o acertam no chão. O vídeo a seguir mostra um gameplay da versão que você irá implementar:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/CK-fD2JSvMo?si=u1GGxFjZxB4p7iwH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

## Código-base

1. Aceite o projeto *tp3-super-mario-bros-1-1* no GitHub classroom [[nesse link]](https://classroom.github.com/a/R6H2vFsw)

2. Clone o seu novo repositório no seu computador:

    ```
    # Substitua <GITHUB_USERNAME> pelo seu usuário do GitHub
    git clone https://github.com/ufmg-dcc192/tp3-super-mario-bros-1-1-<GITHUB_USERNAME>.git
    ```

3. Abra o projeto *tp3-super-mario-bros-1-1* na CLion e, antes de começar a sua implementação, verique com cuidado as definições de métodos e atributos de cada classe:

    - **AnimatorComponent**

        Componente para desenhar game objects com sprites estáticos ou animados (estende `DrawComponent`). 
        
    - **RectComponent**

         Componente para desenhar game objects em formato de retângulo. 

    - **AABBColliderComponent**

        Componente para detecção de colisão entre AABBs. 
        
    - **Block**

        Classe que estende `Actor` para representar um bloco do jogo.

    - **Goomba**

        Classe que estende `Actor` para representar o Goomba, inimigo que se movimenta horizontalmente de um lado para o outro.    

    - **Mario**

        Classe que estende `Actor` para representar o Mário, que é controlado pelo jogador.   

    - **Spawner**

        Classe que estende `Actor` para representar um "gatilho" (ou *trigger*) que cria um goomba quando o jogador está próximo.    

    - **Texture**

        Classe auxiliar para carregamento de texturas da memória secundária

    Observação: o código base desse projeto foi construído a partir do código do projeto anterior [[TP2: Asteroids]](tp2-asteroids), portanto muitas das classes já foram introduzidas anteriormente.

## Instruções

### **Parte 0: Preparação dos Assets**

Antes de começar a programar, você irá preparar os assets (recursos) que serão carregados no início do jogo. Esses assets incluem os sprite sheets dos personagens e dos blocos da primeira fase do Super Mario Bros., além de um arquivo .csv com o tilemap dessa fase.

- **Sprite Sheet**

    Utilize o [[FreeTexturePacker]](https://free-tex-packer.com/app/) para gerar os sprite sheets do mario, goomba e blocos. **Desabilite** a opção `Allow trim`, utilize o formato de dados `json (Array)` e o método `BestAreaFit` para exportar os sprite sheets. Copie os sprite sheets (imagens e dados) para os seus respectivos locais dentro do diretório `Assets` do projeto. Por exemplo, copie o sprite sheet do Mario para o local `Assets/Sprites/Mario`.

- **Tilemap**

    Utilize o [[Tiled]](https://www.mapeditor.org/) para abrir o arquivo `Levels/Level1-1/level1-1.tmx` e exportar seu conteúdo para um arquivo `Levels/Level1-1/Level1-1.csv`. Inspecione o csv gerado para compreender sua estrutura de tiles.

### **Parte 1: Carregando uma fase**

Na primeira parte, você irá ler um arquivo .csv exportado pelo Tiled para carregar a primeira fase do jogo. Para poder visualizar os blocos na tela, você também terá que implementar o componente `AnimatorComponent` para desenhar apenas sprites estáticos por enquanto.

- **Texture.cpp**

    1. Implemente o método `Load` para carregar uma textura com a SDL e enviá-la para a GPU com OpenGL. Veja os slides da aula 11 para referência. Inicialmente, você utilizará a função `IMG_Load` da biblioteca `SDL_image` para carregar a textura da memória secundária. Armazene a altura e largura da textura carregada nos atributos `mWidth` e `mHeight`, respectivamente. Em seguida, utilize as funções da OpenGL para enviar essa textura para a GPU. Quando for alocar a textura na GPU com a função `glGenTextures`, guarde o ID da textura no atributo `mTextureID`. Use esse valor para ligar a textura com `glBindTexture`. Quando for enviar a textura para a GPU com `glTexImage2D`, note que a altura e largura da textura já estão nos atributos `mWidth` e `mHeight`. Como a arte dos sprites é toda feita em pixel art, utilize o valor  `GL_NEAREST` para os filtros `GL_TEXTURE_MIN_FILTER` e `GL_TEXTURE_MAG_FILTER`, isso irá garantir que suas texturas terão alta definição quando aumentadas ou diminuídas. 

    2. Implemente o método `SetActive` para ativar e ligar uma textura no pipeline da GPU. Veja os slides da aula 11 para referência.

- **Shader.cpp** 

    1. Implemente o método `SetIntegerUniform`  enviar um valor inteiro para uma variável uniforme do shader. Ele será usado para indicar a texture unit de um sampler2D. Veja os slides da aula 11 para referência.

- **Renderer.cpp**

    1. No final do construtor da classe, antes de ativar o shader `mBaseShader`, chame o método `SetIntegerUniform` implementado anteriormente para inicializar o uniform `uTexture` com zero.

    2. Implemente o método `CreateSpriteVerts` para criar os vértices e os indíces de um quadrado unitário no sistema de coordenadas normalizado da OpenGL (origem no centro). Esse quadraro será utilizado pelos shaders para desenhar os sprites animados. Por isso, além das posições dos vértices, você terá que adicionar os valores uv de cada um para fazer o mapameamento de texturo. Após criar os vértices e índices, instancia um novo `VertexArray` e o armazene no ponteiro `mSpriteVerts`. Como todos os objetos do jogo são retângulos, é mais eficiente criar esse VertexArray como parte do renderer, pois podemos aproveitar esses mesmos vértices para desenhar todos os game objects. Note que o método `CreateSpriteVerts` é chamado durante a incialização do renderer. 

- **VertexArray.cpp**

    1. Implemente o construtor da classe para configurar os buffers de vértice e índice considerando agora que cada vértice tem, além de suas atributos de posição `(x,y)`, também possui atributos de mapeamentos de textura `(u,v)`. Note no shader de vértice que os atributos de posição estão na `location = 0` e os textura na `location = 1`. Veja slides da aula 11 para referência. 

- **Base.vert e Base.frag**

    1. No shader de vértice, passe a variável `inTexCoord` do shader de vértice para o de gragmento por meio da variável `fragTexCoord`. 

    2. Implemente a função main do shader de fragmento para combinar a cor sólida `uColor` passada como uniform com a textura do objeto `uTexture`. Faça a amostragem da cor da textura com a função `texture()` usando as coordenadas de textura `uTexture` e utilize a função `mix()` para interpolar entre a cor uniforme definida por uColor e a cor da textura, controlando a mistura pelo fator `uTextureFactor`. Armazene o resultado final em outColor, determinando a cor exibida no fragmento renderizado.
    Essa interpolação de cores sólidas e de texturas nos permitirá usar o mesmo shader para desenhar geometricas básicas coloridas e sprites com texutras.

- **AnimatorComponent.cpp**

    1. Implemente o construtor da classe para carregar uma textura usando o método `GetTexture` do renderer. Note o método `GetTexture` já está todo implementado e que ele carrega uma textura da memória secundária, armazenando-a em uma `cash`, para que, caso você queira usar essa textura novamente, ela sera carregada mais rapidamente da memória principal.
    
    2. Implemente o o método `Draw` para desenhar um sprite estático com a função `Renderer::DrawTexture` se o componente estiver marcado como visível `mIsVisible == true`. Utilize a posição, tamanho e rotação do objeto dono do componente para desenhar a textura. Note que a função `Renderer::DrawTexture` já cria a model matrix com base nessas propriedades, então você não precisa passá-la do actor. Aliás, o méotod `Actor::GetModelMatrix` foi removido da class Actor, pois a criação das matrizes de modelo passou a ser responsabilidade do renderer.

- **Block.cpp**

    1. Implemente o construtor da classe para adicionar um componente `AnimatorComponent`, passando uma string vazia no atributo `dataPath`, já que os blocos não possuem animacão. No mário, o tamanho dos blocos são todos iguais `32x32` e, por isso, já foi criada uma variável estática `Game::TILE_SIZE` para representar esse valor. Utilize esse variável para especificar as largura e altura do bloco. Para testar seu código até aqui, crie uma instância de `Block` na função `InitializeActors` com uma textura da sua escolha. 

        ```
        auto block = new Block(this, "../Assets/Sprites/Blocks/BlockA.png");
        block->SetPosition(Vector2(100.0f, 100.0f));
        ```

- **Mario.cpp**

    1. Implemente o construtor da classe para adicionar um componente `AnimatorComponent` no Mario. Por enquanto, utilize o sprite estático `Idle.png` para desenhar o personagem, não o sprite sheet. Para testar seu código até aqui, crie uma instância de `Mario` na função `InitializeActors` com uma textura da sua escolha. 

        ```
        mMario = new Mario(this);
        mMario->SetPosition(Vector2(200.0f, 100.0f));
        ```

- **Game.cpp**

    1. Implemente o método `LoadLevel` para ler o arquivo csv e instanciar uma matriz de inteiros `LEVEL_HEIGHT X LEVEL_WIDTH` representando os IDs dos tiles. Teste o seu código chamando essa função em `InitializeActor`, carregando a fase do arquivo `level1-1.csv`. Imprima a matriz na saída padrão com `SDL_Log` e compare a saída com o conteúdo do arquivo: eles devem ser os mesmos. Note que já foi incluída uma biblioteca CSV.h para te auxiliar no carregamento desse tipo de arquivo.

    2. Implemente o método `BuildLevel` para percorrer a matriz de tiles carregada no item anterior e instanciar game objects para o Mário, os canos e os blocos. Para saber qual ID de tile corresponde a qual textura, abra o arquivo `level1-1.tmx` e inspecione os IDs de cada tile. Inicialize as posições dos objetos no mundo de acordo com as suas posições na matriz de tiles. Guarde a instância do mario no atributo `mMario` da classe game. Os blocos não precisam ser armazenados na classe.
    
    3. Adicione ao método `InitializeActors` a chamada para as funções `LoadLevel` e `BuildLevel`, nessa ordem, para que a fase seja carregada e instanciada no momento de inicialização do jogo.

Ao final dessa etapa, você deveria ver uma saída como a ilustrada na figura abaixo:

<img src="{{ 'assets/images/tp3/1-result.png' | relative_url }}" alt="tp3-smb-1" width="560"/>

### **Parte 2: Movimentação de Câmera**

Na segunda parte, você irá implementar o movimento do jogador e da câmera, para que ele possa navegar horizontalmente sem colisão ao longo da fase.

- **Base.vert**

    1. Modifique o shader de vértice para subtrair a posição da câmera `uCameraPos` da posição do vértice. Essa subtração deve ser feita no sistema de coordenadas do mundo, ou seja, multiplique o vértice pela model matriz antes de fazer a subtração.

- **Game.cpp**

    1. Implemente o médoto `UpdateCamera` para ajustar a posição da câmera conforme o personagem principal se desloca pelo cenário, mantendo-o visível e centralizado na tela. Garanta que ela não ultrapasse os limites esquerdo e direito do nível, resultando em um movimento suave e controlado da visualização. Note que classe `Game` possui constantes `LEVEL_WIDTH`, `LEVEL_HEIGHT` e `TILE_SIZE` que podem te ajudar.

- **Mario.cpp**

    1. Adicione o componente `RigidBodyComponent` no construtor do personagem para habilitar sua movimentação. Utilize a função `SetApplyGravity` para desabilitar a gravidade do personagem enquanto você estiver trabalhando na parte 2: isso irá facilitar o teste da câmera.

    2. Implemente o método `OnProcessInput` para mover o personagem horizontalemte por meio de aplicação de forças `ApplyForce` com o `RigibBodyComponent`. Faça com que, ao pressionar a tecla `D`, o personagem se desloque para a direita e tenha sua escala horizontal `mScale.x` ajustada para a direit `1.0f`; e, ao pressionar a tecla `A`, ele se mova para a esquerda com a escala invertida `-1.0f`, simulando o espelhamento do sprite. Atualize o estado `mIsRunning` para indicar que o personagem está correndo enquanto uma dessas teclas estiver pressionada.

    3. Modifique o método `OnUpdate` para garantir que a posição horizontal do jogador esteja sempre à frente da posição horizontal da câmera.

Ao final dessa etapa, você deveria ver uma saída como no vídeo a seguir:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/4E3_dHmNRXE?si=XmrFUYrkg_nKfQua" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 3: Detecção de Colisão com AABBs**

Na terceira parte, você irá implementar o componente `AABBColliderComponent` para detectar colisões no jogo.

- **AABBColliderComponent.cpp**

    1. Implemente os métodos `GetMin` e `GetMax` para calcular os pontos de mínimo, máximo da AABB, respectivamente
       Lembre-se que a posição do actor `mOwner->GetPosition()` representa o centro da AABB.

    2. Implemente método `Intersect` para verificar se duas AABBs têm interseção

    3. Implemente os método `GetMinVerticalOverlap` e `GetMinHorizontalOverlap` para calcular as sobreposições verticais e horizontais entre duas AABBs, respectivamente. Utilize os valores de Min e Max das duas AABBs envolvidas. Veja slides da aula 8 para referência.

    4. Implemente os métodos `DetectHorizontalCollision` e `DetectVertialCollision` para separar uma AABB após uma colisão horizontal e vertical, respectivamente. Em ambos os casos, percorra todos os coliders ativos, ignorando a si mesmo e os desativados, e teste com `Intersect` se há interseção com cada um deles. Quando uma colisão for detectada, calcule a sobreposição mínima no eixo (`GetMinVerticalOverlap` ou `GetMinHorizontalOverlap`)  e resolva o deslocamento do corpo rígido para eliminar a interpenetração (`ResolveVerticalCollisions` ou `ResolveHorizontalCollisions`). Em seguida, notifique o objeto dono sobre o evento de colisão (`OnVerticalCollision` ou `OnHorizontalCollision`) antes de retornar o valor da sobreposição detectada.

    5. Implemente os métodos `ResolveHorizontalCollisions` e `ResolveVerticalCollisions` para resolver as colisões horizontais e verticais, respectivamente, entre os objetos do jogo. Em ambos os casos, você deve corrigir a posição do objeto após uma colisão. Desloque o objeto na direção oposta à sobreposição mínima detectada, removendo a interpenetração com o obstáculo, e anule a componente de sua velocidade (vertical ou horizontal) no corpo rígido para impedir que ele continue se movendo na direção da colisão. No método `ResolveVerticalCollisions`, você também deve verificar se interpenetração vertical foi de cima para baixo (maior que zero) para, nesse caso, marcar que o actor está no chão `mOwner->SetOnGround()`.

- **Block.cpp**

        1. Adicione o componente `AABBColliderComponent` ao construtor dos blocos para habilitar colisões do jogador com os blocos do nível. Na criação, marque o bloco como estático `isStatic = true` e como sendo da camada `ColliderLayer::Blocks`.

- **Mario.cpp**

    1. Habilite a gravidade no personagem, removendo a chamada para a função `SetApplyGravity` que você adicionou na etapa anterior

    2. Adicione o componente `AABBColliderComponent` no construtor do personagem para habilitar colisões do jogador com os blocos do nível. Por padrão, o componente assume que o objeto não é estático, então não é necessário marcá-lo como não estático.
    Configura camada do jogador como `ColliderLayer::Player`.

    3. Estenda o método `OnProcessInput` para implementar o pulo alterando a velocidade vertical do personagem instântaneamente `SetVelocity`. Note que a classe Mario possui um atributo `mJumpSpeed`, que configura a velocidade do pulo de forma similar ao jogo original. É importante destacar também que o jogador só pode pular quando estiver no chão `mIsOnGround == true`.

Ao final dessa parte, você deveria ser capaz de navegar na primeira fase com as mecânicas de correr e pular, porém sem animação, como no vídeo a segui (ative o modo debug do jogo):

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/Iyoa4kdtAhU?si=nJ3MxXglRDZTto65" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 4: Animações**

Na quarta parte, você irá implementar o componente `AnimatorComponent` para animar os objetos do jogo. 

- **Base.vert**

    Modifique o shader de vértice para, quando for passar as coordenadas uv para shader de fragmento via `fragTexCoord`, ajuste os valores uv `inTexCoord` para que cada vértice seja mapeado para a posição correta da textura dentro do sprite sheet. Para isso, basta multiplicar os valores uv em `inTexCoord` pelos componentes de escala de `uTexRect.zw` e somando o deslocamento `uTexRect.xy`. Não se esqueça de atribuir o resultado final à `fragTexCoord` para que o fragment shader possa amostrar as cores do sprite correspondente.

- **AnimatorComponent.cpp**

    Todos os quadros de um objeto estão armazenados no vetor `mSpriteSheetData`. Cada elemento desse vetor é um `Vector4`, representando as coordenadas e dimensões de um sprite no sprite sheet. Além disso, todas as animações estão armazenadas no mapa `mAnimations`. Uma animação é identificada por um nome (string) e definida por um vetor de índices de quadros (armazenados em `mSpriteSheetData`). A nome da animação corrente é armazenado na variável membro `mAnimName`. 

    1. Adicione no final do construtor da classe uma chamada para o método `LoadSpriteSheetData`

    1. Implemente a função `Update` para atualizar o quadro atual da animação de acordo com o tempo decorrido. Primeiro, verifique se a animação está pausada `mIsPaused` ou se o vetor de animações `mAnimations` está vazio. Em ambos os casos, você pode retornar da função pois não tem nada para atualizar. Se uma das condições anteriores forem falsas, devemos então atualizar o contador de animação  `mAnimTimer`, avançando-o proporcionalmente à taxa de quadros por segundo `mAnimFPS` e ao `deltaTime`, e reinicie o contador quando ele ultrapassar o número total de quadros da animação, garantindo que a animação seja reproduzida em loop contínuo enquanto não estiver pausada. Veja slides da aula 11 para referência.

    2. Modifique o método `Draw` para desenhar o sprite corrente da animação.  Verifique se o componente está visível e, caso exista uma animação ativa, obtenha o índice do quadro atual a partir de `mAnimTimer` para acessar o retângulo de textura correspondente no sprite sheet. Caso não haja animação, utilize a textura completa. Determine se o sprite deve ser espelhado horizontalmente com base na escala do objeto e desenhe-o chamando `renderer->DrawTexture`, informando posição, tamanho, rotação, textura, recorte e posição da câmera para exibir corretamente a animação na tela.

    3. Implemente a função `SetAnimation` para definir a animação atualmente ativa do componente. Atualize o nome da animação para o valor recebido e chame `Update(0.0f)` para garantir que o quadro exibido corresponda imediatamente ao início da nova animação, evitando que o personagem mostre um quadro incorreto ao trocar de estado.

    4. Implemente a função `AddAnimation` para registrar uma nova animação no componente. Associe o nome da animação ao vetor de índices dos quadros correspondentes no sprite sheet, armazenando essa relação no mapa `mAnimations` por meio de `emplace`. Isso permitirá que o componente acesse posteriormente a sequência correta de sprites ao reproduzir a animação com o nome especificado.

- **Mario.cpp**

    1. Modifique o construtor para que o personagem utilize o novo componente de desenho `AnimatorComponent`, agora passando como parâmetros a textura do sprite sheet `../Assets/Sprites/Mario/Mario.png` e seus metados `"../Assets/Sprites/Mario/Mario.json`.
    Crie as animações "dead", "idle", "jump" e "run" com o método `AnimatorComponent::AddAnimation`. Em seguida, configure a animação atual como "idle" usando o método `AnimatorComponent::SetAnimation` e o FPS das animações para `10.0f` usando o método `AnimatorComponent::SetAnimFPS`.
    
        Ao final desse item, você deveria ter o mesmo resultado da Parte 3, ou seja, o mário se movendo e colidindo mas sempre no estado idle.

    2. Implemente a método `ManageAnimations` para selecionar a animação correta enquanto o jogador estiver vivo `!mIsdead`: 
        1. Se estiver no chão e correndo, a animação correta é `"run"`
        2. Se estiver no chão, mas não estiver correndo, a animação `"idle"` 
        3. Se estiver não estiver no chão, a animação é `"jump"` 


Ao final dessa parte, você deveria ver as animações de run, idle e jump tocando de acordo com o estado do jogador, como no vídeo a seguir:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/B8LGpTGoz9s?si=w02KJvMp8div2c8Z" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 5: Inimigos**

Na quinta parte, você irá implementar os goombas e os spawners, que criam goombas quando o jogador está próximo.

- **Game.cpp**

    1. Morifique o método `BuildLevel` para instanciar spawners de acordo com a matriz de tiles. 

- **Goomba.cpp**

    1. Crie os componentes `RigidBodyComponent`, `AABBColliderComponent`, e `AnimatorComponent` no construtor, de forma similar ao Mario

    2. Implemente a função `Kill` para tratar o estado de morte do inimigo. Marque o Goomba como `mIsDying = true`, altere sua animação para `"dead"` para refletir visualmente o evento e desative `SetEnabled(false)` tanto o rigid body quanto o colisor, impedindo novas interações físicas ou colisões enquanto o inimigo é removido ou executa sua animação final.

    3. Implemente a função `OnUpdate` para atualizar o estado do inimigo a cada quadro. Caso o Goomba esteja morrendo, reduza o temporizador de morte e destrua `mState = ActorState::Destroy` o ator quando o tempo se esgotar `mDyingTimer <= 0.0f`. Além disso, verifique se o inimigo saiu da tela verticalmente — por exemplo, caindo além do limite inferior da janela — e, nesse caso, também defina seu estado como Destroy, removendo-o do jogo.

    4. Implemente o método `OnHorizontalCollision` para definir o comportamento do inimigo ao colidir horizontalmente com outros objetos. Faça com que, ao colidir com blocos ou outros inimigos, o Goomba inverta sua direção de movimento conforme o sinal da sobreposição mínima, fazendo ele virar sempre que colidir. Quando a colisão ocorrer com o jogador, invoque o método `Kill()` no objeto colidido para aplicar o efeito de dano ou morte ao personagem principal.

- **Spawner.cpp**

    1. Implemente a função `OnUpdate ` para controlar a geração de inimigos durante o jogo. Verifique se o jogador está a uma distância horizontal menor que `mSpawnDistance` em relação ao spawner e, quando essa condição for atendida, crie um novo objeto Goomba na mesma posição do spawner. Após gerar o inimigo, defina o estado do spawner como Destroy, garantindo que ele seja removido e não produza múltiplos inimigos na mesma posição.

- **Mario.cpp**

    1. Modifique o método `OnUpdate` para fazer com que o Mario perca o estado de estar no chão `mIsOnGround = false` sempre que sua velocidade vertical for diferente de zero, indicando que ele está pulando ou caindo. Verifique também se o Mario caiu abaixo da janela do jogo e, nesse caso, chame o método Kill() para que ele entre no estado de morte. Por fim, atualize as animações do personagem chamando ManageAnimations().

    2. Implemente o método `Kill` para definir o comportamento do personagem ao morrer. Altere a animação atual para `"dead"` para representar visualmente a morte, marque o estado interno como morto e desative tanto o corpo rígido quanto o colisor, impedindo novas interações físicas ou colisões após o evento.

    3. Implemente o método `OnHorizontalCollision` para tratar colisões horizontais do personagem com outros objetos. Verifique se o objeto colidido pertence à camada de inimigos e, nesse caso, chame o método `Kill()` para executar a lógica de morte do Mario, interrompendo seu movimento e ativando a animação correspondente.

    4. Implemente a função `OnVerticalCollision` para definir o comportamento do personagem ao colidir verticalmente com outros objetos. Quando a colisão ocorrer com um inimigo, faça com que o Mario o mate chamando o método `Kill()` no inimigo e aplique uma nova velocidade vertical positiva ao corpo rígido do Mario, simulando o impulso de salto que ocorre ao pisar sobre o inimigo.

Essa etapa conclui as implementações básicas do jogo, que deveria ter um resultado como o mostrado no vídeo do início do roteiro. 

### **Parte 6: Customização**

Na sexta, e última etapa, você irá ajustar as variáveis do jogo para criar uma versão única do Super Mário Bros.

1. Desenhe o fundo do jogo com a textura `Background.png`

2. Crie um nível novo usando o tiled e o carregue no jogo.

3. Implemente a movimentação dos blocos de tijolo (tipo B) quando o mario os acerta por baixo. Nesse caso, o bloco deve se mover para cima e para baixo, retornando exatamente no ponto que estava originalmente.

## Submissão

Para submeter o seu trabalho, basta fazer o commit e o push das suas alterações no repositório que foi criado para você no GitHub classroom.

Observação: você pode realizar quantos commits quiser. Isso é inclusive recomendado para dividir o problema em partes menores. Além disso, as mensagens nos commits não precisam ser necessariamente "Submissão TP3". Você pode também criar branches se quiser, mas **apenas o último commit da branch `main` será avaliado**.

```
git add .
git commit -m 'Submissão TP3'
git push
```

## Barema

- Parte 1: Carregando uma fase (5%)
- Parte 2: Movimentação de Câmera (5%)
- Parte 3: Detecção de Colisão com AABBs (30%)
- Parte 4: Animações (30%)
- Parte 5: Inimigos (10%)
- Parte 6: Customização (20%)

## Referências

- Parte 1: 
    - [Game Programming in C++, Cap 3, Vectors nad Basic Physics](https://learning.oreilly.com/library/view/game-programming-in/9780134598185/ch03.xhtml)