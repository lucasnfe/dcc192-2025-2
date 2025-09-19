---
layout: page
title:  TP2 - Asteroids
permalink: /avaliacoes/tp2-asteroids
nav_exclude: true
---

# TP2: Asteroids

#### Entrega: 01/10/2025 às 23:59h

## Introdução

Assim como o Pong, o Asteroids, lançado pela Atari em 1979, também foi um dos jogos mais populares da era do Arcade. O Asteroids é um jogo de tiro com uma temática espacial onde o jogador controla uma nave com o objetivo de destruir todos os asteroides no mapa sem colidir com nenhum deles. Quando o jogador destrói todos os asteroides, um número maior do que o anterior é criado no mapa, aumentando a dificuldade do jogo. Se o jogador colidir com um asteroide, ele perde uma vida. O jogo termina quando o jogador perder suas três vidas. O vídeo abaixo mostra um *gameplay* do jogo original:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/WYSupJ5r2zo?si=NNIkvpXUij-DR0Zc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Objetivo

Nesse projeto, você irá desenvolver as mecânicas princiais de movimentação, colisão e tiro do Asteroids em C++ e OpenGL/SDL. Nessa versão, o jogador poderá mover-se e atirar com a nave como no jogo original, destruindo os asteroides caso um laser os acerte. O jogo será reiniciado quando a nave colidir com um asteroide. Inicialmente, você irá criar os um modelo de objetos híbrido, com hierarquia de classes e componentes. Em seguida, você irá implemantar os componentes `RigidBodyComponent` e `CircleColliderComponent` para movimentar e detectar as colisões dos objetos do jogo. Por fim, você irá utilizar esses componentes para implementar uma nave que atira raios laser e gerar um dado número de asteroides com geometrias aleatórias. O video a seguir mostra um gameplay da versão que você irá implementar:

<div class="embed-youtube">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/WrbIiKNksL4?si=Qp5D_RNHu9K5nIg5" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Código-base

