
# Some key Mongo Aggregation Framworks 

**In SQL COUNT(*) and with GROUP BY is basically the same as mongo's aggregation** 
_Basic basic syntax of aggregate() method:_ 

db.collection.aggregate(aggregation_operation)

#Let's say we have a collection students
#If I want to display a list of how many submissions each student made, I would write the the following: 

db.students.aggregate([{$group: {_id:"$student_name", numSubs: {$sum: 1}}}]);
# Over here, I'm using student_name as a field, which will replace the _id part in the query output 

#Important Aggregation Expressions
#Let the collection here be students 
* 1. SUM ($sum) 
  * db.students.aggregate([{$group: {_id: "student_name", numSubs: {$sum: $views}}}]); 

* 2. AVERAGE ($avg) 
  * db.students.aggregate([{$group: {_id: "student_name", numSubs: {$avg: $views}}}])

* 3. MINIMUM ($min) 
  * db.students.aggregate([{$group: {_id: "student_name", numSubs: {$min: $views}}}]);
   * returns the minimum of the corresponding values from all documents in a collection
* 4. MAXIMUM ($max) 
 * db.students.aggregate([{$group: {_id: "student_name", numSubs: {$max: $views}}}]);
* 5. PUSH ITT ($push) 
 * db.students.aggregate([{$group:{_id: "student_name", submissions: {$push: $submissions}}}}])
  * yeah I actually still am not 100% sure what push is so here's more examples: 
  
*
