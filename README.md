# Multiple resolutions

O problema de lidar com resoluções é antigo, e muitas abordagens já foram usadas para lidar com isso. A Godot usa uma abordagem muito comum hoje em dia: criar o jogo todo em uma *resolução base* e usar as funcionalidades da engine para encaixar a tela do jogo onde for necessário. Como veremos, a Godot fornece um conjunto de funcionalidades para que esses ajustes de resolução sejam fáceis e práticos. 

Lembre que no tutorial do jogo [Galton Board 4](https://github.com/felipebottega/Games/tree/gh-pages/Manual/2D/Canvas%20layers/Galton%20Board%204#viewport-e-canvas-items) foi dito que o viewport é o espaço delimitado pelo retângulo azul do editor, e que este espaço representa a tela que o jogador enxerga no jogo. A resolução do viewport é esta resolução base (é a mesma que foi mencionada no [tutorial de viewports](https://github.com/felipebottega/Games/tree/gh-pages/Manual/Rendering/Using%20Viewports#size-e-stretch) no caso de subviewport). Portanto, a resolução base nada mais é do que a resolução "ideal" do seu jogo. Você define a resolução do jogo, desenvolve o jogo usando o viewport com estas dimensões, e só depois que o jogo está pronto é que você usa as funcionalidades da engine para ajustar a resolução base para uma outra resolução, a depender do dispositivo e configuração utilizada no jogo. 

Como já foi dito antes, a resolução do viewport (resolução base) é definida em *Project → Project Settings → Display → Window → Size → Viewport width/Height*. Além destes dois parâmetros, há outros essenciais nesta mesma tela de configurações. São estes parâmetros que definem o conjunto de funcionalidades para lidar com múltiplas resoluções.

## Aba Game

Antes de começar a falar dos outros parâmetros, é necessário comentar sobre a aba "Game" que tem no editor e nunca foi usada até agora.

<p align="center">
  <img width="400" src="https://github.com/user-attachments/assets/f1c116b3-ca5b-4c8c-b992-45f69dd76721" />
</p>

Ter as duas opções abaixo marcadas é o default. Ao dar play, ele vai abrir uma nova janela do editor, contendo o jogo sendo executado. Se você desmarcar apenas a opção *Make Game Workspace Floating on Next Play*, o jogo será carregado dentro do editor em vez de criar uma janela nova. Se você desmarcar a opção *Embed Game on Next Play*, ele vai abrir uma janela contendo apenas o jogo ao ser executado. A diferença desta janela para a anterior é que esta mostra apenas o jogo, enquanto que a anterior tem opções do editor.

<p align="center">
  <img width="850" src="https://github.com/user-attachments/assets/0889d3a4-8c68-4e99-9ca3-1a89141f5267" />
</p>

É importante essa distinção pois a janela do editor não é a janela do jogo. Ou seja, se você tentar alterar o tamanho da janela do jogo via script com a opção *Embed Game on Next Play* marcada, nada vai acontecer. Caso queira ver este comportamento no jogo, deve desmarcar a opção *Embed Game on Next Play*. Aí a janela a ser aberta será de fato a janela do jogo, não do editor. 

> PS: Fora do contexto deste tutorial, esse conhecimento pode ser útil para quem não quer mais janelas sendo abertas durante o desenvolvimento. Muitas vezes é mais prático alternar abas na Godot que alternar janelas (editor e jogo).

## Size/Mode

Fora a resolução base vista acima, este é o parâmetro mais simples de todos. Ele determina como a tela é carregada inicialmente quando o jogo abre. Acredito que as opções são claras, com exceção do fullscreen, pois há duas opções neste caso. O recomendado é usar a opção *Exclusive Fullscreen*, pois esta é feita para jogos, enquanto que a outra é para um outro tipo de aplicação específica.

Você pode alterar este parâmetro durante a execução do jogo com o comando `DisplayServer.window_set_mode(i)`, em que $i$ é um inteiro entre $0$ e $4$. Seguem abaixo os valores que você pode passar para a função. Cada valor está associado a uma constante da engine também.
  
    WINDOW_MODE_WINDOWED = 0
    WINDOW_MODE_MINIMIZED = 1
    WINDOW_MODE_MAXIMIZED = 2
    WINDOW_MODE_FULLSCREEN = 3
    WINDOW_MODE_EXCLUSIVE_FULLSCREEN = 4

Ao colocar em modo de janela (windowed), a resolução é ajustada dinamicamente enquanto você altera o tamanho da janela. Ao colocar em tela cheia (fullscreen), a resolução é ajustada para igualar a resolução do monitor. 

## Stretch/Mode

O *stretch mode* determina como a resolução base é esticada para se encaixar na janela ou tela. Você pode alterar este parâmetro durante a execução do jogo com o comando `get_tree().root.content_scale_mode = i`, em que $i$ é um inteiro entre $0$ e $2$. Cada valor está associado a uma constante da engine também.
  
    CONTENT_SCALE_MODE_DISABLED = 0
    CONTENT_SCALE_MODE_CANVAS_ITEMS = 1
    CONTENT_SCALE_MODE_VIEWPORT = 2

- **Disabled:**  Nenhum esticamento é feito. Se o seu jogo está em $640 \times 480$ e você coloca ele para rodar em tela cheia em um monitor de $1920 \times 1080$, ele vai manter o tamanho original e o restante do espaço será simplesmente preenchido (veremos adiante como pode ser feito este preenchimento). Abaixo temos um exemplo de um jogo renderizado em uma janela de dimensões $720 \times 720$ e depois o mesmo renderizado em tela cheia. O stretch mode ele está em *Disabled*, por isso a mudança de tamanho da tela apenas ampliou o espaço visível, sem afetar a resolução do jogo em si. 

<p align="center">
  <img width="350" src="https://github.com/user-attachments/assets/e3a39dac-531c-4fcb-bbcb-9901de9a179b" />
  <img width="1100" src="https://github.com/user-attachments/assets/b6ce5921-403e-4f0f-8ed3-b97fccc3d493" />
</p>

Note que no caso da tela cheia, tivemos um fundo branco que passou a ser cinza de repente. Isso aconteceu porque a tela cheia conseguiu ir além do background branco que tinha no jogo, revelando um pouco do espaço vazio do viewport.

<p align="center">
  <img width="1100" src="https://github.com/user-attachments/assets/363c8f2c-3b76-421a-a72b-20cf73213da0" />
</p>

- **Canvas Items:** Neste modo, o jogo primeiro é renderizado na sua resolução base. Se houver algum esticamento, ele simplesmente vai ampliar o jogo já renderizado. Se o seu jogo está em $640 \times 480$ e você coloca ele para rodar em tela cheia em um monitor de $1920 \times 1080$, ele vai renderizar em $640 \times 480$ e depois ampliar $1920 \times 1080$ sem aplicar nenhum algoritmo extra. Isso pode resultar em pixels grandes. Este modo é ideal quando você quer fidelidade à pixel-art do jogo.

- **Viewport:** Neste modo, o jojo é renderizado na resolução target. Se houver algum esticamente, ele vai considerar a resolução ampliada e renderizar nesta dimensão. Se o seu jogo está em $640 \times 480$ e você coloca ele para rodar em tela cheia em um monitor de $1920 \times 1080$, ele vai renderizar em $1920 \times 1080$, o que pode envolver aplicar nenhum algoritmo extra de up-sampling, por exemplo. É possível inrtroduzir artefatos na pixel-art com este modo, mas ele garante mais qualidade geral para dimensões maiores que a resolução base.

Na figura abaixo nós podemos ver claramente a diferença entre o modo canvas items e viewport. Para começar a análise, é importante mencionar que a resolução base é $180 \times 180$. O jogo está rodando em $1920 \times 720$, então a resolução está sendo aumentada em relação a original. Entre as duas imagens, a única diferença é o stretch mode, sendo canvas items na esquerda e viewport na direita. Toda a UI (*user interface*) é afetada por essa escolha, como podemos ver claramente. 

<p align="center">
  <img width="800" src="https://github.com/user-attachments/assets/6ebe17a9-b450-4a16-a9c4-38c7610a6377" />
</p>

Apesar de não ser evidente, o quadriculado e logo da Godot também sofreram alterações. Originalmente, são imagens pixeladas e com largura de $180$ pixels. Note que no modo canvas items a figura tem um certo embaçamento, resultado do up-scaling aplicado. Podemos trocar estas imagens pequenas por uma de alta resolução. Note que não importa se vamos colocar uma imagem de alta resolução em uma resolução base $180 \times 180$, a engine sabe que a imagem tem resolução alta e vai usar isso se o jogo pedir por uma resolução maior no fim das contas. 

<p align="center">
  <img width="800" src="https://github.com/user-attachments/assets/6f9dcb8c-9a4c-4e42-a329-0452f5b21c4f" />
</p>

> PS: Você pode fazer um jogo inteiro utilizando assets de resolução alta em um viewport de resolução base mais baixa, de modo a ter duas opções de resolução no seu jogo. A figura acima indica isso. Porém, isso não é prático, pois é limitado a duas resoluções apenas. Para ter múltiplas resoluções, o indicado é usar o modo windowed e escolher alguma resolução dentre as várias opções (ou simplesmente esticar a janela, pois a engine ajusta a resolução automaticamente neste caso). Se você quiser múltiplas escolhas de resolução em fullscreen, é melhor usar subviewport. Veremos como fazer isso no próximo tutorial.

Na questão de movimento, também há diferenças entre o modo canvas items e o viewport. olhando os vídeos abaixo, o impulso é dizer que o canvas items é melhor, mas é importante ter em mente que ele cria pixels para o up-scaling, enquanto que o viewport é mais fiel à pixel-art original. Tudo depende do que você quer no seu jogo.

## Aspect/Ignore

Este parâmetro determina como a figura deforma e o que a tela mostra ao esticar. Só tem efeito se o stretch mode não for *disabled*.

- **Ignore:** 
