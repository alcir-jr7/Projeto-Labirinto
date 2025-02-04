<script lang="ts">
    import { goto } from '$app/navigation';
    import { onDestroy } from 'svelte';  // Importando para limpar o intervalo quando o componente for destruído

     class Coordenada {
        linha: number;
        coluna: number;
    

    constructor (linha:number,coluna:number){
        this.linha=linha;
        this.coluna=coluna;
    }
}

   class EstadoJogo {
        posicaoPersonagem: Coordenada;
        posicaoObjetivo: Coordenada;
        mapa: number[][];
        fase: number;
        tempoRestante: number;
        tempoAcabou: boolean;
    
    constructor (posicaoPersonagem: Coordenada, posicaoObjetivo:Coordenada,mapa: number[][], fase: number,tempoRestante: number,tempoAcabou: boolean){
        this.posicaoPersonagem=posicaoPersonagem;
        this.posicaoObjetivo=posicaoObjetivo;
        this.mapa=mapa;
        this.fase=fase;
        this.tempoRestante=tempoRestante;
        this.tempoAcabou=tempoAcabou;
    }
}

    // Função para inicializar o jogo em uma fase
    function inicializarJogo(fase: number) : EstadoJogo {
        let personagem : Coordenada = new Coordenada();
        personagem.linha = 0;
        personagem.coluna = 0;

        let objetivo : Coordenada = new Coordenada();
        
        let mapa : number[][] = [];
        let tempoFase: number;  // Nova variável para armazenar o tempo de cada fase

        if (fase === 1) {
            objetivo.linha = 5;
            objetivo.coluna = 5;
            mapa = [[0, 1, 0, 0, 0, 0],
                    [0, 1, 0, 1, 1, 0],
                    [0, 0, 0, 1, 0, 0],
                    [1, 1, 0, 1, 0, 1],
                    [0, 0, 0, 1, 0, 0],
                    [0, 1, 0, 0, 0, 0]];
            tempoFase = 25; // Tempo para a fase 1
        } else if (fase === 2) {
            objetivo.linha = 7;
            objetivo.coluna = 7;
            mapa = [[0, 1, 0, 0, 0, 0, 0, 0],
                    [0, 1, 0, 1, 1, 1, 0, 0],
                    [0, 0, 0, 1, 0, 0, 0, 1],
                    [1, 1, 0, 1, 0, 1, 1, 0],
                    [0, 0, 0, 1, 0, 0, 0, 0],
                    [0, 1, 1, 1, 1, 1, 1, 0],
                    [0, 0, 0, 0, 0, 1, 0, 0],
                    [0, 1, 1, 1, 1, 1, 0, 0]];
            tempoFase = 20; // Tempo para a fase 2
        } else if (fase === 3) {
            objetivo.linha = 9;
            objetivo.coluna = 9;
            mapa = [[0, 1, 0, 0, 0, 0, 0, 1, 0, 0],
                    [0, 1, 0, 1, 1, 1, 0, 0, 1, 0],
                    [0, 0, 0, 1, 0, 0, 1, 0, 1, 0],
                    [1, 1, 0, 1, 0, 1, 1, 1, 0, 0],
                    [0, 0, 0, 1, 0, 0, 0, 1, 0, 1],
                    [0, 1, 1, 1, 1, 1, 1, 0, 0, 0],
                    [0, 0, 0, 1, 0, 0, 0, 0, 1, 0],
                    [0, 1, 1, 0, 1, 1, 0, 1, 0, 1],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
                    [1, 1, 1, 1, 0, 1, 0, 1, 0, 0]];
            tempoFase = 15; // Tempo para a fase 3
        }

        let estado : EstadoJogo = new EstadoJogo();
        estado.posicaoPersonagem = personagem;
        estado.posicaoObjetivo = objetivo;
        estado.mapa = mapa;
        estado.fase = fase;
        estado.tempoRestante = tempoFase;  // Define o tempo de cada fase
        estado.tempoAcabou = false;

        return estado;
    }

    function houveColisao(posicao : Coordenada, jogo : EstadoJogo) : boolean {
        return (posicao.linha < 0 || posicao.coluna < 0)
            || (posicao.linha >= jogo.mapa.length || posicao.coluna >= jogo.mapa[0].length)
            || jogo.mapa[posicao.linha][posicao.coluna] == 1;
    }

    function onKeyDown(evento) : void {
        if (jogo.tempoAcabou) return; // Impede movimentação se o tempo acabou

        let novaPosicao = new Coordenada();
        novaPosicao.linha = jogo.posicaoPersonagem.linha;
        novaPosicao.coluna = jogo.posicaoPersonagem.coluna;

        switch(evento.keyCode) {
            case 38: // up
                novaPosicao.linha--;
                break;
            case 40: // down
                novaPosicao.linha++;
                break;
            case 37: // left
                novaPosicao.coluna--;
                break;
            case 39: // right
                novaPosicao.coluna++;
                break;
        }

        if (novaPosicao.linha == jogo.posicaoObjetivo.linha && novaPosicao.coluna == jogo.posicaoObjetivo.coluna) {
            alert("Parabéns, você chegou ao objetivo na fase " + jogo.fase);
            if (jogo.fase < 3) {
                jogo = inicializarJogo(jogo.fase + 1);
                jogo.posicaoPersonagem.linha = 0; // Reseta o personagem ao topo
            } else {
                alert("Você completou todas as fases!");
                limparTempo();  // Chama a função para limpar o intervalo
                goto("/"); // Redireciona para a página inicial
            }
        }

        if (!houveColisao(novaPosicao, jogo)) {
            jogo.posicaoPersonagem = novaPosicao;
        }
    }

    function diminuirTempo() {
        if (jogo.tempoRestante > 0) {
            jogo.tempoRestante--;
        } else if (!jogo.tempoAcabou) {
            jogo.tempoAcabou = true;  // Marca que o tempo acabou
            alert("Tempo esgotado! Você perdeu a partida.");
            limparTempo();  // Chama a função para limpar o intervalo
            goto("/");  // Redireciona para a página inicial
        }
    }

    function limparTempo() {
        clearInterval(tempoInterval);  // Limpa o intervalo do tempo
    }

    let jogo : EstadoJogo = inicializarJogo(1);

    // Inicia o contador de tempo a cada 1000ms (1 segundo)
    const tempoInterval = setInterval(diminuirTempo, 1000);
