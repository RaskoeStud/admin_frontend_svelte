<script>
    import axios from 'axios';

    let password;
    let passwordTest;

    async function handleSubmit() {
        let firstname = document.getElementById("firstname").value;
        let lastname = document.getElementById("lastname").value;
        let age = document.getElementById("age").value;
        let email = document.getElementById("email").value;
        let password = document.getElementById("password").value;

        axios({
            url: 'https://dls-admin-backend.azurewebsites.net/customers/',
            method: 'post',
            headers: {
                jwttoken: localStorage.getItem("jwttoken"),
                'Access-Control-Allow-Origin': '*',
                Accept: "application/json",
                "Content-Type": "application/json;charset=UTF-8",
            },
            data: {
                query: `query AddCustomer {
                    customers {
                        ${firstname}
                        ${lastname}
                        ${age}
                        ${email}
                        ${password}
                    }
                }`
            }
        })
        .then(data => {
            if (data.data.errors) {
                alert("An error occured");
                return;
            }
            alert("Customer added");
            window.location.href = "#/customers";
        })
        .catch(error => {
            console.log(error);
            alert("An error occured");
        })
    }


</script>

<h1>Add customer</h1>

<form on:submit|preventDefault="{handleSubmit}">
    Insert first name:
    <input type="text" name="firstname" id="firstname" ><br>
    
    Insert last name:
    <input type="text" name="lastname" id="lastname" ><br>
    
    Insert age:
    <input type="number" name="age" id="age" ><br>
    
    Insert email:
    <input type="email" name="email" id="email" ><br>
    
    Insert password:
    <input bind:value={password} type="password" name="password" id="password" ><br>
    
    Please repeat password:
    <input bind:value={passwordTest} type="password" name="passwordTest" id="passwordTest" ><br>

    {#if password !== passwordTest || password === "" || passwordTest === ""}
        <p style="color: red">Passwords do not match</p>
        <button type="submit" disabled>Add customer</button>
    {:else}
        <button type="submit">Add customer</button>
    {/if}
</form>

<style>
    form {
        position: left;
    }
</style>