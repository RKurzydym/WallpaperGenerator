<script setup lang="ts">

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';
import './components/fonts/fontawesome6/css/all.min.css';
import './components/fonts/fontawesome6/css/brands.css';



import {onMounted, type Ref, ref} from "vue";

  const show = ref(false)
  const Wallpaper: Ref<HTMLCanvasElement | undefined> = ref();
  const context: Ref<CanvasRenderingContext2D | undefined> = ref();
  const color: Ref<string | CanvasGradient | CanvasPattern | undefined> = ref();

  onMounted(() => {
    // Get canvas context. If 'getContext' returns 'null', set to 'undefined', so that it conforms to the Ref typing
    context.value = Wallpaper.value?.getContext('2d') || undefined;
    
    render();
  });

  function render() {
    if (!context.value) {
        return;
    }
    drawBG();
    drawSymbols(6,10);
    
  }
  function drawBG()
  {
    if (!context.value){
    throw new Error("Viewport not found")
    } 
    if (!Wallpaper.value){
    throw new Error("Viewport not found")
    } 
    context.value.fillStyle = "#2e2e2e";
    context.value.fillRect(0, 0, Wallpaper.value.width, Wallpaper.value.height);
    
  }

  function getRandomInt(max:number) {
    return Math.floor(Math.random() * max) + 20;
  }


  function drawSymbols(sides:number, offset:number)
  {
    const radius:number = 40
    if (!Wallpaper.value)
    {
      throw new Error("Wallpaper is not there while drawing Symbols")
    }

    switch (sides)
    {
      case 6: //Hexagons to have a nice Structure
      {
        let i:number = 0
        for (var x = radius; x < Wallpaper.value.width; x += radius*2)
        {
          if(i%2 == 1)
          {
            x -= radius/4 
          }
          for (var y = radius; y < Wallpaper.value.height; y += radius*2)
          {
            if (Math.random() < 0.25) // What chance dose a Tile have to be drawn 
            {
              if(i%2 == 1)
              {
                
                n_Ek(sides,radius,x,y+radius)
              }
              else
              {
                n_Ek(sides,radius,x,y)
              }
            }
            y += offset // adds an Offset to the N_Ecks is only when one want more space between the Symbols 
          }
          if(i%2 == 1)
          {
            x -= radius/4 
          }
          x += offset // adds an Offset to the N_Ecks is only when one want more space between the Symbols 
          
          i++
        }
        break;
      }
      /*case 2: // To have a empty Case Statement
      {
        break;
      } */     
      default: // Is the Structure of the Hexagons, Why? Because its looks nice in the differents things
      {
        let i:number = 0
        for (var x = radius; x < Wallpaper.value.width; x += radius*2)
        {
          if(i%2 == 1)
          {
            x -= radius/4 
          }
          for (var y = radius; y < Wallpaper.value.height; y += radius*2)
          {
            if (Math.random() < 2) // What chance dose a Tile have to be drawn 
            {
              if(i%2 == 1)
              {
                
                n_Ek(sides,radius,x,y+radius)
              }
              else
              {
                n_Ek(sides,radius,x,y)
              }
            }
          }
          if(i%2 == 1)
          {
            x -= radius/4 
          }
          
          i++
        }
      }

    }
    
    
    /*
    for (var i = 0; i < saveAllLocationsofSymbols.length; i++) {
      console.log(saveAllLocationsofSymbols[i]);
    }*/
    
  }

  function colorPallet()
  {
    let randomNumber = Math.random();
    if (randomNumber < 0.1)
    {
      return [232,125,62,1]
    }
    else if (randomNumber < 0.2)
    {  
      return [214,214,214,1]  
    }
    else if (randomNumber < 0.3)
    {  
      return [229,229,229,1]      
    }
    else if (randomNumber < 0.4)
    {  
      return [180,210,115,1]    
    }
    else if (randomNumber < 0.5)
    {  
      return [158,134,200,1]    
    }else if (randomNumber < 0.6)
    {  
      return [176,82,121,1]    
    }else if (randomNumber < 0.7)
    {  
      return [121,121,121,1]     
    }else
    {  
      return [108,153,187,1]    
    }
  }
  function symbolPallet()
  {
    let randomNumber = Math.random();
    if (randomNumber < 0.1)
    {
      return "\uf121"// Code
    }
    else if (randomNumber < 0.2)
    {  
      return "\uf3e2"// Python   
    }
    else if (randomNumber < 0.3)
    {  
      return "\uf41f" //vue
    }
    else if (randomNumber < 0.4)
    {  
      return "\uf109" // Laptop
    }
    else if (randomNumber < 0.5)
    {  
      return "\uf577" // Fingeprint

    }else if (randomNumber < 0.6)
    {  
      return  "\uf120" // Terminal
    }else if (randomNumber < 0.7)
    {  
      return "\uf1b2"  //Cube  
    }else if (randomNumber < 0.8)
    {  
      return "\uf11b"  //Gamepad  
    }else if (randomNumber < 0.9)
    {  
      return "\uf1c0"  //Database  
    }else
    {  
      return "\uf2db"  //Microchip
    }

  }

  function n_Ek(amount: number,radius:number, beginX: number, beginY: number)
  {
    if (!context.value){
    throw new Error("Viewport not found")
    } 
    const angleStep = (2 * Math.PI) / amount;
    
    context.value.beginPath();
    context.value.moveTo(beginX + radius * Math.cos(0), beginY + radius * Math.sin(0));
    for (let i = 1; i < amount; i++) {
        const angle = angleStep * i;
        context.value.lineTo(beginX + radius * Math.cos(angle), beginY + radius * Math.sin(angle));
    }
    context.value.closePath();

    const [Red,Green,Blue] = colorPallet();

    var randomAlpha = Math.random();
    if(randomAlpha < 0.6)
    {
      if(Math.random() < 0.4)
       {
        randomAlpha += 0.4
       }
    }
    context.value.fillStyle = `rgba(${Red},${Green},${Blue},${randomAlpha})`; // Farbe für das Polygon
    context.value.strokeStyle = `rgba(${Red},${Green},${Blue},${randomAlpha})`;

    context.value.fill();

    //console.log(beginY+radius/2);
    
    context.value.stroke();
    context.value.fillStyle = "#2e2e2e"; 
    context.value.font = "40.0px FontAwesome";
    const symbol = symbolPallet();
    if(symbol == "\uf3e2") // To Move the Python Icon 
    {
      context.value.font = "48px FontAwesomeBrands";
      context.value.fillText(symbol,beginX-radius/1.9,beginY+radius/2.2)
    }else if( symbol == "\uf41f") // To Move the Vue Icon 
    { 
      context.value.font = "48px FontAwesomeBrands";
      context.value.fillText(symbol,beginX-radius/1.9,beginY+radius/2)
    }else if(symbol == "\uf1b2" || symbol == "\uf2db" || symbol == "\uf577"  ) // To Move the Cube, Microship and Figerprint Icons 
    {
      context.value.fillText(symbol,beginX-radius/2,beginY+radius/2.7)
    }else if( symbol == "\uf1c0") // To Move the Database Icon 
    {
      context.value.fillText(symbol,beginX-radius/2.3,beginY+radius/2.7)
    }
    else if( symbol == "\uf120")// To Move the Terminal Icon 
    {
      context.value.fillText(symbol,beginX-radius/1.8,beginY+radius/2.7)
    }
    else// To Move Evrything else that do not need a Special set up
    {
      context.value.fillText(symbol,beginX-radius/1.6,beginY+radius/2.7)
    }
  }
  function downloadCanvasAsPNG()
  {
    const canvas = Wallpaper;
      
      // Das Canvas als PNG-URL konvertieren
      const dataURL:any = canvas.value?.toDataURL('image/png');

      // Den Download-Link vorbereiten
      const link = document.createElement('a');
        link.href = dataURL;
        link.download = 'canvas_image.png';
        link.click();
  }
