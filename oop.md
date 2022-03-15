<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Object oriented program</h1>
</div>

## Design Patterns

[22 Design Patterns](https://refactoring.guru/design-patterns/catalog)

## Interface vs Abstract class

- Interface

  Như một bản vẽ, khi mà tất cả những class implement interface đều có những phương thức mà interface này đặt ra. Mục đích **`ràng buộc`** là chính.

  Tất cả các hàm đều là hàm ảo (không có thân chương trình) và public.

- Abstract Class

  Khi có một nhóm các đối tượng có tính chất giống nhau, `Abstract class` đứng ra để xây dụng bộ khung (flow xử lí, hàm chung, hàm mặc định). Từ đó những class con có thể overide hoặc kế thừa lại những phương thức này. Mục đính **`reuse`** là chính.

  Có ít nhất 1 hàm là hàm ảo.

  Khi `Abstract class` có những hàm `public`, thì các hàm con cũng có, điều này cũng thể hiện **`tính ràng buộc`** của `Abstract class.`

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
    e.speak();
  });
  // 'Gau Gau' 'Meo Meo'
  ```

  Trong code để thể hiện tính đa hình có 2 cách:

  - Method Overloading (compile time polymorphism)
  - Method Overriding (run time polymorphism)

- ### Tính kế thừa (Inheritance)

  Là kỹ thuật cho phép kế thừa lại những tính năng mà một đối tượng khác đã có, giúp tránh việc code lặp dư thừa mà chỉ xử lý công việc tương tự.

  <p align="right">(<a href="#top">Back to top</a>)</p>
