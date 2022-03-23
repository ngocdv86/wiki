<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Nestjs</h1>
</div>

## Main concepts.

### 1. Life cycle

## Concept comparison.

### 1. **`onModuleInit`** vs **`constructor`**

### 2. **`Constructor-based injection`** vs **`Property-based injection`**

|          |         Constructor-based injection          |                                    Property-based injection                                     |
| :------: | :------------------------------------------: | :---------------------------------------------------------------------------------------------: |
|  Ready   | All dependency ready in `constructor` scope. | Dependency don't ready in `constructor function` scope. Ready in `onModuleInit function` scope. |
| Testable |                      No                      |                                               Yes                                               |
| Use case |         When extend another provider         |                     Should always prefer using constructor-based injection                      |

[Property-based injection](https://docs.nestjs.com/providers#property-based-injection)
[Other view](http://dillonbuchanan.com/programming/dependency-injection-constructor-vs-property)

### 3. Provider scope: **`DEFAULT`** vs **`REQUEST`** vs **`TRANSIENT`**

  <p align="right">(<a href="#top">Back to top</a>)</p>
