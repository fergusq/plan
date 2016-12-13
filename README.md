Plan
====

Plan is a lightweight preprocessor and site generator.

## Dependencies

Plan requires Röda 0.11 or above. It also needs the `json.röd` library distributed with the official Röda source code.

## Usage

The Plan can be used either as a preprocessor or a site generator. The executable scripts `plan.röd` and `siteplan.röd` correspond to these usages.

### As a preprocessor

From command line:

```sh
röda plan.röd file
```

The script prints the resulting text to the standard output.

From another Röda script:

```c
/* imports */
{
	import "planlib.röd"
}

/* ... */

file_name := "my_page.plan"

variables := new map
variables["title"] = "My page"

execPlan(file_name, variables)
```

### As a site generator

The site can be generated using the following command:

```sh
röda siteplan.röd
```

This will generate the site according to the `data/config.json` file.

#### Example site: a blog

The directory structure should look something like this:

```
.
|
|-- json.röd
|-- planlib.röd
|-- siteplan.röd
|
|-- build/
|   |-- posts/
|
|-- data/
|   |-- config.json
|   |-- posts.json
|
|-- templates/
|   |-- _post.plan
```

`data/config.json`:

```json
{
	"site_name": "Iikka's blog",
	"pageclasses": [
		{
			"name": "Posts",
			"dir": "posts",
			"datafile": "posts.json",
			"templatefile": "_post.plan"
		}
	]
}
```

`data/posts.json`:

```json
[
	{
		"name": "the_first_post",
		"title": "The first post!",
		"author": "Iikka Hauhio",
		"date": "2016-12-13",
		"content": [
			[
				"This is my very first blog post!",
				"Thank you for visiting my blog!"
			],
			[
				"See you soon :)"
			]
		]
	}
]
```

`templates/_post.plan`:

```html
<html>
<head>
	<title>|self.title| - |config.site_name|</title>
</head>
<body>
	<h1>|config.site_name|</h1>
	
	<h2>|self.title|</h2>
	|self.date| - By: |self.author|
	
	| for paragraph in self.content |
		<p>| for part in paragraph |
			| part |
		| end |</p>
	| end |
	
	<h2>Latest posts</h2>
	
	<ul>
	| for post in posts.last_5.reverse |
		<li><a href="|root post.path|">|post.title|</a></li>
	| end |
	</ul>
</body>
</html>
```
