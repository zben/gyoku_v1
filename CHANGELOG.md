## 1.2.2 (2014-09-22)

* Fixed a bug introduced by making GyokuV1 threadsafe. Who knew that `$1` and the block variable that `#gsub` provides are not the same?

## 1.2.1 (2014-09-22)

* Fix : [#46](https://github.com/savonrb/gyoku_v1/pull/46) Fixed an issue where GyokuV1 was not threadsafe. GyokuV1 should now be relatively more threadsafe due to less usage of global variables.

## 1.2.0 (2014-09-18)

* Feature: [#44](https://github.com/savonrb/gyoku_v1/pull/44) support for sorting via :order! with a string key

## 1.1.1 (2014-01-02)

* Feature: [#38](https://github.com/savonrb/gyoku_v1/pull/38) support for building nested Arrays
* Feature: [#36](https://github.com/savonrb/gyoku_v1/pull/36) allow setting any objects content with :content!
* Deprecation:  Support for ree and ruby 1.8.7 will be going away soon.

## 1.1.0 (2013-07-26)

* Feature: [#30](https://github.com/savonrb/gyoku_v1/pull/30) support for building Arrays
  of parent tags using @attributes.

* Fix: [#21](https://github.com/savonrb/gyoku_v1/pull/21) stop modifying the original Hash.
  The original issue is [savonrb/savon#410](https://github.com/savonrb/savon/issues/410).

## 1.0.0 (2012-12-17)

* Refactoring: Removed the global configuration. This should really only affect the
  `GyokuV1.convert_symbols_to` shortcut which was removed as well. If you're using GyokuV1
  with Savon 2.0, there's now an option for that. If you're using GyokuV1 on itself,
  you can pass it the `:key_converter` option instead.

## 0.5.0 (2012-12-15)

Feature: [#19](https://github.com/savonrb/gyoku_v1/pull/19) adds support for explicit XML attributes.

Feature: [#17](https://github.com/savonrb/gyoku_v1/pull/17) adds an `:upcase` formula.

## 0.4.6 (2012-06-28)

* Fix: [#16](https://github.com/rubiii/gyoku_v1/issues/16) Date objects were mapped like DateTime objects.

      GyokuV1.xml(date: Date.today)  # => "<date>2012-06-28</date>"

* Fix: Time objects were also mapped like DateTime objects.

      GyokuV1.xml(time: sunday)  # => "<time>16:22:33</time>"

## 0.4.5 (2012-05-28)

* Fix: [issue 8](https://github.com/rubiii/gyoku_v1/issues/8) -
  Conflict between camelcase methods in Rails.

* Fix: [pull request 15](https://github.com/rubiii/gyoku_v1/pull/15) -
  GyokuV1 generates blank attribute values if there are fewer attribute
  values in attributes! than elements.

* Fix: [issue 12](https://github.com/rubiii/gyoku_v1/issues/12) -
  Don't remove special keys from the original Hash.

## 0.4.4

* Fix: [issue 6](https://github.com/rubiii/gyoku_v1/issues/6) -
  `GyokuV1.xml` does not modify the original Hash.

## 0.4.3

* Fix: Make sure `require "date"` when necessary.

## 0.4.2

* Fix: `Array.to_xml` so that the given :namespace is applied to every element
  in an Array.

## 0.4.1

* Fix: Alternative formulas and namespaces.

## 0.4.0

* Feature: Added alternative Symbol conversion formulas. You can choose between
  :lower_camelcase (the default), :camelcase and :none.

      GyokuV1.convert_symbols_to :camelcase

  You can even define your own formula:

      GyokuV1.convert_symbols_to { |key| key.upcase }

## 0.3.1

* Feature: GyokuV1 now calls Proc objects and converts their return value.

## 0.3.0

* Feature: Now when all Hash keys need to be namespaced (like with
  elementFormDefault), you can use options to to trigger this behavior.

      GyokuV1.xml hash,
        :element_form_default => :qualified,
        :namespace => :v2

## 0.2.0

* Feature: Added support for self-closing tags. Hash keys ending with a forward
  slash (regardless of their value) are now converted to self-closing tags.

## 0.1.1

* Fix: Allow people to use new versions of builder.

## 0.1.0

* Initial version. GyokuV1 was born as a core extension inside the
  [Savon](http://rubygems.org/gems/savon) library.
