https://docs.microsoft.com/en-us/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide?view=sql-server-ver16  


# Isolation levels

there are 6 isolation levels  
Category: pessimistic  
 - read uncommited  
 - read commited  
 - repeatable read  
 - serializable  
 
These 4 do rely strictly on locking  
 
Category: optimistic  
 - read commited snapshot  
 - snapshot  
 
These 2 do rely on locking and row versioning  

# Locks types

## Exclusive - X
row-level  
for Insert, Update, Delete, Merge  
держатся до конца транзакции при любом уровне изоляции (даже read uncommited)  

## Intent  - I
page and table-level  
Ставится вместе с другими row-level блокировками, но на объект: страницу или таблицу.  
Цель - показать другим сессиям, что на строках есть блокировка, чтобы те не сканировали  
всю таблицу целиком.  
IX  
IU  
IS

## Update - U
Ставится на строки во время поиска и проверки предиката.  
Затем сменяется блокировкой X - если строка подходит под обновление.  
Потенциальная проблема - если запрос не попал в индекс. В этом случае будет выполнен table scan и
каждая строка сначала заблокируется по U, а затем блокировка снимется. Если другая транзакция держит  
блокировку X, даже на неподходящей под обновление строке, то наша сессия будет ждать освобождения,  
т.к. U несовместима с X.  

## Shared - S
## 
## 
## 
## 
## 
## 
## 
