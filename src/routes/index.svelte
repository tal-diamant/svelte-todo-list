<script>
	import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';
	import { flip } from 'svelte/animate';
    import Icons from '../components/Icons.svelte';

	const [send, receive] = crossfade({
		duration: d => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

	let uid = 1;

	let todos = [
		{ id: uid++, done: false, description: 'write some docs' },
		{ id: uid++, done: false, description: 'start writing blog post' },
		{ id: uid++, done: true,  description: 'buy some milk' },
		{ id: uid++, done: false, description: 'mow the lawn' },
		{ id: uid++, done: false, description: 'feed the turtle' },
		{ id: uid++, done: false, description: 'fix some bugs' },
	];

	function add(input) {
		const todo = {
			id: uid++,
			done: false,
			description: input.value
		};

		todos = [todo, ...todos];
		input.value = '';
	}

	function remove(todo) {
		todos = todos.filter(t => t !== todo);
	}

	function mark(todo, done) {
		todo.done = done;
		remove(todo);
		todos = todos.concat(todo);
	}

    const date = new Date()
</script>

<main class="input-div">
    <input
        class="text-input"
        placeholder="what needs to be done?"
        on:keydown={e => e.key === 'Enter' && add(e.target)}
    >

    <div class='board'>
        
        <div class='left label-box'>
            <h2>todo</h2>
            {#each todos.filter(t => !t.done) as todo (todo.id)}
                <label
                    in:receive="{{key: todo.id}}"
                    out:send="{{key: todo.id}}"
                    animate:flip
                >
                    <input type=checkbox on:change={() => mark(todo, true)}>
                    {todo.description}
                    <button on:click="{() => remove(todo)}"><Icons icon="trashbin"/></button>
                </label>
            {/each}
        </div>
    
        <div class='right label-box'>
            <h2>done</h2>
            {#each todos.filter(t => t.done) as todo (todo.id)}
                <label
                    class="done"
                    in:receive="{{key: todo.id}}"
                    out:send="{{key: todo.id}}"
                    animate:flip
                >
                    <input type=checkbox checked on:change={() => mark(todo, false)}>
                    {todo.description}
                    <button on:click="{() => remove(todo)}"><Icons icon="trashbin"/></button>
                    
                </label>
            {/each}
        </div>
    </div>
</main>

<div class="footer">
    <a href="https://icons8.com/icon/85081/trash">Trash icon by Icons8</a>
</div>

<style>
    * {
        box-sizing: border-box;
    }

    main {
        text-align: center;
        max-width: 36rem;
		margin: 70px auto;
    }

	.text-input {
        width: 100%;
        font-size: 1.4em;
        margin-bottom: 10px;
        padding: 5px 5px;
    }

    .board {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-gap: 1rem;
        min-height: 72vh;
        /* background: red; */
	}

    .label-box {
        text-align: center;
    }

	h2 {
		font-size: 2em;
		font-weight: 200;
		user-select: none;
		margin: 0 0 0.5em 0;
	}

	label {
		position: relative;
        display: inline-block;
		line-height: 1.2;
		padding: 0.5em 2.5em 0.5em 2em;
		margin: 0 0 0.5em 0;
        width: 250px;
		border-radius: 2px;
		user-select: none;
		border: 1px solid hsl(240, 8%, 70%);
		background-color:hsl(240, 8%, 93%);
		color: #333;
	}

	input[type="checkbox"] {
		position: absolute;
		left: 0.5em;
		top: 0.6em;
		margin: 0;
	}

	.done {
		border: 1px solid hsl(240, 8%, 90%);
		background-color:hsl(240, 8%, 98%);
	}

	button {
		position: absolute;
		top: 0;
		right: 0;
		height: 100%;
		border: none;
        border-top-right-radius: 2px;
        border-bottom-right-radius: 2px;
		opacity: 0;
		transition: opacity 0.2s;
		cursor: pointer;
	}

	label:hover button {
		opacity: 1;
	}

    .footer {
        margin-top: auto;
        text-align: center;
    }

    @media only screen and (max-width: 768px) {
        main {
            margin-top: 20px;
        }

        .board {
		    display: flex;
            flex-direction: column;
            min-height: 76vh;
	    }

        button {
            opacity: 1;
        }
    }
</style>