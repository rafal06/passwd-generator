<script lang="ts">
    let passwd     = '';
    let numOfChars = 10;

    let areUpperChars   = true;
    let areNumbers      = true;
    let areSpecialChars = true;

    const chars        = 'abcdefghijklmnopqrstuwxyz'.split('');
    const upperChars   = 'abcdefghijklmnopqrstuwxyz'.toUpperCase().split('');
    const numbers      = '0123456789'.split('')
    const specialChars = '~`!@#$%^&*()_-+={}|[]\\:";\'<>?,./'.split('');

    import ClipboardJS from 'clipboard';
    new ClipboardJS('.copy-btn');

    function genPasswd() {
        passwd = '';

        let charArr = chars;
        if (areUpperChars)   charArr = charArr.concat(upperChars);
        if (areNumbers)      charArr = charArr.concat(numbers);
        if (areSpecialChars) charArr = charArr.concat(specialChars);

        const randomValues = new Uint8Array(1);

        for (let i = 0; i < numOfChars; i++) {
            let isOk = false;
            while (!isOk) {
                self.crypto.getRandomValues(randomValues);

                if (randomValues[0] < charArr.length) {
                    passwd += charArr[randomValues[0]];
                    isOk = true;
                }
            }
        }
    }

    let copyBtnTxt = 'Copy';
    function clipboardBtnCopyTxt() {
        copyBtnTxt = 'Copied!';
        setTimeout(() => copyBtnTxt = 'Copy', 3000);
    }

    genPasswd();
</script>

<h1>Passwd generator</h1>
<div class="card">
    <pre id="passwd-box" value={passwd}>{passwd}</pre>

    <div class="buttons">
        <button on:click={clipboardBtnCopyTxt} class="copy-btn" data-clipboard-target="#passwd-box">{copyBtnTxt}</button>
        <button on:click={genPasswd}>Generate new</button>
    </div>

    <input type="number" id="charsCheckbox" min="1"  bind:value={numOfChars}        on:change={genPasswd}/>
    <label for="charsCheckbox">characters</label>
    <br>
    <input type="checkbox" id="upperCharsCheckbox"   bind:checked={areUpperChars}   on:change={genPasswd}/>
    <label for="upperCharsCheckbox">include uppercase characters</label>
    <br>
    <input type="checkbox" id="numbersCheckbox"      bind:checked={areNumbers}      on:change={genPasswd}/>
    <label for="numbersCheckbox">include numbers</label>
    <br>
    <input type="checkbox" id="specialCharsCheckbox" bind:checked={areSpecialChars} on:change={genPasswd}/>
    <label for="specialCharsCheckbox">include special characters</label>
    <br>
</div>

<style>
    .card {
        text-align: left;
        width:fit-content;
        padding: 2em;
    }

    .buttons {
        display: flex;
        margin-block: 1rem;
    }
    .buttons button {
        padding-block: 4px;
        padding-inline: 8px;
        margin-right: 1rem;
    }

    input {
        width: 40px;
        padding: 5px;
    }
</style>
