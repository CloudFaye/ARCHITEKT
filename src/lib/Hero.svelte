<script lang="ts">
  import { onMount } from "svelte";
import Background from "./Background.svelte";
  onMount(() => {
    console.log("Hero component mounted");
  });
</script>

<div class="w-full h-screen">
   <Background src="src/assets/texture2.png">
    <div class="w-[200px] p-6 h-full bg-white/10 ">
        <h1 class="text-white text-2xl text-nowrap font-bold">Website builder</h1>
        <p class="text-white/80 text-sm text-nowrap">Build your website with ease</p>
        
        <div class="bg-white/20 w-full h-fit rounded-sm p-3 my-4">
        <p class="text-white text-sm text-balance">An opinionated website builder for the modern web</p>
        </div>
        <div class="bg-white/20 w-full h-fit rounded-sm p-3 my-4">
        <p class="text-white text-sm text-balance">What classes ? just use tailwind</p>
        </div>
        <div class="bg-white/20 w-full h-fit rounded-sm p-3 my-4">
        <p class="text-white text-sm text-balance">Shallow routing ? We got you covered</p>
        </div>
    </div>
   </Background>
  
</div>
