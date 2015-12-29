```
module A
  mount B
end
```
A -> lib

B -> app/contexts

当调用 B.instance_method时，raise A::B.new().instance_method error  
::B.new().instance_method
