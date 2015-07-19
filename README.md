# model

```ruby

class User

  key :id, type: Integer
  
  key :first_name
  key :last_name
  
  key :full_name, depends_on: [:first_name, :last_name] do
    
  end

end

```
