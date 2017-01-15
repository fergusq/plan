Plan
====

Plan is a lightweight preprocessor and site generator.

## Dependencies

* Röda 0.12 or above
* `case` bil
* `json` bil

## Installation

In case you'd wish not to use the Bilar system, please check the old commit
945eebdc4545b465af769358d876967130605ba8, which contains instructions for
installing Plan manually.

The current version of plan should be installed using the Bilar system.
The package for Plan can be downloaded from the releases section of this
Github page.

```sh
bilar install plan.1.0.1.bil.tar.xz
```

To build the package locally, clone the repository and use Bilar to create
an archive.

```sh
git clone https://github.com/fergusq/plan.git
bilar compress plan
bilar install plan.1.0.1.bil.tar.xz
```

## Usage

The Plan can be used either as a preprocessor or a site generator. The executable scripts `plan.röd` and `siteplan.röd` correspond to these usages.

### As a preprocessor

From command line:

```sh
plan file
```

The script prints the resulting text to the standard output.

From another Röda script:

```c
/* imports */
{
	plan := require("plan")
}

/* ... */

file_name := "my_page.plan"

variables := new map
variables["title"] = "My page"

plan.execPlan(file_name, variables)
```

### As a site generator

The site can be generated using the following command:

```sh
siteplan [<pageclasses>]
```

This will generate the site according to the `data/config.json` file.
You can optionally specify which page classes should be generated.

#### Example site: a blog

The directory structure should look something like this:

```
.
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
