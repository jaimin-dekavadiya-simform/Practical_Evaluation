# 1 : Create User Service

## Bug 1 : user.service.ts - Line 15

We are not checking is the user already exists in the Database before creating it.

### Solution :

We should make a DB call to check if the user with the same email already exists in the DB or not before creating and and return apropriate HTTP response.

## Bug 2 : user.contoller.ts - Line 9

We are not validating user input with DTO class, so any invalid input is directly pushed to the DATA base. ( this issue is applicable to all other controllers as we are not using any validators.)

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

# 2 : Order Service :

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

## Bug 4 : order.service.ts - line 83

We are using for loop fr bulk importing the Orders.

### Solution :

We should use one query with Isert Many to bulk import. to avoid multiple query IO.

# 2 : Report Service :

## Bug 1 : report.service.ts - line 56

We are awaiting the notifyReport function. this will introduce unnecessary delay in the response.

### Solution :

We should not await any type of notification service in the Create Services. We can use queue to handle these functions so we can retry or if not that important then we can just let them run in background and return the response.

## Bug 2 : report.service.ts - line 47

Unnecessary nested loops. Just consuming CPU cycles which does nothing. and it will cause Timeout.

### Solution :

Remove the Unnecessary code ( loops ).

## Bug 3 : Generalized

We are using `$queryRawUnsafe` everywhere. This can lead to SQL injection and a security risk.

### Solution

We should use `$queyRaw` which prevents SQL injections and improves security.
