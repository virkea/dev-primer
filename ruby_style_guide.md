# Ruby Style Guide

*Please note that everything that starts with a '(!)' is absolute mandatory*

## General

  - (!) Use english for naming class, methods, variables and everything else
  - (!) Use unix style line endings (double-check if you use windows)
  - (!) Write only **one expression** per line, don't use `;` to separate expression
  - (!) **Avoid side effects or destructive methods** except when **you know what you are doing**
  - No space (before & after) for: `(`, `[`, `]` and `)` except during assignment
  - Use space for `{` and `}` except string interpolation
    ```ruby
      # Space after '{' and before '}' during assignment
      hsh = { jakarta: 1, bandung: 2 }

      # Space after '{' and before '}' when expressing block using '{}'
      [1,2,3].each { |x| puts x } 

      # Interpolation does not need space
      "Virkea #{puts "Empresa"} Sistema"
    ```
  - (!) Avoid more than one line break in a row
  - (!) Unused methods and variables should be deleted, not commented

## Ruby Specific Features

It is recommended to use Ruby-specific feature whenever possible, for examples:

- Keyword Arguments - *available from Ruby 2.0 onward*
  ```ruby
    # Using keyword arguments
    def foo(param1: 'abc', param2: 123)
    end

    # Using old-style hash parameter to achieve the same result
    def foo(opts = {})
    end
  ```
- Literal Array Notation (`%w`, `%i`, etc)
  ```ruby
    str_array = %w(virkea empresa sistema)
    sym_array = %i(virkea empresa sistema)
  ```

## Indentation, Whitespace and Line Columns

  - (!) Use **2 spaces** for indentation not tab character
  - (!) Break long expression with indentation
  - (!) Block must not be called from long method chain. Should extract into local variable first
  ```ruby
  # Bad
  vendor.
    party.
    party_pics.is_primary_sorted.first.
    pic.
    party_contacts.each do |elem|
    
    # process
  end

  # Good
  party_contacts = vendor.
    party.
    party_pics.is_primary_sorted.first.
    pic.
    party_contacts
    
  party_contacts.each do |elem|
    # process
  end
  ```
  - (!) Do not put whitespace at EOL
  - Do put line break at EOF
  - (!) Do not exceed 128 character per line
  - It is recommended to keep every line length below 80 char

## Number

  - Separate long number with underscore
    ```ruby
    e.g. 1_000_000 instead of 1000000 # yes ruby will understand
    ```

## String

  - Remember that
    - `""` will be interpolated
    - `''` will not be interpolated
  - Use `''` for empty string
  - Use string interpolation instead of `concat` or `+` (remember to use `""` if you want to activate string interpolation)

## Array

  - Use `[]` to define empty array instead of `Array.new`
  - Put line break when defining array with many members. Give trailing comma (,) for every members of an array including its last item
  ```ruby
  # Recommended, notice the trailing comma on last array member
  arr = [
    1,
    2,
    3,
  ]
  ```
  - Use literal array notation whenever possible
  ```ruby
  %w(cat dog cow) # preferred
  ['cat', 'dog', 'cow'] # not preferred

  %i(cat dog cow) # preferred
  [:cat, :dog, :cow] # not preferred
  ```

## Hash

  - Use `{}` to define empty hash instead of `Hash.new`
  - Give whitespace on:
    - After `{`
    - After `:` **or** before & after `=>`
    - After `,`
    - Before `}`
    ```ruby
    # Recommended
    { var1: 'test', var2: 'test' }
    { :var1 => 'test', :var2 => 'test'}
    ```
  - Use new syntax when defining hash (*available after ruby 1.9*)
    ```ruby
    { a: 1, b: 2 } # recommended
    { :a => 1, :b => 2 } # not recommended, except if key must be string
    ```
  - Put line break when defining hash with many members. Give trailing comma (,) for every member including the last item
    ```ruby
    # Recommended, notice the trailing comma on last hash member
    hsh = {
      a: 1,
      b: 2,
      c: 3,
    }
    ```
  - Use symbol as key whenever possible (because using symbol is faster)

