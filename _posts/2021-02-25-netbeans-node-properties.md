---
title:  "Netbeans Platform - Node properties"
categories: [Netbeans, Node]
tags: [java, Netbeans, Node]
classes: wide
---

# Netbeans Platform Node Properties

The normal way to construct properties for a node lies in the usage of `PropertySupport` or its tool classes. So to construct a read write property you have to do something like:

```java
new PropertySupport.ReadWrite<String>(name, clazz, displayName, shortDescription) {
    @Override
    public String getValue() {
        return the_property_value;
    }

    @Override
    public void setValue(String val) {
        the_property_value = val;
    }
};
```

Analog there is a `PropertySupport.ReadOnly`class and `PropertySupport.Reflection`. The latter is cool if you want to access variables via its name or its getter names. However, if you want to change its name, then you have to call the instance with the specific method. 

All of your properties have to be returned via a `PropertySet`. 

So you see you always have to overwrite two methods and this is always the same boiler plate code. So why not use the possibility of lambda expressions to reduce all this ceremony.

My solution for this is this little tool method, that constructs a property using internally this `PropertySupport.ReadWrite` class and filling this getter and setter for the value using lambda expressions. 

```java
public static <T> PropertySupport.ReadWrite<T> property(String name, Class<T> clazz, String displayName, String shortDescription, Supplier<T> get, Consumer<T> set) {
    return new PropertySupport.ReadWrite<T>(name, clazz, displayName, shortDescription) {
        @Override
        public T getValue() {
            return get.get();
        }

        @Override
        public void setValue(T val) {
            set.accept(val);
        }
    };
}
```

So now the creation of a property is done via

```java
property("myproperty", String.class, "My property", "edit my property", 
    () -> myvalue.getValue(), 
    value -> myvalue.setValue(value)));
```

IMHO this is much more consice than the original version.

I had a little surprise while testing those property constructions. Netbeans platform comes with some standard editors for basic classes. And Netbeans is able to edit an array of elements and reorder the list. So this one indeed works 

```java
property("myproperty_array", String[].class, "My property array", "edit my property array", 
    () -> mylist.toArray(new String[mylist.size()]), 
    values -> {mylist.removeAll(); mylist.addAll(values)}  );
```

