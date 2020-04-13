# MongoDB Cheatsheet


## Key Mongo Aggregation Frameworks 
* Modeled based on data processing pipelines. 
* Documents enter a multi-stage pipeline that transforms documents into an aggregated result.
* Key steps include:
  * Providing filters (e.g. $match) that operate like queries and document transformations that modify form of the output.
  * Providing for grouping and sorting documents (e.g. $group and $sort) by specific field or fields, and tools for aggregating the contents of arrays, including arrays of documents.

* Example of a well-structured Mongo query would look something like this: 
  * db.students.aggregate([{$match: {grade: "A"}}, {$group: {_id: "studentID", marks: {$avg: "$marks"}}}])
  
* Other possible stages in aggregation framework 
  * $project : To select only specified fields or drop specified fields
  * $match : filtering operation used to drop some stuff you don't need before the next stage (e.g sort or group) 
  * $sort : Just sorts the documents
  * $skip : Skip forward in a given list of documents for a specified number of documents
  * $limit : Just limits the number of documents to view 
  * $unwind : Breaks some elements in a given array into their constituents, creating separate documents. 
   * For example: 
    FIRST: db.students.insertOne({"_id": 1, "school": NUS, courses: ["BT2102", "CS2030"]})
    THEN: db.students.aggregate([{$unwind: "$courses"}])
    RETURNS: 
    {"_id": 1, "school": NUS, "courses": "BT2102"} 
    {"_id": 1, "school": NUS, "courses": "CS2030"}
   
    
### What is an Aggregation Pipeline???

**In SQL COUNT(*) and with GROUP BY is basically the same as mongo's aggregation** 
_Basic basic syntax of aggregate() method:_ 

db.collection.aggregate(aggregation_operation)

Let's say we have a collection students. If I want to display a list of how many submissions each student made, I would write the the following: 

db.students.aggregate([{$group: {_id:"$student_name", numSubs: {$sum: 1}}}]);

_Over here, I'm using student_name as a field, which will replace the _id part in the query output_ 

#### Important Aggregation Expressions
Let the collection here be students 
1. SUM ($sum) 
db.students.aggregate([{$group: {_id: "student_name", numSubs: {$sum: $views}}}]); 

2. AVERAGE ($avg) 
db.students.aggregate([{$group: {_id: "student_name", numSubs: {$avg: $views}}}])

3. MINIMUM ($min) 
db.students.aggregate([{$group: {_id: "student_name", numSubs: {$min: $views}}}]);
_returns the minimum of the corresponding values from all documents in a collection_

4. MAXIMUM ($max) 
db.students.aggregate([{$group: {_id: "student_name", numSubs: {$max: $views}}}]);

5. PUSH ITT ($push) 
db.students.aggregate([{$group:{_id: "student_name", submissions: {$push: $submissions}}}}])
_yeah I actually still am not 100% sure what push is so here's more examples:_ 
* $push is only available in the $group stage 
* syntax is as such: {$push: <expression>}
* refer to Mongo documentation 

6. ADD TO SET ($addToSet) 
db.students.aggregate([{$group: {_id: "student_name", submissions: {$addToSet: $submissions}}}])
* "submissions" is the new field that is created, everything is grouped by the _id and stuff is added to a new array

7. FIRST ($first)
db.students.aggregate({$group: {_id: "student_name", first_sub: {$first: $sub}}}); 
* This will get the first DOCUMENT from the source documents according to the grouping. Typically this makes only sense together with some previously applied “$sort”-stage
* (From MongoDB example) For example, this syntax would make more sense than just $group and $first: 
 * db.sales.aggregate({[{$sort: {item: 1, date: 1}}, {$group: {_id: "$item", firstSale: {$first: "$date"}}}]);

8. LAST ($last)
db.students.aggregate({$group: {_id: "student_name", last_sub: {$last: $sub}}}); 
