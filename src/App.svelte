<script>
  import {onMount}  from 'svelte';
  let pages=[];
  let currentPageIndex = 0;
 let title = "thursay";
 let note = "today is an excellent day!!"

 onMount(() =>{
  const savedPages = localStorage.getItem("pages");
  if(savedPages){
    pages = JSON.parse(savedPages);
    title = pages[currentPageIndex];
    note = localStorage.getItem(title);
  } else{
    addPage();
  }
  // title = localStorage.getItem('title');
  // note = localStorage.getItem('note');
 });

 function saveNote(){
  console.log(title,note);
  const storedPageName = pages[currentPageIndex];

  if(storedPageName != title){
    localStorage.removeItem(storedPageName);
    pages[currentPageIndex] = title;
  }
  localStorage.setItem(title, note);
  localStorage.setItem("pages",JSON.stringify(pages));
 }

 function addPage()
 {
  pages.push("New Page");
  selectPage(pages.length ? pages.length-1 : 0);
 }

 function selectPage(index){
  currentPageIndex = index;
  title = pages[currentPageIndex];
  note = localStorage.getItem(title);
 }

</script>
<aside class="fixed top-0 left-0 z-40 w-60 h-screen ">
  <div class="bg-light-gray overflow-y-auto py-5 px-3 h-full border-r border-gray-200">
    <ul class="space-y-2">
      {#each pages as page, index}
      <li>
         <button on:click={()=> selectPage(index)} class="{index == currentPageIndex ? 'bg-dark-gray' : ''} px-2 py-3 text-gray-900 rounded-lg " >{page}</button>
      </li>
      {/each}
      <li class="text-center"><button on:click={addPage} class="font-medium">+ Add Page </button></li>
    </ul>
  </div>
</aside>
<main class="p-4 ml-60 h-auto ">
  <div class="grid grid-cols-2 items-center mb-3">
    <h1 class="text-3xl font-bold" contenteditable bind:textContent={title}></h1>
    <button class="ml-auto bg-gray-700 text-white px-5 py-2.5 rounded-lg font-medium text-sm hover:bg-gray-900 " on:click={saveNote}>Save</button>
  </div>
  <hr>
  <!-- <input class=" mb-5 block w-full bg-gray-50 border border-gray-300 rounded-lg text-grey-900 p-2.5" bind:value={title}  type="text" > -->
  <textarea class=" mt-5 block w-full bg-gray-50 border border-gray-300 rounded-lg text-grey-900 p-2.5" bind:value={note}></textarea>
</main>

<style>
.bg-light-gray{
  background: #fbfbfb;
}

.bg-dark-gray {
  background: #efefef;
}
</style>