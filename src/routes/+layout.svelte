<script lang="ts">
    import '../app.css';
    import type {LayoutProps} from './$types';

    let {children}: LayoutProps = $props();
</script>

<main>
    {@render children()}
</main>

<style>
    main {
        max-width: 1920px;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0 auto;
        padding: 0.5em;
        height: 100%;
        overflow: hidden;
    }
</style>
