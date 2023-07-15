# Equals & HashCode



Equality - objects of the same class with equal fields

1. _reflexive_: an object must equal itself
2. _symmetric_: _**x.equals(y)**_** must return the same result as **_**y.equals(x)**_
3. _transitive_: if _x.equals(y)_ and _y.equals(z)_ then also _x.equals(z)_
4. _consistent_: the value of _equals()_ should change only if a property that is contained in _equals()_ changes (no randomness allowed)

\
Thus **if we override the **_**equals()**_ method, we also have to override _hashCode()_.hashCode - number of a certain length.

1. _Internal consistency_: the value of _hashCode()_ may only change if a property that is in _equals()_ changes
2. _equals consistency_: **objects that are equal to each other must return the same hashCode**
3. _collisions_: **unequal objects may have the same hashCode**

```
@Override 
public boolean equals(Object obj) { 
    if (obj == null) return false; 
    if (!(obj instanceof MyClass)) 
        return false; 
    if (obj == this) 
        return true; 
    return this.getId() == ((MyClass) obj).getId(); 
}

@Override
public int hashCode() {
    final int prime = 31;
    int result = 1;
    result = prime * result + varA;
    result = prime * result + varB;
    return result;
}
```
