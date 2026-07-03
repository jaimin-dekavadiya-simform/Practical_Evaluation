# 1 : Create User Service

## Bug 1 : user.service.ts - Line 15

We are not checking is the user already exists in the Database before creating it.

### Solution :

We should make a DB call to check if the user with the same email already exists in the DB or not before creating and and return apropriate HTTP response.

## Bug 2 : user.contoller.ts - Line 9

We are not validating user input with DTO class, so any invalid input is directly pushed to the DATA base.

### Solution :

We should create a DTO class with class-validator and validate the user input before running the service.

## Bug 3 : user.service.ts - Line 15

We are creating Local prisma client in the service, which will be created on every incoming request.

### Solution :

We should create a Module/instace/Client once in a config file and use the same DbClient with DI injection everywhere else. (The Prisma service is already Injected, we should use that only.)

## Bug 4 : products.service.ts - Line 24

We are reading the file Synchronously. Which will block the main thread.

### Solution :

We should use the async operation for file read.
Better approach for large file would be create a readStream and pipe it with node res stream.

# 1 : Order Service :

## Bug 1 : order.service.ts - line 23

For calculating total amount we are fetching the rows and then calculating it in the service.

### Solution :

We should run agregate query to calculate it by DB and return the total amount to save latency and bandwidth;

## Bug 2 : order.service.ts - line 41

We are running for loop to insert number of rows. this will execute new query for each Row which is heavy for IO/network and latency will be higher.

### Solution :

We should run the Bulk insert query instead of querying for each row. We should create insert many query by for loop and run it just once.

## Bug 3 : order.service.ts - line 73

We are doing the N+1 query here. for each order we are executing a new query to fetch the order details.

### Solution :

We should execute one query that will join the Table and get us all the data needed for that service. We can use "include" keyword in the prisma also.
