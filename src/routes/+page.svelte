<script>
    import "../app.css";

    let count = $state(0);
</script>
<div class="flex justify-center items-center h-screen bg-slate-900">
    <h1 class="m-4 bg-slate-800 px-16 py-4 rounded-sm shadow-lg text-2xl text-white">Hello World!</h1>
    <button onclick={count++} class="m-4 bg-slate-800 px-16 py-4 rounded-sm shadow-lg text-2xl text-white">{count}</button>
    
</div>
