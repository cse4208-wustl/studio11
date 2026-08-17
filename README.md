# Studio 11

## Copy and Move Semantics

In this studio, you will explore how copy semantics and move semantics affect class behavior in C++. The exercises contrast value-like objects with pointer-like ownership patterns, and introduce `= default`, deleted copy operations, lvalues, rvalues, references, and `std::move`.

## Collaboration

You may complete this studio individually or in a small group.

## Reference

If you need a refresher on the environment setup steps from the previous studios, see [Studio 0](https://github.com/cse4208-wustl/studio0).

## Exercises

Record your answers in `ANSWERS.md` as you work. Include the names of everyone who worked on the studio in your first answer, and number your responses so they are easy to match to the exercises.

1. List the names of the people who worked together on this studio.

2. SSH into `shell.cec.wustl.edu` using your WUSTL Key credentials, then use `qlogin` to connect to one of the Linux Lab machines and confirm that the version of `g++` there is correct, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

   Clone your `studio11` repo using SSH:

   ```bash
   git clone git@github.com:cse4208-wustl/studio11.git
   cd studio11
   ```

   The repo already includes a starter `studio11.cpp` and a `Makefile`. Update them as needed so the repo builds an executable named `studio11`.

   Add a header file declaring a class that initially has only one private member variable of type `std::string`.

   In `main`, declare one object of that class type on the stack, copy-construct another object of the same type from it, and return a descriptively named symbol whose value is `0` to indicate success.

   Build and run your program. In your answers, show your declaration of the class.

3. Declare a public copy constructor for your class in the header file and define it in a new source file. The copy constructor should initialize the private member variable from the source object's private member variable in the member-initialization list, and in the body it should print the addresses of the object being constructed and the object from which it is being copy-constructed.

   Try to build your program. It should fail because the presence of the copy constructor prevents the compiler from automatically synthesizing a default constructor.

   Then declare a default constructor using `= default` so the compiler synthesizes one for you.

   Compile and run your program. In your answers, show the output it produced.

4. Modify the body of your copy constructor so that, in addition to the addresses of the objects involved, it also prints the private member variable of the object being constructed.

   Declare and define a constructor that takes a `const std::string &` and uses it to initialize the private member variable in the member-initialization list. In the body of that constructor, have it print the address and private member variable of the object being constructed.

   Also declare and define a destructor that prints the address and private member variable of the object being destroyed.

   In `main`, replace the line that default-constructs an object of the class type with one that initializes it from a C-style string such as `"hello"`.

   Compile and run your program. In your answers, show the output it produced.

5. Declare and define a public assignment operator for your class that takes a `const` reference to an object of the class type and returns a non-`const` reference to an object of the class type.

   That operator should:

   - print the address and private member variable of the object on which it is called
   - print the address and private member variable of the object that was passed in
   - assign its private member variable from the private member variable of the object that was passed in
   - return a non-`const` reference to the object on which it was invoked

   In `main`, use different C-style strings to construct at least three different objects and then, in a single statement, use the assignment operator twice to assign the first object to the second object and the second object to the third object, for example:

   ```cpp
   c = b = a;
   ```

   Compile and run your program. In your answers, show the output it produced.

6. Remove the contents of `main` except for the statement at the end that returns a success value.

   In `main`, declare a `std::unique_ptr` parameterized with your class type and initialize it with a call to `new` that dynamically constructs an object of your class type from a C-style string. Then declare another `std::unique_ptr` parameterized with your class type and attempt to initialize it from the first `std::unique_ptr`.

   Try to build your program. It should fail because the copy constructor for `std::unique_ptr` is deleted.

   Then modify the initialization of the second `std::unique_ptr` so that it wraps the first one with a call to `std::move`.

   Build and run your program. In your answers, show the output that was produced.

7. In the header and source files for your class, declare and define a public virtual member function that prints the object's address and its private member variable. Also modify the destructor declaration so that it is virtual as well.

   In the source file where you define `main`, define a function with `void` return type that takes a `std::unique_ptr` to your class type by value, and uses it to invoke the public virtual member function of the object to which the `std::unique_ptr` points.

   In `main`, pass the second `std::unique_ptr` into a call to the function you just defined.

   Try to build your program. It should fail again because the copy constructor for `std::unique_ptr` is deleted.

   Then modify the call so that it wraps the second `std::unique_ptr` with a call to `std::move`.

   Build and run your program. In your answers, show the output that was produced.

8. In the source file where you defined `main`, change the function you defined in the previous exercise so that it returns by value the `std::unique_ptr` that is passed into it.

   In `main`, assign the value returned by that function to the first `std::unique_ptr`, and then use the first `std::unique_ptr` to invoke the public virtual member function of the object to which it points.

   Build and run your program. In your answers, show the output the program produced.

## Deliverables

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
