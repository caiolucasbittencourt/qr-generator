<script lang="ts">
  import { onMount } from 'svelte';
  import QRCode from 'qrcode';
  import './main.scss';

  let texto: string = 'https://svelte.dev';
  let canvasEl: HTMLCanvasElement | null = null;

  async function gerar() {
    if (!canvasEl) return;
    await QRCode.toCanvas(canvasEl, texto, { 
      width: 256, 
      errorCorrectionLevel: 'M', 
      margin: 1 
    });
  }

  onMount(() => gerar());
</script>

<main>
  <input 
    bind:value={texto} 
    type="text" 
    placeholder="Digite o link" 
    on:input={gerar} 
  />
  
  <canvas bind:this={canvasEl} width={256} height={256}></canvas>
</main>
