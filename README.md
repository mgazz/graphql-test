
Tutorial from: https://www.howtographql.com/graphql-go/0-introduction/

## Init

Gen graphql project
```
go run github.com/99designs/gqlgen init
```

Define schema in `graph/schema.graphqls`

Generate files from schema
```
go run github.com/99designs/gqlgen generate
```

**NOTE:** the logic is in the `schema.resolvers.go`

## Start mysql db
```
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=dbpass -e MYSQL_DATABASE=hackernews -d mysql:latest
docker exec -it mysql bash
```

## Create the db
```
# mysql -u root -p
mysql > CREATE DATABASE foo
```

### Setup DB migration
```
go get -u github.com/go-sql-driver/mysql
mkdir -p internal/pkg/db/migraions/mysql
go build -tags 'mysql' -ldflags="-X main.Version=1.0.0" -o $GOPATH/bin/migrate github.com/golang-migrate/migrate/v4/cmd/migrate/
migrate create -ext sql -dir internal/pkg/db/migraions/mysql -seq create_users_table
migrate create -ext sql -dir internal/pkg/db/migraions/mysql -seq create_links_table
migrate -database mysql://root:dbpass@/foo -source file://internal/pkg/db/migrations/mysql up
```

Check that the tables are actually there
```
mysql > describe foo.Links;
mysql > describe foo.Users;

```
