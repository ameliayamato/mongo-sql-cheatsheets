
### Some key Mongo Aggregation Framworks 

**In SQL COUNT(*) and with GROUP BY is basically the same as mongo's aggregation** 
_Basic basic syntax of aggregate() method:_ 

db.collection.aggregate(aggregation_operation)

#Let's say we have a collection students
#If I want to display a list of how many submissions each student made, I would write the the following: 

db.students.aggregate([{$group: {_id:"$student_name", numSubs: {$sum: 1}}}]);

_Over here, I'm using student_name as a field, which will replace the _id part in the query output_ 

#Important Aggregation Expressions
#Let the collection here be students 
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
