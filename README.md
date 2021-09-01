[![GitHub license](https://img.shields.io/github/license/Naereen/StrapDown.js.svg)](https://github.com/Naereen/StrapDown.js/blob/master/LICENSE)  [![made-with-javascript](https://img.shields.io/badge/Made%20with-JavaScript-1f425f.svg)](https://www.javascript.com) 
# CURD.js
`CURD.js` gives you wing in your localStorage.

<p>
It is a javascript library that allows you to use your localStorage as a document database.
</p>

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Structure](#structure)
- [Functions](#functions)
  - [Initialization](#functions)
  - [Create Collection](#functions)
  - [Collection Exists Or Not](#functions)
  - [Insert Data](#functions)
  - [Read Data](#functions)
  - [Get Id](#functions)
  - [Update Data](#functions)
  - [Delete Data](#functions)
- [Drawbacks](#conclution)

<h2 id="overview">Overview</h2>

It is a javascript library that allows you to use your localStorage as a document database. With `CURD.js` you can performe each and every opperation that a normal document database has.
Which mean now you can use your localStorage as a temporay database to store some data temporarily which will be synced with your server letter on.
It can also enhance your [PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)'s performance by giving it a offline database.

<h2 id="installation">Installation</h2>

It is really easy and simple to install and use `CURD.js`. Copy and Paste the following script to your code.

```
<script src="dist/curd.min.js">
```

You can also download and add the `CURD.min.js` file to your code.
And you are good to go with superpowers.

<h2 id="structure">Structure</h2>
It's good to have a clear concept of the structure of your database.

Main database is an object which has all the collections containing all the data

---

```
    MainDatabase : {
        firstCollection{
            key1:{
                Data Object
            },
            key2:{
                Data Object
            },
            ...
        },
        secondCollection{
            key1:{
                Data Object
            },
            key2:{
                Data Object
            },
            ...
        },
        ...
    }
```

---

<h2 id="functions">Functions</h2>

- ### Initialization

  Create a `CURD` object and store it in a variable.
  Database will be created by this method if it's not present in the localStorage and if present it will do nothing.
  <br>

  ```
    var DB = new CURD({
        dbname:"mydb"
    })
  ```

  It takes one parameter as object containing `dbname` where you have to mention your database name you want to create. It is optional to pass any value to `CURD` object.

- ### Create Collection

  ```
  DB.createCollection(Collection Name)
  ```

  Create a new `collection` in your database by calling `.createCollection()` method on the `Database` object and store it in a variable.
  <br>

  ```
  var firstCollection = DB.createCollection('firstCollection');
  ```

  It takes `collection name` as first parameter and `DB` as second parameter and return a new `collection` in which we can perform `insert` operation.
  If `collection` alredy exists it will return `false`.
  Now if any `collection` exists we have to call `.getCollection()` method in order to performe insert operations in that `collection`.
  <br>

  ```
  DB.getCollection(collection name,DB);
  ```

  It returs the `collection` object which will be used to perform insert operations on that `collection`
  <br>

  ```
  var oldCollection = DB.getCollection('firstCollection',DB)
  ```

  Now we can performe normal insert operations on this `collection`.

- ### Collection Exists Or Not

  ```
  DB.existsCollection( Collection Name );
  ```

  To check a collection exists or not we can use `DB.existsCollection()`. It will return `true` or `false` based on the collection exists or not.

- ### Insert Data

  - <h4>Insert One Data</h4>

    ```
    firstCollection.insertData({ Object of Data }, DB)
    ```

    Insert data to a collection by calling `.insertData()` method on the `collection` object we got from `.createCollection()`.
    It takes an Object containing all the data as first parameter and takes `DB` object as a second parameter and return `Index` that has been assigned to the currently inserted data.
    <br>

    ```
    var Index = firstCollection.insertData(
                    {
                        key1: your first data,
                        key2: your second data,
                        ...
                    },
                    DB
                );
    ```

    First parameter must be an object or else it will give an error as **_"Expected Object Got None"_**

  - <h4>Insert Many Data</h4>

    ```
    firstCollection.insertMany([array of objects], DB)
    ```

    We can insert multiple data at once by calling `.insertMany()` on the collection object. But we have to pass an array of objects as a parameter insted of a single object and it returns array of `Id`'s
    <br>

    ```
    var arrayOfObjects = [
        {
            key1: your first data,
            key2: your second data,
            ...
        },
        {
            key1: your first data,
            key2: your second data,
            ...
        },
        ...
    ]
    var arrayOfindices = firstCollection.insertMany(
                            arrayOfObjects,
                            DB
                        );

    ```

- ### Read Data

  - <h4>Read All Collections</h4>

    ```
    DB.readData();
    ```

    `DB.readData()` returns all the data of all collection.
    <br>

    ```
    var allData = DB.readData();
    console.log(allData);
    ```

  - <h4>Read One Collection</h4>

    ```
    DB.readData(Collection Name);
    ```

    `DB.readData()` takes `collection` name and returns all the data of specific collection.
    <br>

    ```
    var singleCollection = DB.readData('firstCollection');
    console.log(singleCollection);
    ```

  - <h4>Read Specific Data of One Collection</h4>

    ```
    DB.readData(Collection Name, ID of Data);
    ```

    If you know the `ID` of specific data you can retrive the data by using `DB.readData()` where you have to pass `Collection Name` as first parameter and the `ID` as second parameter.
    <br>

    ```
    var singleData = DB.readData('firstCollection',1);
    console.log(singleData);
    ```

  - <h4>Read Data With Specific Values</h4>

    ```
    var Data = DB.readData(
          Collection Name,
          {
            key1:value1,
            key2 : value2
          }
        );
    ```

    You can search data by specific key value pairs. Here you have to pass `collection name` as first parameter and the object containaing all key value pairs as second parameter.

- ### Get ID

  - <h4>Get ID Of A Data</h4>

    ```
    DB.getId( Data, Index );
    ```

    To know the `ID` of a data inside collection we can use `getId()` method on `DB` object. It takes the object as first parameter and takes Index as an optional second parameter and returns the `ID` of the object.
    By default `getId()` returns an array of `ID`'s but if you want to get a specific one you can pass `Index` number as second parameter
    <br>

    ```
    var Id = DB.getId( Data );
    ```

    **_We got the `Data` object from `readData()`_**

    we also can use it in another way by calling `readData()` inside `getId()`.
    <br>

    ```
    var Id = DB.getId(
        DB.readData(
          Collection Name,
          {
            key1:value1,
            key2 : value2
          }
        )
     );
    ```

  - <h4>Get All Id's Of A Collection</h4>

    ```
    DB.getAllId(Collection Name);
    ```

    This function returns an array of `ID`'s containing all the `ID`s of the objects inside the `collection`.

- ### Update Data

  - <h4>Update Single Data</h4>

    ```
    DB.updateData( Collection Name , Index, { key: value } );
    ```

    `updateData()` Update an object of a collection based on index, we can pass array of indices to update multiple objects.
    <br>

    ```
    DB.updateData(
      'firstCollection' , 1, { v: 1 }
    );
    ```

    This will update 1st object of the collection. It will add `v` to the object if `v` is not exist in the object or else will update the value of `v` to `1` if it is already present in the object.

  - <h4>Update Multiple Data</h4>

    We can pass array of indices to update multiple objects at a time.
    <br>

    ```
    DB.updateData(
      'firstCollection',
       [1,2,3,4,5],
       { v: 1 }
    );
    ```

    This will update all the objects of the collection respect to the array of indices. It will add `v` to the objects if `v` is not exist in the objects or else will update the value of `v` to `1` if it is already present in the objects.

- ### Delete Data

  - <h4>Delete Collection</h4>
  - <h4>Delete Object of An Collection</h4>

    ```
    DB.deleteCollection( Collection Name );
    ```

    To delete a `Collection` we can use `deleteCollection()` function it takes `collection name` and delete it.
    <br>

    ```
    DB.deleteCollection( 'firstCollection' );
    ```

    If we pass `ID` of an object as a second parameter then it will delete the object of the collection.
    <br>

    ```
    DB.deleteCollection( 'firstCollection', 1 );
    ```

  - <h4>Delete A Field of An Object</h4>

    ```
    DB.deleteField( Collection Name, ID, Field);
    ```

    To delete a field of an object we can use `deleteField()` function it takes 3 parameter, first it takes a `collection name` second the `ID` of the object and third the `field` you want to delete.
    <br>

    ```
    DB.deleteField('firstCollection', 1, 'v');
    ```

    It wll delete the field `v` from `1` st object of `firstCollection`.

* ### Drawbacks
  Though it works like a document database but it has it's own drawbacks. As it is using localStorage for storing the data it is not secure that enough though you can store encrypted data, and localStorage gives only 5MB of storage so you can't store more than 5MB data.
  <br>
  **So if you are using it to store data, not in encrypted format, please do not store crusial information.**
  
