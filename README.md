# model

```ruby

class User

  attr_accessor :first_name, :last_name
  
  def full_name
    "#{first_name} #{last_name}"
  end.depends_on(:first_name, :last_name)
  
  def expensive_calculation(x, y)
    # some code
  end.cache

end

```
