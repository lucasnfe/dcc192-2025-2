---
layout: page
title:  TP4 - Star Fox
permalink: /avaliacoes/tp4-star-fox
nav_exclude: true
---

# TP4: Star Fox

#### Entrega: 19/11/2025 às 23:59h

## Introdução

Star Fox 64, lançado pela Nintendo em 1997 para o console Nintendo 64, é um jogo de tiro em terceira pessoa com elementos de simulação espacial. O jogador controla Fox McCloud, líder do esquadrão Star Fox, um grupo de pilotos mercenários que lutam para defender o sistema Lylat da ameaça do cientista maligno Andross. A jogabilidade combina fases em trilhos (on-rails) e seções com liberdade total de movimento (All-Range Mode), nas quais o jogador pilota a nave Arwing e enfrenta inimigos aéreos e terrestres. O jogo se destaca por seus gráficos em 3D, dublagem completa e sistema de caminhos ramificados, que oferece múltiplas rotas e finais diferentes. O vídeo a seguir mostra um gameplay do jogo original:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/GhQp8le67Xo?si=3DBMq4TGwQMvQOTK&amp;start=330" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

## Objetivo

O objetivo desse projeto é praticar a implementação de jogos 3D e de gerenciadores de cenas, HUD e sistemas de áudio. Primeiro, você irá carregar a cena principal do jogo, incluindo a câmera e os modelos 3D da nave, das paredes, dos obstáculos e dos tiros. Em seguida, você irá implementar as mecânicas de movimentação e tiro do personagem, seguido do controle de câmera. Após implementas as mecânicas básicas, você irá implementar as telas de UI e seus elemetos básicos, como textos, botões e imagens. Em seguida, você irá implementar um gerenciador de cenas como uma máquina de estados finita usando if/else (ao invés de switch). Depois, você irá utilizar a estrutura de UI criada para construir o HUD do jogo. Por fim, você irá implementar um sistema para gerenciar um conjunto finito de canais de áudio da SDL_Mixer. O vídeo a seguir mostra um gameplay da versão que você irá implementar:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/yPgIhp-CDt0?si=i4xZ9aFmyeDPHG41" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>


## Código-base

