<script>
    import { createMachine, assign } from "xstate";
    import { useMachine } from "../../hooks/useMachine";

    let name, something, phone, someone, home, email;
    const toggleMachine = createMachine({
        id: "toggle",
        initial: "stepOne",
        context: {
            name: "",
            something: ""
        },
        states: {
            stepOne: {
                on: { TOGGLE: "stepTwo" }
            },
            stepTwo: {
                on: { TOGGLE: "stepThree" },
                entry: assign((context, event) => {
                    console.log(context, event)
                    window.location.hash = '#algo'
                    const {name, something} = event
                    return ({ name, something })
                }),
            },
            stepThree: {
                on:  { TOGGLE: "finish" },
                entry: assign((_, event)=> {
                    window.location.hash = '#cosa'
                    const {phone, someone} = event
                    return ({ phone, someone })                    
                })
            },
            finish: {
                type: 'finish',
                entry: assign((_, event)=> {
                    window.location.hash = '#luz'
                    const {home, email} = event
                    return ({ home, email })                    
                })
            }
        }
    });
    const { state, send } = useMachine(toggleMachine);

    const handleSubmitOne = () => {
        send("TOGGLE", {name, something})
        console.log(name, something)
    } 

    const handleSubmitTwo = () => {
        send("TOGGLE", {phone, someone})
        console.log(phone, someone)
    } 

    const handleSubmitThree = () => {
        send("TOGGLE", {home, email})
        console.log(home, email)
    } 


    $: stepOne = $state.matches("stepOne");
    $: stepTwo = $state.matches("stepTwo");
    $: stepThree = $state.matches("stepThree");
    $: finish = $state.matches("finish");
</script>


<svelte:head>
    {#if stepOne}
	    <title>One</title>
    {/if}
    {#if stepTwo}
        <title>stepTwo</title>
    {/if}
    {#if stepThree}
        <title>stepThree</title>
    {/if}
    {#if finish}
        <title>finish</title>
    {/if}
</svelte:head>

{#if stepOne}
    <h1>One Step </h1>
    <form on:submit|preventDefault={handleSubmitOne}>
        <label for="name">
            Nombre
            <input id="name" type="text" bind:value={name} required>
        </label>
        <label for="something">
            Algo
            <input id="something" type="text" bind:value={something} required>
        </label>
        <button>Enviar</button>
    </form>
{/if}

{#if stepTwo}
    <h1>stepTwo</h1>
    <form on:submit|preventDefault={handleSubmitTwo}>
        <label for="phone">
            Telefono
            <input id="phone" type="text" bind:value={phone} required >
        </label>
        <label for="someone">
            Cosa
            <input type="someone" bind:value={someone} required>
        </label>
        <button>Enviar</button>
    </form>
{/if}

{#if stepThree}
    <h1>stepThree</h1>
    <form on:submit|preventDefault={handleSubmitThree}>
        <label for="email">
            Email
            <input id="email" type="text" bind:value={email} required>
        </label>
        <label for="home">
            Home
            <input id="home"  type="text" bind:value={home} required>
        </label>
        <button>Enviar</button>
    </form>
{/if}

{#if finish}
    <h1>finish</h1>
{/if}   