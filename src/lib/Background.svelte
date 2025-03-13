<script>

    let { children, src } = $props();
   
</script>




<div style="background-image: url({src})" class=" w-full p-10 relative  object-contain  h-screen">
    <div class="bg-black/90 absolute   top-0 left-0 w-full h-full ">
    {@render children()}</div>
   
</div>