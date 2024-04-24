<template>
    <div class="calculator">
        <VTable>
            <thead>
                <tr>
                    <th colspan="4">
                        {{ result }}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>
                        <VBtn>C</VBtn>
                    </td>
                    <td>
                        <VBtn>*</VBtn>
                    </td>
                    <td>
                        <VBtn>/</VBtn>
                    </td>
                    <td>
                        <VBtn>-</VBtn>
                    </td>
                </tr>
                <tr>
                    <td>
                        <VBtn>7</VBtn>
                    </td>
                    <td>
                        <VBtn>8</VBtn>
                    </td>
                    <td>
                        <VBtn>9</VBtn>
                    </td>
                    <td>
                        <VBtn>+</VBtn>
                    </td>
                </tr>
                <tr>
                    <td>
                        <VBtn>4</VBtn>
                    </td>
                    <td>
                        <VBtn>5</VBtn>
                    </td>
                    <td>
                        <VBtn>6</VBtn>
                    </td>
                    <td>
                        <VBtn>%</VBtn>
                    </td>
                </tr>
                <tr>
                    <td>
                        <VBtn>1</VBtn>
                    </td>
                    <td>
                        <VBtn>2</VBtn>
                    </td>
                    <td>
                        <VBtn>3</VBtn>
                    </td>
                    <td>
                        <VBtn>=</VBtn>
                    </td>
                </tr>
                <tr>
                    <td colspan="2">
                        <VBtn style="width: 100%;">0</VBtn>
                    </td>
                    <td>
                        <VBtn>.</VBtn>
                    </td>
                </tr>
            </tbody>
        </VTable>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const input = ref<String>("-1")
const operation = ref<string>("")
const result = ref<String>("NaN")

document.addEventListener("click", event => {
    const tagName = event.target?.tagName
    
    if((tagName === "SPAN") || (tagName === "BUTTON")){
        input.value = event.target?.innerText

        var isNumber = Number(input.value)
        
        if((isNumber >= 0) && (isNumber <= 9)){

            if((result.value === "NaN") || (result.value === "Infinity")){
                result.value = input.value
                operation.value = ""
            }

            else if(result.value.includes(".")){
                result.value += String(input.value)
            }

            else{

                if(operation.value === ""){
                    if(result.value.includes("+")){
                        result.value = String(Number(result.value) + Number(input.value))
                        operation.value = ""
                    }
                    else if(result.value.includes("-")){
                        result.value = String(Number(result.value) - Number(input.value))
                        operation.value = ""
                    }
                    else if(result.value.includes("*")){
                        result.value = String(Number(result.value) * Number(input.value))
                        operation.value = ""
                    }
                    else if(result.value.includes("/")){
                        if(input.value !== "0"){
                            result.value = String(Number(result.value) / Number(input.value))
                            operation.value = ""
                        }
                        else{
                            result.value = "NaN"
                            operation.value = ""
                        }
                    }
                    else if(result.value.includes("%")){
                        result.value = String(Number(result.value) % Number(input.value))
                        operation.value = ""
                    }
                    else if(operation.value === "C"){
                        result.value = "0"
                        operation.value = ""
                    }
                    else{
                        result.value += String(input.value)
                    }
                }
                else{
                    result.value += String(input.value)
                }
            }
        }

        else{
            operation.value = input.value
            
            if((operation.value === "C") || (result.value === "Infinity")){
                result.value = "NaN"
                operation.value = ""
            }

            else if(operation.value !== "="){

                if(!((Number(result.value[result.value.length-1] >= 0)) && (Number(result.value[result.value.length-1] <= 9)))){                    
                    result.value = result.value.substring(0, result.value.length-1)                    
                }

                result.value += operation.value
            }

            else{ // operation is =

                if(result.value.includes("+")){
                    result.value = String(Number(result.value.split("+")[0]) + Number(result.value.split("+")[1]))                    
                    operation.value = ""
                }
                else if(result.value.includes("-")){
                    result.value = String(Number(result.value.split("-")[0]) - Number(result.value.split("-")[1]))
                    operation.value = ""
                }
                else if(result.value.includes("*")){
                    result.value = String(Number(result.value.split("*")[0]) * Number(result.value.split("*")[1]))
                    operation.value = ""
                }
                else if(result.value.includes("/")){
                    result.value = String(Number(result.value.split("/")[0]) / Number(result.value.split("/")[1]))
                    operation.value = ""
                }
                else if(result.value.includes("%")){
                    result.value = String(Number(result.value.split("%")[0]) % Number(result.value.split("%")[1]))
                    operation.value = ""
                }
            }
        }
    }
})
</script>

<style lang="scss">
.calculator{
    width: 400px;
    margin: auto;
}

table{
    background-color: lightgray !important;

    th{
        border-bottom: 1px solid white !important;
    }
}

tbody{
    td{
        border: none !important;
    }
    
    .v-btn{
        background-color: #1d94ce;
        margin: 1rem 0;
    }
}

@media screen and (max-width: 400px) {
    .calculator{
        width: 300px;
    }

    td{
        padding: 5px !important;
    }

    .v-btn{
        height: 30px !important;
        margin: 0 !important;
    }
}
</style>