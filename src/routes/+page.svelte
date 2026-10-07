<script>
// @ts-nocheck


//** this project is still unfinished


const AlphabetArray = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z']
let UnencryptedText = $state("")
let EncryptedText  = $state("")
let UndeencryptedText = $state("")
let DeencryptedText  = $state("")
let EncryptKey = $state(0)
let DeencryptKey = $state(0)
let ErrorMessage = $state("")
function SlideNumber(int1,int2){
    //** this mod doesn't on negative numbers
    return (int1 + int2 + (26 * 999)) % 26;
}
function FindValueFromLetter(Letter) {
    for (let index = 0; index < AlphabetArray.length; index++) {
        if (Letter == AlphabetArray[index]){
            return index
        }
    }
}
function PrepareText(Text){
    return Text.toUpperCase()
}
function Checktext (Text){
    let WorkText = PrepareText(Text)
    ErrorMessage = ""
    for (let index1 = 0; index1 < WorkText.split("",999).length; index1++) {
        console.log("index1 = " + index1)
        let IsValidSymbol = false;
        for (let index2 = 0; index2 < AlphabetArray.length; index2++) {
            if (WorkText.split("",999)[index1].toString() === AlphabetArray[index2] || WorkText.split("",999)[index1].toString() === " "){
                IsValidSymbol = true;
                console.log("index2 = " + index2)
            }
        }
        if (IsValidSymbol == false){
            ErrorMessage = "Invalid Symbols used!"
            console.log(WorkText.split("",999)[index1].toString())
            return false
        }
    } 
    
    return true;
}

function EncryptText(){
    if (Checktext(UnencryptedText)){
        let WorkText = PrepareText(UnencryptedText) 
        let WorkArray = []
        for (let index = 0; index < WorkText.split("",999).length; index++) {
            if (WorkText.split("",999)[index] === " ") {
                WorkArray.push(" ")
            } else {
            WorkArray.push(
                AlphabetArray[
                SlideNumber(
                    FindValueFromLetter(WorkText.split("",999)[index]),
                    EncryptKey)
                ]
                ) 
        }}EncryptedText = WorkArray.join().replaceAll(",","")
    }  
}
function DeencryptText(){
    if (Checktext(UnencryptedText)){
    let WorkText =  PrepareText(UndeencryptedText) 
    let WorkArray = []
    for (let index = 0; index < WorkText.split("",999).length; index++) {
            if (WorkText.split("",999)[index] === " ") {
                WorkArray.push(" ")
            } else {WorkArray.push(
            AlphabetArray[
            SlideNumber(
                FindValueFromLetter(WorkText.split("",999)[index]),
                -1 * DeencryptKey)
            ]
            ) }
    }  DeencryptedText = WorkArray.join().replaceAll(",","")
}}
</script>
<p class="ErrorMessage">{ErrorMessage}</p>
<p>Cypher text:<input type="text" bind:value={UnencryptedText}> key: <input type="number" bind:value={EncryptKey}></p>
<br>
<br>
<button onclick={EncryptText}>encrypt</button>
<br>
<br>
<p>{EncryptedText}</p>
<br>
<br>
<p>text ro decrypt:<input type="text" bind:value={UndeencryptedText}> key:<input type="number" bind:value={DeencryptKey}></p>
<br>
<br>
<button onclick={DeencryptText}>decrypt</button>
<br>
<br>
<p>{DeencryptedText}</p>
<style>
    .ErrorMessage{
        color: red;
    }
</style>