1. Aceite o projeto *tp4-star-fox* no GitHub classroom [[nesse link]](https://classroom.github.com/a/N-GNZaB7)

2. Clone o seu novo repositório no seu computador:

    ```
    # Substitua <GITHUB_USERNAME> pelo seu usuário do GitHub
    git clone https://github.com/ufmg-dcc192/tp4-star-fox-<GITHUB_USERNAME>.git
    ```

3. Abra o projeto *tp4-star-fox* na CLion e, antes de começar a sua implementação, verique com cuidado as definições de métodos e atributos de cada classe:

    - **Ship**

        Classe representando a nave controlada pelo jogador.

    - **Block e BlockObstacle**

        Classes representando os blocos de parede e os blocos de obstáculos que aparecem ao longo do jogo.

    - **Bullet**

        Classe representando os tiros da nave que são emitidos pelo jogador.

    - **UIScreen**

        Classe base para criação de telas de interface, como menus e HUD. 

    - **HUD, MainMenu, GameOver**

        Classes que estendem UIScreen para implementar o HUD do jogo, o menu principal e a tela de fim de jogo. 

    - **UIElement**

        Classe abstrata base para implementação de elementos de UI. Ela define atributos básicos de posição, tamanho e cor, que podem ser usados para desenhar elementos de interface. Essa classe deve ser estendida quando um novo elemento de UI for criado.

    - **UIText**, **UIButton**, **UIImage** e **UIRect**

        Classes que estendem UIElement para implementar três elementos de interface básicos: texto, botões, imagens e retângulos.

    - **Font e Mesh**

        Classes auxiliares para carregar e descarregar fontes em formato true type e malhas de triângulos, resepctivamente.

    - **Camera**

        Classe para o controle de movimentação e parâmetros da câmera 3D.
      
    - **AudioSystem**

        Classe para gerenciamento de sons, permitindo tocar, pausar, resumir e parar sons armazenados em arquivos.

    Observação: o código base desse projeto foi construído a partir do anterior [[TP3: Super Mario Bros 1-1]](tp3-smb), assim. As classes restantes já foram introduzidas anteriormente. 

## Instruções

### **Parte 1: Menu Principal**

Na primeira parte, você irá construir o menu principal do jogo e, para isso, terá que implementar um sistema de UI simples que suporte textos, botões, imagens e retângulos. 

- **UIScreen.cpp**

A classe UIScreen representa uma tela de UI que possui um conjunto de elementos de texto, botão, imagem e retângulo. Ela permite que esses elementos sejam gerenciados em conjunto pelo jogo, principalmente para garantir que apenas a última tela receberá eventos de entrada. Esses eventos são importantes para possibilitar que o usuário selecione diferentes botões na interface. 

1. Implemente o construtor para adicionar a tela de UI no jogo. Utilize o método `Game::PushUI` para isso. Além disso, carregue a fonte passada como parâmetro
   e armazene o ponteiro retornado no atributo `mFont`. Utilize o método `Renderer::GetFont` para isso.

2. Implemente os métodos `AddText`, `AddButton`, `AddImage` e `AddRect` para adicionar elementos de texto, botão, imagem e retângulos a essa instância de tela de UI. Para cada elemento, crie um objeto do tipo desse elemento e o adicione na sua lista correspondente. Por exemplo, na função `AddText`, crie um objeto do tipo `UIText` e o adicone na lista `mTexts`. Sempre retorne o ponteiro para o objeto criado para que quem chamou a função possa manipulá-lo se quiser. Na função `AddButton`, além de criar e adicionar o botão na lista `mButtons`, verifique, após se o botão criado é o primeiro e, em caso positivo, altere o índice do botão selecionado para zero `mSelectedButtonIndex = 0` e marque esse botão como destacado `SetHighlighted(true)`. Assim, o primeiro botão adicionado será o botão selecionado incialmente. 

- **MainMenu.cpp**

1. Implemente o construtor da classe para adicionar a logo do jogo e dois botões: um rotulado para iniciar o jogo e outro para fechar o jogo. Os botões devem ter fundo azul e texto em branco. Quando o usuário selecionar e apertar o botão para iniciar o jogo, a tela de Menu deve ser fechada e o jogo deve começar. Quando ele apertar o botão para fechar o jogo, o jogo deve ser fechado. Use o método `Game::SetScene` para iniciar o jogo, `MainMenu::Close` para fechar a janela de menu e `Game::Quit` para fechar o jogo. 

2. Implemente o método `MainMenu::HandleKeyPress` para que o jogador possa navegar entre os dois botões do menu principal usando as teclas de setas para cima e para baixo do teclado. Além disso, quando o usuário apertar enter no teclado, o botão atualmente selecionado deve ser "clicado" (veja o método `UIButton::OnClick`).

- **Game.cpp**

1. Implemente a máquina de estados para troca de cenas do jogo no método `SetScene`, primeiramente, decarregue os objetos de jogo e UI da cena atual com o método `Game::UnloadScene`, em seguida, utize um comando switch no valor de `nextScene` passado como parâmetro. Caso a cena seja do tipo `GameScene::MainMenu`, instancie uma nova tela de menu `MainMenu` passando a fonte Arial `Arial.ttf` como parâmetro. Essa fonte está no diretório `Assets` do projeto.

Ao final dessa etapa, você deveria ver o meu principal com a logo do jogo e os botões de início e fim de jogo. Quando você apertar o botão de inicio, o menu será fechado e apenas uma cena vazia com fundo preto deve aparecer, como no vídeo a seguir:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/yr4riJhzz0g?si=0sHdAZdobrDu51ki" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 2: Inicializando cena 3D**

Para garantir que você tem uma cena 3D configurada corretamente, na segunda etapa você irá apenas carregar o modelo 3D da nave e garantir que consegue 
visualizar esse objeto corretamente com uma câmera 3D estática com perspectiva. 

- **Ship.cpp**

1. Implemente o construtor da classe adicionando um componente `MeshComponent`, `AABBColliderComponent`, `RigidBodyComponent` e `ParticleSystemComponent` no objeto da nave que está sendo criado. Armazene os ponteiros para cada um desses componenetes em seus respectivos atributos. Os componentes de colisão, movimentação e sistemas de partículas, funcionam exatamente como nos projetos 2D, porém com uma dimensão z a mais. A única novidade no sistemas de partícula é que, nessa novo TP, ele foi implementado como uma classe template, assim você precisa explicar o tipo de partícula quando for chamar o construtor. Já o componente de mesh, foi implementado para desenhar uma malha de triâgulos (mesh) com suas respectivas texturas. O constutor desse componente espera um ponteiro para uma mesh. Para carregar uma mesh de um arquivo, você pode utilizar o método `Renderer::GetMesh`, passando o caminho para o arquivo como uma string. Lembre-se que você pode acessar o ponteiro do renderer pelo ponteiro do jogo `mGame`. O método `GetMesh` carrega a posição de cada vértice, bem como as suas coordenadas uv para mapamento de texturas. Os arquivos de mesh também já especificam as texturas associadas às malhas, portanto elas também já são carregadas no próprio código de carregamento no rendeder. Utilize o método `GetMesh` para carregar a mesh `Arwing.gpmesh` e o método `MeshComponent::SetMesh` para passar a mesh carregada para o componente `MeshComponent`. Em segguida, para que a nave apareça maior na tela, altera a escala do objeto para `2.0` com o método `Actor::SetScale`. Ao criar os outros componentes, utilize os seguintes valores de parâmetros:

    - Tamanho da AABB: `(15.0, 40.0, 25.0)`
    - Camada de colisão: `ColliderLayer::Player`
    - Massa: `1.0`
    - Coeficiente de atrito: `2.0`
    - Velocidade inicial: `(400.0, 0.0, 0.0)`
    - Tamanho do pool de partículas: `50`

Obs: No sistema de coordenadas do mundo desse jogo, o eixo +x está apontando para dentro da tela do computador e o -x para fora da tela. O eixo z representa a dreção vertical (cima/baixo) e o eixo y o eixo horizontal (esquerda/direita).

- **Camera.cpp**

1. Implemente a construtor da classe para criar as matrizes de view e projeção perspectiva inciais, que devem ser passadas para o shader logo na construção da câmera. A matriz de view será atualizada ao longo do jogo, conforme o personagem se movimenta, já a de projeção não, ela será constante ao longo do jogo, pois os parâmetros de projeção não costumam mudar. Utilize os métodos `Matrix4::CreateLookAt` e `Matrix4::CreatePerspectiveFOV` (veja slides da aula 18) para criar essas matrizes e os métodos `Renderer::SetViewMatrix` e `Renderer::SetProjectionMatrix` para passar as matrizes para o renderizador. Note que o objeto câmera tem um ponteiro para o objeto jogo e por meio dele você consegue acessar o renderer. Os parâmetros de crição das matrizes foram todos passados como argumentos do construtor.

- **Game.cpp**

1. Complemente a máquina de estados no método `SetScene` para lidar com caso onde a nova cena é do tipo `GameScene::MainMenu`. Nesse caso, crie um objeto do tipo `Ship` e outro do tipo `Camera`. Os ponteiros devem ser armazenados nos atributos `mShip` e `mCamera` do objeto do jogo. Para a criação da câmera, utilize os seguintes parâmetros:

    - Eye (posição): `(-300.0, 0.0, 0.0)`
    - Target (alvo): `(20.0, 0.0, 0.0)`
    - Up: `(0.0, 0.0, 1.0)`
    - Fov: `70.0`
    - Near plane: `10`
    - Far plane: `10000`

Nesse momento, quando você apertar o botão de início do jogo, você deveria entrar na cena principal e ver uma nave na tela se movendo para a frente, ficando cada vez menor ao longo do tempo, como mostrado no vídeo a seguir:

<div class="embed-youtube">
<iframe width="560" height="315" src="https://www.youtube.com/embed/NFMeohS1pSo?si=H9BMex9tb81Zr8cd" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 3: Camera 3D**

Nessa tercerira etapa, você irá implementar uma câmera 3D em terceira pessoa básica.

- **Camera.cpp**

1. Implemente o método Update da câmera para seguir o jogador a uma distância horizontal `mHDistance` e vertical `mVDistance`, olhando para um
 ponto a uma distancia `mTDistance` a frente do jogador. Lembre-se que a movimentação de câmera nada mais é do que configurar a matriz de View, por isso
 você terá que recalcular, a cada quadro, os novos parâmetros de posição (eye) e alvo (target) em função da posição do jogador. O vetor up não sofrerá modificações ao longo da cena. Com esses parâmetros, você precisa criar uma nova matriz de View e passá-la para o renderer, de forma similar ao que foi feito no construtor da câmera. Veja o último slide da aula 18. Dica: O método `Actor::GetForward()` retorna o vetor de direção para frente de um determinado actor. 

- **Game.cpp**

1. Chame o método Update da câmera no Update do jogo, passando como parâmetro o delta time e o ponteiro para a nave, a qual a câmera irá seguir.

Ao final dessa etapa, você deveria ver a nave centralizada na tela que se mantém com tamanho constante ao longo do tempo. Apesar da nave ainda estar se movendo,
agora a câmera a está seguindo e, como o jogo ainda não tem um ambiente de fundo, vemos uma nave estática na tela, como na imagem abaixo:

<img src="{{ 'assets/images/tp4/1-result.png' | relative_url }}" alt="tp4-star-fox" width="560"/>


### **Parte 4: Movimentação e Level**

Agora que temos uma nave e uma câmera, podemos implementar o movimento da nave e os blocos laterias para formar as paredes, o chão e o céu do mundo.

- **Ship.cpp**

1. Implemente o método `OnProcessInput` para mover a nave para cima, esquerda, baixo e direita com as teclas `W`, `A`, `S` e `D`, respectivamente. Em cada um desses casos, aplique uma força de `1000.0f` com `RigibBody::ApplyForce` na direção correta. Lembre-se que  o eixo +x está apontando para dentro da tela do computador e o -x para fora da tela. O eixo z representa a dreção vertical (cima/baixo) e o eixo y o eixo horizontal (esquerda/direita).

2. No método `OnUpdate`, limite as posições horizontais (eixo y) e verticais (eixo z) entre `(-180.0, 180.0f)` e `(-225.0f, 225.0f)`, respectivamente. 

- **Block.cpp**

1. Implemente o construtor da classe para instanciar uma `MeshComponent`, passando uma instância da mesh `Cube.gpmesh`. Lembre-se que para carregar uma mesh você precisa usar o método `Renderer::GetMesh()`.

2. Implemente o método `OnUpdate` para esse bloco se auto-destruir quando o jogador tiver o ultrapassado. Garanta que o bloco será destruído após ele não estiver mais aparecendo na tela. 

- **Camera.cpp**

1. Modifique a o método `Update` da camera para garantir que a posição vetical `z` da câmera seja sempre igual a zero, para que quando a nave mover para cima e para baixo, o jogador veja as partes baixo e cima da nave, respectivamente. Isso gera um comportamento de câmera mais interessante.

- **Game.cpp**

1. Implemente o método `SpawnWalls` para instanciar um bloco `Block` para a parede esquerda, outro para a direita, outro para o chão e o outro para o céu. Guarde um ponteiro local para cada bloco, para poder alterar as posições, dimensões e texturas de cada um. Altere as escalas de todos os blocos para `500.0`. Altere também as posições e texturas da seguinte forma:

    - Parede esquerda: posição `(mNextBlock * 500.0, 500.0, 0.0)`, textura `0`
    - Parede direita: posição `(mNextBlock * 500.0, -500.0, 0.0)`, textura `0`
    - Chão: posição `(0.0, 0.0, -500.0)`, textura `5`
    - Céu: `(0.0, 0.0, 500.0)`, textura `6`

    Após instanciar os quatro blocos, incremente o atributo mNextBlock de 1 para que na próxima vez que a função for chamada, os próximos blocos sejam criados na frente dos atuais.

2. No método `SetScene`, qunado a nova cena for `GameScene::Level1` chame o método `SpanwWalls` repedidamente para construir um corredor no mundo, inciando com o contador `mNextBlock` em zero e indo até `TUNNEL_DEPTH`.

3. No método `UpdateGame`, execute o método `SpanwWalls` sempre que o jogador avançar `500.0` (tamanho de um bloco) unidades no eixo `x`. 

Ao final dessa etapa, você deveria ser capaz de se movimentar vertial e horizontalmente em um tunel que é construído infinitamente conforme a nave avança automaticamente para a frente, conforme vídeo abaixo:

<div class="embed-youtube">
<iframe width="560" height="315" src="https://www.youtube.com/embed/i9YI4pN2OFQ?si=igenwkCIbLl3wXyU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 5: Obstáculos**

Agora que temos a movimentação básica em um corredor infinito, podemos implementar as paredes de obstáculos que aparecem em ondas ao longo do jogo. Existem dois tipos de obstáculos: blocos sólidos e blocos explosivos. Note que dentro do diretório `Assets/Blocks` do projeto, foram definidos 20 padrões de paredes de obstáculos em arquivos texto: `1.txt`, `2.txt`, ..., `20.txt`. Nesses arquivos, os caraceteres `A` representam blocos sólidos e os `B` blocos explosivos. Os padrões serão instanciados em ordem, na medida em que o jogador avança no tunel. Quando todos os padrões forem instanciados, novas serão selecionados alatóriamente dentro os 20. Sempre que o jogador passar por padrão, ele deve receber um ponto.

- **BlockObstacle.cpp**

1. Implemente o construtor da classe para adicionar um componente `AABBColliderComponent` estático com tamanho `(1.0,1.0,1.0)` e camada `ColliderLayer::Block`.
Armazene o ponteiro para o componente no atributo correspondente. Em seguida, adicione esse obstáculo à lista de obstáculos do jogo com o método `Game::AddObstacle`. 

2. Implemente o destrutor da classe removendo essa instância da lista de obstáculos do jogo com o método `Game::RemoveObstacle`. 

- **Game.cpp**

1. Implemente o método `SpawnObstacles` para instanciar uma parede de blocos. O atributo `mNextObstacle` mantém a contagem de paredes alocadas ao longo do jogo. Se esse atributo for menor do que o número total de padrões em `mObstaclePatterns`, instancie o padrão de índice `mNextObstacle`, se não, instancie um padrão aleatório da lista `mObstaclePatterns`. Após instanciar os obstáculos, altere suas texturas e posições `y` e `z` utilizando os dados correspondente em `mObstaclePatterns`. As escalas dos obstáculos devem ser `(25.0, 25.0, 25.0)` e o posições `y` e `z` devem ter um deslocamento de `-250.0` e  `250.0`, respecticamente. As posições `x` devem ser calculadas para que cada onda de obstáculoas tenha uma distância de `1000.0` unidades entre si. Se o obstáculo tiver a textura de índice `4`, ele é um bloco explosivo, então marque-o como tal usando o método `BlockObstacle::SetExploding`. Ao final do método, incremente o contador `mNextObstacle`. As paredes de obstáculos devem ocupar toda a extensão vertical do corredor e aparecer como definidos nos arquivos.

2. No médodo `SetScene`, quando o tipo da nova cena for `GameScene::Level1`, chame o método `LoadObstaclePatterns` (já implementado) passando o caminho para o diretório que conteém os arquivos com padrões de blocos. Esse método irá armazenar os padrões em um vetor `mObstaclePatterns`, onde cada elemento por sua vez também é um vetor do tipo `BlockObstacleItem`, que possui coordenadas `(i,j)` e um id textura. Após chamar o método `LoadObstaclePatterns`, inicialize o contador de obstáculos `mNextObstacle` com zero e instancie os primeiros 3 padrões.

3. No método `UpdateGame`, execute o método `SpawnObstacles` sempre que o jogador passar um obstáculo. Isso irá gerar um novo no final do túnel.

Ao final dessa etapa, você deveria ver ondas de obstáculos vindo em direção ao jogador na ordem especificada pelos arquivos em `Assets/Blocks`. A nave ainda não terá colisão com os obstáculos. Quando o jogador passar por todos os obstáculos, novas ondas com padrões selecionados aleatóriamente deverão aparecer, por tempo indeterminado:

<div class="embed-youtube">
<iframe width="560" height="315" src="https://www.youtube.com/embed/7bWbBx7JoE4?si=7EiPnmZ2Z5l-Ut5r" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### Parte 6: Colisões da Nave

Para terminar as mecânicas básicas do jogo, precisamos adicionar colisão entre a nave e os obstáculos, causando dano na nave quando isso ocorrer. Quando a nave colidir com um obstáculo, este deve ser destruído e a nave deve levar um ponto de dano. Ao levar dano, a nave deve ficar invencível por por `2` segundos. Durante esse tempo, novas colisões não causam dano na nave, mas continuam destruindo os obstáculos. Alguns obstáculos são explosivos, causando a destruição dos obstáculos visinhos. Caso os visinhos também sejam explosivos, isso deve gerar um efeito em cascata. 

- **BlockObstacle.cpp**

1. Implemente o método `Explode` para explodir um obstáculo. Esse método será usado quando a nave ou um tiro colidir com um obstáculo. Primeiramente, destrua esse obstáculo alterando o seu estado para `ActorState::Destroy`. Caso ele seja do tipo explosivo (atributo `mIsExploding`), ele também deve explodir os outros blocos vizinhos que estão a `50.0` unidades de distância. Cada um dos visinhos que também sejam explosivo, vão disparar uma nova explosão, gerando um efeito em castata de explosões. Cuidado para não causar um loop infinito quando for implementar essa funcionalidade.

- **Ship.cpp**

1. Implemente o método `DealDamage` para diminuir em um ponto a vida do jogador, representada pelo atribudo `mHealth`. Antes de reduzir o contador de um, verifique se o jogador está em estado invencível (atribudo `mIsInvincible`). Se estiver, não cause o dano. Se não, marque o atributo `mIsInvincible` como verdadeiro e o inicialize o contador `mInvinsibleCooldown` igual a `2.0`. Ao final, verifique se a vida do jogador é menor ou igual a zero, nesse caso, altere o estado da nave para `GameState::Paused`.

2. Modifique o método `OnUpdate` para verificar colisão entre a nave e os obstáculos do jogo. A lógica de colisão é similar ao do TP2: Asteoids. Utilize o método `AABBColliderComponent::Intersect` para verificar colisão entre as AABBs da nave e dos obstáculos. Note que o método `Game::GetObstacles` retorna a lista de obstáculos atualmente no jogo. Se houver colisão, chame o método `BlockObstacle::Explode` para explodir o obstáculo e o `Ship::DealDamage` para causar dano na nave. É importante destacar que é muito provável que, em um determinado quadro, quando houver uma colisão da nave contra um obstáculo, também haverá colisão contra outros obstáculos, pelo próprio tamanho da AABB da nave. Lembre-se que a lógica de invencibilidade deve garantir que, em situações assim, todos os blocos são destruídos, mas a nave levará apenas um de ano, já que ao entrar em contato com um obstáculo, ela deve ficar invencível por `2.0` segundos. 

3. Ainda no método `OnUpdate`, antes de verificar colisão, decremente o contador `mInvinsibleCooldown` pelo `deltaTime` caso o jogador esteja no estado invencível. Se esse contador chegar a zero, atualize o estado `mIsInvincible` para falso, pois se passaram os `2.0` segundos de invencibilidade.

Ao final dessa etapa, você deveria ser capaz de colidir com os obstáculos, causando dano na nave e destruindo os obstáculos quando isso ocorrer. Obstáculos explosivos também explodem seus vizinhos, como no vídeo abaixo:

<div class="embed-youtube">
<iframe width="560" height="315" src="https://www.youtube.com/embed/lDRaVmQ7VXs?si=KiUSVJsaTMroU2YO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 7: Tiros**

A lógica para implementar os tiros é muito parecida com a do TP2: Asteroids. Você irá utilizar um sistema de partículas para pré-carregar os tiros e emitir essas partículas quando o jogador apertar a tecla espaço. No TP4, os tiros devem explodir apenas os obstáculos explosivos, ou seja, caso os tiros colidam com um obstáculo sólido, o tiro deve ser destruído, mas o obstáculo não.

- **Bullet.cpp**

1. Implemente o construtor da classe para instanciar uma `MeshComponent`, passando uma instância da mesh `Laser.gpmesh`. Lembre-se que para carregar uma mesh você precisa usar o método `Renderer::GetMesh()`. Altera a escala dessa instância `Actor::SetScale` para `(5.0, 5.0, 5.0)`. Instâncie também componentes `RigidBodyComponent` e `AABBColliderComponent`, utilizando os seguintes valores de parâmetros:

    - Tamanho da AABB: `(10.0, 10.0, 10.0)`
    - Camada de colisão: `ColliderLayer::Bullet`
    - Massa: `1.0`
    - Coeficiente de atrito: `0.0`
    - Velocidade inicial: `(0.0, 0.0, 0.0)`

2. Modifique o método `OnUpdate` para verificar colisão entre a partícula e os obstáculos. Lembre-se que o método `Game::GetObstacles` retorna a lista de obstáculos atualmente no jogo. Se houver colisão, mate a partícula com o método `Bullet::Kill()` e, caso o obstáculo seja explosivo, exploca o obstáculo com o método `BlockObstacle::Explode`. 

- **Ship.cpp**

1. Complete o método `OnProcessInput` para que quando o jogador apertar a tecla espaço, a nave emita uma partícula com um segundo de vida e com o dobro da velocidade da nave. O deslocamento inical `offset` da partícula deve ser calculado para que ela nasça na ponta da frente da nave. A lógica de tiro deve adicionar um cooldown na verificação de entrada da tecla espaço, para que quando o jogador segurar essa tecla, os tiros ocorram com uma frequência de `0.25` secundos. O atributo `mLaserCooldown` pode ser usado para isso.

Ao final dessa etapa, você deveria ser capaz de atirar continuamente apertando a tecla espaço, explodindo blocos explosivos em caso de colisão, conforme o vídeo abaixo:

<div class="embed-youtube">
<iframe width="560" height="315" src="https://www.youtube.com/embed/WROadIg45Wc?si=YuWblDecdCkTsKuk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 8: HUD e Game Over**

Agora que todas as mecânicas estão implementadas, vamos implementar o HUD e a tela de fim de jogo. O HUD irá mostrar a vida da nave com uma barra de progresso e os pontos do jogador com um texto. Como a nave tem 3 pontos de vida, a barra de progresso é formada por quatro imagens: o fundo, o início (vida = 1), o meio (vida = 2) e o fim (vida = 3) do progresso da vida. 

- **HUD.cpp**

1. A classe HUD é uma tela de UI, portanto estende `UIScreen`. Implemente o construtor da classe para adicionar as 4 imagens da barra de progresso no HUD. Como o HUD é uma UIScreen, lembre-se que pode usar o método `HUD::AddImage` para isso. A ordem de adição das texturas é a ordem com a qual elas serão desenhadas na tela, ou então você pode estipular uma ordem customizada de desenho passando um valor para o parâmetro `drawOrder`. Adicione uma imagem com textura `ShieldOrange.png`, seguida de `ShieldRed.png`, `ShieldBlue.png` e `ShieldBar.png`. Todas as imagens devem ter escala `0.75`. Adicione também dois textos `HUD::AddText` no HUD, uma para a palavra "score: " e outro para o contador de pontos em si, que inicialmente vale zero. Armazene os ponteiros para as 3 texturas de progresso e o texto do contador nos seus respectivos atributos, pois precisaremos modifica-los ao longo do jogo. 

2. Implemente o método `SetHealth` para atualizar a barra de progresso de vida quando a nave levar dano. Quando a nave tiver 3 de vida, as três imagens de progresso devem aparecer. Caso a vida seja igual a 2, apenas as imagens de progresso vermelha e laranja devem aparecer. Caso a vida seja igual a 1, apenas a imagem de progresso vermelha deve aparecer. Se a vida for igual a zero, nenhuma das texturas de progresso devem aparecer. 

3. Implemente o método `SetScore` para alterar a string do elemento de texto da UI que conta pontos. Como a classe `HUD` possui um atributo que é o ponteiro para o contador de pontos, você pode usat o método `UIText::SetText` para alterar os pontos.

- **GameOver.cpp**

1. Implemente o construtor da classe para adicionar um elemento de texto escrito "Game Over" e um botão escrito "Press Enter", que o usuário pode apertar para voltar para o menu principal. 

2. Implemente o método `HandleKeyPress` para clicar no botão selecionado pelo jogador.

- **Ship.cpp**

1. Modifique o método `DealDamage` para que quando a vida do jogador chegar a zero, uma tela de Game Over seja criada.

- **Game.cpp**

1. Modifique o método `UpdateGame` para adicionar um ponto para o jogador toda vez que ele passar por uma parede de obstáculos, mesmo que o jogador tenha sofrido colisão contra algum obstáculo da parede. Lembre-se que os obstáculos possuem uma distância de `1000.0` unidades entre si.

Ao final dessa etapa, o jogo deveria ter um HUD mostrando a vida do jogador e os seus pontos atuais. Além disso, quando a vida do jogador chegar a zero, o jogo deve mostrar uma tela de game over, como no vídeo abaixo:

<div class="embed-youtube">
<iframe width="560" height="315" src="https://www.youtube.com/embed/ypuJLT-KUR4?si=I9-IV5eRs4yOdLgB" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

### **Parte 9: Sistema de Áudio**

Agora vamos construir um sistema de áudio para tocar músicas de fundo e efeitos sonoros. Esse sistema será implementado na classe `AudioSystem` e seguirá as mesmas ideias vistas na Aula 17. Para simplificar o trabalho de implementação, o sistema já está praticamente pronto, você só terá que implementar a lógica para tocar um som.

- **AudioSystem.cpp**

1. Implemente o método `PlaySound(const std::string& soundName, bool looping)` para tocar um determinado som passado como parâmetro.  
O parâmetro `soundName` é o nome do arquivo de áudio armazenado no diretório `Assets/Sounds`. Note não é preciso adicionar esse prefixo quando for tocar um som, pois o próprio sistema de som já faz isso para você. O parâmetro `looping` controla se o som tocará repetidamente ou não. Lembre-se que quando esse método for chamado, pode ser que todos os canais estão ocupados, então temos que aplicar uma política para parar sons atualmente em reprodução. Usaremos a mesma política definida na Aula 16.

- **Game.cpp**

1. Instancie um `AudioSystem` no método `Initialize` e armazene-o no monteiro `mAudio`

2. Chame o update dessa audio system no método `UpdateGame`

Ao implementar esses métodos, você deveria ser capaz de tocar arquivos de áudio com o método `AudioSystem::PlaySound`, além de parar, pausar e retomar um som os métodos `AudioSystem::StopSound`, `AudioSystem::PauseSound` e `AudioSystem::ResumeSound`, respectivamente. Note que o método `AudioSystem::PlaySound` tem um parâmetro `looping` que permite tocar sons em loop. Utilize esses métodos para implementar os sons do jogo, conforme instruções a seguir:

- Sons sem loop:
    - Quando o jogador sofrer dano (mas não morrer), reproduza "ShipHit.wav"
    - Quando o jogador morrer, reproduza "ShipDie.wav"
    - Quando o jogador atirar, reproduza "Shoot.wav"
    - Quando o jogador fizer uma manobra de rolamento, reproduza "Boost.wav"
    - Quando um bloco for destruído por uma bala, reproduza "BlockExplode.wav" (certifique-se de que este som seja reproduzido apenas uma vez, mesmo que haja uma reação em cadeia, ou o resultado ficará ruim)

- Sons com loop:
    - A música "Music.ogg" deve ser reproduzida continuamente
    - Reproduza "ShipLoop.ogg" enquanto o jogador estiver vivo. Assim que o jogador morrer, pare o som.
    - Para o arquivo "DamageAlert.ogg" em loop:
    - Ele deve começar a tocar quando o nível do escudo do jogador chegar a 1.
    - Pare o som se o nível do escudo subir novamente (mas ele ainda deve começar a tocar novamente na próxima vez que o nível do escudo chegar a 1).
    - Pare o som se o jogador morrer.

Essa etapa conclui as implementações básicas do jogo, que deveria ter um resultado como o mostrado no vídeo do início do roteiro. 

### **Parte 10: Customização**

Na quinta, e última etapa, você irá ajustar as variáveis do jogo para criar uma versão única do Super Mário Bros.

1. Implemente um menu de pausa quando o jogador apertar a tecla enter 
2. Adicione um efeito cross-fade (fade our + fade in) no gerenciador de cenas. 
3. Quando a nave se mover, adicione leves rotações na direção do movimento, como no jogo original

## Submissão

Para submeter o seu trabalho, basta fazer o commit e o push das suas alterações no repositório que foi criado para você no GitHub classroom.

Observação: você pode realizar quantos commits quiser. Isso é inclusive recomendado para dividir o problema em partes menores. Além disso, as mensagens nos commits não precisam ser necessariamente "Submissão TP4". Você pode também criar branches se quiser, mas **apenas o último commit da branch `main` será avaliado**.

```
git add .
git commit -m 'Submissão TP4'
git push
```
