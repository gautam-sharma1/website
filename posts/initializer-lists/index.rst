.. title: Initializer Lists
.. slug: initializer-lists
.. date: 2021-03-23 20:12:55 UTC-07:00
.. tags: c++
.. category: 
.. link: 
.. description: uniform initialization is an integral part of modern c++
.. type: text

Uniform Initialization
======================


Before C++17
------------

Broadly speaking there are two main types of initialization in C++

.. code-block:: cpp
   :number-lines:

   vector<int> height{1};     // direct initialization
   vector<int> height = {1};  // copy initialization

The main difference between the two is the that ``direct initialization`` directly constructs the object ``{1}`` and assigns it to ``height`` whereas 
copy initialization first constructs the object ``{1}`` (think an extra default constructor call) and then assigns it to ``height``. Let me give you a solid
example.

Another important thing to note is that copy initialization is more strict in terms of implict conversions than direct initialization. It does not allow any
implicit conversion. 

The STL data structures like ``vector`` also make use of direct initialization as follows:

.. code-block:: cpp
   :number-lines:

   std::vector<int> height{1,2,3,4}; // allowed

It is able to do it since it has a constructor that takes in arguments of type ``std::initializer_list<T>``. 

Important
+++++++++

The constructor of type ``std::initializer_list<T>`` always gets precedence. Eg:

.. code-block:: cpp
   :number-lines:

   class Point{
   public:
    Point(double a, double b):_a(a),_b(b){};
    Point(std::initializer_list<double> pt){};

    private:
        double _a;
        double _b;
   };

   Point({1,2}) // calls constructor with  std::initializer_list<double>

Now you may ask that what's the difference between ``std::vector<double> height{5}`` and ``std::vector<double> height(5)``.
As discussed above calling ``std::vector<double> height{5}`` will implicitly call the constructor with ``std::initializer_list<double>``
and make a ``std::vector<double>`` containing single element 5. On the other hand ``std::vector<double> height(5)`` will construct a 
``std::vector<double>`` of size 5;

Another thing to note is the above mentioned strictness of implicit conversion during brace initialization. According to C++ standard
`paragraph 8.5.4 <link:http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4296.pdf>`__ braze initialization does not allow narrowing conversion. A narrowing conversion is defined as

.. code-block:: cpp
   :number-lines:

    int height{4.99}; // error
    int height{static_cast<int>(4.99)}; // ok
    double g = 9.81;
    float a{g}; //error


After C++ 17
------------

``C++17`` solves the above problem. In the new standard can use `auto` to solve some of the problems. 
For example:

.. code-block:: cpp
   :number-lines:

   auto height{42}; // int
   auto height = {4,2}; // std::initializer_list<T>
   auto d{4,2}; // error - too many values to unpack

In ``C++11`` all of the above triggered ``std::initializer_list<T>``


For a more expansive overview visit `initializer_list <link:https://en.cppreference.com/w/cpp/utility/initializer_list>`__. 

Please feel free to add any comments, opinion etc. below.

Signing out

-Gautam
