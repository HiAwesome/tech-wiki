# Groovy

### [How do I construct a (non-static) Java inner class from Groovy](https://stackoverflow.com/a/27980914/7379661)

```groovy
def a = new A()
A.B.newInstance(a, "foo")
```
