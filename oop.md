<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Object oriented program</h1>
</div>

## Interface vs Abstract class
    _todo_
## 4 Tính chất của OOP

- ### Tính đóng gói (Encapsulation)

  Là cách để che dấu những tính chất xử lý bên trong của đối tượng, những đối tượng khác `không thể` tác động trực tiếp làm `thay đổi trạng thái` chỉ có thể tác động thông qua các method public của đối tượng đó.

- ### Tính trừu tượng(Abstraction)

  Tính trừu tượng giống như một phiên bản mở rộng của tính đóng gói vì nó `giấu đi những tính chất và phương thức` cụ thể để giao thức của các đối tượng `đơn giản hơn`.

- ### Tính đa hình (Polymorphism)

  Là hai đối tượng cùng kế thừa một lớp cha có thể thực hiện một phương thức theo cách khác nhau.

  ```js
  class Animal {
    speak() {}
  }

  class Dog extends Animal {
    speak() {
      console.log("Gau Gau");
    }
  }

  class Dog extends Animal {
    speak() {
      console.log("Meo Meo");
    }
  }

  [new Dog(), new Cat()].forEach((e) => {
    e.speck();
  });
  // 'Gau Gau' 'Meo Meo'
  ```

  Trong code để thể hiện tính đa hình có 2 cách:

  - Method Overloading (compile time polymorphism)
  - Method Overriding (run time polymorphism)

- ### Tính kế thừa (Inheritance)
  Là kỹ thuật cho phép kế thừa lại những tính năng mà một đối tượng khác đã có, giúp tránh việc code lặp dư thừa mà chỉ xử lý công việc tương tự.
