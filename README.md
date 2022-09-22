# autodocs.jl 

Provides the `@autodocs` macro, which, if invoked at the end of a Julia file, will generate basic documentation for all declared Julia objects (functions, structs, macros, etc.) up until that point. If an object already has documentation, it will be expanded (but not overwritten).

This allows lazy developers to raise their documentation level to the bare minimum, while more motivated developers can skip writing all the boilerplate  and stick to the actual description of functions. 

# Features
+ output can be used with `Documenter.jl`
+ documentation style can be configured
+ hand-written documentation 

# Usage

```julia
# undocumented_file.jl

module UndocumentedModule 

  struct UndocumentedStruct
    _private_field::Any
    public_field
  end
  export UndocumentedStruct

  function undocumented_function(x::T) where {T <: Number}
    # ...
  end
  export undocumented_function
  
  # do foo
  function partially_documented_function(x; kwarg_01 = 1234)
    # ...
  end
  #no export
  
  const global_variable = # ...
end

autodocs.@autodoc "documented_file.jl"
```
Generates:

```julia
# documented_file.jl

"""
`UndocumentedModule` (module)

# Exports

## Structs
+ `UndocumentedStruct`

## Functions
+ `undocumented_function`

## Variables
+ `global_variable::Any`
"""
module UndocumentedModule

  """
  `UndocumentedStruct` (immutable)
  
  ## Public Members
  + `public_field::Any`
  
  ## Constructors
  + `UndocumentedStruct(_private_field::Any, public_field::Any)`
  """
  struct UndocumentedStruct
    _private_field::Any
    public_field
  end
  export undocumented_struct

  """
  `undocumented_function(x <: Number) -> Any`
  """
  function undocumented_function(x::T) where {T <: Number}
    # ...
  end
  export undocumented_function
  
  """
  `partially_documented_function(x::Any, [kwarg_01 = 1234]) -> Any`
  
  ## Brief
  do foo
  """
  function partially_documented_function(x; kwarg_01 = 1234)
    # ...
  end
  
  """
  `global_variable::Any` (const)
  """
end
```
