# Django

Django is a template engine create by [flosch](https://github.com/flosch/pongo2), to see the original syntax documentation please [click here](https://docs.djangoproject.com/en/dev/topics/templates/)

### Basic Example

_**./views/index.django**_
```html
{% include "views/partials/header.django" %}

<h1>{{ Title }}</h1>

{% include "views/partials/footer.django" %}
```
_**./views/partials/header.django**_
```html
<h2>Header</h2>
```
_**./views/partials/footer.django**_
```html
<h2>Footer</h2>
```
_**./views/layouts/main.django**_
```html
<!DOCTYPE html>
<html>

<head>
  <title>Main</title>
</head>

<body>
  {{embed}}
</body>

</html>
```

```go
package main

import (
	"github.com/gofiber/fiber"
	"github.com/gofiber/template/django"
)

func main() {
	// Create a new engine
	engine := django.New("./views", ".django")

  // Or from an embedded system
  // See github.com/gofiber/embed for examples
  // engine := html.NewFileSystem(http.Dir("./views", ".django"))

	// Pass the engine to the Views
	app := fiber.New(&fiber.Settings{
		Views: engine,
	})

	app.Get("/", func(c *fiber.Ctx) {
		// Render index
		c.Render("index", fiber.Map{
			"Title": "Hello, World!",
		})
	})

	app.Get("/layout", func(c *fiber.Ctx) {
		// Render index within layouts/main
		c.Render("index", fiber.Map{
			"Title": "Hello, World!",
		}, "layouts/main")
	})

	app.Listen(3000)
}

```
