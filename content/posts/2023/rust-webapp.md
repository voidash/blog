---
title: "Pragmatic way to create a Rust based Web application | From Start to Deployment"
date: 2023-12-14T22:22:33+05:45
draft: false
cover:
  image: "https://i.imgur.com/2LE2gGU.png"
---

A straightforward tutorial on how to setup a Rust based web app with actix and basic templating functionalities. A useless endeavour in post GPT era. I assure you all of this is thought out and planned by a human who wasted some human hours on this.

## The tools for the trade 

- Actix : Web framework for Rust
- Tera : Templating engine that is one to one copy of Jinja Templating engine
- Nginx : HTTP Server


## `Cargo.toml`


```toml
actix-files = "0.6.2"
actix-web = "4.4.0"
lazy_static = "1.4.0"
tera = "1.19.1"
```

## Discovering Actix 

A simple HTTP server that responds with "Hello World". Look through [Getting started with Actix](https://actix.rs/docs/getting-started) for more information.

```rust
// Don't copy this code, just for example purposes

use actix_web::{get, App,HttpResponse, HttpServer, Responder};
use actix_files as fs;

#[get("/")]
async fn hello() -> impl Responder {
    HttpResponse::Ok().body("Hello World")
}


#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}

```

## Templating

One can just open up a file using `include_str!` , then serve the string through `HttpResponse`. If dynamic content is needed then we can always manipulate the string, but that is a hassle. A little touch of metaprogramming won't hurt here. We can manage server side state straight forward. But we don't want statefulness and virtual-dom like React. Our main goal here is to achieve performance while understanding every bits of abstraction without making the program bloated. So i chose [Tera](https://keats.github.io/tera/docs/).

Suppose Templates directory has the following format.

```
templates/
  countdown.html
  base.html
  products/
    product.html
    price.html
```

### The contents of `base.html`

The base of our templates. For for information about templating refer to [jinja2 documentation](https://jinja.palletsprojects.com/en/3.1.x/templates/#blocks)


```
// copy this code and save it as templates/base.html
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <link rel="stylesheet" href="./style.css" />
    <title>{% block title %}{% endblock title %}</title>
    {% endblock head %}
</head>
<body>
    <div id="content">{% block content %}{% endblock content %}</div>
    <div id="footer">
        {% block footer %}
        &copy; Copyright 2023 <a href="http://thapa-ashish.com.np">Ashish Thapa</a>.
        {% endblock footer %}
    </div>
</body>
</html>

```

Lets focus on **Blocks**

Blocks are used for inheritance and act as both placeholders and replacements at the same time. 

```
    {% block head %}
        <link rel="stylesheet" href="./style.css" />
        <title>{% block title %}{% endblock title %}</title>
    {% endblock head %}
```

Later other templates can use this as a ground source of truth and extend from the template.

Now lets define a child template `countdown.html`

```
//save it on templates/countdown.html 

{% extends "base.html" %}
{% block title %}Index{% endblock title %}
{% block head %}
    {{ super() }}
    <style type="text/css">
        .important { color: #336699; }
    </style>
{% endblock head %}
{% block content %}
    <h1>Index</h1>
    <p class="important">It's a final countdown</p>
{% endblock content %}
```

We can go through all the templates with  

```
#[macro_use]
extern crate lazy_static;

// code is executed at runtime to be initialized when one uses lazy_static 
lazy_static! {
    pub static ref TEMPLATES: Tera = {
        let mut tera = match Tera::new("templates/**/*") {
            Ok(t) => t,
            Err(e) => {
                println!("Parsing error(s): {}", e);
                ::std::process::exit(1);        
            }
        };
        // to understand why autoescaping is done go
        // https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html

        tera.autoescape_on(vec![".html"]);
        tera.register_filter("do_nothing", do_nothing_filter);
        tera
    };
}
```

## Connecting the dots `index.html`


```
// you can copy this code
#[macro_use]
extern crate lazy_static;

extern crate tera;

use tera::{Tera,Context};


lazy_static!{
    pub static ref TEMPLATES: Tera = {
        let mut tera = match Tera::new("./templates/**/*") {
            Ok(t) => t,
            Err(e) => {
                println!("Parsing error(s): {}", e);
                ::std::process::exit(1);
            }
        };
        tera.autoescape_on(vec![".html", ".sql"]);
        tera
    };
}

use actix_web::{get, App,HttpResponse, HttpServer, Responder};
use actix_files as fs;

#[get("/")]
async fn hello() -> impl Responder {
    let context = Context::new();

    let response = TEMPLATES.render("countdown.html",&context).unwrap();
    HttpResponse::Ok().body(response)
}


#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            //we also want static files like css files to be exposed by the server.
            .service(fs::Files::new("/static","./static/").show_files_listing())
            .service(hello)
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}

```

Once done, try running the app with `cargo run`, and if successful, we can get to setting up setting Nginx. 

## Create a systemd service for the app

Create a file named /etc/systemd/system/rust-app-countdown.service with the following content:

```
[Unit]
Description=rust-app

[Service]
ExecStart=/path/to/your/my_rust_app
WorkingDirectory=/path/to/your
User=your_username
Restart=always
```

Enable and start the service with

```
sudo systemctl enable rust-app-countdown
sudo systemctl start rust-app-countdown
```

## Nginx setup 

Install nginx

```
sudo apt update
sudo apt install nginx
sudo ufw allow 80
// create a server block

sudo nano /etc/nginx/sites-available/rust-app-countdown
```

Add the following configuration on the file rust-app-countdown

```
server {
    listen 80;
    server_name your_domain_or_droplet_ip;

    location / {
        proxy_pass http://localhost:your_rust_app_port;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}

```
Lastly create a symbolic link, verify the configuration and restart nginx


```
sudo ln -s /etc/nginx/sites-available/my_rust_app /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl restart nginx

```

















