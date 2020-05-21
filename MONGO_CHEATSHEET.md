# MongoDB cheatsheet
By Toki

## Features
* **Open-source _NoSQL_ database** ---> Catch-all term for databases that generally aren’t relational and don’t have a query language like SQL 
* **Document-oriented** ---> Documents Are Just JSON-like Objects
* **Great for unstructured data**, especially when you have a lot of it

## Installing

'''
[Click here to follow the manual instructions](https://dangphongvanthanh.wordpress.com/2017/06/12/add-mongos-bin-folder-to-the-path-environment-variable/)
'''

## Comparing MongoDB to RDBMS
SQL is relational and MongoDB is document-oriented.

![mongo_comparisson](img/mongolito.JPG)

## Starting the Shell
```
$ mongo
>
```
### Use or create a new database
```
> use reviews # Switches to use the database and creates it if doesn't exist when we write to it

> db # reviews
```

### Show list of commands
```
> help
db.help()       ...
...             ...
...             ...

> show dbs
test          0.078GB
reviews       0.078GB
```

## Login into a cluster

```
mongo "mongodb+srv://cluster0-qwc17.mongodb.net/test" --username TypeYourUsername
```
## Some basic commands
* Inserting a Document Into a Collection, if doesn't exist, one will be created
```
> db.YourCollectionName.insert(
  {.
   "name": "Toki",
   "another_field": "Yomi's"
  }
  )
WriteResult({ "nInserted": 1 })
```
