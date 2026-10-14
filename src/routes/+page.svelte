<script>
// @ts-nocheck

// Variables and a constant
const AlphabetArray = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z']
let UnencryptedText = $state("")
let EncryptedText  = $state("")
let UndeencryptedText = $state("")
let DeencryptedText  = $state("")
let EncryptKey = $state(0)
let DeencryptKey = $state(0)
let ErrorMessage = $state("")

//is used to "slide" numbers across the alphabet, if its goes over 26 or under 0 it loops back around
function SlideNumber(int1,int2){
    // this mod doesn't on negative numbers
    return (int1 + int2 + (26 * 999)) % 26;
}
//finds the number value of a letter by its order in the alphabet
function FindValueFromLetter(Letter) {
    // a loop is run comparing the inputed letter to each letter in the alphabet, at most takes 26 iterations
    for (let index = 0; index < AlphabetArray.length; index++) {
        if (Letter == AlphabetArray[index]){
            return index
        }
    }
}
//prapares text for use, its a function because it used to do more
function PrepareText(Text){
    return Text.toUpperCase()
}
//checks if the text can be used for cyptographic functions, returns true if yes and false if no
function Checktext (Text){
    // checks if the text is empty or is only spaces
    if (Text.trim() === ""){
        ErrorMessage = "Fill in the input!"
        return false
    }
    // preparations
    let WorkText = PrepareText(Text)
    ErrorMessage = ""
    //runs a loop going through every letter in the text (max 999)
    for (let index1 = 0; index1 < WorkText.split("",999).length; index1++) {
        console.log("index1 = " + index1)
        //resets "isValidSymbol" to false before checking if the letter is valid
        let IsValidSymbol = false;
        //runs a loop for every letter in the text comparing it with every letter in the alphabet, if its in the alphabet or is a space then its valid
        for (let index2 = 0; index2 < AlphabetArray.length; index2++) {
            if (WorkText.split("",999)[index1].toString() === AlphabetArray[index2] || WorkText.split("",999)[index1].toString() === " "){
                IsValidSymbol = true;
                console.log("index2 = " + index2)
            }
        }
        //by this point if "IsValidSymbol" is false then the letter is not valid and the function returns false
        if (IsValidSymbol == false){
            ErrorMessage = "Invalid Symbols used!"
            console.log(WorkText.split("",999)[index1].toString())
            return false
        }
    } 
    //by this point every letter was checked and is valid so, the function returns true
    return true;
}
function CheckKey(key){
    //if the the key is too small or too big or null or not an integer then the function returns false, if not then true
    if (key < 0 || key > 25 || key === null || Math.floor(key) != key){
        ErrorMessage = "Key Invalid!"
        return false
    } else {
        console.log(key)
        return true
    }
}
function EncryptText(){
    // only encrypts if both key and text are valid
    if (Checktext(UnencryptedText) && CheckKey(EncryptKey)){
        //preparations
        let WorkText = PrepareText(UnencryptedText) 
        let WorkArray = []
        //runs a loop going through every letter in the text (max 999)
        for (let index = 0; index < WorkText.split("",999).length; index++) {
            // if the letter is a space then it adds a space to the result
            if (WorkText.split("",999)[index] === " ") {
                WorkArray.push(" ")
            } else {
                // if not then it adds to the array...    
                WorkArray.push(
                    //from the "AlphabetArray" by the index of...
                    AlphabetArray[
                        //the value of "Slidenumber" thats...
                        SlideNumber(
                            //the number value of the letter plus the key.
                            FindValueFromLetter(WorkText.split("",999)[index]),
                            EncryptKey)
                        ]
                    ) 
        }}// so what it has done is that it took a letter, encrypted it with the key and got another letter which it put into the result
        //so the final result is a text encrypted by the key and that what it shows on the website
        EncryptedText = WorkArray.join().replaceAll(",","")
    }  
}
// the Decrypt function is exactly like the encrypt function exept...
function DeencryptText(){
    if (Checktext(UndeencryptedText) && CheckKey(DeencryptKey)){
    let WorkText =  PrepareText(UndeencryptedText) 
    let WorkArray = []
    for (let index = 0; index < WorkText.split("",999).length; index++) {
            if (WorkText.split("",999)[index] === " ") {
                WorkArray.push(" ")
            } else {WorkArray.push(
            AlphabetArray[
            SlideNumber(
                // its the number value MINUS the key
                FindValueFromLetter(WorkText.split("",999)[index]),
                -1 * DeencryptKey)
            ]
            ) }
    }  
    //so the result is a text encypted by the exact opposite of the key
    //and if the text was already encrypted by that key the result will be the unencrypted message
    DeencryptedText = WorkArray.join().replaceAll(",","")
}}
</script>
<p class="ErrorMessage">{ErrorMessage}</p>
<p>Cypher text:<input type="text" bind:value={UnencryptedText}> key: <input type="number" min="0" max="25" bind:value={EncryptKey}></p>
<br>
<br>
<button onclick={EncryptText}>encrypt</button>
<br>
<br>
<p>{EncryptedText}</p>
<br>
<br>
<p>text to decrypt:<input type="text" bind:value={UndeencryptedText}> key:<input type="number" min="0" max="25" bind:value={DeencryptKey}></p>
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