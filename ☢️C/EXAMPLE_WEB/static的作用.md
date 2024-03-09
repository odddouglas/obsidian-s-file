在C语言中，`static` 也有不同的用途和含义，与在Java等面向对象编程语言中略有不同。以下是C语言中`static`的常见用法：

1. **静态变量 (Static Variables)**:
   - 在C语言中，`static` 可以用于创建静态变量。与普通的局部变量不同，静态变量在函数调用之间保持其值。它们在程序的整个生命周期内存在，并且只初始化一次。
   - 示例：
     ```c
     void myFunction() {
         static int count = 0; // 静态变量
         count++;
         printf("Count: %d\n", count);
     }
     ```

2. **静态函数 (Static Functions)**:
   - 使用`static`关键字可以将函数声明为静态函数。静态函数的作用域仅限于当前源文件，不能被其他源文件中的函数访问。
   - 示例：
     ```c
     static int add(int a, int b) {
         return a + b;
     }
     ```

3. **静态全局变量 (Static Global Variables)**:
   - 在全局变量前加上`static`关键字会将其限定在当前源文件中，其他源文件无法访问。
   - 示例：
     ```c
     static int globalVar = 42; // 静态全局变量
     ```

4. **静态数组 (Static Arrays)**:
   - 静态数组是在栈上分配的，其大小在编译时确定，不会动态分配内存。它们的生命周期与程序相同。
   - 示例：
     ```c
     static int myArray[10]; // 静态数组
     ```

总之，C语言中的`static`关键字通常用于限定变量、函数和数组的作用域或寿命。它可以用于创建静态变量以保留其值，或用于将函数或全局变量限定在当前源文件中，以避免与其他源文件中的符号冲突。这有助于模块化和隐藏实现细节。