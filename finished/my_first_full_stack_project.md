<h1>My first FullStack React Website</h1>

<p> This story starts at the beginning of phase 3 and ends when I managed to pull my pastry id into my delete function making my final CRUD action complete on the project. At that moment I stepped away and breathed a sigh of relief that Alexia and I were able to finish our website in 4 days. </p>

<p>Creating a Fullstack website required putting together an idea that involved creating, reading, updating, and deleting data. A task that seems simple but as the project went along we were constantly tweaking our database, react components, and general ideas to bend to the requirements of the project. Although I am happy with our website, I am even happier with the lessons we learned and the struggles we faced along the way.</p>

<p>Let’s start with our database schema: </p>

<img src="https://miro.medium.com/max/1400/1*OlfMPwIj8usbPD3y9MbyeQ.png">


<p>We started with a pastry and a recipe. We made a table with pastries so we could have staple pastries that would many recipes created for them. In turn, we needed to have someone who create recipes, but not pastries. We added users to be our recipe authors, users only have a name attribute. Our recipe table had the typical recipe attributes but the name referred to the name of the recipe and not the staple pastry. For example, ‘pain au chocolate’ Chocolate Croissant, would belong to the Croissant pastry. Lastly, we have our Pastries table which holds information about a pastry such as the country it's from, description, and photo but it does not contain its recipe. Countries' information was a separate table allowing many pastries to belong to one country. In the end, our database comprised of countries table, pastry table, recipe table, and user table.</p>

<h2>Our four CRUD paths were:</h3>

<h3>Search bar Pastries and we used the same one with Recipe switched for pastry:</h5>

<p>frontend-</p>

```
useEffect(()=> {

    fetch(`http://localhost:9292/userrecipes/${search}`)

    .then((resp) => resp.json())

    .then((data) => setPastry(data))

},[handleSubmit])
    
```

<p>backend-</p>
    
```

get “/pastry/:name” do

    pastry = Pastry.where(“name LIKE ?”, “%#{params[:name]}%”)

    pastry.to_json

end

```
<h3>Create a recipe:</h3>

<p>frontend-</p>

```

const postSet = {

    method:’POST’,

    headers: {‘Content-Type’:’application/json’},

    body: JSON.stringify(data)}

    function handleSubmit(event) {

    fetch(‘http://localhost:9292/create-recipe', postSet)

}

```

<p>backend-</p>

```
post “/create-recipe” do

    new_recipe = Recipe.create(pastry_id: params[:id], name: params[:name], user_id: params[:user_id], rating: params[:rating], description: params[:description], prep_time: params[:prep_time], bake_time: params[:bake_time], total_time: params[:total_time], recipe_ingredients: params[:ingredients], recipe_instructions: params[:instructions])

    new_recipe.to_json

end
```

<h3>Update a pastry:</h3>

<p>frontend-</p>

```
function handleChange(e) {

    e.preventDefault();

    const updatedPastry = {

    photo: updatedPhoto,

    description: updatedDescription,

    }

    console.log(updatedName)

    const name = updatedName

    fetch(`http://localhost:9292/pastry/${name}`, {

    method: “PATCH”,

    headers: {“Content-Type”: “application/json”},

    body: JSON.stringify(updatedPastry),

    })

}
```

<p>backend-</p>

```
patch “/pastry/:name” do

    pastry = Pastry.where(name: params[:name])

    pastry.update({photo:params[:photo], name:params[:name], description:params[:description]})

    pastry.to_json

end
```

<h3>Delete a pastry:</h3>

<p>frontend-</p>

```
function handleDelete(pastry){

    const url = `http://localhost:9292/delete/${pastry.id}`

    fetch(url,{

    method:’DELETE’,

    headers:{‘Content-Type’:’application/json’}

    })

    .then(res => res.json())

    .then(data => setPastry(data));

    document.location.reload();

}
```

<p>backend-</p>

```
delete “/delete/:id” do

    recipe = Recipe.find(params[:id])

    recipe.destroy

    recipe.to_json

end
```

<p>When creating these paths, the most difficult ones for me were the patch and delete. I had trouble connecting the logic of how I pull parameters required for finding the specific item to delete each time I render the pastry cards. For the patch, I realized that I was automatically passing the states into the function and didn’t need to declare them as arguments or parameters to be passed into.</p>

<p>For the delete function, my mistake was how i was passing the pastry that was being mapped directly into the handle delete function that occurred onClick. I created an arrow function within the OnClick to pass the pastry directly into the handle delete. Then I passed the pastry into handle delete as a parameter and pushed the id of that pastry into my backend search. The last step to have the page reload is to add a ‘document.location.reload()”. However, I do wonder if I could put part of this into a useEffect that changed when a user clicked delete so the page didn’t have to fully reload. I look forward to learning more and figuring out how to accomplish that and have the website working fast.</p>

<p>Overall, this project had lots of front-end struggles and discoveries I learned that I prefer working in the backend and creating a database. I look forward to learning rails and continuing to learn more full-stack development tools.</p>