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
  
  def some_list options
    return to_enum(__callee__, options) unless block_given?
    
    # some code
  end

end


```

after 

```ruby

class User

  attr_accessor :first_name, :last_name
  
  # value will be calculated once and cached until 
  # first_name and last_name change
  depends_on :first_name, :last_name
  def full_name
    "#{first_name} #{last_name}"
  end
  
  # cache will be stored for each combination of x, y
  # but the total number of cached values will be 
  # no more than 5
  cache_with_all_arguments(keep: 5)
  def expensive_calculation(x, y)
    # slow code
  end
  
  # calculate the value once and cache it for later use
  cache
  def memoize
    # slow code
  end
  
  method_enum
  def some_list options
    # some code
  end

end

```

### other

https://github.com/adamcooke/memist
