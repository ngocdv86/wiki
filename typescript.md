<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Typescript</h1>
</div>

## Concept comparison.

## Some notes

- Interface & Abstract Class can only be used in TypeScript files
- Interface method modifier

  Since Interface is schema that anyone can use to access some class features, its fields can not be `private` or `protected`

  ```js
  interface Service {
    private getHello(); // [x] Wrong
  }
  ```

- `private` modifier cannot be used with `abstract` modifier in Abstract Class.

  ```js
  abstract class AbstractClass {
    private abstract getHello(); // [x] Wrong
  }
  ```

  <p align="right">(<a href="#top">Back to top</a>)</p>
