<a name="additionalInformation"></a>
#Aditional Information

<a name="migrationTypes"></a>
##Data Types for Migration
The following concepts 
 - String (255 characters)
 - Text (A lot more then 255 characters)
 - Interger (2,4,8 bit give you different sizes)
 - Float (number with decimal)
 - Boolean (true/false)
 - Serialize(Hash/Array)

These are some of many but very common varibable types you can declare in your migration.
``` ruby
t.string :name
t.text :description
t.integer :age
t.float :height
t.boolean :awsome #always true
#database actually can't store hash or array so what we do is translate them to string to store it. 
t.text :myHash #to serialize, add "serialize :myHash, Hash" to the model
t.text :myArray #to serialize, add "serialize :myHash, Array" to the model
```

<a name="modelAssociations"></a>
##Types of Associations for Models
The following is a list of association which you will all use in the future.
 - belongs_to
 - has_one
 - has_one, through:
 - has_many
 - has_many, through:
 - has_and_belongs_to_many
 - Polymorphic
 - Self

The common ones we would be using are ```belongs_to``` and ```has_many```. Although ```has_many, through:``` is also very common we don't need it for now.

<a name="databaseMethods"></a>
##Methods for Using the Database
The following is a list of common methods for using the database for your reference.
``` ruby
#all - get all entries
  User.all
#first/last - get first/last entry
  User.first
  User.last
#count - counts how many entries there is
  User.count
#order - order entries by time created at ascending order
  User.order(created_at: :asc)
#limit - get the first 10 entry
  User.order(created_at: :asc).limit(10)
#offset - get the 21 to 30 entry
  User.order(created_at: :asc).limit(10).offset(20)
#find(n) - get entry with ID = 1. Only works with id!!!!
  User.find(1)
#find_by - get entry with specific params. works with any params!!!!
  User.find_by(first_name: ‘denis’, last_name: ‘cheung’)
#where - get entries with specific params
  User.where(age: 40)[0][:email]
#distinct/uniq - get all uniq non-duplicate entries
  User.distinct
#pluck - returns and array of target params from entries
  {id: 1, religion: "Buddist"}
  {id: 2, religion: "Christianity"}
  User.pluck(:religion) #returns ["Buddist", "Christianity"]
```