## The name of the methods in a builder

``` java
import com.google.auto.value.AutoValue;

@AutoValue
abstract class Animal {
  abstract String name();
  abstract int number_legs();

  static Builder builder() {
    return new AutoValue_Animal.Builder();
  }

  @AutoValue.Builder
  abstract static class Builder {
    abstract Builder setName(String value);

    // If you use 'set' as a prefix,
    // the first letter must be Capital, and the name of the method must be same.
    abstract Builder setNumber_legs(int value); 

    abstract Animal build();
  }
}
```
