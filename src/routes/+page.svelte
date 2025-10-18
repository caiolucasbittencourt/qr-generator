<script lang="ts">
  import { onMount } from 'svelte';
  import QRCode from 'qrcode';
  import './main.scss'

  let texto: string = 'https://github.com/caiolucasbittencourt';
  let canvasEl: HTMLCanvasElement;
  let size: number = 300;
  let colorDark: string = '#FFFFFF';
  let colorLight: string = '#000000';
  let activeColor: 'white' | 'black' = 'white';

  async function gerar() {
    if (!canvasEl) return;
    await QRCode.toCanvas(canvasEl, texto, {
      width: size,
      errorCorrectionLevel: 'H',
      margin: 2,
      color: {
        dark: colorDark,
        light: colorLight,
      },
    });
  }

  function handleSetColor(type: 'black' | 'white') {
    activeColor = type;
    if (type === 'black') {
      colorDark = '#000000';
      colorLight = '#FFFFFF';
    } else {
      colorDark = '#FFFFFF';
      colorLight = '#000000';
    }
    gerar();
  }

  function downloadQR() {
    if (!canvasEl) return;
    const link = document.createElement('a');
    link.download = 'qrcode.png';
    link.href = canvasEl.toDataURL('image/png');
    link.click();
  }

  onMount(gerar);
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;600&display=swap" rel="stylesheet">
</svelte:head>

<main>
  <div class="card">
    <div class="left-panel">
      <div class="color-selector">
        <p>Set color</p>
        <div class="swatches">
          <button
            class="color-swatch white"
            class:active={activeColor === 'white'}
            on:click={() => handleSetColor('white')}
            aria-label="Set QR code to white"
          />
          <button
            class="color-swatch black"
            class:active={activeColor === 'black'}
            on:click={() => handleSetColor('black')}
            aria-label="Set QR code to black"
          />
        </div>
      </div>
      <canvas bind:this={canvasEl} width={size} height={size} />
      <button class="btn btn-download" on:click={downloadQR}>Download</button>
    </div>

    <div class="right-panel">
      <h1>QR Code <span>Generator</span></h1>

      <div class="form-group">
        <label for="url-input">Submit URL or text</label>
        <input id="url-input" bind:value={texto} type="text" placeholder="Enter text or URL here" />
      </div>

      <div class="form-group">
        <label for="size-input">Image size</label>
        <input id="size-input" type="text" value="{size}x{size}" readonly />
      </div>
      
      <button class="btn btn-generate" on:click={gerar}>Generate</button>
    </div>
  </div>
</main>