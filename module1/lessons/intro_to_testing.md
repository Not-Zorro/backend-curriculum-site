---
layout: page
title: Intro to Testing
length: 120
tags: ruby, testing
---

## Learning Goals

* Understand why we use tests
* Define the stages of a test
* Define a minitest test
* Use a variety of assertion methods

## Vocabulary

* Gem
* Test
* Assertion

## Warm Up

* Thinking back to your capstone work; how did you know if your program was working?
* What are some potential drawbacks to this approach?

## Test Etiquette

### File Structure

- Test files live in their own `test` directory
- Implementation code files live in a sibling `lib` directory
- Test files should reflect the class they're testing with `_test` appended to the file name, e.g. `test/name_of_class_test.rb`
- In your test, you'll now `require "./lib/name_of_class.rb"`
- Run your test files from the root of the project directory, e.g. `ruby test/name_of_class_test.rb`; running the test will invoke your program

```
.
├── lib
|   └── name_of_class.rb
└── test
    └── name_of_class_test.rb
```

### minitest Setup

[Minitest](http://docs.seattlerb.org/minitest/) is a framework used for automated testing. It is the testing framework used on many of the homework exercises you've been assigned.

```
gem install minitest
```

* Require `minitest/autorun` - the easy and explicit way to run all your tests
* Require `minitest/pride` - vivid color explosion

### minitest Convention

- Test Class Name: `class NameOfClassTest`
* Test class inherits from `Minitest::Test`, e.g. `class NameOfClassTest < Minitest::Test`
  * `test` is a `minitest` module; `::` is a scope resolution operator
  * minitest/test is a small and incredibly fast unit testing framework. It provides a rich set of assertions to make your tests clean and readable.
- `def test_something` for names of methods in test file -- **MUST start with `test_`**
- It's good practice to reference your method in the test name `test_method_name_does_what_I_want_it_to`
* assert_equal starts with the **assertion method**, followed by the **expected** value, followed by the **actual** value

```ruby
assert_equal 'expected', 'actual'
```

## Code-Along

### Scenario Specifications

```ruby
pry(main)> require './lib/student'
=> true

pry(main)> student = Student.new('Penelope')
=> #<Student:0x007fa71e12c1f0 @cookies=[], @name="Penelope">

pry(main)> student.name
=> "Penelope"

pry(main)> student.cookies
=> []

pry(main)> student.add_cookie('Chocolate Chunk')
pry(main)> student.add_cookie('Snickerdoodle')

pry(main)> student.cookies
=> ["Chocolate Chunk", "Snickerdoodle"]
```

Now, let's write tests based on the interaction pattern above.

```ruby
# student_test.rb
require 'minitest/autorun'
require 'minitest/pride'

class StudentTest < Minitest::Test
  # test it exists
  # test it has a name
  # test it has cookies
  # test it can add cookies
end
```

Let's build out our Student Test!

```ruby
# student_test.rb
require 'minitest'
require 'minitest/autorun'
require 'minitest/pride'

class StudentTest < Minitest::Test
  def test_it_exists
    student = Student.new('Penelope')
    assert_instance_of Student, student
  end
  # test it has a name
  # test it has cookies
  # test it can add cookies
end
```

* Write tests
* Run tests
* Thoroughly read errors & failures
* Write implementation code

```ruby
class Student

end
```

* Do it all again

```ruby
# student_test.rb
require 'minitest'
require 'minitest/autorun'
require 'minitest/pride'

class StudentTest < Minitest::Test
  def test_it_exists
    student = Student.new("Penelope")
    assert_instance_of Student, student
  end

  def test_student_has_a_name
    student = Student.new("Penelope")
    assert_equal "Penelope", student.name
  end
  # test it has cookies
  # test it can add cookies
end
```

```ruby
class Student
  def initialize(name)
    @name = name
  end

  def name
    @name
  end
end
```

## S.E.A.T

Each test that we create needs 4 components to be a properly built test.

* Setup - The setup of a test is all of the lines of code that need to be executed in order to verify some behavior. Because each test is run individually, we often see the same setup being created multiple times.
* Execution - The execution is the actual running of the method we are testing.  This sometimes happens on the same line as the assertion, and sometimes happens prior to the assertion.
* Assertion - The verification of the behavior we are expecting.  This is really the main focus of the test; without the assertion, we have no test.
* Teardown - After we complete a test, we want to delete all of our setup, and clear the scope for our next test.  In minitest this is done automatically! So, you won't need to worry about this in Mod1, but it is important to know for other testing frameworks.

With a partner, see if you can identify each of the components in the following tests:

```ruby
# student_test.rb
require 'minitest'
require 'minitest/autorun'
require 'minitest/pride'
require './lib/student'

class StudentTest < Minitest::Test
  def test_it_exists
    student = Student.new("Penelope")

    assert_instance_of Student, student
  end

  def test_student_has_a_name
    student = Student.new("Penelope")

    assert_equal "Penelope", student.name
  end

  def test_it_has_cookies
    student = Student.new("Penelope")

    assert_equal [], student.cookies
  end

  def test_it_can_add_cookies
    student = Student.new("Penelope")

    student.add_cookies('Chocolate Chunk')
    student.add_cookies('Snickerdoodle')

    assert_equal ['Chocolate Chunk', 'Snickerdoodle'], student.cookies
  end
end
```

# Testing Cont. Warmup
What do you think the following assertion methods do?

- `assert_instance_of`
- `assert_equal`
- `assert`
- `assert_nil`
- `refute`
- `refute_equal`

## Additional Test Intricacies
- Tests will overwrite previous tests with the same name; **give each test a new name**
- Each test is independent of the next; **don't depend on tests to run in order** of how they're written
  - However, it clarifies your code to other humans to write in order of complexity; aim to start from most basic to most complex functionality and keep tests grouped by method
- You can create a setup method
- Tests will generally return an `E` for error, `F` for failure & `.` for passing

```ruby
class StudentTest < Minitest::Test
  attr_reader :student

  def setup
    @student = Student.new
  end
  ...
end
```

### Ensuring Dynamic Functionality

We should make sure that all of our methods can handle different cases, ensuring that our implementation code is dynamic, e.g.:

```ruby
# student_test.rb
require 'minitest'
require 'minitest/autorun'
require 'minitest/pride'

class StudentTest < Minitest::Test
  def test_it_exists
    student = Student.new
    assert_instance_of Student, student
  end

  def test_student_has_a_name
    student = Student.new("Penelope")
    assert_equal "Penelope", student.name
  end

  def test_student_can_have_a_different_name
    student = Student.new("Hermione")
    assert_equal "Hermione", student.name
  end
  # test it has cookies
  # test it can add cookies
end
```

### Testing Edge Cases

* Ensure that your implementation code can handle things we might not expect, e.g.:

```ruby
# student_test.rb
require 'minitest'
require 'minitest/autorun'
require 'minitest/pride'

class StudentTest < Minitest::Test
  def test_it_exists
    student = Student.new
    assert_instance_of Student, student
  end

  def test_student_has_a_name
    student = Student.new("Penelope")
    assert_equal "Penelope", student.name
  end

  def test_student_can_have_a_different_name
    student = Student.new("Hermione")
    assert_equal "Hermione", student.name
  end

  def test_student_cant_be_created_with_integer_name
    student = Student.new(13)
    assert_equal "Name not Provided", student.name
  end
  # test it has cookies
  # test it can add cookies
end
```

## Recap

* What 2 directories should we have within our project directory?
* `minitest` setup
  * What do you have to require in a test file?
  * What does your test class inherit from?
  * What is the syntax for a minitest test? What's the best name for a test?
  * Do tests need unique names? Should they be written in a particular order? Do they necessarily run in that order?
* Name 3 assertion methods you learned about today & describe their syntax.

## Resources
* Explore the minitest gem! [https://github.com/seattlerb/minitest](https://github.com/seattlerb/minitest)
