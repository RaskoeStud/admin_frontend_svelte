<script>
    import axios from "axios";

    let errortext = "";

    async function handleSubmit() {
        let usernameT = document.getElementById("username").value;
        let passwordT = document.getElementById("password").value;

        const query = `
        mutation Mutation($username: String!, $pass: String!) {
            Login(username: $username, pass: $pass) {
                email
                admin_id
                username
                is_superuser
                jwttoken
            }
        }`;
        // Define the variables for the query
        const variables = {
            username: usernameT,
            pass: passwordT
        };
        const header = {
            'Access-Control-Allow-Origin': '*',
            "Accept": "application/json",
            "Content-Type": "application/json;charset=UTF-8",
        }
        
        const response = await axios('http://localhost:3000/graphql', {
            method: 'POST',
            headers: header,
            body: JSON.stringify({
                query: query,
                variables: variables
            })
        }).then((response) => {
            if(response.error){
                alert(response.error);
            }else{
                const result = response.json();
                const { Login } = result.data;
                console.log(result);

                localStorage.setItem("jwttoken", Login.jwttoken);
                localStorage.setItem("admin_id", Login.admin_id);
                localStorage.setItem("username", Login.username);
                localStorage.setItem("email", Login.email);
                localStorage.setItem("is_superuser", Login.is_superuser);
                window.location.href = "#/home";
                window.location.reload();
            }
        }).catch((error) => {
            console.error("Error:", error);
            errortext = error;
        });
    } 
</script>

<main>
    <h1>Login</h1>
    <form on:submit|preventDefault="{handleSubmit}">
        <p>Insert username</p>
        <input
            id="username"
            type="text"
            placeholder="Username"
        />
        <p>Insert password</p>
        <input
            id="password"
            type="password"
            placeholder="Password"
        /> <br> <br>

        <p style="color: red">{errortext}</p>
        
        <button type="submit" class="form-field">
            Login
        </button>
    </form>
</main>
