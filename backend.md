<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Backend</h1>
</div>

## Concept comparison.

### 1. Stateful & Stateless.

- **Stateless** means there is no memory of the past. Every transaction is performed as if it were being done for the very first time.

  ```js
  // The state is derived by what is passed into the function

  function int addOne(int number)
  {
      return number + 1;
  }
  ```

- **Stateful** means that there is memory of the past. Previous transactions are remembered and may affect the current transaction. Store in `memory`(RAM in server, not database).

  To have to ensure a specific user’s requests always hit the same server, `hard to scale`.

  ```js
  // The state is maintained by the function

  private int _number = 0; //initially zero

  function int addOne()
  {
    _number++;
    return _number;
  }
  ```

  <p align="right">(<a href="#top">Back to top</a>)</p>
