# Multiple resolutions

O problema de lidar com resoluções é antigo, e muitas abordagens já foram usadas para lidar com isso. A Godot usa uma abordagem muito comum hoje em dia: criar o jogo todo em uma *resolução base* e usar as funcionalidades da engine para encaixar onde for necessário. Como veremos, a Godot fornece um conjunto de funcionalidades para que esses ajustes de resolução sejam fáceis e práticos. 

Esta *resolução base* é a mesma que foi mencionada no [tutorial de viewports](https://github.com/felipebottega/Games/tree/gh-pages/Manual/Rendering/Using%20Viewports#size-e-stretch). Lembre que no tutorial do jogo [Galton Board 4](https://github.com/felipebottega/Games/tree/gh-pages/Manual/2D/Canvas%20layers/Galton%20Board%204#viewport-e-canvas-items) foi dito que o viewport é o espaço delimitado pelo retângulo azul do editor, e que este espaço representa a tela que o jogador enxerga no jogo. A resolução do viewport é esta resolução base. Portanto, a resolução base nada mais é do que a resolução "ideal" do seu jogo. Você define a resolução do jogo, desenvolve o jogo usando o viewport com estas dimensões, e só depois que o jogo está pronto é que você usa as funcionalidades da engine para ajustar a resolução base para uma outra resolução, a depender do dispositivo e configuração do jogador. 

Como já foi dito antes, a resolução do viewport (resolução base) é definida em *Project → Project Settings → Display → Window → Size → Viewport width/Height*. Além destes dois parâmetros, há outros essenciais nesta mesma tela de configurações. São estes parâmetros que definem o conjunto de funcionalidades para lidar com múltiplas resoluções. 

## Size/Mode

Fora a resolução base vista acima, este é o parâmetro mais simples de todos. Ele determina como a tela é carregada inicialmente quando o jogo abre. Acredito que as opções são claras, com exceção do fullscreen, pois há duas opções neste caso. O recomendado é usar a opção *Exclusive Fullscreen*, pois esta é feita para jogos, enquanto que a outra é para um outro tipo de aplicação específica.

Você pode alterar este parâmetro durante a execução do jogo com o comando `DisplayServer.window_set_mode(i)`, em que $i$ é um inteiro entre $0$ e $4$. Seguem abaixo os valores que você pode passar para a função. Vale notar que cada valor está associado a uma constante da engine também.
  
    WINDOW_MODE_WINDOWED = 0
    WINDOW_MODE_MINIMIZED = 1
    WINDOW_MODE_MAXIMIZED = 2
    WINDOW_MODE_FULLSCREEN = 3
    WINDOW_MODE_EXCLUSIVE_FULLSCREEN = 4


## Stretch/Mode


