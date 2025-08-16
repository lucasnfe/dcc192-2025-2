---
layout: page
title:  TP0 - Configuração Inicial
permalink: /avaliacoes/tp0-config-inicial
nav_exclude: true
---

# TP0: Configuração Inicial

#### Entrega: 27/08, 23:59h

## Introdução

Jogos digitais são projetos de software relativamente grandes e complexos. Sendo assim, em contextos profissionais, eles são tipicamente desenvolvidos de maneira estruturada em um ambiente de desenvolvimento integrado (IDE) com um sistema de controle de versão.
Nesse projeto você irá configurar o ambiente de desenvolvimento de jogos que será utilizado ao longo do curso.

Primeiro você irá baixar e instalar a IDE CLion para programação e teste dos jogos. Em seguida, você irá criar um repositório GitHub (via Gihub Classroom) para o controle de versão e submissão do seu trabalho. Além disso, como primeiro projeto do seu repositório, você irá escrever um pequeno programa em C++ usando a biblioteca SDL para desenhar um quadrado em uma janela, como ilustrado na figura abaixo:

<img src="{{ 'assets/images/tp0/1-result.png' | relative_url }}" alt="tp0-config-inicial" width="1024"/>

Ao terminar esse programa, você terá que customizá-lo adicionando algumas funcionalidades extras, que estão descritas no final desse roteiro.

## Código-base

Siga as instruções abaixo para baixar o código-base desse trabalho prático:

