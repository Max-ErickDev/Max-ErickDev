##👋Olá, eu sou Max-ErickDev
pacman.c
#include  <unistd.h>  // biblioteca para o delay => usleep()
#include  <stdlib.h>
#include  <time.h>
#include  <ncurses.h>
#define  H 25 // altura do mapa
#define  W 42 // largura do mapa
#definir  UP 0
#definir  DIREITA 1
#definir PARA  BAIXO 2
#definir  ESQUERDA 3

// Mapa
 mapa de caracteres [ H ][ W ] = {
{ '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , ' #' , '#' , '#' , '#' , ' # ' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '# ' , '#' , '# ' , '#' , '#' , ' # ' , ' # ' , '#' , '#' , '#' , '#' , ' #' , '#' , '#' , '#' , '#' , '#' , ' # ' },
{ '#' , 'x' , '.' , '. ' , ' .' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '#' , '.' , '.' , '.' , '.' , '. ' , '.' , '. ' , ' . ' , '.' , '.' , '. ' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '. ' , '.' , '. ' , ' .' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' .' , '. ' , '.' , ' . ' , '.' , '#' , '#' , '#' , '#' , '#' , '# ' , '#' , ' #' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '#' , '.' , ' .' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , ' . ' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , ' . ' , ' .' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '. ' , '.' , ' .' , '.' , '#' },
{ '#' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , '.' , 'O' , '#' },
{ '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '# ' , ' #' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' , '#' }
};

int  pontuação_do_jogo  =  0 ;

// Estrutura do Pacman
typedef  struct  _TAG_hero {
  int  pos [ 2 ];
   direção inteira ;
   rosto de carvão ;
} herói ;

// Estrutura dos extras
typedef  struct  _TAG_ghost {
  int  pos [ 2 ];
} fantasma ;

/*
herói pacman
fantasma piscando; // VERMELHO
rosa fantasma; // ROSA
fantasma escuro; // CIANO
fantasma clyde; // LARANJA
*/

// Mostrar o mapa.
// Recebe a posição do pacman, do blink e, futuramente, dos outros fantasmas
void  show_map ( int  pacman_pos [ 2 ], int  blinky_pos [ 2 ]) {
  int  i , j ;

  mover ( 0 , 0 ); // Move o cursor para a posição 0,0 da tela

  para ( i  =  0 ; i  <  H ; i ++ ) {
    para ( j  =  0 ; j  <  W ; j ++ ) {

      if ( j  ==  pacman_pos [ 0 ] &&  i  ==  pacman_pos [ 1 ] ) { // se for a posicao do pacman colore
        addch ( mapa [ i ][ j ] | COLOR_PAIR ( 1 ));
      } else  if ( j  ==  blinky_pos [ 0 ] &&  i  ==  blinky_pos [ 1 ]) { // se for a posicao do blinky
        addch ( mapa [ i ][ j ] | COLOR_PAIR ( 2 ));
      } else { // se não for o pacman e nenhuma fantasminha
        addch ( mapa [ i ][ j ]);
      }
    }
    addch ( '\n' ); // quebra a linha
  }
  mvprintw ( H  +  1 , 0 , "PONTUAÇÃO: %d" , pontuação_jogo ); // por fim, mostra a pontuação
  atualizar (); // e essa é a função para exibir tudo na tela.
}

// Função para mover o pacman no mapa
// recebe a direção desejada, a posição xey, e qual o caractere dele
void  movae_pacman ( int  direção , int  * x , int  * y , char  face ) {

  alternar ( direção ) {
    caso  KEY_UP :
      if ( map [ * y  -  1 ][ * x ] !=  '#' ) { // verifique se pra cima não tem parede
        mapa [ * y ][ * x ] =  ' ' ; // colocar espaço em branco por onde ele passou
        if ( map [ * y  -  1 ][ * x ] ==  '.' ) { // se a posição desejada tiver um . entao aumenta a pontuação
          pontuação_do_jogo  +=  10 ;
        }
        mapa [ - * y ][ * x ] =  face ; // coloca o pacman na nova posição
      }
      quebrar ;
    // os próximos passos fazem o mesmo que o anterior, mudando apenas a direção
    caso  KEY_DOWN :
      se ( map [ * y  +  1 ][ * x ] !=  '#' ) {
        mapa [ * y ][ * x ] =  ' ' ;
        se ( map [ * y  +  1 ][ * x ] ==  '.' ) {
          pontuação_do_jogo  +=  10 ;
        }
        mapa [ ++ * y ][ * x ] =  face ;
      }
      quebrar ;

    caso  KEY_LEFT :
      se ( map [ * y ][ * x  -  1 ] !=  '#' ) {
        mapa [ * y ][ * x ] =  ' ' ;
        se ( map [ * y ][ * x  -  1 ] ==  '.' ) {
          pontuação_do_jogo  +=  10 ;
        }
        mapa [ * y ][ -- * x ] =  face ;
      }
      quebrar ;

    caso  KEY_RIGHT :
      se ( map [ * y ][ * x  +  1 ] !=  '#' ) {
        mapa [ * y ][ * x ] =  ' ' ;
        se ( map [ * y ][ * x  +  1 ] ==  '.' ) {
          pontuação_do_jogo  +=  10 ;
        }
        mapa [ * y ][ ++ * x ] =  face ;
      }
      quebrar ;
  }
}


// Movimento do Blinky, código que não está funcionando
char  blinky_last_char  =  '.' ;
int  blinky_last_pos  =  -1 ;

void  movae_blinky ( int  * x , int  * y , int  pacman_pos [ 2 ]) {
  srand ( relógio () );
  int  dir  =  rand () % 4 ;

  se ( ( * x  -  pacman_pos [ 0 ]) < ( * y  -  pacman_pos [ 1 ]) ) {
    se ( blinky_last_pos  !=  LEFT ) {
      se ( map [ * y ][ * x  -  1 ] !=  '#' ) {
        mapa [ * y ][ * x ] =  último_caractere_piscante ;
        blinky_last_char  =  map [ * y ][ * x  -  1 ];
        mapa [ * y ][ -- * x ] =  'O' ;
        blinky_last_pos  =  ESQUERDA ;
      }
    }

    se ( blinky_last_pos  !=  DIREITA ) {
      se ( map [ * y ][ * x  +  1 ] !=  '#' ) {
        mapa [ * y ][ * x ] =  último_caractere_piscante ;
        blinky_last_char  =  map [ * y ][ * x  +  1 ];
        mapa [ * y ][ ++ * x ] =  'O' ;
        blinky_last_pos  =  DIREITA ;
      }
    }
  } outro {

  }

}

int  main () {

   chave inteira ;
  herói  pacman ;
  fantasma  blinky /*, pinky, inky, clyde*/ ;

  pacman.pos [ 0 ] = 1 ; // x​ 
  pacman.pos [ 1 ] = 1 ; // y​ 
  pacman.face = ' x ' ;  

  blinky.pos [ 0 ] = 40 ; // x​ 
  blinky.pos [ 1 ] = 23 ; // y​ 

  initcr (); //inicia o ncurses
  teclado ( stdscr , TRUE); // ativa o teclado numérico e setinhas
  conjunto_curs ( 0 ); // esconde o cursor da tela
  atraso no nó ( stdscr , VERDADEIRO); // ativa o nodelay para não parar no getch()
  start_color (); //inicia o sistema de núcleos do ncurses
  init_pair ( 1 , COLOR_YELLOW , COLOR_BLACK ); // cores do pacman
  init_pair ( 2 , COLOR_RED , COLOR_BLACK ); // núcleos fazem blinky

  show_map ( pacman.pos , blinky.pos ) ;​​​ //mostra o mapa

  // Loop do jogo
  fazer {
    chave  =  getch (); // pegar tecla do teclado, vai retornar ERR se não pegar nada (ver nodelay())
    if ( key  !=  ERR ) { // verifique se pegou alguma tecla, se der ERR foi pq não pegou
      pacman . direção  =  chave ; //atualiza a direção do pacman
    }

    movae_pacman ( pacman . direção , & pacman . pos [ 0 ], & pacman . pos [ 1 ], pacman . face ); // chama a função de mover o pacman
    movae_blinky ( & blinky . pos [ 0 ], & blinky . pos [ 1 ], pacman . pos ); // chama a função de mover o blinky

    show_map ( pacman.pos , blinky.pos ) ;​​​ // mostra o mapa

    // Atraso do jogo
    // esse if é por causa do tamanho das colunas serem diferentes
    // do tamanho das linhas, entao para que ele ande na "mesma"
    // a velocidade tem um atraso para quando ele estiver movendo na
    // vertical e outro para quando ele se move na horizontal
    se ( pacman . direção  ==  KEY_UP  ||  pacman . direção  ==  KEY_DOWN ) {
      dormir ( 130000 ); // Atraso maior pelo tamanho das colunas
    } outro {
      usleep ( 85000 );
    }
  } enquanto ( tecla  !=  'e' );

  conjunto_curs ( 1 ); //voltar o cursor para a configuração inicial
  endwin (); //finaliza um ncurses
  retornar  0 ;
}
**Max-ErickDev/Max-ErickDev** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
