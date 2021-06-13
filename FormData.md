# form data notes
This is about sending HTML forms: with or without files, and so on

## What is FormData?
- The object that represent HTML form data. 
- Constructor `let formData = new FormData([form]);`
	- if Form is provided, it automatically captures its fields
	- Can be used with fetch
	- What is happening here is that when the submit button is clicked, it will run a async function, passing in the value of e. 
	- Then we call a fetch request, specify that we are doing a POST (updating / creating a new source to server) instead of GET (default) and in the body, we specify the values. Here, all we have to do is call body: new FormData(formElem"just a identifier")

## FormData Methods
- `formData.append(name, value)` – add a form field with the given name and value,
- `formData.append(name, blob, fileName)`  – add a field as if it were <input type="file">, the third argument fileName sets file name (not form field name), as it were a name of the file in user’s filesystem,
- `formData.set(name, value) and formData.set(name, blob, fileName)` are the same as append  but the only difference is that they remove all fields with the given name and append a new field so it will make sure it only has one field. With append, it will lead to additional adding of sameName fields. 
- `formData.delete(name)`  – remove the field with the given name,
- `formData.get(name)`  – get the value of the field with the given name,
- `formData.has(name)`  – if there exists a field with the given name, returns true, otherwise false
