.. title: auto keyword
.. slug: auto-keyword
.. date: 2021-03-03 21:28:15 UTC-07:00
.. tags: modern c++,auto, cpp
.. category: automatic type deduction
.. link: 
.. description: auto is one of the most important feature in modern c++
.. type: text
.. author: Gautam Sharma

Using auto
==========

With C++11 came a new keyword called ``auto``. This has made writing and reading programs so much simpler. For example previously we used to do something like this:

.. code-block:: cpp
   :number-lines:

   int x = 50;
   string s = "hello";

Now we can do something like this:

.. code-block:: cpp
   :number-lines:

   auto x = 50;
   auto s = "hello";

Well you'll say that's basic stuff. What's the big deal? Well it's kind of a big deal. Let's say you are working with non primitive data types like ``std::vector``.
To get the iterator to the vector you needed to do something like:

.. code-block:: cpp
   :number-lines:

   std::vector<int>::iterator it = vector<int> my_vec {1,2,3};

Well that does not seem that bad. Let's see the next example. Here we are using ``std::map`` of ``vector<vector<int>> and std::vector<int>::iterator it``.

.. code-block:: cpp
   :number-lines:

   // my_map is defined as : std::map<vector<vector<int>>, std::vector<int>::iterator> my_map
   std::map<vector<vector<int>>, std::vector<int>::iterator>::iterator my_iter = my_map.begin();

Using ``auto`` it simplifies to:

.. code-block:: cpp
   :number-lines:

    auto my_iter = my_map.begin();

Using lambdas
-------------

Another great use of auto is with ``lambdas``.

.. code-block:: cpp
   :number-lines:

    std::function<int(int, int)> sum = [](int a, int b){return a+b;}; // without auto 
    auto sum = [](int a, int b){return a+b;}; // using auto

It is also great to use for automatically deducing argument types

.. code-block:: cpp
   :number-lines:

    auto sum = [](auto const a, auto const b){return a+b;}; // using auto

    // another example
    template<class A, class B>
    auto product(A a, B b){
        return A.value * B. value;
    }

Best use of ``auto``
--------------------

In my opinion the best use of ``auto`` is in initializing variables. It makes sure that every variable gets initialized. 

.. code-block:: cpp
   :number-lines:

    auto x; // will throw an error

    auto x = 1; // no error and a good practice too!

    string s = "Hello"
    int x = s.size(); // loss of data since unsigned int being converted to int
    auto x = s.size(); // no loss of data

Few caveats
-----------

Be very careful while working with references. ``auto`` in general is not able to deduce referenced return type. 

.. code-block:: cpp
   :number-lines:
   
    static int val = 5;
    int & get_val(){
        return val;
    }

    int main(){
        auto y = get_val();
        val = 6;
        cout<<y<<endl; // gives 5
                       // new value of val not propogated to y
    }

In the above program the data type of y is ``int`` and not ``&int`` as expected. The compiler is not able to deduce the return type as ``&int``. This issue is easily solved by using ``&auto`` instead
of ``auto``.

.. code-block:: cpp
   :number-lines:
   
    static int val = 5;
    int & get_val(){
        return val;
    }

    int main(){
        auto &y = get_val();
        val = 6;
        cout<<y<<endl; // gives 6
    }


For a more expansive overview visit `auto <link:https://en.cppreference.com/w/cpp/language/auto>`__. Another great `post <link:https://herbsutter.com/2013/08/12/gotw-94-solution-aaa-style-almost-always-auto/>`__ 
by `Herb Setter <link:https://herbsutter.com>`__

Please feel free to add any comments, opinion etc. below.

Signing out

-G