</script>

<h1>Escape the Maze</h1>

<p class="tempo-restante">Tempo restante: {jogo.tempoRestante} segundos</p> <!-- Exibe o tempo restante -->

<table>
    {#each jogo.mapa as linha, i}
        <tr>
            {#each linha as celula, j}
                <td class="celula {i === jogo.posicaoPersonagem.linha && j === jogo.posicaoPersonagem.coluna ? 'personagem' : 
                                  i === jogo.posicaoObjetivo.linha && j === jogo.posicaoObjetivo.coluna ? 'objetivo' : 
                                  jogo.mapa[i][j] === 0 ? '' : 'bloco'}">
                    {#if i === jogo.posicaoPersonagem.linha && j === jogo.posicaoPersonagem.coluna}
                        <img src="/images/personagem.gif" alt="Personagem" class="personagem-gif" />
                    {/if}
                </td>
            {/each}
        </tr>
    {/each}  
</table>

<br />

<a class="menu" href="/" on:click|preventDefault={() => { limparTempo(); goto("/"); }}>VOLTAR AO MENU</a>

<svelte:window on:keydown|preventDefault={onKeyDown} />

<audio autoplay loop>
    <source src="/audio/suspense.mp3" type="audio/mp3">
    Seu navegador não suporta o elemento de áudio.
</audio>