## Operation

  - Put whitespace around operator, e.g. `a + b` **not** `a+b` (except `**` operator)
  - (!) Do not nest conditional operator (`?`)
  - (!) Do not write conditional operator within multiple lines, use `if..else` instead
  ```ruby
  # Use conditional operator (?) for one-liner & simple condition only
  person.age > 18 ? puts "Adult" : puts "Child"

  # For complex condition, use if..else instead
  if person.domicile.city == 'Jakarta'
    puts "Use same-day delivery"
  elsif person.domicile.city == 'Bekasi'
    puts "Use interstellar spaceship"
  else
    puts "Use intercity carrier"
  end
  ```

## Assignment

  - Put whitespace around assignment operator
  - (!) Do not write assignment operator in conditional clause
    ```ruby
    # Watch out for this, notice that the conditional clause 
    # use `=` (assignment) instead of `==` (equality)
    current_date = Date.current
    if current_date = person.birthdate
      puts "Happy Birthday!"
    end
    ```

## Control Structure

  - Use unless instead of `if !`
  - Use until instead of `while !`
  - (!) Do not use `else` for `unless`
  - (!) Put a line break after conditional & loop (`if`, `unless`, `case`, `while`, `until`)
  - if condition is long, extract into a predicator method

## Method Calls
  
  - Do not omit `()` except
    - Calling method without argument
    - Global methods, e.g. `p`, `puts`, `print`
    - Declarative method provided by rails, e.g. `private`, `attr_*`
    - Methods provided by ActionController, e.g. `respond_to`
    - Other DSL-like methods
  - Do not put space between method name and `()`
  - Should omit `{}` when `{}` is the last argument
  - When calling block of method calls
    - Use `do..end` if return value is not used
    - Use `{}` if return value is used or the block method call is one-liner
  - Use space appropriately
    ```ruby
    # good
    [1, 2, 3].each { |x| puts x }
    ```
  - Put line break when calling method with many args. Don't forget to put `,`
    ```ruby
    Foo.new(
      a,
      b,
      c,
    )
    ```

## Module & Class Definition

  - Use `attr_accessor`, `attr_reader`, `attr_writer` appropriately
  - `private`, `protected`, `public` usage
    ```ruby
    # example 1 : passing symbol to private method
    def foo
    end
    private :foo # no extra line break with method definition

    # example 2: passing block to private method
    def bar
    end

    private # notice the extra indentation below
      def foo
      end
    ```

## Comment

  - Use Markdown & YARD format
  - Don't put empty line between comment & method definition
  - Header comment is important and should be written properly

## Method Definition

  - (!) Must use `()` except when there are no parameters to be defined
  - (!) Don't put whitespace between method name and parameter definition
  - Do not write complex or hard to understand method without any comment. Even better, you can break it into multiple methods (*Single Responsibility Pricinple*)
  - Do not call destructive method towards supplied arguments (e.g. bang method), because it can cause side-effect

  ```ruby
  # Don't do this
  def destructive_method(array_param)
    array_param.sort!
  end
  ```

## Variable

  - (!) Global variable is not allowed, ever.
  - (!) Class variable is also not allowed, if you have to, you can use `class.attribute` instead.
  - Use english as variable name. Don't shorten, but if you have to then use common abbreviation / remove vowel
  - Don't use single variable for multiple purpose
  - Reduce the usage of local variable

## Be Consistent

> If you're editing code, take a few minutes to look at the code around you and determine its style. If they use spaces around all their arithmetic operators, you should too. If their comments have little boxes of hash marks around them, make your comments have little boxes of hash marks around them too.

> The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you're saying rather than on how you're saying it. We present global style rules here so people know the vocabulary, but local style is also important. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.

-- *Google C++ Style Guide*