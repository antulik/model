# model

Smart caching of ruby methods based on other properties.

Inspired by [Ember.js Computed Properties](http://guides.emberjs.com/v1.10.0/object-model/computed-properties/)

before

```ruby
class User

  attr_accessor :first_name, :last_name
  
  def full_name
    keys = [first_name, last_name]
    if @full_name_cache_args == keys
      @full_name
    else
      @full_name_cache_args = keys
      @full_name = "#{first_name} #{last_name}"
    end
  end
  
  def expensive_calculation(x, y)
    keys = [x, y]
    if @expensive_calculation_cache_args == keys
      @expensive_calculation
    else
      @expensive_calculation_cache_args = keys
      @expensive_calculation = begin
        # slow code
      end
    end
  end
  
  def memoize
    @memoize ||= begin
      # slow code
    end
  end

end


```

after 

```ruby

class User

  attr_accessor :first_name, :last_name
  
  def full_name
    "#{first_name} #{last_name}"
  end.depends_on(:first_name, :last_name)
  
  def expensive_calculation(x, y)
    # slow code
  end.cache
  
  def memoize
    # slow code
  end.cache

end

```
