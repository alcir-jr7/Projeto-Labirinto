<script lang="ts">
  import { onMount } from 'svelte';

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

  // Aguardar o componente ser montado antes de associar o áudio
  onMount(() => {
    musica = document.getElementById('musica') as HTMLAudioElement;
  });
</script>

<div class="quadro-conteudo">
  <img class="logo" src="/images/logogame.png" alt="logo IPFE" />
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
<<<<<<< HEAD
<audio id="musica" autoplay loop>
=======
<audio autoplay loop>
>>>>>>> 65857543419a99c60845b2321474421098b2bbd2
  <source src="/audio/esporte1.mp3" type="audio/mp3">
  Seu navegador não suporta o elemento de áudio.
</audio>
