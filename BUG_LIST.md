# 1 : Create User Service

## Bug 1 : user.contoller.ts - Line 9

We are not checking is the user already exists in the Database before creating it.

### Solution :

We should make a DB call to check if the user with the same email already exists in the DB or not before creating and and return apropriate HTTP response.

## Bug 2 :

We are not validating user input with DTO class, so any invalid input is directly pushed to the DATA base.

### Solution :

We should create a DTO class with class-validator and validate the user input before running the service.

## Bug 3 :

We are creating Local prisma client in the service, which will be created on every incoming request.

### Solution :

We should create a Module/instace/Client once in a config file and use the same DbClient with DI injection everywhere else. (The Prisma service is already Injected, we should use that only.)
