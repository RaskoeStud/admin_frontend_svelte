<script>
    import axios from "axios";

    let errortext = "";

    async function handleSubmit() {
        let username = document.getElementById("username").value;
        let password = document.getElementById("password").value;

        const query = `query Login($username: String!, $pass: String!) {
                    Login(username: $username, pass: $pass) {
                        admin_id
                        email
                        id
                    }
                }`

        // Define the variables for the query
        const variables = {
            username: username,
            pass: password
        };

        // Make the Axios call to the GraphQL endpoint
        axios.post('http://localhost:3000/graphql', {
        query: query,
        variables: variables
        })
        .then((data) => {
            if (data.error) {
                alert(data.error);
            } else {
                console.log(data)
                localStorage.setItem("jwttoken", data.jwttoken);
                localStorage.setItem("admin_id", data.admin_id);
                localStorage.setItem("username", data.username);
                localStorage.setItem("email", data.email);
                localStorage.setItem("is_superuser", data.is_superuser);
                window.location.reload();
                window.location.href = "#/home";
            }
        })
        .catch((error) => {
            console.error("Error:", error);
            errortext = error;
        });










        // await axios({
        //     method: 'post',
        //     url: 'https://dls-admin-backend.azurewebsites.net/auth/login',
        //     headers: {
        //         'Access-Control-Allow-Origin': '*',
        //         Accept: "application/json",
        //         "Content-Type": "application/json;charset=UTF-8",
        //     },
        //     data: {
        //         query: `query Login($username: String!, $pass: String!) {
        //             Login(username: $username, pass: $pass) {
        //                 admin_id
        //                 email
        //                 id
        //             }
        //         }`,
        //         variables: {
        //             username: username,
        //             pass: password
        //         }
        //     }
        // })
        // .then((data) => {
        //     if (data.error) {
        //         alert(data.error);
        //     } else {
        //         console.log(data.data.errors[0], data.data.errors[1]);
        //         localStorage.setItem("jwttoken", data.data.jwttoken);
        //         localStorage.setItem("admin_id", data.data.admin_id);
        //         localStorage.setItem("username", data.data.username);
        //         localStorage.setItem("email", data.data.email);
        //         localStorage.setItem("is_superuser", data.data.is_superuser);
        //         window.location.reload();
        //         window.location.href = "#/home";
        //     }
        // })
        // .catch((error) => {
        //     console.error("Error:", error);
        //     errortext = error;
        // });
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
