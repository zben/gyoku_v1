# GyokuV1

GyokuV1 translates Ruby Hashes to XML.

``` ruby
GyokuV1.xml(:find_user => { :id => 123, "v1:Key" => "api" })
# => "<findUser><id>123</id><v1:Key>api</v1:Key></findUser>"
```

[![Build Status](https://secure.travis-ci.org/savonrb/gyoku_v1.png?branch=master)](http://travis-ci.org/savonrb/gyoku_v1)
[![Gem Version](https://badge.fury.io/rb/gyoku_v1.png)](http://badge.fury.io/rb/gyoku_v1)
[![Code Climate](https://codeclimate.com/github/savonrb/gyoku_v1.png)](https://codeclimate.com/github/savonrb/gyoku_v1)
[![Coverage Status](https://coveralls.io/repos/savonrb/gyoku_v1/badge.png?branch=master)](https://coveralls.io/r/savonrb/gyoku_v1)


## Installation

GyokuV1 is available through [Rubygems](http://rubygems.org/gems/gyoku_v1) and can be installed via:

``` bash
$ gem install gyoku_v1
```

or add it to your Gemfile like this:

``` ruby
gem 'gyoku_v1', '~> 1.0'
```


## Hash keys

Hash key Symbols are converted to lowerCamelCase Strings.

``` ruby
GyokuV1.xml(:lower_camel_case => "key")
# => "<lowerCamelCase>key</lowerCamelCase>"
```

You can change the default conversion formula to `:camelcase`, `:upcase` or `:none`.  
Note that options are passed as a second Hash to the `.xml` method.

``` ruby
GyokuV1.xml({ :camel_case => "key" }, { :key_converter => :camelcase })
# => "<CamelCase>key</CamelCase>"
```

Hash key Strings are not converted and may contain namespaces.

``` ruby
GyokuV1.xml("XML" => "key")
# => "<XML>key</XML>"
```


## Hash values

* DateTime objects are converted to xs:dateTime Strings
* Objects responding to :to_datetime (except Strings) are converted to xs:dateTime Strings
* TrueClass and FalseClass objects are converted to "true" and "false" Strings
* NilClass objects are converted to xsi:nil tags
* These conventions are also applied to the return value of objects responding to :call
* All other objects are converted to Strings using :to_s


## Special characters

GyokuV1 escapes special characters unless the Hash key ends with an exclamation mark.

``` ruby
GyokuV1.xml(:escaped => "<tag />", :not_escaped! => "<tag />")
# => "<escaped>&lt;tag /&gt;</escaped><notEscaped><tag /></notEscaped>"
```


## Self-closing tags

Hash Keys ending with a forward slash create self-closing tags.

``` ruby
GyokuV1.xml(:"self_closing/" => "", "selfClosing/" => nil)
# => "<selfClosing/><selfClosing/>"
```


## Sort XML tags

In case you need the XML tags to be in a specific order, you can specify the order  
through an additional Array stored under the `:order!` key.

``` ruby
GyokuV1.xml(:name => "Eve", :id => 1, :order! => [:id, :name])
# => "<id>1</id><name>Eve</name>"
```


## XML attributes

Adding XML attributes is rather ugly, but it can be done by specifying an additional  
Hash stored under the`:attributes!` key.

``` ruby
GyokuV1.xml(:person => "Eve", :attributes! => { :person => { :id => 1 } })
# => "<person id=\"1\">Eve</person>"
```

## Explicit XML Attributes

In addition to using the `:attributes!` key, you may also specify attributes through keys beginning with an "@" sign.
Since you'll need to set the attribute within the hash containing the node's contents, a `:content!` key can be used
to explicity set the content of the node. The `:content!` value may be a String, Hash, or Array.

This is particularly useful for self-closing tags.

**Using :attributes!**

``` ruby
GyokuV1.xml(
  "foo/" => "", 
  :attributes! => {
    "foo/" => {
      "bar" => "1", 
      "biz" => "2", 
      "baz" => "3"
    }
  }
)
# => "<foo baz=\"3\" bar=\"1\" biz=\"2\"/>"
```

**Using "@" keys and ":content!"**

``` ruby
GyokuV1.xml(
  "foo/" => {
    :@bar => "1",
    :@biz => "2",
    :@baz => "3",
    :content! => ""
  })
# => "<foo baz=\"3\" bar=\"1\" biz=\"2\"/>"
```

**Example using "@" to get Array of parent tags each with @attributes & :content!**

``` ruby
GyokuV1.xml(
  "foo" => [
    {:@name => "bar", :content! => 'gyoku_v1'}
    {:@name => "baz", :@some => "attr", :content! => 'rocks!'}
  ])
# => "<foo name=\"bar\">gyoku_v1</foo><foo name=\"baz\" some=\"attr\">rocks!</foo>"
```

Naturally, it would ignore :content! if tag is self-closing:

``` ruby
GyokuV1.xml(
  "foo/" => [
    {:@name => "bar", :content! => 'gyoku_v1'}
    {:@name => "baz", :@some => "attr", :content! => 'rocks!'}
  ])
# => "<foo name=\"bar\"/><foo name=\"baz\" some=\"attr\"/>"
```

This seems a bit more explicit with the attributes rather than having to maintain a hash of attributes.

For backward compatibility, `:attributes!` will still work. However, "@" keys will override `:attributes!` keys
if there is a conflict.

``` ruby
GyokuV1.xml(:person => {:content! => "Adam", :@id! => 0})
# => "<person id=\"0\">Adam</person>"
```

**Example with ":content!", :attributes! and "@" keys**

``` ruby
GyokuV1.xml({ 
  :subtitle => { 
    :@lang => "en", 
    :content! => "It's Godzilla!" 
  }, 
  :attributes! => { :subtitle => { "lang" => "jp" } } 
}
# => "<subtitle lang=\"en\">It's Godzilla!</subtitle>"
```

The example above shows an example of how you can use all three at the same time. 

Notice that we have the attribute "lang" defined twice.
The `@lang` value takes precedence over the `:attribute![:subtitle]["lang"]` value.
