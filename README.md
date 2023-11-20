# React Design Patterns

As a software engineer, you may not have encountered the term "Design Patterns," but rest assured, you are likely already applying similar principles in your software development work. Design patterns can be thought of as clever approaches to assembling blocks of code in order to effectively address recurring problems in programming. They provide efficient and proven solutions to common challenges, making the process of building software more streamlined and effective.

Design patterns are typically categorized into three main groups: creational, structural, and behavioral patterns. But since this is related to React, I will add in an additional group as UI patterns.

# Creation Pattern

### Singleton Pattern
Ensures that a class has only one instance and provides a global point of access to that instance. This is useful when exactly one object is needed to coordinate actions across the system.

Note: This class is not reactive to the rest of the App in React. If you want data change to trigger UI update, you can use `createContext` and `provider` to accomplish Singleton.

```javascript
class Singleton {
  constructor() {
    // Mark this instance as the singleton instance
    Singleton.instance = this;
  }

  // method
  importantMethod() {
    console.log(`important method`);
  }

  // Static method to get the singleton instance
  static getInstance() {
    if (!Singleton.instance) {
      // If no instance exists, create a new one
      Singleton.instance = new Singleton();
    }
    // Return the singleton instance
    return Singleton.instance;
  }
}
```

### Abstract Factory Pattern
Provides an interface for creating families of related or dependent objects without specifying their concrete classes. 

```javascript
const Button = ({ onClick, label }) => (
  <button onClick={onClick}>{label}</button>
);

const Link = ({ href, label }) => (
  <a href={href}>{label}</a>
);

const ComponentFactory = (props) => {
  if(props.href !== null) return <Link {...props} />
  return <Button {...props} />;
};
```

### Builder Pattern
Separates the construction of a complex object from its representation, allowing the same construction process to create different representations. It's useful when an object needs to be constructed with a variety of parts.

```javascript
// Builder class
class ProductBuilder {
  constructor() {
    this.product = new Product();
  }
  setTitle(title) {
    this.product.props.title = title;
    return this;
  }
  setDescription(description) {
    this.product.props.description = description;
    return this;
  }
  setPrice(price) {
    this.product.props.price = price;
    return this;
  }
  build() {
    return this.product;
  }
}

// Client component
const App = () => {
  // Using the builder to construct a product with specific properties
  const product1 = new ProductBuilder()
    .setTitle('Product A')
    .setDescription('Description for Product A')
    .setPrice(19.99)
    .build();
...
```

### Prototype Pattern
Prototype Pattern involves creating new objects by copying an existing object, known as the prototype. It's useful when the cost of creating an object is more expensive or complex than copying an existing one. In React along with JSX, prototype pattern is demonstrated in JSX creation of object in markup.

```javascript
const TimerPrototype = (props) => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount((prevCount) => prevCount + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <p>{props.label}: {count} seconds</p>
    </div>
  );
};

// Clone the prototype to create a new component
const Timer1 = (props) => <TimerPrototype label={props.label} />;
const Timer2 = (props) => <TimerPrototype label={props.label} />;
```

