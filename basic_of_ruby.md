# Ruby Basics
- http://learnrubythehardway.org/book/intro.html
- Go through tryruby.org quickly
#### Calling methods and properties is the same for Ruby. Different from Javascript
```ruby
"Harry".reverse
```
#### convert to string
```ruby
40.to_s
# .to_i
# .to_d
# .to_f
```
### Numeric Types
- http://www.postgresql.org/docs/9.1/static/datatype-numeric.html
#### variables and object methods
```ruby
grades = [90, 47, 97].max
```
#### does not change the original variable
```ruby
grades.sort
```
#### change the original variable
```ruby
grades.sort!
```
```
poem.lines.to_a.reverse.join
```

#### Commenting
``` ruby
# use '#' in the beginning to comment
```

#### String manipulations
http://ruby-doc.org/core-2.1.5/String.html
```ruby
.include?
.reverse
.downcase
.upcase
.delete
.gsub
.index
```

#### String and numbers
```ruby
puts "What is 3 + 2? #{3 + 2}"
```

#### hash
```ruby
fruits = {}
```
```ruby
fruits['apple'] = :great
fruits.keys
fruits.values
```
```ruby
credits = Hash.new(0)

# {} is a shorthand for Hash.new
```

#### Array
- Array methods: http://www.ruby-doc.org/core-2.1.5/Array.html
```ruby
new_array = []
new_array << 44

# looping through an array
```

#### symbols
```ruby
:splendid
```

#### for loop
```ruby
fruits.values.each do |rate|
  ratings[rate] += 1

5.times { print "Hello!" }

for i in 0..5
  # do something
end

(0..5).each do |i|
  # do something
end

# 'break' terminate the most internal loop
for i in 0..5
   if i > 2 then
      break
   end
   puts "Value of local variable is #{i}"
end

# 'next' iteration
for i in 0..5
   if i < 2 then
      next
   end
   puts "Value of local variable is #{i}"
end
```

#### while loop
```ruby
while conditional do
  # do something

begin
  # do something
end while conditional

code until conditional
begin
  # code
end until conditional

unless conditional
  # do something
end
```

#### if statements
```ruby
batman = "awesome"
if batman == "awesome"
  puts "great"
else
  puts "not great"
end

code if conditional

if conditional1 && conditional2
end
```

#### Assignment
``` ruby
grade = nil || 100
grade = 99 || nil
grade = 99 || 100

messages = facebook.messages || [] # if nil, default to empty array

batman = if saved_world
  "awesome"
else
  "not awesome"
end
```

#### rescue
```ruby
begin
  # do something
rescue
  # handles error
  retry # restart from beginning
end
```

#### Case statement
```ruby
formula = case "addition"
  when "addition"
    # do something
  when "subtraction"
    # do something
  else
    # do something
end
```

#### functions
```ruby
def functionName(args)
  "okay"
end

# return the last value by default

def messages(sender, location)
end

def messages(sender, location = "Hong Kong")
end

# pass options as a hash argument
def messages(sender, location, options = {})
  importance = options[:importance]
end

messages("Harry", "HK",
 :importance => "high"
 )
```

#### Class
```ruby
class BlogEntry
  attr_accessor :title
end

entry = BlogEntry.new
entry.title = "Hello"
```
```ruby
class BlogEntry
  def initialize( title, mood, fulltext )
    @time = Time.now
    @title, @mood, @fulltext = title, mood, fulltext
  end
end
```

```ruby
Time.now - 2.weeks
```

```ruby
class Name
 def initialize(first, last = nil)
 @first = first
 @last = last
 end
 def full_name
 [@last, @first].compact.join(', ')
 end
end

name = Name.new('Victor', 'Lin')
name.full_name
```

```ruby
class Tweet
 attr_accessor :status
:created_at
 def initialize(status)
 @status = status
 @created_at = Time.new
 end
end
```

```ruby
tweet = Tweet.new("Eating breakfast.")
tweet.created_at =
Time.new(2084, 1, 1, 0, 0, 0, "-07:00")
```

#### self
```ruby
class UserList
 attr_accessor :name
 def initialize(name)
 self.name = name
 end
end
```

#### OOP
- http://zetcode.com/lang/rubytutorial/oop/
- http://zetcode.com/lang/rubytutorial/oop2/

#### Inheritance
```ruby
class Animal
  attr_accessor :name, :size, :url
  def to_s
    "#{@name}, #{@size}"
  end
end

class Dog < Animal
end

class Cat < Animal
  def meow
    puts "Cat can meow"
  end
end

# DRY - Don't Repeat Yourself

Dog.ancestors
```

#### Super

#### Overwriting methods

#### Modules  / Mixins

##### Prompting
```ruby
temp = gets.chomp
```