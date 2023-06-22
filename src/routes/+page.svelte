<svelte:head>
	<title>Home page</title>
</svelte:head>

<script>
    import "../global.css"

    // Parameters for the password
    $: popinActive = false;
    $: password = generatePassword(parameters);
    const letters = "abcdefghijklmnopqrstuvwxyz";
    const numbers = "0123456789";
    const symbols = "!@#$%^&*_-+=";
    const parameters = {
        lowercase: true,
        uppercase: true,
        numbers: true,
        symbols: true,
        length: 16
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

    const copyPassword = () => {
        navigator.clipboard.writeText(password);

        if(popinActive) return
        popinActive = true;
        setTimeout(() => { popinActive = false }, 2000);
    }
</script>

<main class="flex">
    {#if !parameters.lowercase && !parameters.uppercase && !parameters.numbers && !parameters.symbols}
        <div class="info-card">
            You have to select at least one parameter
        </div>
    {/if}

    {#if popinActive}
        <div class="info-card">
            Password copied to clipboard
        </div>
    {/if}

    <section id="main-section">
        <h1>
            When all you need is a <span class="accent">robust & shiny</span> new password
        </h1>

        <div>
            <input type="text" class="password-info" placeholder="Hey! That's an Easter egg 🥚" value={password} disabled/>
        </div>

        {#if password.length}
            <section class="button-section">
                <button class="button-section-icon" on:click={copyPassword}>
                    <img src="/src/assets/copy-icon.png" alt="Copy the Password">
                </button>

                <button class="button-section-icon" on:click={() => password = generatePassword(parameters)}>
                    <img src="/src/assets/refresh-icon.png" alt="Refresh Password">
                </button>
            </section>
        {/if}
    </section>

    <section id="configuration-section">
        <div class="options flex">
            {#each Object.keys(parameters) as parameter}
                <div class="options_field flex">
                    {#if parameter === "length"}
                        <input type="number" class="parameter-info" placeholder="8 - 128" min="8" max="128" bind:value={parameters[parameter]} />
                        <input type="range" name="{parameter}" id="{parameter}" min="8" max="128" bind:value={parameters[parameter]} on:change={parameters.length} />
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
    @mixin flex($justify: center, $align: center) {
        display: flex;
        justify-content: $justify;
        align-items: $align;
    }

    @mixin full-size($width: 100%, $height: 100%) {
        width: $width;
        height: $height;
    }

    @mixin gradient() {
        background: linear-gradient(to right, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1);
        background-size: 200%;
        animation: moveGradient 4s ease infinite;
    }

    main {
        @include flex();
        @include full-size();
        text-align: center;
        flex-direction: column;
        background: var(--black);
        font-family: 'Nunito', sans-serif;

        .info-card {
            @include flex();
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 5vh;
            padding: 1rem;
            font-size: 1.25rem;
            font-weight: bold;
            color: var(--text-colo);
            background: var(--white);
        }

        #main-section {
            @include flex(top, center);
            margin-top: 5vh;
            flex-direction: column;
            flex: 1;
            @media screen and (min-width: 768px) {
                @include flex();
            }

            h1 {
                font-size: clamp(var(--fs-h2), 5vw, var(--fs-h1));
                width: 90vw;
                margin-top: 2rem;
                color: var(--white);
                @media screen and (min-width: 768px) {
                    margin-top: 5rem;
                }

                .accent {
                    @include gradient();
                    -webkit-background-clip: text;
                    background-clip: text;
                    -webkit-text-fill-color: transparent;
                    white-space: nowrap;
                }
            }

            input[type="text"] {
                font-size: clamp(var(--fs-h3), 5vw, var(--fs-h2));
                width: 90vw;
                max-width: 1200px;
                margin: 1rem;
                padding: 1rem;
                outline: none;
                border-radius: 1rem;
                border: 0.5rem solid transparent;
                background:
                    linear-gradient(var(--white), var(--white)) padding-box,
                    linear-gradient(to left top, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1) border-box;
                background-size: 200%;
                animation: moveGradient 5s ease infinite;
                @media screen and (min-width: 768px) {
                    min-width: fit-content;
                    width: 75vw;
                }
            }

            .button-section {
                @include flex();
                margin: 1rem;
                &-icon {
                    background: linear-gradient(to right top, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1);
                    background-size: 200%;
                    animation: moveGradient 14s ease infinite;
                    border-radius: 100%;
                    padding: 1.5rem;
                    margin: 0 2rem;
                    border: none;
                    &:nth-child(2) {
                        background: linear-gradient(to left top, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1);
                        background-size: 200%;
                        animation: moveGradient 6s ease infinite;
                    }
                    img {
                        width: 2.5rem;
                        cursor: pointer;
                    }
                }
            }
        }

        .options {
            background: var(--white);
            width: 90vw;
            margin: 1rem 0;
            max-width: 1200px;
            text-transform: capitalize;
            user-select: none;
            flex-wrap: wrap;
            border-radius: 1rem;
            background:
                linear-gradient(var(--white), var(--white)) padding-box,
                linear-gradient(to top right, #d16ba5, #c777b9, #ba83ca, #aa8fd8, #9a9ae1, #8aa7ec, #79b3f4, #69bff8, #52cffe, #41dfff, #46eefa, #5ffbf1) border-box;
            background-size: 200%;
            animation: moveGradient 9s ease infinite;
            border: 0.5rem solid transparent;

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

                input {
                    &[type='number'] {
                        width: 15vw;
                        border: 2px solid var(--black);
                        padding: 0.5rem 1rem;
                        border-radius: 8px;
                        text-align: center;
                        font-weight: bold;
                        outline: none;
                        font-family: 'Nunito', sans-serif;
                        @media screen and (min-width: 768px) {
                            width: 5vw;
                            padding: 0.2rem 0.5rem;
                        }
                    }

                    &[type='range'] {
                        width: 100%;
                        margin-top: 0.5rem;
                    }

                    &[type='checkbox'] {
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
        }
    }

    @keyframes moveGradient {
        0% {
            background-position: 0% 50%;
        }
        50% {
            background-position: 100% 0%;
        }
        100% {
            background-position: 0% 50%;
        }
    }

    @keyframes initPopin {
        0% {
            transform: translateY(-1000%);
        }
        50% {
            transform: translateY(0);
        }
        100% {
            transform: translateY(-100%);
        }
    }
</style>