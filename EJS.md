# EJS
EJS will by default look into the `views` folder. But you can specify what folder you want to use instead

```
app.set("view engine", "ejs");
app.set("views", "myviews");
```

## Parsing Data into The `View` (template)
* `<% const name = "weibo" %>`
* When outputting values, `<%= name %>`

Now, what you really want is to pass data into the database. To do that, 
1. Pass data through render method: 
  * `  res.render("index", { title: "Weibo's Home" });`
2. Use it directly in the index.ejs file `<title><%= title %></title>`

## Parsing a array works just as well too.
```
  <div class="blogs">
    <h1>All blogs</h1>
    <% if(blogs.length > 0) { %>
      <% blogs.forEach(blog => { %>
        <h3><%= blog.name %> </h3>
        <p><%= blog.email %> </h3>
        <p><%= blog.body %> </h3>
      <% }) %>  
    <% } %>
  </div>
```

## Partials
Think of a navbar, you have to repeat that every single time in every ejs file. In the future if you wish to edit this, you would have to go through each file to edit each one slowly. That is really not efficient at all. What you can do is use `partials`. With partials, you can create a file that just held the navbar and import that into the view so that you only have to update one file for all views. 
