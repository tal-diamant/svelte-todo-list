<script>
    export let icon;

    const icons = {
        trashbin: "trash.png",
        test: "/favicon.png",
    }
</script>

<span><img src="{icons[icon]}" alt="trash bin icon"/></span>

<style>
    span {
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>