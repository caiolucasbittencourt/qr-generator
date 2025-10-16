<script lang="ts">
  import { onMount } from 'svelte';
  import QRCode from 'qrcode';

  let texto: string = 'https://svelte.dev';
  let canvasEl: HTMLCanvasElement | null = null;

  async function gerar() {
    if (!canvasEl) return;
    await QRCode.toCanvas(canvasEl, texto, { width: 256, errorCorrectionLevel: 'M', margin: 1 });
  }

  onMount(() => gerar());
</script>

<main>
  <input bind:value={texto} type="text" placeholder="Digite o link" on:input={gerar} />
  <canvas bind:this={canvasEl} width={256} height={256}></canvas>
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    font-family: system-ui, sans-serif;
    background: #fff;
    color: #111;
    gap: 1rem;
  }
  input {
    padding: 1.5rem;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 48px;
    width: 300px;
    text-align: center;
  }
  canvas {
    margin-top: 1rem;
    background: #fff;
    border: 1px solid #ccc;
  }
</style>