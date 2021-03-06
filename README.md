# Terrastate HTTP

> A simple standalone webserver for [Terraform HTTP remote state](https://www.terraform.io/docs/backends/types/http.html) written in [Go](https://golang.org/).

## Features
- Basic Authentication
- State Locking
- Overview of stored states

## Usage
### Known issue
- For now you have to provide a JSON config file. The most minimum content has to be: `{}`

1. Starting docker container:
   ```bash
   docker run --rm --detach \
     --name terrastate-http \
     --publish 8080:8080 \
     --volume $(pwd)/example-config.json:/mnt/configs/config.json \
     --volume $(pwd)/sqlite3/:/mnt/sqlite3/ \
     psolru/terrastate-http
   ```

2. Configure Terraform (e.g. in backend.yml):
   ```yaml
   terraform {
     backend "http" {
       address = "http://terrastate.example.com:8080/my_terraform_project"
       lock_address = "http://terrastate.example.com:8080/my_terraform_project/lock"
       unlock_address = "http://terrastate.example.com:8080/my_terraform_project/unlock"
       username = "my_username"
       password = "my_password"
     }
   }
   ```

3. optional: Setup Reverse Proxy
4. enjoy your secure, remote and self managed Terraform Statemanagement

## SSL
SSL is currently **not** supported. You have to manage this by your own. (But i planned it to be a future feature.)


### Routes
Terrastate is written by considering the specifications of [Terraforms http backend](https://www.terraform.io/docs/backends/types/http.html).  
Therefore the routes are self mostly explaining.  
"/list" is an exception and added by the idea to have a slight overview of the stored states.

Overview: 
- "/list" [GET] - Returns all States and their ID, eg.: `{"13":"my_fancy_cluster"}`
- "/{ident}" [GET]
- "/{ident}" [POST]
- "/{ident}/lock" [LOCK]
- "/{ident}/unlock" [UNLOCK]

### Notice / Contribution
This is my first go project. There are many things which could be done better and I already have a little todo list. If you think there are some critical things which should be prioritized please open an issue about it. Thank you!
