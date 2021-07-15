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
  2. `  res.render("index", { title: "Weibo's Home" });`
3. Use it directly in the index.ejs file `<title><%= title %></title>`
