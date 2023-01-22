<svelte:head>
	<title>Home page</title>
</svelte:head>

<script>
    // Import global styles here
    import "../global.css"

    $: password = generatePassword(parameters);
    const letters = "abcdefghijklmnopqrstuvwxyz";
    const numbers = "0123456789";
    const symbols = "!@#$%^&*_-+=";
    const parameters = {
        lowercase: true,
        uppercase: true,
        numbers: true,
        symbols: true,
        length: 8
    }

    const updatelength = () => {
        return parameters.length;
    }

    const generatePassword = (parameters) => {
        let password = "";
        const activeParameters = Object.keys(parameters).filter(parameter => parameters[parameter] === true);

        for(let i = 0; i < parameters.length; i++) {
            const randomizer = Math.floor(Math.random() * activeParameters.length);
            switch(activeParameters[randomizer]) {
                case "lowercase":
                    password += letters[Math.floor(Math.random() * letters.length)];
                    break;
                case "uppercase":
                    password += letters[Math.floor(Math.random() * letters.length)].toUpperCase();
                    break;
                case "numbers":
                    password += numbers[Math.floor(Math.random() * numbers.length)];
                    break;
                case "symbols":
                    password += symbols[Math.floor(Math.random() * symbols.length)];
                    break;
                default:
                    break;
            }
        }
        return password;
    }
</script>

<main class="flex">
    <input type="text" class="bold" value={password} disabled/>
    {#if !parameters.lowercase && !parameters.uppercase && !parameters.numbers && !parameters.symbols}
        <small>You have to select at least one parameter</small>
    {/if}

    <section class="configuration">
        <div class="options flex">
            {#each Object.keys(parameters) as parameter}
                <div class="options_field flex">
                    {#if parameter === "length"}
                        <p class="parameter-info">{parameters[parameter]}</p>
                        <input type="range" name="{parameter}" id="{parameter}" min="8" max="128" bind:value={parameters[parameter]} on:change={updatelength} />
                    {/if}

                    {#if parameter === "lowercase" || parameter === "uppercase" || parameter === "numbers" || parameter === "symbols"}
                        <input type="checkbox" name="{parameter}" id="{parameter}" bind:checked={parameters[parameter]} />
                        <label for="{parameter}">{parameter}</label>
                    {/if}
                </div>
            {/each}
        </div>
    </section>
</main>

<style lang="scss">
    main {
        height: 100%;
        width: 100%;
        flex-direction: column;
        text-align: center;
        background: var(--secondary-color);
        font-family: 'Nunito', sans-serif;

        input[type='text'] {
            font-size: clamp(var(--fs-h3), 5vw, var(--fs-h2));
            margin: 1rem;
            width: 100vw;
            border: 0.5rem solid var(--primary-color);
            padding: 1rem;
            border-radius: 1rem;
            outline: none;
            border: 0.5rem solid #ccc;
            @media screen and (min-width: 768px) {
                min-width: fit-content;
                width: 75vw;
            }
        }

        .options {
            position: fixed;
            bottom: 0px;
            transform: translate(-50%, 0);
            background: #fff;
            width: 100vw;
            text-transform: capitalize;
            user-select: none;
            flex-wrap: wrap;
            border: 0.5rem solid #ccc;

            @media screen and (min-width: 768px) {
                bottom: 50px;
                padding: 0 2rem;
                min-width: fit-content;
                width: 75vw;
                border-radius: 75px;
            }

            .options_field {
                flex-direction: column;
                flex-grow: 1;
                margin: 1rem;
                font-weight: bold;

                input[type='checkbox'] {
                    display: none;

                        + label {
                        position: relative;
                        height: 100%;
                        width: 100%;
                        padding: 1rem 1rem 2rem;
                        cursor: pointer;
                        transition: transform 0.2s ease-in-out;

                        &:before {
                            content: "✖";
                            color: red;
                            position: absolute;
                            left: 50%;
                            top: 75%;
                            font-size: 1.25rem;
                            transform: translate(-50%, -50%);
                            font-weight: bold;
                        }

                    }

                    &:checked + label {
                        &:before {
                            content: "✔";
                            color: green;
                        }
                    }
                }
            }

        }
        small {
            color: #fff;
            font-size: 2rem;
            margin: 0.5rem;
            text-transform: initial;
        }
    }
</style>