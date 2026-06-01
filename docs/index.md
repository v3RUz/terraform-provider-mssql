# Microsoft SQL Server Provider

The SQL Server provider exposes resources used to manage the configuration of resources in a Microsoft SQL Server and an Azure SQL Database. It might also work for other Microsoft SQL Server products like Azure Managed SQL Server, but it has not been tested against these resources.

## Example Usage

```hcl
terraform {
  required_providers {
    mssql = {
      source = "betr-io/mssql"
      version = "0.1.0"
    }
  }
}

provider "mssql" {
  debug                   = false
  max_open_connections    = 10
  max_idle_connections    = 10
  connection_max_lifetime = 300
  connection_max_idle_time = 120
}

resource "mssql_login" "example" {
  server {
    host = "localhost"
    login {
      username = "sa"
      password = "MySuperSecr3t!"
    }
  }
  login_name = "testlogin"
  password   = "NotSoS3cret?"
}

resource "mssql_user" "example" {
  server {
    host = "localhost"
    login {
      username = "sa"
      password = "MySuperSecr3t!"
    }
  }
  username   = "testuser"
  login_name = mssql_login.example.login_name
}
```

## Argument Reference

The following arguments are supported:

* `debug` - (Optional) Either `false` or `true`. Defaults to `false`. If `true`, the provider will write a debug log to `terraform-provider-mssql.log`.
* `max_open_connections` - (Optional) Maximum number of open connections to the database. Limits concurrent sessions to prevent exhausting server session limits. Defaults to `10`.
* `max_idle_connections` - (Optional) Maximum number of idle connections kept in the pool, ready for reuse. Avoids the latency of establishing new connections under load. Defaults to `10`.
* `connection_max_lifetime` - (Optional) Maximum lifetime of a connection in seconds. Expired connections are closed gracefully after use. Set to `0` for unlimited. Defaults to `300`.
* `connection_max_idle_time` - (Optional) Maximum time in seconds a connection may be idle before being closed. Helps free resources from unused connections. Set to `0` to disable. Defaults to `120`.
