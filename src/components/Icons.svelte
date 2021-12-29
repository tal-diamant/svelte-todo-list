<script>
    export let icon;

    const icons = {
        trashbin: "https://img.icons8.com/material-outlined/24/000000/trash--v1.png",
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