</script>

<template>
  <link ref="stylesheet" href="./components/fonts/fontawesome6/css/all.css">
	<div>
  <canvas id="clip" ref="Wallpaper" width="8000" height="8000" >
    <button @click="show = !show">Toggle Slide + Fade </button>
    <Transition name="slide-fade">
      <div v-if="show"> 
        
      </div>
      
    </Transition>
  </canvas>
  <div id="ControllBar">
    <p >
      Wallpaper Generator
    </p>
    <hr style="color:yellow;background-color:gray;width: 100%;">
    <p id="text">
      The image, on the left, is a Preview of the image. <FontAwesomeIcon :icon="faArrowRight" />
    </p>
    <button @click="downloadCanvasAsPNG" style="font-family: Geist_Mono;">Download PNG
    </button>
  
  </div>
    <a ref="downloadLink" style="display: none"></a>
  </div>  
</template>




<style scoped>
header {
  line-height: 1.5;
}
@font-face {
  font-family: FontAwesome;
  src: url('./components/fonts/fontawesome6/webfonts/fa-solid-900.ttf');
}
@font-face {
  font-family: FontAwesomeBrands;
  src: url('./components/fonts/fontawesome6/webfonts/fa-brands-400.ttf');
}
@font-face {
  font-family: Geist_Mono;
  src: url('./components/fonts/Geist_Mono/GeistMono-VariableFont_wght.ttf');
}

canvas
{
  overflow:hidden;
  z-index: -2;
  font-family: FontAwesome;
}
#text {
  font-size: large;
  text-align:center;
}

#ControllBar {
  width:220px;
  height: 280px;
  background-color: #1E1F1C;
  border-radius: 0px 0px 50px 0px;
  margin:auto;
  text-align:center;
  font-size: x-large;
  font-family: Geist_Mono;
  display: flex;
  align-content: flex-start;
  justify-content: center;
  flex-wrap: wrap;
}


.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active { 
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}

p {
  margin-top: 0.5em;
  margin-bottom: 0.5em;
}


</style>