1. Se você não tiver uma conta no GitHub, crie uma seguindo o tutorial [**[nesse link]**](https://git-scm.com/book/pt-br/v2/GitHub-Configurando-uma-conta)
2. Se você não tiver o git instalado no seu computador, faça a instalação seguindo o tutorial [**[nesse link]**](https://git-scm.com/book/pt-br/v2/Come%C3%A7ando-Instalando-o-Git)
3. Aceite o projeto `TP0: Configuração Inicial` no GitHub classroom [**[nesse link]**](https://classroom.github.com/a/6Q71KAMV). Nesse momento, o GitHub Classroom irá te mostrar a lista de alunos matriculados nas disciplina. **Selecione o seu nome para que eu possa associar a sua conta GitHub com o seu número de matrícula na UFMG. Se seu nome não estiver na lista, NÃO prossiga e entre em contato com o professor.**
4. Clone o seu novo repositório no seu computador:

    ```
    # Substitua <GITHUB_USERNAME> pelo seu usuário do GitHub
    git clone https://github.com/ufmg-dcc192/tp0-config-inicial-<GITHUB_USERNAME>.git
    ```

## Instruções

### **Parte 1: Instalação**

Na primeira parte, você irá baixar e instalar a IDE CLion e a biblioteca SDL.

1. **Instalar a IDE CLion**

    1. Acesse o site da CLion [**[nesse link]**](https://www.jetbrains.com/clion) e siga as instruções de instalação para o seu sistema operacional;

    2. Durante a instalação, crie uma conta utilizando o seu email do DCC (ou da UFMG), o que irá ativar uma licensa gratuita.

2. **Instalar a bilioteca SDL**

    - **Linux (Ubuntu/Debian)**

        Para instalar a SDL no Ubuntu, basta executar o seguinte comando no terminal:

        ```
        sudo apt-get install libsdl2=2.32.2 libsdl2-dev=2.32.2
        ```

    - **Linux (Outras Distribuições)**

        1. Acesse o site da versão 2.32.2 da SDL [**[nesse link]**](https://github.com/libsdl-org/SDL/releases/tag/release-2.32.2) e baixe o pacote `Source code.zip`

        2. Extraia o conteúdo do pacote no diretório temporário `/tmp/SDL2/`

        4. Instale a biblioteca no diretório `/opt/SDL2/`:

            ```
            cd /tmp/SDL2/
            ./configure --prefix /opt/SDL2/
            make all
            sudo make install
            ```

    - **Windows (64 bits)**

        1. Acesse o site da versão 2.32.2 da SDL [**[nesse link]**](https://github.com/libsdl-org/SDL/releases/tag/release-2.32.2) e baixe o pacote `SDL2-devel-2.32.2-VC.zip`

        2. Extraia o conteúdo do pacote no diretório `C:\Arquivos de Programas\SDL2\`, como na figura a seguir:

            <img src="{{'/assets/images/tp0/2-parte1-win.png' | prepend: site.baseurl }}" alt="p1-parte1" width="1024"/>

        3. Copie o arquivo `SDL2.dll` do diretório `C:\Arquivos de Programas\SDL2\lib\x64\` para o diretório `C:\Windows\System32\`, como na figura a seguir:

            <img src="{{'/assets/images/tp0/3-parte2-win.png' | prepend: site.baseurl }}" alt="p1-parte2" width="1024"/>            

    - **Mac**
        1. Acesse o site da versão 2.32.2 da SDL [**[nesse link]**](https://github.com/libsdl-org/SDL/releases/tag/release-2.32.2) e baixe o pacote `SDL2-2.32.2.dmg`

        2. Clique na imagem para abrí-la e copie e o pacote `SDL2.framework` para o diretório `/Library/Frameworks/`. Ao final dessa etapa, a SDL deve estar configurada dessa maneira:

            <img src="{{'/assets/images/tp0/4-parte1-mac.png' | prepend: site.baseurl }}" alt="p1-parte1" width="1024"/>

### **Parte 2: primeiro programa SDL**

Nessa etapa, você irá utilizar a IDE Clion para escrever um programa em C++/SDL2 que desenha um quadrado em uma janela. Siga as instruções a seguir para completar a tarefa. As instruções estão relacionadas com  comentários ordenados começando com `TODO` no código-base, como ilustrado na figura a seguir:

<img src="{{'/assets/images/tp0/5-main.png' | prepend: site.baseurl }}" alt="tp0-parte5" width="1024"/>

1. Inicialize o subsistema de vídeo da SDL e verifique se a inicialização ocorreu com sucesso. Se não, imprima uma mensagem de erro para o usuário com a função `SDL_Log` e retorne -1.

2. Crie uma janela e verifique se a criação ocorreu com sucesso. Se não, imprima uma mensagem de erro para o usuário com a função `SDL_Log` e retorne -1;

3. Crie um renderer utlizando as flags de aceleração gráfica e sincronização vertical (vsync). Verifique se a criação ocorreu com sucesso. Se não tiver ocorrido com sucesso, imprima uma mensagem de erro para o usuário com a função SDL_Log e retorne -1;

4. Escolha uma cor para ser a cor do fundo e configure ela para ser utilizada ao desenhar na tela;

5. Limpe o buffer de fundo com a cor configurada no item anterior;

6. Escolha outra cor diferente da do fundo para ser a cor do quadrado que será desenhado e altere novamente a cor utilizada para desenhar na tela;

7. Crie um quadrado com `SDL_Rect` e desenhe-o com o renderer.

8. Chame a função `SDL_RenderPresent` para atualizar a tela com as chamadas de desenho (Draw) feitas anteriormente. Lembre-se que a SDL utiliza buffer duplo para desenhar objetos na tela, portanto precisamos chamar essa função para trocar o buffer da frente com o buffer de fundo.

9. Implemente um loop que processa eventos de entrada enquanto não houver um evento do tipo `SDL_QUIT`.

10. Quando o loop terminar, destrua o renderer e janela criados. Em seguida, finalize o subsistema de vídeo da SDL.

### **Parte 3: Customização**

Na terceira, e última etapa, você irá adicionar algumas funcionalidades extras:

1. Desenhe pelo menos mais um quadrado na tela preenchido com uma cor diferente do primeiro.

2. Utilize a função `SDL_RenderDrawLine` para implementar uma função que desenha um polígono regular de N lados. Utilize essa função para aproximar um círculo, escolhendo um valor alto para N (ex. 64).

3. Utilize os eventos de entrada do mouse para mover esse círculo na tela quando usuário clicar com o botão direito no círculo e arrastar o mouse com o botão pressionado. Dica: para saber se o usuário clicou no círculo, você terá que verificar se o ponto clicado está dentro do círculo.

Nessa etapa, sinta-se à vontade para deixar o resultado do seu TP mais criativo, incrementando a interatividade, compondo desenhos interessantes, etc.

## Submissão

Para submeter o seu trabalho, basta fazer o commit e o push das suas alterações no repositório que foi criado para
você no GitHub classroom.

```
git add .
git commit -m "Submissão TP0"
git push
```

Observação: você pode realizar quantos commits quiser. Isso é inclusive recomendado para dividir o problema em partes menores. Além disso, as mensagens nos commits não precisam ser necessariamente "Submissão TP0". Você pode também criar branches se quiser, mas **apenas o último commit da branch `main` será avaliado**.

## Barema

- Parte 1: Instalação (0%)
- Parte 2: Um primeiro programa SDL (30%)
- Parte 3: Customização (70%)

## Referências

- Git: [https://git-scm.com/book/pt-br/v2](https://git-scm.com/book/pt-br/v2)
- Instalação da SDL: [https://lazyfoo.net/tutorials/SDL/01_hello_SDL/index.php](https://lazyfoo.net/tutorials/SDL/01_hello_SDL/index.php)