1. Aceite o projeto *tp2-asteroids* no GitHub classroom [[nesse link]](https://classroom.github.com/a/eBBq1CaW)

2. Clone o seu novo repositório no seu computador:

    ```
    # Substitua <GITHUB_USERNAME> pelo seu usuário do GitHub
    git clone https://github.com/ufmg-dcc192/tp2-asteroids-<GITHUB_USERNAME>.git
    ```

3. Abra o projeto *tp2-asteroids* na CLion e, antes de começar a sua implementação, verique com cuidado as definições de métodos e atributos de cada classe:
    
    - **Component**

        Classe base para todos os componentes do jogo, contendo métodos para processamento de eventos de entrada e atualização. 

    - **DrawComponent**

        Componente de desenho de objetos com retângulos coloridos. 

    - **RigidBodyComponent**

        Componente de movimentação de objetos rígidos. 

    - **CircleColliderComponent**

        Componente de detecção de colisão baseado em comparações entre círculos. 

    - **ParticleSystemComponent**

        Componente para emissão de partículas, que pode ser usado para atirar os lasers.     

    - **Ship**

        Classe que estende `Actor` para representar a nave.
        
    - **Asteroid**

        Classe que estende `Actor` para representar um asteroide.

    - **Laser**

        Classe que estende `Actor` para representar uma partícula de laser, que é atirada pela nave quando o jogador pressiona a tecla **Espaço**.    

    Observação: o código base desse projeto foi construído a partir do código do projeto anterior [[TP1: Pong]](tp1-pong), portanto muitas das classes já foram introduzidas anteriormente.

## Instruções

### **Parte 1: Modelo de Objetos**

Na primeira parte, você irá implementar uma estrutura de objetos com hierarquia de classes e componentes, o que possibilitará a criação e gerenciamento de objeto de uma maneira mais flexível.

1. **Actor.cpp**

    1. Implemente o construtor `Actor` para adicionar ao jogo o ator que está sendo criado 
    
    2. Implemente o destrutor `~Actor` para retirar o ator da lista de atores do jogo. Em seguida, percorra o vetor de componentes, deletando-os um a um, e finalize esvaziando esse vetor para evitar vazamentos de memória.

    3. Complete o método `Update` para, quando o ator estiver ativo, chamar o update dos seus componentes

    4. Complete o método `ProcessInput` para, quando o ator estiver ativo, chamar o processador de entrada dos seus componentes

    5. Implemente o método `AddComponent` para registrar novos componentes em um ator. Primeiro, adicione o componente ao final do vetor de componentes. Em seguida, ordene esse vetor para garantir que os componentes fiquem organizados conforme seu `updateOrder`, de modo que sejam atualizados na ordem correta durante a execução do jogo.

2. **Game.cpp**

    1. Implemente a função `AddActor` para adicionar novos atores ao jogo. Caso mUpdatingActors esteja ativo, insira o ator em mPendingActors; caso contrário, adicione-o diretamente em mActors
    
    2. Implemente a função `RemoveActor` para remover um ator tanto de mPendingActors quanto de mActors. Para evitar cópias desnecessárias, use std::iter_swap para trocar o elemento encontrado com o último da lista e depois remova-o com pop_back().

    3. Implemente a função `UpdateActors` ativando a flag `mUpdatingActors` antes de atualizar cada ator e desativando-a ao final do loop, garantindo que novos atores criados sejam armazenados em `mPendingActors`. Depois, mova todos os atores pendentes para mActors e use `mPendingActors.clear()` para esvaziar a lista. Por último, crie um vetor temporário  `deadActors` com os atores em estado `Destroy` e, em seguida, percorra esse vetor deletando cada ator para liberar a memória corretamente.

    4. Implemente a função `AddDrawable` para adicionar um novo `DrawComponent` à lista de desenháveis. Em seguida, ordene a lista usando `std::sort`, garantindo que os componentes fiquem organizados pelo valor de `GetDrawOrder()`.

    5. Implemente a função `RemoveDrawable` para localizar o `DrawComponent` na lista de desenháveis usando `std::find` e removê-lo com `erase`.

    6. Complete a parte final de `ProcessInput` percorrendo todos os atores em `mActors` e chamando `ProcessInput(state)` para cada um, usando o estado de teclado obtido em `SDL_GetKeyboardState`.

    7.  Implemente a função `GenerateOutput` para desenhar todos os elementos da cena. Entre o `Renderer::Clear` e o `Renderer::Present`, percorra a lista `mDrawables` chamando `Draw` para cada componente, e caso `mIsDebugging` esteja ativo, percorra também os componentes do ator dono chamando `DebugDraw` em cada um. Dessa forma, os elementos são renderizados normalmente e, em modo de depuração, são exibidas informações extras sobre seus componentes.

    8. No inicío do método `Shutdown`, percorra a lista de atores do jogo, deletando todos eles

3. **DrawComponent.cpp**

    1. Implemente o construtor `DrawComponent`, adicionando o componente que está sendo criado como desenhável no jogo `Game::AddDrawable`. Em seguida, crie os arrays de vértices e índices a partir do vetor de pontos fornecido `vertices`. Por fim, inicialize `mDrawArray` com os dados construídos.

    2. Implemente o destrutor `~DrawComponent` para remover o componente da lista de desenháveis do jogo. Libere a memória de `mDrawArray` e atribua `nullptr` para evitar ponteiros soltos.

    3. Implemente o método `Draw` para desenhar o componente chamando o método `Draw` do Renderer. Use a matriz de modelo do ator dono, os dados de vértices `mDrawArray` e a cor branca como parâmetros.

Ao final dessa primeira etapa, teste a sua implementação criando um actor com um `DrawComponent` na função `Game::Initialize()`. Por exemplo, experimente criar um actor representado por um triângulo com o código abaixo:

```
std::vector<Vector2> vertices;
vertices.emplace_back(Vector2(0.0f, -50.0f));
vertices.emplace_back(Vector2(-50.0f, 50.0f));
vertices.emplace_back(Vector2(50.0f, 50.0f));

Actor *testActor = new Actor(this);
testActor->SetPosition(Vector2(WINDOW_WIDTH / 2.0f, WINDOW_HEIGHT / 2.0f));

new DrawComponent(testActor, vertices);
```

O seu jogo deveria mostrar a seguinte saída:

<img src="{{ 'assets/images/tp2/1-result.png' | relative_url }}" alt="tp1-pong-1" width="560"/>

### **Parte 2: Movimentação de Objetos Rígidos**

Na segunda parte, você irá implementar os componentes `RigidBodyComponent` e `CircleColliderComponent` para movimentar e detectar as colisões dos objetos do jogo.

- **RigidBodyComponent.cpp**

    1. Implemente a função `ApplyForce` para atualizar a aceleração do corpo rígido somando a força recebida dividida pela massa (`force / mMass`).

    2. Implemente a função `Update` para integrar a física do corpo rígido: atualize a velocidade a partir da aceleração (limitando-a ao máximo), use essa velocidade para mudar a posição aplicando *screen wrap*, e depois zere a aceleração. Corrija velocidades muito pequenas para zero e, por fim, atualize a rotação do ator somando a **velocidade angular vezes o delta de tempo**, de forma análoga ao cálculo da translação, mas aplicado ao ângulo.

    3. Implemente a função `ScreenWrap` para garantir que o ator reapareça do lado oposto da tela quando sair dos limites. Caso a coordenada `x` seja menor que 0, defina como `WINDOW_WIDTH`, e se for maior que `WINDOW_WIDTH`, defina como 0 (o mesmo vale para `y` em relação a `WINDOW_HEIGHT`). Isso cria o efeito de teletransporte contínuo típico do jogo *Asteroids*.

- **CircleColliderComponent.cpp**

    1. Implemente o construtor `CircleColliderComponent` gerando os vértices que formam um círculo aproximado com 10 pontos igualmente espaçados, usando seno e cosseno para calcular as coordenadas. Depois, crie os índices que conectam os vértices em pares sequenciais e inicialize `mDrawArray` com esses dados para desenhar a malha do círculo. Esses vértices serão utilizados para visualizar a geometria de colisão desse componente, para fins de depuração.

    2. Implemente o destrutor `~CircleColliderComponent` para liberar a memória usada em `mDrawArray`. Após o `delete`, atribua `nullptr` ao ponteiro para evitar acessos inválidos.

    3. Implemente a função `Intersect` para verificar se dois círculos colidem, como visto em sala de aula.

    4. Implemente o método `DebugDraw` para desenhar graficamente o colisor circular, facilitando a depuração. Para isso, chame o método `Draw` do `renderer`, passando a matriz de modelo do ator, o `mDrawArray` e a cor verde.

Ao final dessa segunda etapa, teste a sua implementação adicionando componentes  `RigidBodyComponent` e `CircleColliderComponent` ao actor de criado ao final da etapa anterior. Aplique um força qualquer a esse actor, como no exemplo abaixo:

```
RigidBodyComponent *rb = new RigidBodyComponent(testActor, 1.0f);
CircleColliderComponent *cc = new CircleColliderComponent(testActor, 20.0f);
rb->ApplyForce(Vector2(0.0f, 1000.0f));
``` 

O seu jogo deveria mostrar uma saída como esta:

<iframe width="560" height="315" src="https://www.youtube.com/embed/KNRpZ-riUhQ?si=r-I9yf-AnpGSApcP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### **Parte 3: Objetos do Asteroids**

Na terceira parte, você irá utilizar os novos componentes para implementar uma nave que atira raios laser e gerar um dado número de asteroides com geometrias aleatórias.

- **Ship.cpp**

    1. Implemente o construtor de `Ship` para construir um triângulo de vértices para representar a nave e use-o para criar o `DrawComponent`, além de adicionar um `RigidBodyComponent` e um `CircleColliderComponent` para física e colisão.

    2. Implemente a função `OnProcessInput` para processar as teclas de controle da nave. Use **W** para aplicar uma força na direção para frente, **A** e **D** para ajustar a velocidade angular negativa ou positiva, respectivamente. 

    3. Implemente a função `OnUpdate` para atualizar o estado da nave a cada quadro. Primeiro, diminua o tempo de recarga do laser (`mLaserCooldown`) pelo `deltaTime`. Em seguida, percorra todos os asteroides do jogo e, se o colisor circular da nave intersectar o colisor de algum asteroide, finalize o jogo chamando `Quit()`.

Teste o seu código instanciando um objeto do tipo `Ship` no método `Game::InitializeActors`. Você deveria ser capaz de mover a nave com as teclas W, A e D, como no vídeo abaixo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/ho8f-YWqxfc?si=fuqhS-XHRmHGoEaC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- **Asteroid.cpp**
    
    1. Implemente a função `GenerateVertices` para criar os vértices que formam a silhueta irregular de um asteroide. Para isso, distribua `numVertices` pontos igualmente espaçados ao longo da circunferência (incrementando o ângulo a cada iteração) e, em cada ponto, calcule um comprimento aleatório. Use esse comprimento para gerar as coordenadas `(x, y)` e adicione-as ao vetor de vértices, que será retornado ao final.

    2. Implemente a função `CalculateAverageVerticesLength` para calcular o raio médio de um polígono. 

    3. Implemente a função `GenerateRandomStartingForce` para criar uma força inicial aleatória que movimenta o asteroide.

    4. Implemente o construtor de `Asteroid` gerando os vértices de um polígono irregular com `numVertices` e raio `radius`, calcule o comprimento médio desses vértices para usar como raio de colisão e crie os componentes `DrawComponent`, `RigidBodyComponent` e `CircleColliderComponent`. Por fim, aplique uma força inicial aleatória ao corpo rígido para movimentar o asteroide e registre-o no jogo com `AddAsteroid(this)`.

    5. Implemente o destrutor `~Asteroid` para remover esse arteroide da lista de asteroides do jogo. Isso garante que, ao ser destruído, ele não continue registrado no sistema de atualização e colisão.

Teste o seu código instanciando múltiplos (ex. 5) objetos do tipo `Asteroid` no método `Game::InitializeActors`. O jogo deveria ser fechado quando a nave colidir com um dos asteroids, como no vídeo abaixo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/YIo1VGZ-dXw?si=XRuuEBmmIAt9r3O5" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- **ParticleSystemComponent.cpp**

    1. Implemente o construtor de `Particle` criando os componentes `DrawComponent`, `RigidBodyComponent` e `CircleColliderComponent` para que a partícula tenha desenho, física e colisão. Por fim, configure o estado inicial da partícula como pausado, torne-a invisível e marque `mIsDead` como verdadeiro até que seja ativada pelo sistema de partículas.

    2. Implemente o método `Particle::Kill` para desativar a partícula quando sua vida terminar. Marque `mIsDead` como verdadeiro, pause o ator e torne o `DrawComponent` invisível. Por fim, reinicie sua velocidade para zero no `RigidBodyComponent`, garantindo que não continue em movimento após ser “morta”.

    3. Implemente o método `Particle::Awake` para reativar a partícula quando for emitida. Defina o tempo de vida (`mLifeTime`) recebido, marque `mIsDead` como falso e mude o estado para ativo, tornando o `DrawComponent` visível. Por fim, ajuste a posição e a rotação da partícula conforme os parâmetros recebidos.

    4. Implemente o método `Particle::OnUpdate` para controlar o ciclo de vida da partícula. Primeiro, reduza o tempo restante (`mLifeTime`) e, se ele chegar a zero ou menos, chame `Kill()` para desativar a partícula. Caso contrário, percorra todos os asteroides do jogo e, se houver colisão do colisor circular da partícula com o de algum asteroide, finalize a partícula chamando `Kill()` e marque o asteroide como destruído (`SetState(ActorState::Destroy)`).

    5. Implemente o construtor `ParticleSystemComponent` chamando o construtor da classe base `Component`. Em seguida, crie um **pool de partículas**: para cada índice até `poolSize`, instancie uma nova `Particle` com os vértices fornecidos e adicione-a ao vetor `mParticles`. Isso garante que o sistema tenha um conjunto fixo de partículas disponíveis para emissão durante o jogo.

    6. Implemente o método `ParticleSystemComponent::EmitParticle` para ativar uma partícula inativa do pool. Para isso, percorra o vetor `mParticles` e, ao encontrar a primeira morta, acorde-a (`Awake`) na posição do ator dono somada ao `offsetPosition`, usando a rotação atual e o tempo de vida fornecido. Em seguida, aplique uma força na direção do ator multiplicada pela velocidade, e encerre o loop para emitir apenas uma partícula por chamada.

Para testar o seu sistema de partículas, modifique o método `Ship::OnProcessInput` para verificar se a tecla **espaço** for pressionada. Caso esteja, verique o tempo de recarga, se ele estiver zerado, dispare um projétil pelo sistema de partículas e reinicie o `mLaserCooldown`. Essa etapa conclui as implementações básicas do jogo, que deveria ter um resultado como o mostrado no vídeo do início do roteiro.

### **Parte 4: Customização**

Na quarta, e última etapa, você irá ajustar as variáveis do jogo para criar uma versão única do Asteroids.

1. Ajuste os parâmetros do jogo da forma que achar mais interessante: tamanhos, cores, aceleração, velocidade escalar, velocidade máxima, velocidade mínima, massa. Garante que o jogador terá um tempo razoável para escapar dos asteroides no início do jogo.

2. Implemente a a funcionalidade de gerar três novos asteroides menores e mais rápidos quando um grande é destruído. 

3. Modifique o gráfico da nave para que ela parece mais uma letra 'A', do que um triângulo e adicione um propulsor na parte traseira, que pisca quando você acelera a nave (veja o vídeo do jogo original).

4. Implemente a animação de destruição dos asteroides. Quando um asteroide for destuído, crie um objeto com um sistema de partículas onde cada partícula é um ponto que é atirado para uma direção aleatória e vive por poucos segundos. 

## Submissão

Para submeter o seu trabalho, basta fazer o commit e o push das suas alterações no repositório que foi criado para você no GitHub classroom.

Observação: você pode realizar quantos commits quiser. Isso é inclusive recomendado para dividir o problema em partes menores. Além disso, as mensagens nos commits não precisam ser necessariamente "Submissão TP2". Você pode também criar branches se quiser, mas **apenas o último commit da branch `main` será avaliado**.


```
git add .
git commit -m 'Submissão TP2'
git push
```

## Barema

- Parte 1: Desenhos Vetoriais (10%)
- Parte 2: Movimentação de Objetos Rígidos (20%)
- Parte 3: Objetos do Asteroids (20%)
- Parte 4: Customização (50%)

## Referências

- Parte 1: 
    - [Game Programming in C++, Cap 3, Vectors nad Basic Physics](https://learning.oreilly.com/library/view/game-programming-in/9780134598185/ch03.xhtml)
