<script lang="ts">
  let tituloDoJogo: string = "Escape the Maze";

  // Estado dos botões
  let musicaLigada: boolean = true;
  let efeitosLigados: boolean = true;

  // Criar referência para o elemento de música
  let musica: HTMLAudioElement;

  // Função para alternar música
  function alternarMusica(): void {
    musicaLigada = !musicaLigada;
    if (musicaLigada) {
      musica.play(); // Toca a música
    } else {
      musica.pause(); // Pausa a música
    }
  }

  // Função para alternar efeitos sonoros
  function alternarEfeitos(): void {
      efeitosLigados = !efeitosLigados;
      // Lógica para ativar/desativar efeitos sonoros pode ser adicionada aqui
  }
</script>

<div class="quadro-conteudo">
  <h1>{tituloDoJogo}</h1>

  <img class="logo" src="/images/emp.png" alt="logo IPFE" />

  <br />
  <br />
  <a class="menu" href="/jogar">JOGAR</a>
  <br />
  <a class="menu" href="/sobre">SOBRE</a>
</div>

<!-- Botões no canto inferior direito -->
<div class="configuracoes-som">
  <button class="botao-som musica" on:click={alternarMusica}>
      <img src="/images/nota.jpeg" alt="Música" class="{musicaLigada ? 'ativo' : 'desligado'}" />
  </button>
  <button class="botao-som efeitos" on:click={alternarEfeitos}>
      <img src="/images/efeitos.jpeg" alt="Efeitos" class="{efeitosLigados ? 'ativo' : 'desligado'}" />
  </button>
</div>

<!-- Elemento de áudio para a música -->
<audio autoplay loop>
  <source src="/audio/esporte1.mp3" type="audio/mp3">
  Seu navegador não suporta o elemento de áudio.
</audio>
