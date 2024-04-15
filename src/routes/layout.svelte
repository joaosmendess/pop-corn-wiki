<!-- src/routes/__layout.svelte -->
<script>
    // Seu código de script aqui, se necessário
</script>

<style global>
    html, body {
        margin: 0;
        padding: 0;
       
        color: #fff; /* Texto branco, se necessário */
        font-family: 'Poppins', sans-serif;
        height: 100%; /* Garante que o fundo cubra toda a altura */
    }
</style>

<slot></slot> <!-- Renderiza o conteúdo das páginas filho -->
