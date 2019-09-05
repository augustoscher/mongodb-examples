## MongoDB

## BASICS
1) Access mongodb container: ```docker exec -it containerid bash```

2) Init mongo: ```mongo```

3) Databases: ```show dbs```

4) Use database: ```use mymoney```

5) What database I'm: ```db```

6) Show collections: ```show collections```

7) Create collection: ```db.createCollection('test')```

8) Drop collection: ```db.test.drop()```

9) Drop current database: ```db.dropDatabase()```

## INSERT DATA
1) Insert (collection doesn't need to be created):  
```
db.billingCycle.insert({name: "Jan-2019, month: 1, year: 2019});
```

```
db.billingCycle.insert({
  name: "Jan-2019,
  month: 1,
  year: 2019,
  credits: [
    {name: "Salário", value: 10000}
  ],
  debts: [
    {name: "Energia", value: 100, status: "PAGO"},
    {name: "Condominio", value: 100, status: "PAGO"}
  ],
});
```

2) Save - Update or Insert collection    
```
db.billingCycle.save({name: "Fev-2019, month: 2, year: 2019});
```
```
db.billingCycle.save({_id: "32009diasdi090923u0jd0", name: "Fev-2019, month: 2, year: 2019});
```

## QUERY
1) FIND - Returns all documents or by filter  
```
db.billingCycle.find();
```
```
db.billingCycle.find().pretty();
```
```
db.billingCycle.find({$or: [{month: 1}, {month: 2}]}).pretty();
```

2) Show only docs who has "credits"  
```
db.billingCycle.find({credits:{$exists: true}}).pretty();
```

3) FIND ONE - return first doc or by filter  
```
db.billingCycle.findOne();
```
```
db.billingCycle.findOne({month: 2});
```

4) Pagination  
```db.billingCycle.find({year: 2019}).skip(1);```  
```db.billingCycle.find({year: 2019}).skip(1).limit(1);```

5) Return only name of credits  
```db.billingCycle.find({credits:{$exists:true}}, {_id:0, name:1}).pretty();```

## AGGREGATION
1) New aggregation  
```
db.billingCycle.aggregate([{
    $project:{credit:{$sum:"$credits.value"}, debt:{$sum:"$debts.value"}}
  }, {
    $group: {_id:null, credit:{$sum:"$credit"}, debt:{$sum:"$debt"}}
  }
]);
```

## UPDATE
1) Update collection  
```
db.billingCycle.update(
  {$and:[{month: 1}, {year: 2019}]},
  {$set:{credits:[{name: "Salário", value: 10000}]}}
)
```

## COUNT  
```db.billingCycle.count();```

## REMOVE  
1) Removing by filter  
```db.billingCycle.remove({month: 2});```

2) Removing by filter and number of objects that will be removed  
```db.billingCycle.remove({year: 2019}, 1);```




