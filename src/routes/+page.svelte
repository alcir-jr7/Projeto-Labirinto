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
      musica.muted = false; // Desmuta a música
    } else {
      musica.pause(); // Pausa a música
      musica.muted = true; // Muta a música
    }
  }

  // Função para alternar efeitos sonoros
  function alternarEfeitos(): void {
    efeitosLigados = !efeitosLigados;
    // Lógica para ativar/desativar efeitos sonoros pode ser adicionada aqui
  }

  // Função para fechar o vídeo e começar a música
  function fecharVideo() {
    mostrarVideo = false;
    musica.play(); // Começa a tocar a música quando o vídeo é fechado
    musica.muted = false; // Desmuta a música quando o vídeo é fechado
    // Armazenar no sessionStorage para garantir que o vídeo não seja mostrado novamente durante a sessão
    sessionStorage.setItem('videoAssistido', 'true');
  }

  // Aguardar o componente ser montado antes de associar o áudio e vídeo
  onMount(() => {
    musica = document.getElementById('musica') as HTMLAudioElement;
    musica.pause(); // Garantir que a música esteja pausada inicialmente
    
    // Verifica se o vídeo já foi assistido nesta sessão
    const videoAssistido = sessionStorage.getItem('videoAssistido');
    mostrarVideo = !videoAssistido; // Exibe o vídeo apenas se não tiver sido assistido
  });

  // Estado do vídeo
  let mostrarVideo = false;
</script>

<!-- Vídeo exibido ao carregar a página, mas só uma vez por sessão -->
{#if mostrarVideo}
  <div class="video-container">
    <video autoplay>
      <source src="/video/runner2.mp4" type="video/mp4">
      Seu navegador não suporta o elemento de vídeo.
    </video>
    <button on:click={fecharVideo}>Fechar vídeo</button>
  </div>
{/if}

<!-- Conteúdo da página -->
<div class="quadro-conteudo">
  <img class="logo" src="/images/logogame.png" alt="logo IPFE" />
  <a class="menu" href="/jogar" on:click={() => mostrarVideo = true}>JOGAR</a>
  <br />
  <a class="menu" href="/sobre">SOBRE</a>
</div>

<!-- Botões de configuração de som -->
<div class="configuracoes-som">
  <!-- Botão de música -->
  <button class="botao-som efeitos" on:click={alternarEfeitos}>
    <img src="/images/efeitos.jpeg" alt="efeitos" class="{efeitosLigados ? 'ativo' : 'desligado'}" />
  </button>
  <!-- Botão de efeitos -->
  <button class="botao-som musica" on:click={alternarMusica}>
    <img src="/images/nota.jpeg" alt="musica" class="{musicaLigada ? 'ativo' : 'desligado'}" />
  </button>
</div>

<!-- Elemento de áudio para a música -->
<audio id="musica" autoplay loop>
  <source src="/audio/esporte1.mp3" type="audio/mp3">
  Seu navegador não suporta o elemento de áudio.
</audio>


