Log in by creating an account under the customer section. Once logged in, go to **Your Account** to update your username, email, and password.  You'll notice the username and email fields pre-filled in with your information.  

We'll start by investigating how this information gets pre-filled. If you ***open your browser developer tools, select the network tab and then refresh the page***, you'll see a call to an endpoint with the path `/api/v1/customer?id={user_id}`

This page returns in JSON format your user id, username and email address. We can see from the path that the user information shown is taken from the query string's id parameter